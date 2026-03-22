

> [!warning] Atenção: Escopo do Projeto
> É crucial **definir a delimitação dos padrões visuais das feridas** no contexto de visão computacional. Isso guiará toda a modelagem.

---

## 📋 Plano de Ação (Para a Próxima Reunião)

- [x] Levantar e trazer as métricas utilizadas nos artigos de referência da literatura.
- [x] Estudar e documentar o conceito de **Quantização em visão computacional** aplicada a imagens complexas.
- [x] Descrever detalhadamente o processo de construção do conhecimento da Rede Sequencial.
- [x] Pesquisar e definir quais são os valores de métricas (ex: Acurácia) considerados **aceitáveis**:
	- [x] No contexto geral de Visão Computacional.
	- [x] No contexto específico de Saúde / Feridas.
- [x] Estruturar a justificativa técnica: **Por que não utilizar outras versões da [[MobileNetV2]]?**

---

## 🗣️ Tópicos Discutidos & Desafios

### Protótipo e Encapsulamento
- **Fluxo definido:** Protótipo ➡️ Validação ➡️ Encapsulamento de App.
- **Ponto de Atenção:** O encapsulamento do aplicativo foi levantado como um grande ponto de discussão e o principal desafio técnico atual desta etapa.

### Escolha de Modelos e Redes
- Fui questionados sobre a exclusividade do uso da arquitetura [[MobileNetV2]].  É necessário ter a defesa dessa escolha bem consolidada no texto.
- O mapeamento da Rede Sequencial precisa ser melhor descrito para demonstrar como o modelo "aprende" os padrões.


---

### Respostas do Check-List


## 📊 1. Métricas em Visão Computacional (Contexto Geral)

Na literatura de visão computacional (classificação e detecção):

Link : [Métricas de Avaliação de Classificação - Guia Completo | Wiki de IA](https://artificial-intelligence-wiki.com/machine-learning/model-evaluation-and-validation/classification-evaluation-metrics)

### ✔️ Acurácia (Accuracy)

Mede a proporção geral de predições corretas.

$$Accuracy = \frac{TP + TN}{TP + TN + FP + FN}$$

|**Faixa**|**Interpretação**|
|---|---|
|**< 70%**|Baixo|
|**70–80%**|Razoável|
|**80–90%**|Bom|
|**> 90%**|Excelente|

### ✔️ Precisão (Precision)

De todas as classificações positivas feitas pelo modelo, quantas eram realmente positivas.

$$Precision = \frac{TP}{TP + FP}$$

|**Faixa**|**Interpretação**|
|---|---|
|**< 0.70**|Baixo|
|**0.70–0.85**|Aceitável|
|**0.85–0.95**|Bom|
|**> 0.95**|Excelente|

### ✔️ Recall (Sensibilidade)

De todos os casos positivos reais, quantos o modelo conseguiu encontrar.

$$Recall = \frac{TP}{TP + FN}$$

|**Faixa**|**Interpretação**|
|---|---|
|**< 0.70**|Ruim|
|**0.70–0.85**|Aceitável|
|**0.85–0.95**|Bom|
|**> 0.95**|Excelente|

### ✔️ F1-Score

Média harmônica entre Precisão e Recall. Útil para datasets desbalanceados.

$$F1 = 2 \cdot \frac{Precision \cdot Recall}{Precision + Recall}$$

|**Faixa**|**Interpretação**|
|---|---|
|**< 0.70**|Fraco|
|**0.70–0.85**|Aceitável|
|**0.85–0.95**|Bom|
|**> 0.95**|Excelente|

> [!info] 💡 Interpretação Geral
> 
> Em visão computacional comum (ex: identificar objetos, gatos, carros), modelos com **F1-score acima de 0.85** já são considerados bons, enquanto valores **acima de 0.90** indicam alto desempenho.

---

## 🏥 2. Métricas em Saúde 

> [!warning] Atenção: Mudança de Paradigma
> 
> Em saúde, o critério é **MAIS RIGOROSO**. Erros são críticos, com foco principal em evitar **Falsos Negativos**.

**Regra geral da área médica:**

1. Prioriza-se um **Recall alto** (não perder casos doentes/graves).
    
2. Depois, otimiza-se a precisão.
    

### Valores aceitos na Literatura Médica:

### ✔️ Acurácia (Accuracy)


|   Faixa    |  Interpretação   |
| :--------: | :--------------: |
| **< 80%**  |   Inaceitável    |
| **80–90%** | Mínimo aceitável |
| **90–95%** |       Bom        |
| **> 95%**  |  Nível clínico   |


### ✔️ Recall


|Faixa|Interpretação|
|:-:|:-:|
|**< 85%**|Inadequado|
|**85–90%**|Aceitável|
|**90–95%**|Bom|
|**> 95%**|Ideal|



_📌 **Por quê?** Evitar falso negativo é vital._

### ✔️ Precision


|Faixa|Interpretação|
|:-:|:-:|
|**< 80%**|Baixo|
|**80–90%**|Aceitável|
|**> 90%**|Bom|


_📌 Menos crítica que o recall na maioria das triagens._

### ✔️ F1-Score


|Faixa|Interpretação|
|:-:|:-:|
|**< 0.80**|Fraco|
|**0.80–0.90**|Aceitável|
|**0.90–0.95**|Bom|
|**> 0.95**|Alto nível|



---

## 🩹 3. Contexto Específico: Feridas (Visão Computacional)

> [!abstract] Base de Referência
> 
> Valores baseados em revisões sistemáticas (incluindo literatura brasileira da SBC) e artigos utilizando arquiteturas U-Net / CNN.

### Classificação de Feridas

|**Métrica**|**Valores Comuns**|
|---|---|
|**Acurácia**|85% – 98%|
|**Recall**|85% – 97%|
|**Precisão**|80% – 95%|
|**F1-score**|0.82 – 0.96|

### Segmentação (Mais relevante para feridas)

|**Métrica**|**Valores Comuns**|
|---|---|
|**Dice**|0.85 – 0.95|
|**IoU**|0.75 – 0.90|

> [!tip] 💡 Insight Importante
> 
> Trabalhos de ponta na área de feridas geralmente apresentam **F1 ≥ 0.90**, **Recall ≥ 0.90** e **Dice ≥ 0.90**.

Figura do Artigo 1 : [s41598-024-56626-w.pdf](file:///C:/Users/peuja/Downloads/s41598-024-56626-w.pdf)

![[Pasted image 20260320214543.png]]


Artigo 2 [Multi-Class Wound Classification via High and Low-Frequency Guidance Network](https://pmc.ncbi.nlm.nih.gov/articles/PMC10740846/pdf/bioengineering-10-01385.pdf) 


### Valores atingidos pelo HLG-Net no artigo:

Conforme os resultados apresentados nas páginas 11, 15, 16, 19, 20 e 21 do artigo:

- Para **classificação de 4 classes** (mais desafiadora), o modelo HLG-Net atingiu:
    
- Accuracy (Acurácia): **82.61%**
    
- Precision (Precisão): **79.10%**
    
- Recall (Sensibilidade): **75.80%**
    
- F1-score: **77.20%** Estes valores estão em um patamar aceitável e condizentes com as dificuldades do problema.
    
- Para **classificação de 3 classes** (exemplo DSV – Diabético, Cirúrgico, Venoso):
    
- Accuracy: **92.11%**
    
- Precision: **89.80%**
    
- Recall: **90.00%**
    
- F1-score: **89.60%**
    
- Para **classificação binária (2 classes)**, em diversos pares (exemplo SV – Surgical vs. Venous):
    
- Accuracy alcançou até **98.00%**
    
- Recall chegou a **100%** em alguns casos, indicando que o modelo não perdeu amostras positivas.
    
- Precision e F1-score também estavam acima de 90% em geral.
    
- Na avaliação por AUC (curvas ROC), para a maioria das classificações binárias e triplas, os valores estavam próximos ou acima de 0,9, o que indica excelente capacidade discriminativa. Algumas tarefas mais difíceis apresentaram valores próximos a 0,8-0,85.



Artigo 3  [s10916-025-02153-8.pdf](file:///C:/Users/peuja/Downloads/s10916-025-02153-8.pdf)




|Estudo / Referência|Acurácia (%)|Precisão (%)|Recall (%)|F1-score (%)|Observação|
|---|---|---|---|---|---|
|[57]|-|-|93.4|-|AUC = 0.962|
|[58]|-|95.4|94.5|94.5|DFU-QUTNet CNN +SVM|
|[65]|92.6|84.2 -94.7|84.2 -94.7|84.2 -94.7|Mask-RCNN para classificação PU|
|[67]|-|97.3|94.5|95.8|Hybrid CNN modelos|
|[71], [72]|-|-|-|AUC ~ 0.86 - 0.95|RF para detecção de PU|
|[9], [33]|-|-|-|ICC entre 0.93 e 0.99|Medição automatizada confiável|

---

## 🎯 4. Análise & Conclusão

> 
> Observa-se que, enquanto na visão computacional geral valores acima de **0.85** já são considerados satisfatórios, no contexto da saúde há uma exigência significativamente mais rigorosa, especialmente em relação ao **recall**, devido ao impacto crítico dos falsos negativos no prognóstico do paciente.
> 



##  🧩 5. Arquitetura Sequencial ( Construindo Aprendizado )

A arquitetura do modelo foi desenvolvida de forma incremental, por meio de estudo autodidata e experimentação prática. Inicialmente, foram explorados conceitos fundamentais de redes neurais convolucionais, como definição de parâmetros de entrada, ajuste de hiperparâmetros e organização de camadas.

O modelo foi estruturado utilizando uma abordagem sequencial, na qual as camadas são organizadas de forma linear, permitindo a propagação progressiva das características extraídas da imagem. Durante esse processo, foram definidos parâmetros essenciais, como o tamanho de entrada das imagens (224x224 pixels), número de épocas (epochs), tamanho de lote (batch size) e estratégias de aumento de dados (data augmentation).

Ao longo do desenvolvimento, foram identificadas limitações iniciais, como a ausência de separação adequada entre conjuntos de treino, validação e teste, o que impactava diretamente na capacidade de generalização do modelo. A partir dessas observações, o processo foi refinado, incorporando boas práticas da literatura, como divisão adequada dos dados e uso de técnicas de regularização.

O aprendizado do modelo ocorre de forma hierárquica, onde as camadas iniciais capturam características de baixo nível, como bordas e texturas, enquanto camadas mais profundas extraem padrões mais complexos e semanticamente relevantes para a identificação de feridas.

### 🧠 5.2 Parâmetros

### ✔️ Input size (224x224)

📌 Por quê?

- Padrão de redes pré-treinadas (ImageNet)
- Equilíbrio entre:
    - detalhe
    - custo computacional

---
### 🧠 5.3 CAMADAS (LAYERS) — O CORAÇÃO DO MODELO

### ✔️ 5.3.1 Camada Convolucional (Conv2D)

📌 Função:

- Extrair características da imagem

📌 O que ela aprende:

- bordas
- texturas
- padrões da ferida

📌 Parâmetros importantes:

- **filtros (filters)** → quantidade de detectores
- **kernel size (ex: 3x3)** → área analisada
- **stride** → passo do filtro

---

### ✔️ 5.3.2 Função de Ativação (ReLU)

f(x)=max⁡(0,x)f(x) = \max(0, x)f(x)=max(0,x)

📌 Função:

- introduzir não-linearidade

📌 Por que usar:

- evita que a rede vire apenas uma regressão linear

---

### ✔️ 5.3.3 Pooling (MaxPooling)

📌 Função:

- reduzir dimensionalidade

📌 Benefícios:

- diminui custo computacional
- reduz overfitting

📌 Exemplo:

- 224x224 → 112x112

---

### ✔️ 5.3.4 Flatten

📌 Função:

- transformar matriz em vetor

👉 necessário antes das camadas densas

---

### ✔️ 5.3.5 Camada Densa (Dense)

📌 Função:

- realizar classificação final

📌 Exemplo:

- identificar tipo de ferida

---

### ✔️ 5.3.6 Dropout (IMPORTANTÍSSIMO)

📌 Função:

- desativar neurônios aleatoriamente durante treino

📌 Objetivo:

- evitar overfitting

📊 Valores comuns:

- 0.2 – 0.5

---

### 🧠 5.4 OTIMIZADORES (ESSENCIAL PRA DEFESA)

### ✔️ O que é?

👉 Algoritmo que ajusta os pesos da rede

---

### 🔥 Principais

---

### ✔️ SGD (Stochastic Gradient Descent)

📌 Atualização simples:

w=w−η⋅∇Lw = w - \eta \cdot \nabla Lw=w−η⋅∇L

📌 Vantagens:

- estável
- boa generalização

📌 Desvantagem:

- lento

---

### ✔️ Adam (MAIS USADO)

📌 Combina:

- momentum + adaptação da taxa de aprendizado

📌 Vantagens:

- rápido
- converge bem

📌 Por isso:  
👉 padrão em muitos trabalhos de visão computacional

---

### ✔️ RMSProp

📌 Ajusta taxa de aprendizado automaticamente

📌 Bom para:

- problemas com dados complexos

---

### 🎯 O QUE USEI

👉 Se você usou Adam:

> “Foi escolhido devido à sua capacidade de convergência rápida e bom desempenho em problemas com grande dimensionalidade, como imagens.”

---

### 🧠 5.5 FUNÇÃO DE PERDA (LOSS FUNCTION)

### ✔️ Classificação binária:

- Binary Cross-Entropy

### ✔️ Multiclasse:

- Categorical Cross-Entropy

---

### 📌 Fórmula (conceito):

L=−∑ylog⁡(y^)L = - \sum y \log(\hat{y})L=−∑ylog(y^​)

---

### 💡 Interpretação:

- mede erro entre predição e valor real

---

### 🧠 5.6 HIPERPARÂMETROS IMPORTANTES

### ✔️ Learning Rate

📌 Controla:

- tamanho do passo na atualização

📊 Valores típicos:

- 0.001 (padrão Adam)

---

### ✔️ Batch Size

📌 Definição:  
Quantidade de imagens processadas por vez

📊 Impacto:

- Pequeno batch → mais preciso, mais lento
- Grande batch → mais rápido, menos generalização

- 16, 32, 64 comuns

---

### ✔️ Epochs

📌 Definição:  
Número de vezes que o modelo vê todo o dataset

📌 Problema:

- Poucas → underfitting
- Muitas → overfitting

- 10–100 dependendo do dataset

---

### ✔️ Early Stopping

📌 Para treino automaticamente quando:

- validação para de melhorar

---

### 🧠 5.7 DATA AUGMENTATION (DETALHAR MELHOR)

### ✔️ Técnicas pra feridas:

- rotação (±15°)
- flip horizontal
- zoom
- ajuste de brilho

---

### 💡 Por que é crítico no seu caso:

- datasets médicos são pequenos
- melhora generalização

---

### 🧠 5.8 NORMALIZAÇÃO

### ✔️ O que é?

- transformar pixels para escala [0,1]

---

### 📌 Fórmula:

x=x255x = \frac{x}{255}x=255x​

---

### 💡 Por quê?

- acelera aprendizado
- melhora estabilidade

---

### 🧠 5.9 TRANSFER LEARNING (SE USAR MobileNet)

### ✔️ Conceito:

- usar modelo pré-treinado (ImageNet)

---

### ✔️ Benefícios:

- menos dados necessários
- melhor desempenho

## ☕ 6. Justificando o uso do MobileNetV2

### 📌 Contextualização

> A família MobileNet foi projetada para eficiência computacional em dispositivos com recursos limitados.

---

### 🔍 Comparação técnica

### ✔️ MobileNetV1

### 📉 Limitações:

- arquitetura mais simples
- menor eficiência
- menor desempenho

📌 Problema:

- não utiliza inverted residuals

---

### ✔️ MobileNetV2 

### ✔️ Vantagens:

- inverted residual blocks
- linear bottlenecks
- melhor eficiência computacional
- bom equilíbrio precisão vs custo

👉 **padrão em aplicações reais**

---

### ✔️ MobileNetV3

### ✔️ Melhorias:

- otimizações com NAS (Neural Architecture Search)
- melhor performance

### ⚠️ Problemas:

- mais complexa
- menos interpretável
- otimizada para casos específicos

---

### 📊 RESUMO COMPARATIVO

| Modelo | Precisão   | Custo   | Complexidade | Uso ideal         |
| ------ | ---------- | ------- | ------------ | ----------------- |
| V1     | Média      | Baixo   | Baixa        | Básico            |
| V2     | ⭐ Alta     | ⭐ Baixo | ⭐ Média      | ✔️ Melhor escolha |
| V3     | Muito alta | Médio   | Alta         | Produção avançada |

---

### 🎯 ARGUMENTO

> “A escolha da MobileNetV2 se justifica por apresentar um equilíbrio adequado entre desempenho e eficiência computacional, sendo amplamente utilizada em aplicações de visão computacional embarcadas.
> 
> A versão V1 apresenta limitações estruturais, enquanto a V3, embora mais recente, introduz maior complexidade e otimizações específicas que não são essenciais para o escopo deste trabalho.”


## 👨‍🔬 7.  Definindo Padrões Visuais 

Na prática, isso significa que o modelo não "vê" uma ferida, mas sim variações de intensidade de pixels, agrupadas nas seguintes categorias:

- **Bordas e Contornos (Delimitação espacial):** Transições abruptas de contraste que separam a pele íntegra do leito da ferida.
    
- **Colorimetria (Classificação de tecidos):** Padrões RGB que definem o tipo de tecido presente. Por exemplo, vermelho vivo para tecido de granulação, amarelo para esfacelo/exsudato e preto para tecido necrótico.
    
- **Textura (Topografia e profundidade):** Variações de alta frequência espacial que indicam a presença de relevo, crostas ou umidade na área lesionada.

![[Pasted image 20260322114010.png]]


### A Decomposição da Imagem: Arquiteturas Diferentes

 Embora o princípio fundamental seja o mesmo (usar filtros convolucionais para extrair características hierárquicas, começando por linhas simples até chegar a formas complexas), a _mecânica_ matemática de como essa decomposição ocorre e o objetivo final variam drasticamente entre os modelos.

**1. Arquitetura Sequencial (Vanilla CNN)**

Em uma CNN tradicional, a imagem passa por blocos lineares de convolução padrão e _pooling_. A decomposição é "força bruta": o filtro espacial e a combinação de canais (profundidade) são processados simultaneamente em cada camada. A imagem é gradativamente reduzida em tamanho espacial, mas aumentada em profundidade de características, culminando em uma camada densa que cospe uma probabilidade única (ex: "Isso é uma ferida com necrose").

**2. MobileNetV2**

Aqui a decomposição é fatorada para ser extremamente leve, ideal para operar de forma offline e diretamente no dispositivo. Em vez de uma convolução padrão pesada, a MobileNetV2 utiliza **Convoluções Separáveis em Profundidade** (_Depthwise Separable Convolutions_).

Ela quebra o processo em duas etapas: primeiro aplica um filtro espacial em cada canal individualmente (_depthwise_), e depois usa uma convolução 1x1 (_pointwise_) para combinar esses canais. Isso extrai os mesmos padrões visuais da ferida (bordas, cores), mas com uma fração do custo computacional.

**3. YOLOv3 (You Only Look Once)**

O foco do YOLO não é apenas classificar se há uma ferida na imagem, mas dizer **onde** e o **que** é cada elemento dela simultaneamente. A decomposição é feita dividindo a imagem em um grid.

Para cada célula do grid, a arquitetura (usando o _backbone_ Darknet-53) tenta prever caixas delimitadoras (_bounding boxes_) e probabilidades de classe ao mesmo tempo, avaliando o contexto global da imagem em diferentes escalas. Ele não apenas classifica a imagem inteira, mas delimita espacialmente o tecido.

---

### Comparativo de Processamento Visão Computacional

| **Arquitetura**         | **Método de Decomposição**                   | **Foco do Processamento**                                    | **Saída Final**                                         |
| ----------------------- | -------------------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------- |
| **Sequencial Clássica** | Convolução padrão (Espacial + Canais juntos) | Extração direta e linear de características.                 | Probabilidade de classificação da imagem inteira.       |
| **MobileNetV2**         | Convolução separável em profundidade         | Eficiência computacional e baixo uso de memória.             | Probabilidade de classificação da imagem inteira.       |
| **YOLOv3**              | Divisão em Grid + Múltiplas Escalas          | Contexto global para localização e classificação simultânea. | Coordenadas da lesão (caixa) + Classificação do tecido. |


#### 📄 7.2 Aprendizado Hierárquico 

Em visão computacional, chamamos isso de **aprendizado hierárquico de características** (Hierarchical Feature Learning).

A pequena correção conceitual é que **borda e coloração não acontecem em etapas separadas**, elas são extraídas simultaneamente logo no início. Já a "profundidade" (ou seja, a textura e o relevo da ferida) vem logo em seguida.

Todas as arquiteturas que mencionamos (Sequencial, MobileNetV2 e YOLOv3) seguem exatamente essa mesma ordem lógica de aprendizado. O que muda entre elas é apenas a matemática usada para fazer o cálculo, mas o que elas "enxergam" segue esta hierarquia:

### A Ordem de Extração nas Camadas (Hierarquia)

**1. Camadas Iniciais (Baixo Nível): Bordas e Cores Juntas** Assim que a imagem da ferida entra na rede, os primeiros filtros convolucionais olham para os canais RGB (Vermelho, Verde e Azul) ao mesmo tempo.

- O que a rede busca aqui são padrões primários: linhas verticais, horizontais, diagonais (bordas) e manchas de cor (contrastes).
    
- Ela identifica, por exemplo, um agrupamento de pixels muito vermelhos ao lado de pixels mais claros (a borda entre a pele e a lesão), mas ainda não sabe o que isso significa.
    

**2. Camadas Intermediárias (Médio Nível): Textura e "Profundidade"** Conforme a informação avança, a rede começa a combinar aquelas linhas e manchas de cor. É aqui que a "profundidade" visual que você mencionou começa a ser percebida.

- As redes não enxergam profundidade 3D real em uma foto 2D, mas elas inferem o relevo através das **texturas** e do jogo de luz e sombra.
    
- A combinação de bordas irregulares com tons de amarelo e preto começa a formar o aspecto "rugoso" de um tecido necrótico ou a aparência úmida do esfacelo.
    

**3. Camadas Profundas (Alto Nível): Significado Semântico** Nas últimas camadas, a rede já não olha para pixels ou texturas isoladas. Ela junta tudo o que aprendeu nas camadas anteriores para formar conceitos complexos.

- É aqui que a arquitetura finalmente diz: "Essa combinação de borda bem delimitada + cor vermelha viva + textura granulada úmida = **Tecido de Granulação**".
    

---

### Resumindo o conceito

Independentemente de  usar uma CNN Vanilla para testar ou a MobileNetV2 para rodar offline, o **caminho visual** é o mesmo: do simples (cor e linha) para o complexo (textura e conceito). A MobileNetV2 só faz isso gastando menos bateria e memória porque separa os cálculos de cor/espaço, mas a ordem de descoberta das características da ferida permanece intacta.