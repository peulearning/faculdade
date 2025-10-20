
## Parâmetros Essenciais para Ajustes

#### **`include_top` e `weights`**

Esses dois parâmetros trabalham em conjunto e são a base da transferência de aprendizado.

- **`include_top=False`**: Este é o ajuste **mais crucial**. `True` (padrão) mantém a camada final de classificação da rede, que foi treinada para as 1000 classes do ImageNet. Ao definir como `False`, você **remove essa camada final**. Isso permite que você adicione suas próprias camadas de classificação no topo, adaptadas ao seu problema (ex: classificar 10 tipos de flores em vez das 1000 classes originais).
    
- **`weights='imagenet'`**: Este parâmetro carrega os "pesos" (o conhecimento) que o modelo aprendeu ao ser treinado no enorme banco de dados ImageNet. Usar `'imagenet'` economiza um tempo imenso de treinamento e geralmente resulta em uma precisão muito maior, pois o modelo já sabe como identificar formas, texturas e padrões básicos.
    
    - **`weights=None`**: Use esta opção se quiser treinar o modelo do zero, sem nenhum conhecimento prévio. Isso só é recomendado se você tiver um conjunto de dados muito grande (milhões de imagens).
        

> **Resumo prático**: Para adaptar o modelo para sua tarefa, quase sempre você usará a combinação **`weights='imagenet'`** e **`include_top=False`**.


## Controle de Complexidade e Desempenho

Esses parâmetros permitem que você equilibre a precisão do modelo com o custo computacional (velocidade e uso de memória).

#### **`alpha` (Multiplicador de Largura)** 🧠

Este parâmetro controla a "largura" da rede, ou seja, o número de filtros em cada camada. É uma forma de tornar o modelo maior ou menor.

- **`alpha < 1.0`** (ex: `0.5`, `0.75`): Reduz proporcionalmente o número de filtros. O resultado é um modelo **mais leve e mais rápido**, ideal para aplicações em dispositivos móveis ou com hardware limitado. A contrapartida é uma possível queda na precisão.
    
- **`alpha = 1.0`** (padrão): Usa a arquitetura original do paper.
    
- **`alpha > 1.0`** (ex: `1.3`, `1.4`): Aumenta o número de filtros, criando um modelo **maior e computacionalmente mais caro**. Ele tem o potencial de ser mais preciso, mas será mais lento.
    

> **Quando ajustar**: Se o seu modelo estiver muito lento para a sua aplicação, experimente reduzir o `alpha`. Se você precisa de máxima precisão e tem poder computacional de sobra, pode tentar aumentá-lo.

#### **`input_shape`**

Define o tamanho (altura, largura, canais de cor) das imagens de entrada.

- Só pode ser definido se **`include_top=False`**. Se `include_top=True`, o tamanho é fixo em `(224, 224, 3)`.
    
- **Exemplo**: `input_shape=(128, 128, 3)` para imagens de 128x128 pixels com 3 canais de cor (RGB).
    
- **Impacto**: Imagens maiores podem conter mais detalhes e levar a uma melhor precisão, mas exigem mais memória e processamento. Imagens menores são mais rápidas de processar. O ideal é encontrar um equilíbrio. O mínimo recomendado é `32x32`.

## Parâmetros Adicionais

Estes são outros parâmetros úteis, principalmente quando `include_top=False`.

#### **`pooling`**

Controla como a saída do bloco convolucional final é processada antes de ser entregue às suas camadas personalizadas.

- **`None`** (padrão): A saída é o mapa de características 4D completo da última camada. Você mesmo precisaria achatá-lo (`Flatten`) ou aplicar pooling.
    
- **`'avg'`**: Aplica _Global Average Pooling_. Esta é a opção **mais comum e recomendada**. Ela calcula a média de cada mapa de características, resultando em um vetor 2D. É uma forma eficiente de resumir as informações espaciais.
    
- **`'max'`**: Aplica _Global Max Pooling_. Pega o valor máximo de cada mapa, focando nas características mais proeminentes.
    

#### **`classes`**

Este parâmetro só é relevante se você usar `include_top=True` e `weights=None` (treinando do zero). Ele define o número de neurônios na camada de saída. Por exemplo, `classes=10` para um problema de 10 classes.

## Exemplo : 


```
import keras

# 1. Carregar o modelo base sem a camada de classificação e com pesos pré-treinados
base_model = keras.applications.MobileNetV2(
    weights='imagenet',          # Usar o conhecimento prévio
    include_top=False,           # Remover a camada de 1000 classes
    input_shape=(150, 150, 3)    # Definir o tamanho da sua imagem
)

# 2. (Opcional) Congelar o modelo base para não destruir os pesos durante o treino inicial
base_model.trainable = False

# 3. Adicionar suas próprias camadas no topo
inputs = keras.Input(shape=(150, 150, 3))
x = base_model(inputs, training=False) # Passar a entrada pelo modelo base
x = keras.layers.GlobalAveragePooling2D()(x) # Aplicar pooling para criar um vetor
x = keras.layers.Dense(128, activation='relu')(x) # Uma camada densa intermediária
outputs = keras.layers.Dense(2, activation='softmax')(x) # Camada final para 2 classes

# 4. Criar o modelo final
model = keras.Model(inputs, outputs)

# Agora você pode compilar e treinar este novo modelo!
model.compile(optimizer='adam', loss='categorical_crossentropy', metrics=['accuracy'])

```

Link : https://colab.research.google.com/drive/1NI_MpCluA6Nu-ug-N3FU9IOrm5EEzfcl?usp=sharing