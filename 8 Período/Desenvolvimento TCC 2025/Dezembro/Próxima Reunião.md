 ## 1. 🖼️ O Processo de Decomposição (Extração de Características)

O coração de qualquer CNN é a **Camada Convolucional**, que atua como um detector de _features_ (características) por meio de **filtros** (ou _kernels_).

	 1.1 A Imagem como uma Matriz (O Input)

Imagine uma imagem minúscula em preto e branco, de 5x5 pixels.

Cada pixel tem um valor de brilho: 0 (preto) a 10 (branco) — na vida real vai de 0 a 255.

Matriz da Imagem ($I$):

$$\begin{bmatrix} 10 & 10 & 10 & 0 & 0 \\ 10 & 10 & 10 & 0 & 0 \\ 10 & 10 & 10 & 0 & 0 \\ 10 & 10 & 10 & 0 & 0 \\ 10 & 10 & 10 & 0 & 0 \end{bmatrix}$$

_Nota:_ Você consegue ver uma "borda" vertical onde os 10s viram 0s? O nosso objetivo é que a matemática encontre essa borda sozinha.

---

	1.2 O Filtro / Kernel (A "Lente")

O filtro é uma matriz menor (geralmente 3x3) cheia de **Pesos ($W$)**. Esses pesos são o que a rede "aprende". No início, são aleatórios. Depois do treino, eles assumem padrões.

Vamos usar um filtro clássico de **detecção de borda vertical**:

$$\begin{bmatrix} 1 & 0 & -1 \\ 1 & 0 & -1 \\ 1 & 0 & -1 \end{bmatrix}$$

---

	1.3. A Operação de Convolução (O Cálculo)

A "janela deslizante" pega o filtro 3x3 e o coloca sobre o canto superior esquerdo da imagem.

**O cálculo é o Produto Escalar (Dot Product):** Multiplica-se o valor do pixel pelo valor correspondente no filtro e soma-se tudo.

Passo 1 (Canto Superior Esquerdo):

Pegamos o recorte 3x3 da imagem:

$\begin{bmatrix} 10 & 10 & 10 \\ 10 & 10 & 10 \\ 10 & 10 & 10 \end{bmatrix}$

Multiplicamos pelo filtro:

$(10 \times 1) + (10 \times 0) + (10 \times -1)$

$+ (10 \times 1) + (10 \times 0) + (10 \times -1)$

$+ (10 \times 1) + (10 \times 0) + (10 \times -1)$

$= (10 + 0 - 10) + (10 + 0 - 10) + (10 + 0 - 10)$

$= 0 + 0 + 0 = \mathbf{0}$

> **Interpretação:** O resultado é **0**. Isso diz para a rede: "Não há nada interessante aqui (região lisa)".

Passo 2 (Movendo uma coluna para a direita - O "Stride"):

Agora a janela vê a transição do 10 para o 0.

Recorte da imagem:

$\begin{bmatrix} 10 & 10 & 0 \\ 10 & 10 & 0 \\ 10 & 10 & 0 \end{bmatrix}$

Multiplicamos pelo filtro:

$(10 \times 1) + (10 \times 0) + (0 \times -1)$

$+ ...$ (repetido 3 vezes)

$= (10 + 0 + 0) \times 3$

$= \mathbf{30}$

> **Interpretação:** O resultado é **30**. Um número alto! Isso grita para a rede: "**ACHEI UMA BORDA AQUI!**".

---

	 1.4. O Mapa de Características (O Output)

Após passar o filtro por toda a imagem, teremos uma nova matriz (Feature Map) que não se parece mais com a foto original, mas sim com um "mapa de calor" de onde estão as bordas verticais.

Se a imagem original era o rosto de uma pessoa:

1. Camada 1 (Matemática acima): O output é um mapa de contornos.
    
2. Camada 2: Pega o mapa de contornos e aplica filtros para achar formas (nariz, olho).
    
3. Camada 3: Pega as formas e calcula se é "humano" ou "gato".
    

---

	1.5 As Diferenças Matemáticas nas Arquiteturas

Aqui é onde o MobileNet e o YOLO mudam a equação para serem especiais:

#### **MobileNetV2 (Matemática Econômica)**

#####  Depthwise Separable Convolutions (Convolução Separável em Profundidade)

Imagine que você é um chef de cozinha e precisa cortar legumes (cebola, tomate, pimentão) e depois misturá-los para fazer um molho.

- **Na Convolução "Normal" (Clássica):** Você corta um pedaço de cebola, um de tomate, um de pimentão e já mistura tudo na mesma facada. Você faz o corte (espacial) e a mistura (profundidade) ao mesmo tempo. Isso é **muito cansativo** (computacionalmente pesado).
    
- **Na Convolução Separável (A Mágica):** Você divide o trabalho em dois passos:
    
    1. **Depthwise (Corte):** Primeiro, você corta _só_ as cebolas. Depois, _só_ os tomates. Depois, _só_ os pimentões. Cada canal de cor é tratado separadamente.
        
    2. **Pointwise (Mistura):** Agora que está tudo cortado, você joga tudo numa panela e mistura. Isso é feito com um filtro $1 \times 1$.
        

**O Ganho:** O resultado final é praticamente o mesmo (o molho), mas matematicamente você faz cerca de **9 vezes menos cálculos**. É uma estratégia de "dividir para conquistar".

Na convolução normal (acima), se a imagem for colorida (RGB - 3 canais), o filtro também tem que ser 3D (3x3x3). Isso gera muitas multiplicações.

- **O Truque Matemático (Depthwise):** O MobileNet diz: "Não misture as cores ainda".
    
    - Ele aplica um filtro 2D apenas no canal Vermelho ($R \times filtroA$).
        
    - Aplica outro no Azul ($B \times filtroB$).
        
    - Aplica outro no Verde ($G \times filtroC$).
        
    - Só no final ele faz uma soma simples.
        
    - **Resultado:** Reduz o número de multiplicações em até 8 ou 9 vezes, mantendo o resultado muito parecido.
        


##### Inverted Residual Blocks (Blocos Residuais Invertidos)

Para entender o "Invertido", precisamos lembrar do padrão (ResNet). O padrão antigo era como uma ampulheta: largo nas pontas e estreito no meio (Grosso $\rightarrow$ Fino $\rightarrow$ Grosso).

O MobileNetV2 **inverteu** isso. O bloco dele é **Fino $\rightarrow$ Grosso $\rightarrow$ Fino**.

**Como funciona o processo dentro do bloco (A "Expansão"):**

1. **Entrada "Fina" (Comprimida):** Os dados chegam compactados na memória (poucos canais) para economizar espaço.
    
2. **Expansão (Fica "Grosso"):** A rede "infla" esses dados multiplicando os canais (ex: de 24 canais para 144).
    
    - _Por que?_ Imagine que você está numa oficina pequena. Para trabalhar bem, você pega as peças e espalha tudo numa bancada grande. A rede precisa de espaço (mais dimensões) para encontrar características complexas sem perder informação.
        
3. **Processamento (Depthwise):** Agora que os dados estão "espalhados" na bancada, ela aplica os filtros leves (que expliquei acima) para extrair as características.
    
4. **Compressão/Projeção (Volta a ficar "Fino"):** Depois de extrair o que precisava, a rede "arruma a bancada", comprime as informações importantes de volta num pacote pequeno e manda para o próximo bloco.
    

O "Residual" (O Atalho):

Existe um "fio" que conecta o início do bloco direto ao final (pula o processamento). Se a rede perceber que aquele bloco não está ajudando em nada, ela pode simplesmente passar a informação original direto pelo atalho. Isso evita que a rede "esqueça" o que aprendeu nas camadas anteriores.


#### **YOLOv3 (Matemática de Regressão)**

O YOLO não termina com uma classificação simples (ex: "Gato: 90%"). A saída matemática dele é um vetor (uma lista de números) para cada célula do grid.

A fórmula de saída para cada célula é um vetor $y$:

$$y = [p_c, b_x, b_y, b_h, b_w, c_1, c_2, ...]$$

- $p_c$: Probabilidade de ter _algum_ objeto ali (0 a 1).
    
- $b_x, b_y$: Coordenadas do centro do objeto (Matemática de geometria).
    
- $b_h, b_w$: Altura e largura da caixa (Bounding Box).
    
- $c_1, c_2$: Probabilidade da classe (ex: é carro? é pedestre?).
    

O YOLO calcula o "erro" (Loss Function) comparando essas coordenadas matemáticas preditas com as reais.

---



### A. Decomposição em Níveis (Hierarquia de Características)

Os modelos não veem a imagem como um todo de uma vez, mas a decompõem em uma hierarquia de complexidade:

| **Princípio**              | **Descrição**                                                                                                                                                                                                                                            |
| -------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **A. Visão Hierárquica**   | Todos decompõem a imagem em uma série de camadas que aprendem _features_ de complexidade crescente: **Bordas** (camadas iniciais) $\rightarrow$ **Texturas/Padrões** (camadas intermediárias) $\rightarrow$ **Partes da Ferida/Forma** (camadas finais). |
| **B. Funções de Ativação** | Todos utilizam funções de ativação não-lineares (como **ReLU**) nas camadas convolucionais para introduzir complexidade e permitir o aprendizado de relações complexas.                                                                                  |
| **C. Camada de Decisão**   | Todos finalizam o processo de decomposição e extração com o achatamento dos _feature maps_ (camada **Flatten**), seguidos por camadas **Densa** (Fully Connected) para a tomada de decisão final.                                                        |
| **D. Saída (Softmax)**     | Todos utilizam a função **Softmax** na última camada para gerar um vetor de probabilidades, escolhendo a classe com a maior probabilidade entre as 6 possíveis (**Diabete, Pressure, Venous, etc.**).                                                    |

| **Camada da CNN**          | **O que o Modelo "Vê" (Decompõe)**                                       | **Aplicação em Imagens de Feridas**                                                                                                                                         |
| -------------------------- | ------------------------------------------------------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Camadas Iniciais**       | **Características Simples:** Bordas, linhas, curvas, gradientes de cor.  | Detecção da **borda da ferida** (forma), separação entre o tecido necrótico (cor escura) e o tecido de granulação (cor vermelha viva).                                      |
| **Camadas Intermediárias** | **Características Complexas:** Texturas, padrões, combinações de formas. | Detecção de **tecidos específicos** (esfacelo, necrose, granulação), padrões de **hiperpigmentação perilesional** (venosa), ou a **linearidade de uma sutura** (cirúrgica). |
| **Camadas Finais**         | **Características Semânticas:** Partes do objeto (ferida completa).      | Reconhecimento do **padrão de uma úlcera de pressão** em proeminência óssea ou o **formato de saca-bocado** da úlcera diabética.                                            |

*Esfacelo : ==é um **tecido morto, necrótico e inviável** que se acumula no leito de uma ferida, aparecendo como uma massa **amarelada, esbranquiçada ou acinzentada**, úmida e que dificulta a cicatrização, impedindo o crescimento de tecido novo==.* 

*Granulação em feridas :  é o ==processo natural de formação de um tecido novo, vermelho e granular (como "carne moída") que preenche a lesão, rico em pequenos vasos sanguíneos (angiogênese) e colágeno==, sendo um sinal vital da fase proliferativa da cicatrização, que preenche o "buraco" e prepara para a formação de pele nova, mas precisa de umidade e nutrientes para ocorrer bem, com excesso podendo indicar infecção ou má circulação.*

*A **hiperpigmentação perilesional** refere-se ao ==**escurecimento da pele que ocorre ao redor de uma lesão** ou área de inflamação existente==.*

### B. O Papel do _Feature Map_

A cada camada convolucional, a imagem de entrada é transformada em um **mapa de características** (_Feature Map_). Esse mapa é a representação numérica da intensidade com que um _feature_ específico (por exemplo, uma borda vertical) está presente em diferentes regiões da imagem. O modelo está, literalmente, **decompondo** a imagem em milhares de mapas de características.


## 2. 🎯 Como Cada Modelo Decompõe de Forma Diferente

Enquanto a **CNN Sequencial** é rasa e aprende _features_ mais específicas do seu _dataset_ local, as arquiteturas pré-treinadas (MobileNetV2 e ResNet50) usam decomposições mais robustas.

## 2.2🧠 Como os Modelos Decompõem e Classificam as Imagens de Feridas

Os modelos (CNN Sequencial, MobileNetV2 e YOLOv3+ResNet50) são Redes Neurais Convolucionais (CNNs). Eles decompõem e classificam as imagens de feridas em um processo de múltiplas etapas, que vai da extração de características visuais simples até a decisão final da classe.

![[Pasted image 20251215172701.png]]

---

## 3. ⚖️ O Processo de Classificação (Decisão)

Após a decomposição, os modelos passam para a fase de tomada de decisão.

### A. Achatamento (_Flatten_) e Camadas Densa (_Dense_)

Os **Mapas de Características** finais (a decomposição da imagem) são **achatados** (_flattened_) em um único vetor longo. Este vetor, que contém as _features_ mais importantes da ferida, é alimentado nas **Camadas Densa** (ou _Fully Connected_).

- ***Flatten (Achatamento):** Estica a matriz transformando-a em um único vetor longo (uma fila indiana de números).*
    
- ***Global Average Pooling (GAP):** Uma abordagem mais moderna (usada no MobileNet) que tira a média de cada mapa de características, gerando um vetor muito menor e mais eficiente.*

Este vetor entra nas **Camadas Densas**. Aqui, cada neurônio está conectado a _todos_ os neurônios da camada anterior (por isso "Densa").

- **O papel matemático:** É aqui que ocorre o raciocínio lógico final. A rede combina as características extraídas (ex: "tem borda curva" + "tem textura de pelo") e atribui pesos para decidir a qual classe aquilo pertence.

**Saída ( Output) **

A última camada aplica uma função de ativação final (geralmente **Softmax** para classificação ou **Sigmoid** para detecção binária) que transforma os números brutos em **probabilidades** (ex: 85% Cachorro, 15% Gato).

### B. Geração de Probabilidades (Softmax)

A última camada Densa do seu modelo tem 6 neurônios (um para cada classe de ferida: Diabetic, Pressure, Venous, etc.).

- A função de ativação **Softmax** é aplicada a esta camada, que transforma as pontuações brutas dos neurônios em **probabilidades**, onde a soma de todas as probabilidades é igual a $1.0$ (ou 100%).
    
- **Exemplo de Decisão:**
    
    - Diabetic: $0.01$
        
    - Pressure: $0.05$
        
    - Sirurgical: $0.02$
        
    - Venous: $0.85$
        
    - Normal: $0.03$
        
    - Background: $0.04$
        
    - **Classificação Final:** O modelo escolhe a classe com a maior probabilidade, neste caso, **'Venous'** ($0.85$).
        

### C. Por que 'Diabete' Apresenta Erros (Ambiguidade)

Os erros na classe 'Diabete' ocorrem porque as _features_ extraídas (textura, profundidade, bordas) são muito próximas das _features_ de 'Pressure' e 'Venous'.

Se uma imagem de úlcera diabética (Real: Diabete) tiver _features_ que se parecem muito com um padrão aprendido de úlcera venosa, o neurônio de 'Venous' pode ter uma probabilidade ligeiramente maior (ex: $0.45$) do que o de 'Diabete' ($0.40$), resultando em um erro de classificação (Falso Negativo para Diabete e Falso Positivo para Venous).

### D. O Que Eles Têm em Comum? (Por que os resultados são parecidos?)

Você notou que, no final, todos podem acertar que é um "cachorro". O motivo da semelhança nos resultados finais é que **o objetivo matemático (Loss Function) é o mesmo**.

1. **O "Cérebro" Final é similar:** Independente se a rede usou uma lupa simples (Sequencial) ou um microscópio avançado (MobileNet) para ver a imagem, a etapa final (Densa/Classificador) funciona do mesmo jeito: ela tenta desenhar uma linha matemática que separa "Classe A" de "Classe B".
    
2. **Mesmos Dados de Treino:** Se você treinar as três redes com as mesmas fotos, elas vão tentar convergir para a mesma "verdade".
    
3. **Features Discriminativas:** Todas elas, de formas diferentes, buscam isolar o que torna o objeto único. Se o objeto tem uma característica muito óbvia (ex: a tromba de um elefante), todas as três arquiteturas vão acabar encontrando essa característica, gerando resultados parecidos.

**O Que Difere Cada Um? (A Comparação Técnica)**

Suas intuições estavam certíssimas. Aqui está o detalhamento técnico do que acontece de diferente em cada abordagem:

#### 1. Rede Sequencial (CNN Customizada)

- **A abordagem:** _"Tabula Rasa"_ (Folha em branco).
    
- **O processo:** Você começa com pesos aleatórios. A rede precisa aprender **do zero** o que é uma linha reta, depois o que é uma curva, até chegar no objeto.
    
- **Diferencial:** Você tem controle total da arquitetura, mas exige muito mais dados e tempo para chegar no mesmo resultado que as outras, pois ela "nasce sabendo nada".
    
- **Risco:** Se tiver poucas imagens, ela apenas decora os exemplos (Overfitting) em vez de aprender.
    

#### 2. MobileNetV2

- **A abordagem:** _"Eficiência Arquitetural"_.
    
- **O processo:** Ela usa os **Depthwise Separable Convolutions** e **Inverted Residual Blocks**. A grande diferença na fase de decisão é que ela evita usar `Flatten` puro (que gera milhões de parâmetros). Ela usa **Global Average Pooling** antes da camada densa.
    
- **Diferencial:** Ela consegue extrair as mesmas características que uma rede gigante, mas usando 10x menos cálculos. Ela é "esperta", não "bruta".
    

#### 3. YOLOv3 + Transfer Learning

- **A abordagem:** _"Especialista Readaptado"_.
    
- **O processo (Transfer Learning):** A rede já "viu" milhões de imagens (ImageNet). Ela já sabe detectar bordas, texturas e formas complexas (pesos congelados).
    
- **O Diferencial (Fine-Tuning):** Você não ensina a rede a "ver". Você só ajusta a **última camada** (a cabeça da rede). É como pegar um médico formado e ensiná-lo apenas os protocolos de um novo hospital.
    
- **Detecção vs. Classificação:** Diferente das outras duas que geralmente dizem "O que é a imagem", o YOLO divide a decisão final em grid, prevendo **Caixa (Onde)** + **Classe (O que)** simultaneamente.

### 4. ⚠️ Parametrização do Modelo

- **MobileNetV2 e YOLOv3 + ResNet50:** Utilizaram o conceito de **Transfer Learning**. Isso significa que a maior parte dos parâmetros (pesos) foi **inicialmente parametrizada** com o conhecimento adquirido na base de dados **ImageNet** (visão geral de objetos) e depois **ajustada** (fine-tuning) para as suas imagens de feridas.
    
- **CNN Sequencial:** Foi parametrizada a partir do zero (randomização), aprendendo todos os seus pesos apenas com o seu _dataset_ de feridas.
    
- **Comum a todos (Configurações da Camada de Saída):** O parâmetro mais importante que você ajustou para a sua tarefa é a **camada de saída** (classificação). Ela foi parametrizada com **6 unidades** (uma para cada classe) e, provavelmente, a função de ativação **Softmax** para gerar a probabilidade de cada classe.

- **Qual é o mais fidedigno?**
    
    - O **MobileNetV2** é o mais fidedigno em termos de métricas ($0.98$), indicando a melhor capacidade de generalização e menor taxa de erro no seu conjunto de testes.
        
- **O que será melhor: Precisão ou Sensibilidade (Recall)?**
    
    - Em diagnóstico médico (como classificação de feridas), geralmente o **Recall (Sensibilidade)** é preferível, especialmente para as classes de doenças (Diabete, Pressure, Venous). Um alto Recall significa que o modelo **não deixa de detectar** um caso positivo (baixo Falso Negativo).
        
    - Nesse caso, como o Recall está muito alto em todas as classes no MobileNetV2 ($0.98$), o modelo consegue ser **preciso e sensível**. Se as classes estivessem mais desbalanceadas ou os erros fossem mais críticos, seria necessário um ajuste que priorizasse o Recall.
    
#### 🎯 4.1 Idêntico (O que foi mantido igual)

- **Dataset:** Todos os modelos foram treinados e testados utilizando o **mesmo Dataset unificado** (MedTec + AZH).
    
- **Classes de Saída:** Todos classificam nas **6 mesmas classes** (Background, Diabetic, Normal, Pressure, Sirurgical, Venous).
    
- **Pré-processamento Básico:** Compartilharam as mesmas etapas de **Redimensionamento, Normalização (0-1)** e a mesma estratégia de **Data Augmentation**.
    
- **Função de Perda (_Loss Function_):** Embora não detalhado, é altamente provável que todos tenham usado a mesma função de perda para a classificação multi-classe, como **Categorical Cross-Entropy**.
### ⚔️ 5. Atacar Futuramente o Risco de Overfitting / Vazamento de Dados

 Identificar que acurácias acima de 95% levantam uma bandeira vermelha 🚩. Para atacar o problema futuramente e ter resultados mais confiáveis, as seguintes ações são essenciais:

1. **Validação Cruzada (Cross-Validation):** Em vez de uma única divisão Treino/Teste, use a K-Fold Cross-Validation. Isso garante que cada imagem tenha a chance de estar no conjunto de teste, dando uma estimativa mais robusta do desempenho.
    
2. **Análise de Desempenho no Conjunto de Validação:**
    
    - **Ação:** Plote a curva de **Loss/Acurácia** do conjunto de **Treinamento** versus o conjunto de **Validação** ao longo das épocas.
        
    - **Resultado Esperado:** Se a **Loss de Treinamento continuar a cair**, mas a **Loss de Validação começar a subir** em um determinado ponto, isso é a **evidência clássica de Overfitting** (superajuste).
        
3. **Inspeção de Imagens Confusas:** Use técnicas de **Interpretabilidade** como **Grad-CAM** (Gradient-weighted Class Activation Mapping) para visualizar **quais pixels** o modelo MobileNetV2 está realmente usando para tomar decisões. Se o mapa de calor mostrar que ele está focando em _pixels de background_ ou _marcas d'água_ específicas, isso pode indicar que o modelo está aprendendo um padrão acidental (vazamento de dados ou viés de coleta) em vez da própria ferida.

### 🥊 6. Guardar Termos para Discussão (Heterogeneidade vs. Homogeneidade)


- **Heterogeneidade:** Refere-se à **diversidade visual** dentro de uma mesma classe ou entre classes que causam confusão.
    
    - **Discussão:** A confusão do modelo na classe **'Diabete'** com **'Pressure'** e **'Venous'** pode ser atribuída à **alta heterogeneidade visual** dessas úlceras. Uma úlcera diabética pode parecer muito diferente dependendo da infecção, estágio e localização, o que a torna visualmente ambígua para o modelo. A heterogeneidade é o que a sua etapa de **Data Augmentation** tenta mitigar (tornar o modelo robusto a variações).
        
- **Homogeneidade:** Refere-se à **similaridade visual** das imagens.
    
    - **Discussão:** A alta acurácia do MobileNetV2 ($0.98$) sugere que, apesar das ambiguidades, as classes de feridas possuem **padrões visuais altamente homogêneos e discrimináveis** (características únicas) que a CNN conseguiu extrair com sucesso. O 'Normal' (background) e o 'Sirurgical' (incisão linear) provavelmente têm alta homogeneidade dentro de si, facilitando a classificação.