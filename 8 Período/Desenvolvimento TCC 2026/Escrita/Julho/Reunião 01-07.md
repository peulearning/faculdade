# Planejamento de Experimentos - HealScan

**Data da Reunião:** **Objetivo:** Otimização da pipeline de treinamento, avaliação de novos splits e reestruturação de classes do dataset.

---

## 🧪 Fase 1: Sem Intervenção no Código (Ajuste de Configurações)

*Modificações focadas apenas na divisão dos dados e remoção de classes.*

- [x] **[[Exp 01 - Avaliação de Splits (Sem Validação)]]**
  - Remover conjunto de validação, focando apenas em Treino e Teste.
  - Avaliar o desempenho do modelo nas proporções:
    - [x] 70 / 30
    - [x] 80 / 20
    - [x] 75 / 25

- [x] **[[Exp 02 - Redução para 5 Classes]]**
  - Remover a classe de feridas **Cirúrgicas** do dataset.
  - Executar treinamento e comparar métricas com o baseline anterior.

---

## 🛠️ Fase 2: Com Intervenção no Código (Adaptação de Classes)

*Modificações no pipeline para adaptar as lógicas das feridas Cirúrgicas e por Pressão.*

- [x] **[[Exp 03 - Avaliação Completa com 6 Classes]]**
  - Reintegrar e adaptar as classes usando o código abaixo.

```
import tensorflow as tf
from tensorflow.keras import layers, models

# Entrada com a resolução atualizada
inputs = tf.keras.Input(shape=(320, 320, 3)) 
x = base_model(inputs, training=False)

# --- NOVO: BLOCO DE REFINAMENTO LOCAL ---
# Feridas cirúrgicas exigem foco em linhas e pontos de sutura. 
# Feridas por pressão exigem foco em profundidade. Uma convolução 1x1 
# ajuda a recombinar os canais do base_model focando nessas nuances.
x_refined = layers.Conv2D(256, (1, 1), padding='same', activation='relu')(x)

# --- DUAL POOLING NO BLOCO REFINADO ---
avg_pool = layers.GlobalAveragePooling2D()(x_refined) # Captura a extensão da lesão (grau da escara)
max_pool = layers.GlobalMaxPooling2D()(x_refined)     # Captura pontos de sutura/bordas cirúrgicas
x_concat = layers.Concatenate()([avg_pool, max_pool])

# --- CABEÇA DE CLASSIFICAÇÃO ADAPTADA COM REGULARIZAÇÃO ---
# Aumentamos o dropout levemente para evitar que o modelo decore o cenário cirúrgico limpo
x_dense = layers.Dense(512)(x_concat)
x_dense = layers.BatchNormalization()(x_dense)
x_dense = layers.Activation('relu')(x_dense)
x_dense = layers.Dropout(0.45)(x_dense) # Aumento sutil contra overfitting

x_dense = layers.Dense(256)(x_dense)
x_dense = layers.BatchNormalization()(x_dense)
x_dense = layers.Activation('relu')(x_dense)
x_dense = layers.Dropout(0.35)(x_dense)

# Saída do modelo
outputs = layers.Dense(train_generator.num_classes, activation='softmax')(x_dense)

model = models.Model(inputs, outputs)

```

---

## 📈 Fase 3: Expansão do Dataset

- [ ] **[[Exp 04 - Aumento da Base de Background e Normal]]**
  - Condição: Executar *apenas* após a conclusão e análise dos experimentos 1 ao 3.
  - Adicionar novas amostras de background.
  - Adicionar novas amostras da classe Normal.
  - Reavaliar o balanceamento geral das classes antes de treinar.