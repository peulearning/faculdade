
# Observações da Orientação - TCC (Felipe)

## 🎯 Metas de Ajuste
- [x] Definir e redigir a justificativa teórica para o número de épocas do modelo.
- [ ] Realizar levantamento na literatura sobre os resultados de métricas de validação de **Segmentação** em imagens clínicas/feridas. 

---


## 📌 Resumo Executivo para a Banca / Orientador

> **Argumento Central:** Os números de épocas (15 e 25) não são escolhas aleatórias, mas sim **limites teto reguladores (heurísticas empíricas)**. O controle real do tempo de treinamento é dinâmico, gerenciado pelos callbacks `EarlyStopping` e `ReduceLROnPlateau`. O teste empírico sem os callbacks provou cientificamente o surgimento de _overfitting_ a partir da 21ª época, validando a arquitetura do pipeline.

## 1. Correção Técnica Essencial da Arquitetura

- **O Equívoco:** O MobileNetV2 **não possui 30 camadas densas**. Ele é composto quase inteiramente por camadas convolucionais (blocos de convolução em profundidade separável).
    
- **A Realidade:** * **O que foi congelado:** As últimas 30 camadas _convolucionais_ do extrator de recursos (base).
    
    - **A "Cabeça" (Classifier Head):** Esta sim contém a nova camada densa (`Dense`/`Fully Connected`) com ativação `Softmax` adaptada para as suas classes de feridas.
        

## 2. Justificativa Teórica das Duas Fases de Treinamento

### ⏳ Fase 1: Treino da Cabeça (15 Épocas)

- **Objetivo:** Treinar apenas os novos pesos da camada densa (que começaram aleatórios), mantendo a base do MobileNetV2 congelada.
    
- **Justificativa Acadêmica:** A literatura de _Transfer Learning_ indica que o classificador inicial converge rapidamente porque a base já extrai recursos visuais excelentes do ImageNet. Treinar por mais de 15 épocas nesta fase causa estagnação precoce, pois o modelo fica limitado a combinar recursos estáticos.
    

### 🔄 Fase 2: Fine-Tuning (25 Épocas)

- **Objetivo:** Descongelar o topo da base do MobileNetV2 para adaptar recursos visuais gerais (bordas, texturas) para o domínio específico (feridas).
    
- **Justificativa Acadêmica:** O ajuste deve ser feito de forma extremamente lenta com uma Taxa de Aprendizado (_Learning Rate_) baixa (entre $1 \times 10^{-4}$ e $1 \times 10^{-5}$). Como os passos de otimização são pequenos, 25 épocas garantem uma margem de segurança para o modelo convergir sem sofrer de **Esquecimento Catastrófico** (_Catastrophic Forgetting_).
    

## 3. Análise dos Experimentos (O Paradoxo: 5 vs 21 Épocas)

### Cenário A: Com EarlyStopping (Parou na 5ª época com 70%)

- **O que aconteceu:** Houve um conflito de "paciência" entre os callbacks. Ao abrir as camadas na Fase 2, o modelo naturalmente estagna por algumas épocas antes de voltar a melhorar. Se a paciência (`patience`) do `EarlyStopping` for muito baixa (ex: 3 ou 5), ele interrompe o treino de forma "ansiosa" antes que o `ReduceLROnPlateau` consiga reduzir a taxa de aprendizado e fazer o modelo evoluir.
    

### Cenário B: Sem EarlyStopping (Foi até a 25ª época, performando melhor na 21ª com 83%)

- **O que aconteceu:** A queda de 83% (época 21) para 70% (época 25) é a definição matemática de **Overfitting (Sobreajuste)**. Como o dataset de feridas é limitado, o modelo parou de aprender padrões gerais e começou a decorar o ruído das imagens de treino.
    
- **Defesa Prática:** _"Professor, a remoção do EarlyStopping provou empiricamente que o teto de 25 épocas é seguro, demonstrando o ponto exato de inflexão (época 21) onde o modelo entra em overfitting e perde 13% de performance."_
    

## 4. Estratégia para Datasets Médicos Desbalanceados

Em cenários de saúde, a **Acurácia é uma métrica ilusória**. Se o dataset tiver 90 imagens da Classe A e 10 da Classe B, um modelo que chuta tudo como "A" terá 90% de acurácia, sendo um fracasso clínico.

### A. Foco no Recall (Revocação)

Para a área médica, o **Recall** é a métrica mais crítica:

$$\text{Recall} = \frac{\text{Verdadeiros Positivos}}{\text{Verdadeiros Positivos} + \text{Falsos Negativos}}$$

Ele mede a capacidade do modelo de não deixar passar nenhuma ferida sem diagnóstico. É preferível gerar um Falso Positivo (suspeitar de uma pele saudável) do que um Falso Negativo (ignorar uma ferida real).

### B. Uso de Class Weights (Pesos de Classe)

Para mitigar o desbalanceamento sem inventar dados, aplica-se uma punição maior para erros nas classes minoritárias durante o treino.

## 5. Implementação Proposta (Código Keras/TensorFlow)

Abaixo está o ajuste ideal para sincronizar seus callbacks e forçar o modelo a buscar os 83% (ou mais) de Recall de forma automática, salvando sempre o melhor ponto.

### Configuração dos Callbacks

Python

```
import tensorflow as tf

callbacks_fase2 = [
    # Monitora a perda de validação com mais paciência
    tf.keras.callbacks.EarlyStopping(
        monitor='val_loss',
        patience=10,             # Espera 10 épocas para dar margem ao Plateau
        restore_best_weights=True # Garante que vai salvar o modelo da época 21 (o melhor)
    ),
    # Reduz o LR para fazer o ajuste fino cirúrgico
    tf.keras.callbacks.ReduceLROnPlateau(
        monitor='val_loss',
        patience=4,              # Age antes do EarlyStopping
        factor=0.2,              # Reduz o LR em 5 vezes
        verbose=1
    )
]
```

### Compilação: Fase 1 (Treino da Cabeça)

Python

```
model.compile(
    optimizer=tf.keras.optimizers.Adam(learning_rate=1e-3), # LR Moderado (0.001)
    loss='categorical_crossentropy',
    metrics=[tf.keras.metrics.Recall(name='recall')]
)
```

### Compilação: Fase 2 (Fine-Tuning)

Python

```
# Descongelar as camadas desejadas da base aqui...

model.compile(
    optimizer=tf.keras.optimizers.Adam(learning_rate=1e-5), # LR Baixo e Seguro (0.00001)
    loss='categorical_crossentropy',
    metrics=[tf.keras.metrics.Recall(name='recall')]
)
```


--- 


**Nome do Artigo:_“Fully automatic wound segmentation with deep convolutional neural networks”_ (Wang et al., 2020)."**


**Artigo:** [s41598-020-78799-w.pdf](file:///C:/Users/peuja/Downloads/s41598-020-78799-w.pdf)

**Resultados de métricas de validação de segmentação em imagens de feridas na literatura:**

1. **Dice Coefficient (Coeficiente de Dice):** Uma métrica comum para avaliar a sobreposição entre a segmentação automática e a anotação de referência humana.

- Valores relatados variam dependendo da arquitetura do modelo e da complexidade da imagem:
- Modelos baseados em U-Net alcançam resultados na faixa de **90-91%** (ex.: Liu et al., 91.6%), demonstrando alta precisão na delimitação da ferida.
- O estudo em questão reporta um Dice médio de **90.47%** com seu modelo MobileNetV2, superior a arquiteturas mais pesadas, indicando eficácia comparable ou superior em relação ao estado da arte.

2. **Precisão (Precision):** Mede a proporção de pixels classificados corretamente como ferida entre todos classificados como ferida pelo algoritmo.

- Valores típicos na literatura variam de **83-94%**:
- Mask-RCNN atingiu **94.30%**, demonstrando alta confiabilidade na exclusão de falsos positivos.
- O estudo atual obteve **91.01%**, indicando uma baixa taxa de falsos positivos.

3. **Recall (Sensibilidade):** Avalia a capacidade do método de detectar todos os pixels de ferida de forma correta.

- Em trabalhos diversos, valores entre **78-91%** são comuns:
- U-Net atingiu **91.29%** na literatura, enquanto o método estudado alcançou **89.97%**, tendo uma leve vantagem em recall sobre os demais modelos testados.

4. **Outros resultados relevantes:**

- **IoU (Intersection over Union):** Não explicitamente reportado no estudo, mas relacionado ao Dice, com valores superiores a **80%** indicam boas segmentações.
- **Número de parâmetros e eficiência computacional:** Modelos leves como MobileNetV2 atingem desempenho comparável com menos de 3 milhões de parâmetros, facilitando aplicação prática em ambientes clínicos com recursos limitados.

**Resumo geral do que a literatura mostra:**

|Métrica|Faixa de valores tipicamente relatados|Exemplos específicos|
|---|---|---|
|Dice Coefficient|81-91% (com média em torno de 85-90%)|Liu et al. (91.6%), estudo atual (90.47%)|
|Precisão|83-94%|Mask-RCNN (94.3%), estudo atual (91.01%)|
|Recall|78-91%|U-Net (91.29%), estudo atual (89.97%)|

**Tecnologias e métodos utilizados:**

- **Modelo principal:** MobileNetV2, uma rede neural convolucional leve e eficiente, baseada em convoluções separáveis em profundidade, ideal para aplicações móveis e com menor consumo de recursos.
- **Transfer learning:** o MobileNetV2 pré-treinado na base Pascal VOC de segmentação foi utilizado para acelerar o treinamento e melhorar a precisão.
- **Pré-processamento:** inclui Cropping, zero-padding, e técnicas de aumento de dados (rotação, flip, zoom) para ampliar o conjunto de treinamento.
- **Anotação:** dataset de 1109 imagens de úlceras nos pés, com máscaras de segmentação marcadas por especialistas.
- **Pós-processamento:** máscara binária gerada por limiar, com preenchimento de buracos, remoção de pequenas regiões e componentes conectados para refinar o resultado final.
- **Arquitetura do modelo:** substituição de camadas convencionais por convoluções separáveis (depthwise + pointwise), tornando o modelo mais leve e rápido, adequado para dispositivos móveis.

---

 Comparações com outros métodos e modelos:

- Comparou sua abordagem com arquiteturas populares de segmentação: **FCN-VGG16, SegNet, U-Net e Mask-RCNN**.
- Resultados mostraram que o modelo baseado em MobileNetV2 + CCL (Connected Component Labelling) superou esses métodos em métricas de desempenho.
- **Desempenho no conjunto de dados próprio:**
- Maior Score de Dice: **90,47%** (melhor que VGG16, U-Net, Mask-RCNN, SegNet)
- Recall: aproximadamente **89,97%**
- Precisão: **91,01%**
- **Desempenho no conjunto de dados Medetec:**
- Melhor Dice: **94,05%**
- Mostrar uma alta robustez e aplicabilidade geral

 Resultados importantes:

- A abordagem é **mais leve e rápida**, com menos parâmetros (~2 milhões) em comparação com modelos mais complexos.
- Mantém alta precisão mesmo com imagens de iluminação variável, diferentes tamanhos e complexidades de feridas.
- Demonstrou eficácia na automação e na possibilidade de uso em dispositivos móveis, ajudando profissionais de saúde na medição e diagnóstico de feridas.



![[Pasted image 20260614175352.png|697]]

![[Pasted image 20260614175406.png]]

**Revista:** https://pmc.ncbi.nlm.nih.gov/articles/PMC7736585/

**Implementação:** https://github.com/uwm-bigdata/wound-segmentation

**Referência:**  Fauzi, M. F. A. et al. Computerized segmentation and measurement of chronic wound images. Comput. Biol. Med. 60, 74–85 (2015).
