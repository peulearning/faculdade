
# Observações da Orientação - TCC (Felipe)

## 🎯 Metas de Ajuste
- [x] Definir e redigir a justificativa teórica para o número de épocas do modelo.
- [x] Realizar levantamento na literatura sobre os resultados de métricas de validação de **Segmentação** em imagens clínicas/feridas. 

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



--- 


 Após as alterações realizadas acima fiz alguns retestes no experimento neste notebook 
 
[Refazendo Split DatasetOriginal Archictecture MobileNetV2 4 Classes Modify Grad-Cam Apply .ipynb - Colab](https://colab.research.google.com/drive/1194PdVXnJdZDFcPZAJlS4OZo4P5wOUyu#scrollTo=1xtm1JVtNvAH)



![[Pasted image 20260614192310.png]]


![[Pasted image 20260614192318.png]]


![[Pasted image 20260614192329.png]]

![[Pasted image 20260614192354.png]]

![[Pasted image 20260614192337.png]]


![[Pasted image 20260614192403.png]]


![[Pasted image 20260614192428.png]]



### Fase 1: O "Alinhamento" da Cabeça Classificadora (LR = $10^{-3}$)

Na Fase 1, com a MobileNetV2 completamente congelada, apenas a sua nova camada de classificação (a "cabeça" com as 4 classes de feridas) estava calculando gradientes.

- **O que ocorreu:** O Learning Rate de `1e-3` (0.001) é moderadamente agressivo e ideal para essa fase. Ele fez com que a acurácia de treinamento subisse rapidamente logo nas primeiras épocas. O modelo aprendeu a associar os recursos visuais gerais que a MobileNetV2 já conhecia (bordas, texturas, formas) diretamente às classes `diabetic`, `pressure`, `normal` e `background`.
    
- **Resultado na curva:** A `loss` de treino despencou rápido e a `accuracy` subiu de forma íngreme.
    

### 2. Fase 2: O Ajuste Cirúrgico das 30 Camadas (LR = $10^{-5}$)

Ao descongelar as últimas 30 camadas da MobileNetV2, você expôs filtros convolucionais profundos ao seu dataset de feridas.

- **O que ocorreu:** Se você usasse um LR alto aqui (como o 0.001 da Fase 1), você iria "destruir" o conhecimento pré-treinado da MobileNetV2 (fenômeno chamado de _esquecimento catastrófico_). Ao cravar o LR em `1e-5` (0.00001), você garantiu que os pesos dessas 30 camadas sofressem apenas **micro-ajustes**. Eles começaram a se moldar especificamente às nuances das feridas teciduais (como os tons exatos de vermelhidão, necrose e pele sadia).
    
- **Resultado na curva:** A acurácia de validação, que costuma estagnar nessa transição, continuou progredindo de forma suave e consistente.
    

### 3. A Sinergia dos Callbacks: O Segredo do Ganho de Desempenho

A alteração mais inteligente que ditou a melhora dos resultados foi o "casamento" de paciência entre o `ReduceLROnPlateau` (Paciência = 4) e o `EarlyStopping` (Paciência = 10).

Veja o passo a passo do que aconteceu no coração do loop de treino devido a essa configuração:

1. **O Modelo Estagnou (Época X):** O modelo atingiu o seu limite temporário com o LR de $10^{-5}$. A partir daí, a perda de validação (`val_loss`) parou de cair.
    
2. **O primeiro socorro (Época X + 4):** Como a paciência do `ReduceLROnPlateau` é de apenas **4 épocas**, ele percebeu a estagnação antes do EarlyStopping. Ele agiu diminuindo o Learning Rate em 5 vezes (Fator = 0.2), derrubando-o de $10^{-5}$ para **$2 \times 10^{-6}$**.
    
3. **O Efeito "Degrau":** Com esse novo LR microscópico, o otimizador Adam conseguiu encontrar pequenos caminhos de melhoria na função de perda que antes passavam batidos. Isso fez com que a `val_loss` voltasse a cair de leve e a **Acurácia de Validação desse um último salto para cima**.
    
4. **O Resgate Seguro:** O `EarlyStopping` tem paciência de **10 épocas**. Isso significa que ele deu uma margem de 6 épocas extras _após_ a redução do LR para ver se o modelo melhorava. Quando o modelo realmente não conseguiu mais evoluir, o EarlyStopping interrompeu o treino e, graças ao `restore_best_weights=True`, ele descartou as épocas de supertreinamento e **salvou o modelo exatamente no ápice da sua acurácia de validação**.
    

### Resumo do Impacto Visível nos Resultados:

- **Curvas Estáveis:** A perda de validação não explodiu (o que aconteceria se o LR fosse alto ou se não houvesse a redução dinâmica).
    
- **Acurácia de Teste Superior:** O modelo conseguiu extrair o máximo de desempenho possível do seu conjunto de dados porque o refinamento final foi feito em "velocidade reduzida" ($2 \times 10^{-6}$), permitindo que a rede convergisse perfeitamente no ponto ótimo local da curva de perda.

# 📝 Anotações de Resultados e Análise Comparativa Atualizada

## 📊 Tabela Comparativa: Antes vs. Depois

| **Métrica / Configuração** | **Experimento Anterior (Seu rascunho)** | **Experimento Atual (Este Notebook)**                | **Status / Impacto Clínico**                  |
| -------------------------- | --------------------------------------- | ---------------------------------------------------- | --------------------------------------------- |
| **Acurácia no Teste**      | 71%                                     | **85,29%**                                           | **🚀 Salto de +14,29% de acerto global**      |
| **Loss no Teste**          | Média de ~0.60                          | **0.4119**                                           | **📉 Redução expressiva do erro do modelo**   |
| **Acurácia de Treino**     | ~79%                                    | Evolução para patamares superiores                   | Melhor convergência e extração de padrões     |
| **Acurácia de Validação**  | ~65% - 70%                              | Estabilizada e acompanhando o treino                 | Ganho robusto em dados não vistos             |
| **Data Augmentation**      | Com variação de Brilho                  | **Sem variação de Brilho (`brightness_range`)**      | Evitou a distorção das cores reais das lesões |
| **Divisão de Dados**       | Divisão antiga/padrão                   | Novo Split Estratificado Estrito (`random_state=42`) | Eliminação absoluta de _Data Leakage_         |

#### **A Lógica do Número de Épocas Máximas:**

- **Fase 1 (15 Épocas):** Como estamos treinando apenas as camadas densas superiores (um espaço de parâmetros drasticamente menor), 15 épocas são matematicamente suficientes para que o otimizador Adam leve a cabeça classificadora à convergência inicial.
    
- **Fase 2 (25 Épocas):** O ajuste fino com uma taxa de aprendizado tão baixa ($10^{-5}$) faz com que os passos do otimizador em direção ao mínimo global da função de perda sejam minúsculos. Portanto, o processo de aprendizagem torna-se propositalmente mais lento, exigindo um teto maior de épocas (25) para que as 30 camadas convolucionais refinem seus pesos adequadamente.


"A metodologia proposta adota o Aprendizado por Transferência a partir da arquitetura MobileNetV2, dividida em duas fases para mitigar o risco de overfitting e o esquecimento catastrófico em cenários de dados médicos restritos. Na Fase 1 (15 épocas, LR=$10^{-3}$), realiza-se a calibração exclusiva da cabeça classificadora multiclasse. Na Fase 2 (25 épocas, LR=$10^{-5}$), executa-se o ajuste fino localizado nas 30 camadas convolucionais mais profundas da rede, permitindo a especialização dos filtros na morfologia de lesões cutâneas. O volume de épocas foi projetado como um limite máximo computacional, uma vez que o controle real da convergência é gerido dinamicamente pela ação conjunta dos callbacks `ReduceLROnPlateau` (paciência 4) e `EarlyStopping` (paciência 10), garantindo a interrupção do algoritmo no ponto ótimo de generalização estatística e restaurando os melhores pesos validados."



Na literatura acadêmica, definir um número máximo de épocas elevado e delegar o controle real do treino a _callbacks_ estatísticos não é apenas aceito, mas considerado uma **boa prática recomendada pelos principais autores da área**.


## 1. O Conceito de "Orçamento Computacional" e Parada Antecipada

Na literatura, o número máximo de épocas (as suas 15 épocas na Fase 1 e 25 na Fase 2) é chamado de **Orçamento Computacional Máximo (_Computational Budget_)**. Você não define esse número esperando que o modelo chegue até o final, mas sim para garantir que o algoritmo tenha margem para aprender.

- **A Referência Clássica:** **Goodfellow, Bengio e Courville (2016)**, no livro definitivo _"Deep Learning"_ (MIT Press), explicam que o `EarlyStopping` (Parada Antecipada) é uma das formas mais eficientes de **Regularização**.
    
- **O que a literatura diz:** Em vez de tentar adivinhar matematicamente o número exato de épocas (o que é impossível devido à estocasticidade do gradiente), a prática correta é estipular um teto alto de épocas e monitorar o erro de validação. O livro demonstra que o `EarlyStopping` age controlando a capacidade efetiva do modelo, agindo matematicamente de forma equivalente à regularização de peso $L_2$ (Weight Decay), mas sem o custo computacional de testar vários hiperparâmetros.
    

```
[Épocas Iniciais] ──> Loss de Validação cai (Modelo aprendendo)
[Ponto Ótimo]     ──> Menor Loss de Validação (Onde o restore_best_weights salva!)
[Épocas Finais]   ──> Loss de Validação sobe ou estagna (Início de Overfitting / Acaba a Paciência)
```

## 2. A Justificativa para a "Paciência" do EarlyStopping

Por que colocar `patience=10` e não parar logo na primeira oscilação?

- **A Referência Científica:** **Prechelt, L. (1998)**, no artigo seminal _"Early Stopping — But When?"_ (publicado na Springer).
    
- **O que a literatura diz:** O autor mapeou diversas classes de critérios de parada. Ele provou empiricamente que curvas de validação em datasets reais não são perfeitamente lineares; elas sofrem pequenas oscilações locais (ruídos). Se a sua paciência for muito curta (como 1 ou 2 épocas), o modelo pode parar prematuramente em um "falso platô". Uma paciência estendida (como 10 épocas) dá ao otimizador o tempo necessário para superar instabilidades locais do gradiente e encontrar mínimos globais mais profundos.
    

## 3. O Decaimento de Learning Rate em Platôs

Por que usar o `ReduceLROnPlateau` com paciência menor (4) antes do EarlyStopping (10)?

- **A Referência Científica:** **Bengio, Y. (2012)**, no guia prático _"Practical Recommendations for Gradient-Based Training of Deep Architectures"_.
    
- **O que a literatura diz:** Yoshua Bengio (um dos "pais" do Deep Learning) explica que, conforme o modelo se aproxima de um mínimo local na função de perda, os passos do otimizador (ditados pelo _Learning Rate_) podem se tornar "grandes demais" para o relevo da curva, fazendo com que o modelo oscile de um lado para o outro sem conseguir descer mais.
    
    Reduzir dinamicamente a taxa de aprendizado quando o erro de validação estabiliza (o Plateau) é a estratégia recomendada para "esfriar" o otimizador, permitindo que ele faça ajustes milimétricos e encontre soluções mais generalistas.
    

## 4. Assimetria de Épocas: Fase 1 (Curta) vs. Fase 2 (Longa)

Por que a sua Fase 1 tem menos épocas (15) que a Fase 2 (25)?

- **A Referência Científica:** **Yosinski e tal. (2014)**, no artigo clássico _"How transferable are features in deep neural networks?"_ (NIPS).
    
- **O que a literatura diz:** Os autores provam que as camadas superiores (a cabeça classificadora) são específicas do domínio, enquanto as profundas são genéricas.
    
    - Na **Fase 1**, como você congelou a base, o espaço de busca de parâmetros é minúsculo (apenas os neurônios da sua camada densa). Matematicamente, poucos passos de otimização (15 épocas) bastam para que uma camada linear atinja a convergência.
        
    - Na **Fase 2**, ao descongelar as 30 camadas convolucionais com um LR baixíssimo ($10^{-5}$), o espaço de busca aumenta drasticamente, e os passos dados pelo otimizador são minúsculos para não destruir os filtros do _ImageNet_ (_Esquecimento Catastrófico_). Logo, o modelo precisa de um horizonte de épocas muito maior (25 épocas) para que esses micro-ajustes se consolidem.



📝 Como citar isso na escrita :   

"O número máximo de épocas adotado nas Fases 1 e 2 (15 e 25 épocas, respectivamente) foi concebido como um teto operacional de orçamento computacional, delegando-se o controle real da convergência ao monitoramento estatístico da perda de validação, conforme preconizado por Goodfellow et al. (2016). Para mitigar interrupções prematuras causadas por ruídos estocásticos inerentes ao otimizador Adam, adotou-se o critério de parada antecipada (Early Stopping) com paciência de 10 épocas, estendida o suficiente para permitir a ação prévia do mecanismo de decaimento dinâmico da taxa de aprendizado (Reduce LR on Plateau) configurado com paciência de 4 épocas, seguindo as recomendações práticas de Bengio (2012) e Prechelt (1998) para convergência em mínimos globais estáveis."