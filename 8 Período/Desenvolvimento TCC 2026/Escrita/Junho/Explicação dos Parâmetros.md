
## 1) Carregando a base pré-treinada (Transfer Learning)


```python
base_model = tf.keras.applications.MobileNetV2(
    input_shape=(224, 224, 3),
    include_top=False,
    weights='imagenet'
)
base_model.trainable = False  # Etapa 1: congelar a base
```

### Explicação dos parâmetros:

- **`tf.keras.applications.MobileNetV2`**
    
    - Define qual rede pré-treinada será usada.
    - MobileNetV2 é leve e eficiente, bom para classificação de imagens.
- **`input_shape=(224, 224, 3)`**
    
    - Tamanho que a rede vai receber:
        - **224 x 224**: altura e largura das imagens.
        - **3**: número de canais (RGB).
    - Importante: suas imagens precisam ser redimensionadas para esse formato pelo seu gerador (`train_generator` e `val_generator`), caso contrário dará erro.
- **`include_top=False`**
    
    - Remove a “cabeça final” original da MobileNetV2 (camadas de classificação).
    - Isso é feito porque você vai criar **uma nova cabeça** para o seu problema (quantidade de classes do seu dataset).
- **`weights='imagenet'`**
    
    - Carrega pesos treinados no dataset **ImageNet**.
    - Ou seja: a rede já sabe extrair características gerais (bordas, texturas, padrões), economizando treino.
- **`base_model.trainable = False`**
    
    - Congela a base na **Etapa 1**.
    - Assim, durante o primeiro treinamento, **não atualiza os pesos** da MobileNetV2: só treina a nova cabeça que você adicionou.


## 2) Criando a nova cabeça de classificação


```python
inputs = tf.keras.Input(shape=(224, 224, 3))
x = base_model(inputs, training=False)
x = layers.GlobalAveragePooling2D()(x)
x = layers.Dropout(0.5)(x)
x = layers.Dense(128, activation='relu')(x)
x = layers.BatchNormalization()(x)
x = layers.Dropout(0.5)(x)
outputs = layers.Dense(train_generator.num_classes, activation='softmax')(x)

model = models.Model(inputs, outputs)
```

### Explicação detalhada:

#### a) Entrada do modelo

- **`tf.keras.Input(shape=(224, 224, 3))`**
    - Define a forma esperada da entrada.
    - Deve bater com `input_shape` da MobileNetV2.

#### b) Passando pela base congelada

- **`x = base_model(inputs, training=False)`**
    - Isso força o comportamento “de inferência” para camadas como BatchNorm e Dropout dentro da base.
    - Como você congelou a base, isso ajuda a manter consistência.

#### c) Redução espacial (transformar mapas em vetor)

- **`layers.GlobalAveragePooling2D()`**
    - Recebe um tensor do tipo `(altura, largura, canais)` e transforma em um vetor com tamanho igual ao número de canais.
    - Em vez de usar flatten (que aumenta parâmetros), o GlobalAveragePooling2D reduz dimensionalidade de forma mais eficiente.

#### d) Dropout (redução de overfitting)

- **`layers.Dropout(0.5)`**
    - Remove aleatoriamente **50% das ativações** durante o treino.
    - Parâmetro **0.5**:
        - valores mais altos → mais regularização (mas pode prejudicar se ficar demais).
    - Você usa duas vezes:
        - uma antes da Dense
        - outra após BatchNorm

#### e) Camada totalmente conectada (classificador intermediário)

- **`layers.Dense(128, activation='relu')`**
    - **128**: número de neurônios.
    - **ReLU**: função de ativação.
    - Serve para aprender uma representação “mais específica” do seu problema.

#### f) Normalização das ativações

- **`layers.BatchNormalization()`**
    - Normaliza as ativações para estabilizar o treino e melhorar a convergência.
    - Ajuda especialmente quando há camadas Dense e dropout.

#### g) Camada final de classificação

- **`layers.Dense(train_generator.num_classes, activation='softmax')`**
    - **`train_generator.num_classes`**:
        - quantidade de classes do seu dataset.
    - **Softmax**:
        - transforma saídas em probabilidades (soma 1).
        - compatível com `categorical_crossentropy` quando seus rótulos são one-hot.

#### h) Montando o modelo final

- **`models.Model(inputs, outputs)`**
    - Cria o grafo computacional completo:
        - entrada → base MobileNetV2 → pooling → dropout/dense/bn → softmax.

## 3) Callbacks: EarlyStopping e ReduceLROnPlateau



```python
from tensorflow.keras.callbacks import EarlyStopping, ReduceLROnPlateau

callbacks = [
    EarlyStopping(
        monitor='val_loss',
        patience=10,
        restore_best_weights=True
    ),
    ReduceLROnPlateau(
        monitor='val_loss',
        patience=4,
        factor=0.2,
        verbose=1
    )
]
```


### Explicação dos parâmetros:

#### a) EarlyStopping

- **`monitor='val_loss'`**
    
    - Observa a **loss de validação**.
    - Motivo: validação mede generalização (evita overfitting).
- **`patience=10`**

    - Aguarda **10 épocas** sem melhora.
    - Se `val_loss` não melhorar dentro desse período, para o treino.
- **`restore_best_weights=True`**
    
    - Ao parar, ele volta automaticamente para os pesos da **época com melhor `val_loss`**.
    - Isso evita ficar com um modelo que “piorou” depois.

#### b) ReduceLROnPlateau

- **`monitor='val_loss'`**
    
    - Também olha `val_loss`.
- **`patience=4`**
    
    - Após **4 épocas** sem melhora, ele reduz o learning rate.
- **`factor=0.2`**
    
    - O learning rate é multiplicado por 0.2.
    - Ou seja: reduz para **20% do valor atual** (≈ “fica 5 vezes menor”).
- **`verbose=1`**
    
    - Imprime informações quando o LR for reduzido.

## 4) Treinamento Fase 1: treinar só a cabeça



```python
history = model.fit(
    train_generator,
    validation_data=val_generator,
    epochs=15,
    callbacks=callbacks
)
```


### Explicação dos parâmetros:

- **`train_generator`**
    
    - Fluxo de treino que gera imagens e labels.
    - Normalmente criado com `ImageDataGenerator.flow_from_directory(...)` ou similar.
- **`validation_data=val_generator`**
    
    - Usado para calcular `val_loss` e `val_accuracy` ao final de cada época.
- **`epochs=15`**
    
    - Limite máximo de épocas.
    - Mas pode parar antes por causa do `EarlyStopping`.
- **`callbacks=callbacks`**
    
    - Aplica EarlyStopping e ReduceLROnPlateau.


## 5) Fine-tuning (Fase 2): destravar parte da base



```python
base_model.trainable = True
for layer in base_model.layers[:-30]:
    layer.trainable = False
```


### Explicação:

- **`base_model.trainable = True`**
    
    - Permite que a base seja treinada (pesos podem mudar).
    -
- **`for layer in base_model.layers[:-30]: layer.trainable = False`**
    
    - `base_model.layers[:-30]` = todas as camadas **exceto as últimas 30**.
    -
    - Então você:
        - **congela** a maior parte da rede
        - **deixa treinável** apenas as **últimas 30 camadas**
        
    - Motivo: as primeiras camadas aprendem features mais genéricas (bordas/texturas) e normalmente não precisam mudar tanto.

---

## 6) Compilando o modelo na Fase 2 (com LR bem baixo)


```python
model.compile(
    optimizer=Adam(learning_rate=1e-5),
    loss='categorical_crossentropy',
    metrics=['accuracy']
)
```


### Explicação:

- **`optimizer=Adam(learning_rate=1e-5)`**
    
    - Otimizador Adam.
    - **learning_rate=1e-5**:
        - bem baixo para fine-tuning (evita destruir o que foi aprendido no ImageNet).
        - quanto menor, mais “cuidadoso” o ajuste.
        
- **`loss='categorical_crossentropy'`**
    
    - Loss para **classificação multiclasse** com rótulos one-hot.
    - Funciona bem com `softmax`.

- **`metrics=['accuracy']`**
    
    - Métrica adicional para acompanhar desempenho durante treino/validação.

---

## 7) Treinamento Fase 2: fine-tuning


```python
fine_history = model.fit(
    train_generator,
    validation_data=val_generator,
    epochs=25,
    callbacks=callbacks
)
```

### Explicação:

- Treina por até **25 épocas**, com os mesmos callbacks.
- Agora:
    - base não está totalmente congelada (apenas parte)
    - então o modelo ajusta melhor às suas classes específicas.

---

# Resumo do “porquê” (fluxo geral)

1. **Congela a MobileNetV2** e treina só a nova cabeça (fase rápida, aprende o padrão das suas classes).
2. **Destrava parte final da base** para ajustar características mais específicas ao seu dataset (fine-tuning).
3. Usa **EarlyStopping** para parar quando melhorar parar.
4. Usa **ReduceLROnPlateau** para baixar LR quando o modelo estagnar.