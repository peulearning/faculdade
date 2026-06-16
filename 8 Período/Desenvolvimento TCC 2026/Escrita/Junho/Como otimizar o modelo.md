
```
MobileNetV2 (ImageNet)
↓
GlobalAveragePooling2D↓Dropout(0.3)
↓
Dense(128, relu)
↓
Dropout(0.3)
↓
Dense(2, softmax)
```

e o treinamento está sendo feito em duas fases:

- Fase 1: Base congelada
- Fase 2: Fine-tuning das últimas 30 camadas
- LR = 1e-3 → 1e-5

Isso já está relativamente bem configurado.

---

# O que eu faria primeiro

## 1) Liberar mais camadas da MobileNetV2

Hoje:

```
for layer in base_model.layers[:-30]:    layer.trainable = False
```

Você está treinando apenas as 30 últimas camadas.

Experimente:

```
for layer in base_model.layers[:-50]:    layer.trainable = False
```

ou até

```
for layer in base_model.layers[:-70]:    layer.trainable = False
```

Em problemas médicos, frequentemente as características aprendidas no ImageNet não são ideais para tecidos, pele e feridas.

Liberar mais camadas costuma aumentar:

- Recall
- F1-score
- Sensibilidade

---

# 2) Ajustar o Learning Rate do Fine-Tuning

Hoje:

```
Adam(learning_rate=1e-5)
```

Experimente:

```
Adam(learning_rate=5e-6)
```

ou

```
Adam(learning_rate=1e-6)
```

Muitas vezes o modelo já está próximo do ótimo e um LR menor permite refinamentos mais finos.

---

# 3) Melhorar a cabeça classificadora

Hoje:

```
Dense(128)
```

Experimente:

```
x = layers.GlobalAveragePooling2D()(x)
x = layers.BatchNormalization()(x)
x = layers.Dense(    256,    activation='relu',kernel_regularizer=tf.keras.regularizers.l2(0.001))(x)
x = layers.Dropout(0.5)(x)
outputs = layers.Dense(2, activation='softmax')(x)
```

Isso costuma funcionar muito bem em datasets médicos pequenos.

---

# 4) Utilizar Label Smoothing

Uma técnica extremamente útil em classificação médica.

Troque:

```
loss='categorical_crossentropy'
```

por

```
loss=tf.keras.losses.CategoricalCrossentropy(    label_smoothing=0.1)
```

Benefícios:

- reduz overfitting
- melhora generalização
- aumenta F1-score

---

# 5) Aumentar a resolução

Hoje:

```
224x224
```

Feridas possuem muitos detalhes finos.

Teste:

```
256x256
```

ou

```
300x300
```

A MobileNetV2 suporta isso.

Frequentemente vemos:

- +2%
- +5%

de ganho em acurácia.

---

# 6) Melhorar a augmentação

Hoje você usa:

```
rotation_range=20zoom_range=0.1horizontal_flip=True
```

Eu adicionaria:

```
brightness_range=[0.8,1.2]channel_shift_range=20
```

porque iluminação é um dos maiores problemas em imagens de feridas.

---

# 7) Testar CLAHE

Você comentou anteriormente sobre CLAHE.

Na sua pesquisa eu colocaria como prioridade:

### Ordem dos testes

1. RGB Original
2. Escala Cinza
3. CLAHE
4. Canny
5. CLAHE + Escala Cinza

Dos pré-processamentos, o CLAHE é o que tem maior chance de gerar ganho real.

---

# 8) Treinar mais épocas

Hoje:

```
15 épocas+25 épocas
```

Total ≈ 40 épocas.

Eu faria:

```
20 épocas+40 épocas
```

com:

```
EarlyStopping(    patience=12)
```

e deixaria o modelo decidir quando parar.

---

# 9) A mudança que mais tende a ajudar

Se eu tivesse que escolher apenas uma modificação para seu TCC agora:

### Experimento A

```
Label Smoothing
```

### Experimento B

```
Fine-tuning das últimas 50-70 camadas
```

### Experimento C

```
CLAHE
```

São as três alterações com maior probabilidade de aumentar:

- Accuracy
- Precision
- Recall
- F1-score

sem mudar a arquitetura.


---


- MobileNetV2 atual (baseline)
- Fine-tuning 50 camadas
- Fine-tuning 70 camadas
- Fine-tuning 70 camadas + LR = 5e-6
- Fine-tuning 70 camadas + L2 + Dropout 0.5

---  

Como atacar esses pontos ? 


### 1. Data Augmentation "Agressivo"

Quando você diz "agressivo", significa criar variações substanciais nas imagens de treino para que o modelo aprenda características robustas, e não apenas decore as imagens. Se o seu conjunto de dados é pequeno, isso é obrigatório.

#### Onde ajustar no seu código:

Você deve modificar os parâmetros dentro do seu `ImageDataGenerator` (ou na camada de pré-processamento, se estiver usando a API mais nova do Keras).

#### O que fazer (Exemplo Prático):

Em vez de usar apenas um `horizontal_flip=True`, tente uma combinação mais forte:

Python

```
from tensorflow.keras.preprocessing.image import ImageDataGenerator

# Configuração agressiva de Data Augmentation para TREINO
train_datagen = ImageDataGenerator(
    rescale=1./255,               # Essencial: normalizar para [0,1]
    rotation_range=40,            # Rotação aleatória de até 40 graus
    width_shift_range=0.2,        # Deslocamento horizontal (até 20% da largura)
    height_shift_range=0.2,       # Deslocamento vertical (até 20% da altura)
    shear_range=0.2,              # Cisalhamento (distorção)
    zoom_range=0.3,               # Zoom aleatório (entre 70% e 130%)
    horizontal_flip=True,         # Inversão horizontal
    vertical_flip=False,          # Inversão vertical (depende do seu problema - ex: órgãos: não, células: sim)
    fill_mode='nearest',          # Como preencher pixels vazios criados por rotação/deslocamento
    brightness_range=[0.7, 1.3]  # Variação de brilho aleatória
)

# Para VALIDAÇÃO e TESTE, mantenha APENAS o rescale. Nunca faça augmentation neles.
validation_datagen = ImageDataGenerator(rescale=1./255)
```

**Nota:** Acompanhe a `loss` de treino. Se ela estiver muito alta e não descer, seu augmentation pode estar _tão_ agressivo que o modelo não consegue aprender o padrão básico. Se for o caso, suavize um pouco.

### 2. Liberar Camadas Congeladas (Fine-Tuning Progressivo)

Esta é a técnica de transfer learning para "adaptar" os recursos genéricos que o MobileNetV2 aprendeu (bordas, texturas básicas) para os recursos específicos do seu problema.

O segredo aqui é **não descongelar tudo de uma vez** e **usar uma Taxa de Aprendizado (Learning Rate) muito baixa**.

#### Onde ajustar no seu código:

Logo após definir seu modelo base e adicionar suas camadas no topo.

#### O que fazer (A Estratégia Vencedora):

**Estágio 1: Treinar apenas o Topo (Você provavelmente já fez isso)**

1. Congele todo o modelo base: `base_model.trainable = False`.
    
2. Compile o modelo.
    
3. Treine por algumas épocas para que as novas camadas de saída se adaptem.
    

**Estágio 2: Fine-Tuning (Onde você quer chegar)**

1. **Descongele apenas as últimas camadas** do modelo base. Comece com as últimas 20 ou 30.
    

Python

```
# Supondo que você já tenha o 'model' e o 'base_model' (MobileNetV2)

# 1. Descongelar o modelo base inteiro para poder escolher quais camadas ativar
base_model.trainable = True

# 2. Vamos ver quantas camadas o MobileNetV2 tem
#print(f"Número de camadas no MobileNetV2: {len(base_model.layers)}") # Ex: ~154

# 3. Congelar as camadas iniciais. Vamos liberar apenas as últimas ~20.
# Uma abordagem comum é congelar até uma camada específica ou usar índices.
fine_tune_at = 130 # Exemplo: congela da camada 0 até a 129

# Congela todas as camadas ANTES da marca 'fine_tune_at'
for layer in base_model.layers[:fine_tune_at]:
    layer.trainable = False
```

2. **RECOMPILAR o modelo (Crucial!)**: Após alterar o `trainable` das camadas, você _deve_ recompilar o modelo para que as mudanças surtam efeito.
    
3. **Usar um Learning Rate MUITO baixo**: O modelo já está pré-treinado. Se você usar um LR alto, você "destruirá" o que ele aprendeu. Use um valor 10x a 100x menor que o usado no Estágio 1.
    

Python

```
from tensorflow.keras.optimizers import Adam, SGD

# Recompilação para Fine-Tuning
model.compile(
    # Use Adam com LR bem baixo ou SGD com momentum
    optimizer=Adam(learning_rate=1e-5), # Ex: 0.00001
    # optimizer=SGD(learning_rate=0.0001, momentum=0.9),
    loss='binary_crossentropy', # Ou categorical_crossentropy se tiver 2+ classes
    metrics=['accuracy']
)
```

### 3. Ajuste nos Callbacks

Você mencionou que já os acertou, mas vale a pena revisar se eles estão configurados para suportar o Data Augmentation agressivo e o Fine-Tuning.

#### Onde ajustar:

Na lista de `callbacks` passada para o `model.fit()`.

#### O que conferir:

1. **`EarlyStopping`**: Como o augmentation agressivo torna o aprendizado mais lento, você pode precisar aumentar a `patience` (paciência). Em vez de parar após 3 ou 5 épocas sem melhora na `val_loss`, aumente para **10 ou 15**.
    
2. **`ReduceLROnPlateau`**: Este é seu melhor amigo no fine-tuning. Ele deve ter uma paciência menor que o `EarlyStopping` (ex: `patience=5`) para tentar reduzir o LR e "espremer" o máximo de performance antes que o treinamento seja interrompido.
    
3. **`ModelCheckpoint`**: Certifique-se de salvar apenas o melhor modelo (`save_best_only=True`) monitorando a `val_loss` (ou `val_accuracy`).
    

Python

```
from tensorflow.keras.callbacks import EarlyStopping, ReduceLROnPlateau, ModelCheckpoint

my_callbacks = [
    EarlyStopping(
        monitor='val_loss',
        patience=15,          # Aumentado para lidar com o aprendizado mais lento
        restore_best_weights=True # Garante que você termine com os pesos do melhor modelo
    ),
    ReduceLROnPlateau(
        monitor='val_loss',
        factor=0.2,           # Reduz o LR para 1/5 do atual
        patience=7,           # Paciência menor que EarlyStopping
        min_lr=1e-7           # Limite inferior para o LR não zerar
    ),
    ModelCheckpoint(
        'best_model_tuned.h5', # Nome do arquivo para salvar o melhor modelo
        monitor='val_loss',
        save_best_only=True   # Salva apenas se for melhor que o anterior
    )
]
```



--- 



# 1) CLASS WEIGHTS

## Onde inserir?

Logo após criar o `train_generator`.

### Nova célula

```
from sklearn.utils.class_weight import compute_class_weightimport numpy as np# Classes do generatorclasses = train_generator.classes# Calcular pesosweights = compute_class_weight(    class_weight='balanced',    classes=np.unique(classes),    y=classes)class_weights = dict(enumerate(weights))print("Pesos das classes:")print(class_weights)
```

---

## Onde usar?

Na sua célula de treinamento.

Atualmente:

```
history = model.fit(    train_generator,    validation_data=val_generator,    epochs=15,    callbacks=callbacks)
```

Trocar para:

```
history = model.fit(    train_generator,    validation_data=val_generator,    epochs=15,    callbacks=callbacks,    class_weight=class_weights)
```

---

Também na fase de Fine-Tuning:

```
fine_history = model.fit(    train_generator,    validation_data=val_generator,    epochs=25,    callbacks=callbacks,    class_weight=class_weights)
```

---

# 2) BATCH NORMALIZATION

## Onde alterar?

Na sua célula de arquitetura.

### Atual

```
inputs = tf.keras.Input(shape=(224, 224, 3))x = base_model(inputs, training=False)x = layers.GlobalAveragePooling2D()(x)x = layers.Dropout(0.3)(x)x = layers.Dense(128, activation='relu')(x)x = layers.Dropout(0.3)(x)outputs = layers.Dense(    train_generator.num_classes,    activation='softmax')(x)
```

---

### Sugestão

```
inputs = tf.keras.Input(shape=(224, 224, 3))x = base_model(inputs, training=False)x = layers.GlobalAveragePooling2D()(x)x = layers.Dense(256, activation='relu')(x)x = layers.BatchNormalization()(x)x = layers.Dropout(0.4)(x)outputs = layers.Dense(    train_generator.num_classes,    activation='softmax')(x)model = models.Model(inputs, outputs)
```

---

## Justificativa para o TCC

Você pode escrever:

> A camada Batch Normalization foi adicionada para estabilizar a distribuição das ativações durante o treinamento, reduzindo o risco de overfitting e acelerando a convergência do modelo.

---

# 3) LABEL SMOOTHING

## Onde alterar?

Na compilação.

---

### Atual

```
model.compile(    optimizer=Adam(learning_rate=1e-3),    loss='categorical_crossentropy',    metrics=['accuracy'])
```

---

### Novo

```
model.compile(    optimizer=Adam(learning_rate=1e-3),    loss=tf.keras.losses.CategoricalCrossentropy(        label_smoothing=0.1    ),    metrics=['accuracy'])
```

---

## Também no Fine-Tuning

Troque:

```
model.compile(    optimizer=Adam(learning_rate=1e-5),    loss='categorical_crossentropy',    metrics=['accuracy'])
```

por:

```
model.compile(    optimizer=Adam(learning_rate=1e-5),    loss=tf.keras.losses.CategoricalCrossentropy(        label_smoothing=0.1    ),    metrics=['accuracy'])
```

---

## Justificativa

Como Pressure e Diabetic apresentam características visuais semelhantes, o Label Smoothing reduz a superconfiança do modelo e melhora a capacidade de generalização.

---

# 4) FINE-TUNING MAIS FORTE

## Atualmente

```
for layer in base_model.layers[:-30]:    layer.trainable = False
```

---

## Teste 1

```
for layer in base_model.layers[:-40]:    layer.trainable = False
```

---

## Teste 2

```
for layer in base_model.layers[:-50]:    layer.trainable = False
```

---

⚠️ Eu não passaria de 50 camadas para um dataset de apenas 222 imagens.

---

# 5) MELHORAR CALLBACKS

### Atual

```
ReduceLROnPlateau(    monitor='val_loss',    patience=4,    factor=0.2,    verbose=1)
```

---

### Sugestão

```
ReduceLROnPlateau(    monitor='val_loss',    patience=3,    factor=0.5,    min_lr=1e-7,    verbose=1)
```

---

# Arquitetura Final que eu testaria primeiro

```
inputs = tf.keras.Input(shape=(224, 224, 3))x = base_model(inputs, training=False)x = layers.GlobalAveragePooling2D()(x)x = layers.Dense(256, activation='relu')(x)x = layers.BatchNormalization()(x)x = layers.Dropout(0.4)(x)outputs = layers.Dense(    train_generator.num_classes,    activation='softmax')(x)model = models.Model(inputs, outputs)
```

Compilação:

```
model.compile(    optimizer=Adam(learning_rate=1e-3),    loss=tf.keras.losses.CategoricalCrossentropy(        label_smoothing=0.1    ),    metrics=['accuracy'])
```

Fine-Tuning:

```
base_model.trainable = Truefor layer in base_model.layers[:-40]:    layer.trainable = False
```

Treinamento:

```
history = model.fit(    train_generator,    validation_data=val_generator,    epochs=15,    callbacks=callbacks,    class_weight=class_weights)
```

Essa combinação é a que eu considero mais coerente com o seu contexto atual de TCC: **dataset pequeno, classes visualmente semelhantes e possível desbalanceamento**, sem aumentar demais a complexidade do modelo nem elevar excessivamente o risco de overfitting.