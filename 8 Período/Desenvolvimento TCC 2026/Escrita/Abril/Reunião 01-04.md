
# 📝 Reunião de Orientação - Abril

## 📚 Pesquisa e Aprofundamento Teórico

- [x] **Análise do Data Augmentation nas Métricas**
  - [x] Investigar o impacto dos dados sintéticos nas métricas de resultado. 
  - [x] *Ponto de atenção:* O grau de assertividade está muito alto. Avaliar se há vazamento de dados (*data leakage*) — por exemplo, garantir que o aumento de dados ocorreu **apenas** no conjunto de treinamento e não antes da separação de validação/teste.

- [x] **Revisão de Arquiteturas de Redes Neurais**
  - [x] Revisar a estrutura de uma **Arquitetura Sequencial** padrão (como as camadas de Convolução, Pooling e Dense/Fully Connected se conectam).
  - [x] Revisar a arquitetura da **MobileNetV2**. Focar especialmente em como ela otimiza o processamento para dispositivos móveis:
    - [ ] *Depthwise Separable Convolutions*
    - [ ] *Inverted Residual Blocks*
    - [ ] *Linear Bottlenecks*

- [ ] **Fundamentos de Separação de Dados**
  - [ ] Documentar o conceito e o papel do conjunto de **Treinamento** (onde a rede ajusta os pesos).
  - [ ] Documentar o conceito do conjunto de **Validação** (usado para ajustar hiperparâmetros e monitorar a rede durante o treino, evitando overfitting).
  - [ ] Documentar o conceito do conjunto de **Teste** (dados nunca vistos, usados apenas no final para a métrica de vida real).

---

## 💻 Ajustes no Código (Implementação)

- [ ] **Correção Visual de Épocas no Matplotlib**
  - [ ] Ajustar o eixo X (épocas) dos gráficos de treino e validação para mostrar apenas valores exatos/inteiros. (Ex: usar `plt.gca().xaxis.set_major_locator(MaxNLocator(integer=True))`).

- [ ] **Interpretação do Erro (Loss) na Validação**
  - [ ] Verificar a curva de **Erro por Época (Validação)**.
  - [ ] Documentar a interpretação: o que significa quando a loss de treino cai, mas a loss de validação começa a subir? (Sinal clássico de *Overfitting*).
  - [ ] Entender a diferença prática entre olhar para a métrica de Acurácia vs. a métrica de Loss (Erro) durante a validação do modelo.

---
# Revisão das Camadas Arquiteturais


## Arquitetura Sequencial

![[Pasted image 20260401204633.png]]
### 1. A Distribuição Hierárquica (A Arquitetura)

- **Fase de Extração de Características (Base Convolucional):** É a primeira metade da sua rede, onde as camadas `conv2d` e `max_pooling2d` se alternam. O objetivo aqui é "olhar" para a imagem e encontrar padrões.
    
    - **Hierarquia de aprendizado:** As primeiras camadas (como a `conv2d`) aprendem coisas simples, como bordas e linhas. Conforme a informação avança para as camadas mais profundas (`conv2d_1`, `conv2d_2`), a rede combina essas linhas para reconhecer formas complexas, texturas e partes de objetos.
        
    - **Dimensões:** Note que a "resolução" espacial diminui (de 222x222 para 111x111, depois 54x54...), mas a "profundidade" ou quantidade de filtros aumenta (de 32 para 64, e depois 128). A rede está trocando resolução espacial por riqueza de características.
        
- **Fase de Classificação (Cabeça da Rede):** É a parte final, composta pelo `flatten`, `dropout` e as camadas `dense`. Depois que a primeira fase descobre _o que_ tem na imagem, essa segunda fase pega esse resumo e decide a qual classe a imagem pertence (ex: é um gato ou um cachorro?).
    

---

### 2. O Que Faz Cada Camada

Aqui está a tradução e a função de cada camada listada no seu modelo:

- **`conv2d` (Camada de Convolução 2D):**
    
    - **O que faz:** É o "olho" da rede. Ela desliza pequenos filtros (matrizes matemáticas) por toda a imagem para detectar características (bordas, cores, texturas) .
        
    - **No seu modelo:** A primeira tem 32 filtros, a segunda 64 e a terceira 128. O aumento de parâmetros (Param #) reflete o aprendizado de padrões cada vez mais complexos.
        
- **`max_pooling2d` (Agrupamento Máximo):**
    
    - **O que faz:** Reduz o tamanho da imagem gerada pela camada anterior, resumindo a informação . Ela desliza uma janela (geralmente 2x2) e guarda apenas o valor máximo daquela área.
        
    - **Por que usar:** Reduz drasticamente a quantidade de cálculos do computador e ajuda a rede a reconhecer um objeto mesmo que ele esteja um pouco deslocado na imagem (invariância à translação). Não possui parâmetros treináveis (Param # é 0).
        
- **`flatten` (Achatamento):**
    
    - **O que faz:** É uma ponte de ligação. A saída das camadas de pooling é um bloco 3D (no seu caso, um bloco de tamanho 26 x 26 x 128). As camadas Densas no final só entendem listas de dados 1D (vetores). O Flatten "achata" esse bloco em uma única linha longa.
        
    - **No seu modelo:** $26 \times 26 \times 128 = 86.528$. Essa é a origem do Output Shape de 86528.
        
- **`dropout` (Descarte Aleatório):**
    
    - **O que faz:** É uma técnica de regularização. Durante o treinamento, ela "desliga" aleatoriamente uma porcentagem dos neurônios (ex: 20% ou 50%).
        
    - **Por que usar:** Impede que a rede fique "viciada" nos dados de treino (problema conhecido como _overfitting_). Ao desligar neurônios, a rede é forçada a não depender de um único caminho e aprende características mais robustas e generalizáveis.
        
- **`dense` (Camada Densa ou Totalmente Conectada):**
    
    - **O que faz:** É a camada tradicional de redes neurais artificiais, onde cada neurônio está conectado a todos os neurônios da camada anterior. Elas pegam todas as características processadas e fazem o trabalho matemático de classificação. É aqui que está o maior "peso" da sua rede (mais de 11 milhões de parâmetros).
        
    - **No seu modelo:** A primeira (`dense`) tem 128 neurônios para processar a informação. A última (`dense_1`) tem apenas **2 neurônios**. Isso é uma pista importantíssima: como a saída final é 2, seu modelo foi feito para uma **classificação binária** (ou seja, separar imagens em duas categorias, como "gatos vs cães" ou "doente vs saudável").


## Arquitetura MobileNetV2 

![[Pasted image 20260401204648.png]]

Enquanto a primeira era uma rede construída "do zero" (camada por camada), esta utiliza uma técnica avançada e extremamente popular chamada **Transferência de Aprendizado (Transfer Learning)** .

Em vez de ensinar uma rede a enxergar desde o início, "alugamos" o **MobileNetV2** (um modelo poderoso criado pelo Google, já treinado em milhões de imagens para reconhecer milhares de objetos) e colocamos um "novo cérebro" no topo dele para resolver o seu problema específico.


### 1. A Base Especialista (Extração de Características)

- **`input_layer_1` (Camada de Entrada):**
    
    - **O que faz:** Define o formato das imagens que entrarão na rede.
        
    - **No seu modelo:** O `(None, 224, 224, 3)` indica que a rede espera imagens com 224x224 pixels e 3 canais de cores (Vermelho, Verde e Azul - RGB).
        
- **`mobilenetv2_1.00_224` (A Rede Pré-treinada):**
    
    - **O que faz:** Este é o "corpo" principal da sua rede . Em vez de listar dezenas de camadas de convolução individuais, a tabela agrupa tudo isso aqui. Ele atua como um extrator de características super otimizado (projetado para ser rápido e leve, ideal para celulares, por isso o nome "Mobile").
        
    - **No seu modelo:** Ele pega a imagem de 224x224 e, após muito processamento interno, devolve um bloco de informações resumidas no formato `(7, 7, 1280)`. Ou seja, um mapa de características muito profundo.
        

### 2. A Transição

- **`global_average_pooling2d` (Agrupamento Médio Global):**
    
    - **O que faz:** Lembra do `flatten` da rede anterior que "achatava" tudo? O Global Average Pooling é uma alternativa mais moderna e eficiente . Em vez de desenrolar aquele bloco de 7x7x1280 em uma tripa gigante de dados, ele calcula a **média** de cada um dos 1280 mapas de 7x7.
        
    - **Por que usar:** Ele reduz o formato para `(None, 1280)`. Isso derruba drasticamente a quantidade de parâmetros da rede (evitando que ela fique muito pesada) e ajuda a prevenir o _overfitting_ (quando a rede decora o treino em vez de aprender).
        

### 3. A Nova Cabeça Classificadora

- **`dropout` e `dropout_1`:**
    
    - **O que faz:** Assim como na rede anterior, essas camadas "desligam" aleatoriamente algumas conexões durante o treinamento para forçar a rede a aprender de forma mais robusta e não depender de caminhos únicos.
        
- **`dense`:**
    
    - **O que faz:** A primeira camada da sua nova cabeça de classificação. Ela pega as 1280 características resumidas pelo MobileNetV2 e começa a combiná-las usando 128 neurônios para entender como elas se relacionam com o seu problema.
        
- **`dense_1` (A Saída Final):**
    
    - **O que faz:** A última camada, responsável pelo veredito.
        
    - **No seu modelo:** Como o Output Shape é `(None, 3)`, isso revela que o seu modelo está configurado para uma **classificação multiclasse de 3 categorias** (por exemplo: "pedra", "papel" e "tesoura").
        

---

### O Detalhe Mais Importante: Os Parâmetros

Observe o resumo no final da imagem:

- **Total params:** ~5.8 milhões.
    
- **Trainable params (Treináveis):** ~1.69 milhões.
    
- **Non-trainable params (Não-treináveis):** 731,584.
    

Esse número alto de "Non-trainable params" confirma que você **congelou** uma parte do MobileNetV2. Isso significa que você disse à rede: _"Mantenha o conhecimento visual básico que você já tem (como reconhecer bordas e texturas) intacto, e atualize apenas os pesos das camadas finais para aprender as minhas 3 categorias específicas"_. Isso economiza muito tempo e exige menos imagens de treinamento.

--- 

# Análise de Data Augmentation nas Métricas

### Referências na Literatura para cada ponto:

**1. Prevenção de _Overfitting_ e 2. Aumento da Generalização** Esses dois pontos geralmente são tratados juntos na literatura, pois a prevenção do _overfitting_ resulta diretamente no aumento da generalização.

- **A Referência Clássica (O início da era do Deep Learning):**
    
    - **Artigo:** _"ImageNet Classification with Deep Convolutional Neural Networks"_ (Krizhevsky, Sutskever, & Hinton, 2012).
        
    - **O que diz:** Este é o famoso artigo da **AlexNet**, que revolucionou a Visão Computacional. Na Seção 4.1 ("Data Augmentation"), os autores afirmam explicitamente: _"A maneira mais fácil e comum de reduzir o overfitting em dados de imagem é ampliar artificialmente o dataset usando transformações que preservam os rótulos [labels]"_. Eles provam que, sem o _Data Augmentation_ (cortes e espelhamentos), a rede sofreria um _overfitting_ massivo e não generalizaria.
        
- **O Estudo Definitivo (O melhor resumo do tema):**
    
    - **Artigo:** _"A survey on Image Data Augmentation for Deep Learning"_ (Shorten & Khoshgoftaar, _Journal of Big Data_, 2019).
        
    - **O que diz:** Este artigo é uma revisão bibliográfica completa. Ele conclui taxativamente que o _Data Augmentation_ é uma técnica de regularização essencial que resolve o problema de conjuntos de dados limitados, melhorando a capacidade de generalização do modelo para imagens não vistas durante o treino.
        

**3. Balanceamento de Classes** A literatura foca bastante em como dados sintéticos resolvem o viés do modelo em relação à classe majoritária.

- **A Referência sobre Desbalanceamento em Redes Neurais:**
    
    - **Artigo:** _"A systematic study of the class imbalance problem in convolutional neural networks"_ (Buda, Maki, & Mazurowski, _Neural Networks_, 2018).
        
    - **O que diz:** Os autores investigam o impacto do desbalanceamento de classes nas métricas de CNNs e concluem que o _oversampling_ (que frequentemente inclui gerar dados sintéticos/Data Augmentation da classe minoritária) é o método mais eficaz para melhorar métricas como Recall e F1-Score, evitando que a rede fique enviesada para a classe com mais imagens.
        

**4. O Cuidado Necessário: Quando a influência é negativa** A literatura chama isso de "Transformações que Preservam a Classe" (_Label-preserving transformations_). Se a transformação não preserva a classe, ela destrói o modelo.

- **A Referência sobre o Perigo do Augmentation Incorreto:**
    
    - Voltamos ao artigo _"A survey on Image Data Augmentation for Deep Learning"_ (Shorten & Khoshgoftaar, 2019).
        
    - **O que diz:** Na seção que discute a "Segurança" (_Safety_) do Data Augmentation, os autores usam exatamente o exemplo que citei (embora seja um exemplo universal nas aulas de ML): _"Deve-se ter cuidado ao aplicar transformações como rotações ou espelhamentos para garantir que eles não alterem o rótulo da imagem. Por exemplo, rotacionar um '6' ou um '9' ou espelhar um 'b' e um 'd' no reconhecimento de dígitos/letras"_. Eles afirmam que dados aumentados que quebram a semântica da imagem introduzem ruído nos rótulos, o que degrada severamente a performance (métricas) do modelo.

--- 


# Fundamentos da Separação dos Dados 


---

# Correção Visual de Épocas


----

# Interpretação de Erro (Loss) Validação

