
Executar os Steps e Explicar [ X ] 
Entender porque a dimensão 224x224 foi utilizada [ X ]  ( Possível Testar em resolução 256x256 ou 512x512 )  
Procurar Artigos 5 á 10 quais motivações de utilizar Visão Computacional com Feridas. [ x ]  
### 📌 **1. Padrão de Modelos Pré-Treinados**

A resolução **224×224** foi popularizada pelo modelo **AlexNet** (2012) e depois utilizada em redes como **VGG16, ResNet, MobileNet e EfficientNet**. Muitas arquiteturas modernas usam esse tamanho como entrada padrão, pois os pesos pré-treinados esperam essa dimensão.

**Exemplo:** Modelos como `ResNet-50` e `MobileNetV2` aceitam 224x224 como entrada padrão.
 
### 📌 **2. Eficiência Computacional e Memória**

Redes neurais convolucionais (CNNs) operam melhor quando a imagem tem **tamanho fixo**. Se as imagens fossem maiores, exigiriam mais memória e processamento. Se fossem muito pequenas, perderiam detalhes importantes.

- **224x224** é um **compromisso entre qualidade e eficiência**.
    
- **Menores que isso**: perda de detalhes.
    
- **Maiores que isso**: aumento do custo computacional.
    

**Exemplo prático:**  
Uma imagem de **1024x1024** tem **1.048.576 pixels**. Já uma de **224x224** tem **50.176 pixels**, sendo **20× menor**, reduzindo o tempo de treinamento.

### **📌 3. Preservação de Informações Relevantes**

Ao redimensionar para **224x224**, **mantemos detalhes essenciais** da imagem sem comprometer a qualidade para redes neurais convolucionais (CNNs).  
Se usarmos um tamanho muito pequeno, a CNN pode perder informações importantes, especialmente para **segmentação e classificação de lesões cutâneas**.

**Exemplo:**  
Uma lesão pequena pode desaparecer se reduzirmos para **64x64**, mas em **224x224** ainda conseguimos ver bordas e texturas.

### 📌 **4. Aplicação no Reconhecimento de Lesões Cutâneas**

Em aplicações médicas, como a **segmentação e classificação de feridas**, a dimensão **224x224** é suficiente para capturar características como:

- **Textura da pele**
    
- **Bordas da lesão**
    
- **Padrões de coloração**
    
- **Detalhes pequenos, mas importantes**
    

Se aumentarmos muito a resolução, apenas aumentamos o custo de processamento **sem melhorar significativamente a precisão**.

### 📌 **5. Consistência entre Amostras**

Usar **224x224** para todas as imagens padroniza os dados de entrada, tornando o treinamento mais eficiente e evitando que a CNN precise lidar com tamanhos variados.

Sem um tamanho fixo, precisaríamos de **camadas adaptativas** ou realizar **padding**, o que poderia introduzir distorções.


### ⚡ **1. Redes  Convolucionais Parâmetros Técnicos**

https://www.youtube.com/watch?v=lb5ocJiDLgg&t=1268s - Redes Neurais Convolucionais

Estamos Tratando de Dados Complexos ( Imagens )

* Imagem , Texto, Grafos, Aúdio.

Dados Complexos : Alta dimensionalidade (Grande número de Atributos e Características ) em outras palavras cada pixel e um dado.  Uma imagem de 7x7 pixels e menos que um ícone ou seja mesmo pequenas imagem pequenas existe uma quantidade absurda de atributos. ainda assim os dados são " NÃO ESTRUTURADOS " pois as informações necessárias estão distribuídas em vários pixels, dando o fato que precisa ser LIMPADO para conseguirmos extrair as características necessárias.

Problemas dos dados  Complexos : Redundância doa dados, padrões distribuídos em muitos atributos .  Feature Engineering  ( Limpeza de Dados, Transformação dos Dados, Seleção das Características ), Reconhecimento de Padrões . 

Exemplo  : Conjunto de imagem de Dígitos  cujo objetivo e identificar dentro das imagens os subconjuntos de pixels (atributos características ) que identifique um determinado padrão  então o algoritmo deve encontrar as características mais fortes de uma classe para outras e após a extração desse conjunto de pixels são as características e após extraídos 

Aprendizagem de Máquina Clássico ( Shallow ) era utilizado essa etapas até agora feito a mão

* Atributos Simples, Estruturados, Baixa Dimensionalidade  
*  Previamente Selecionados / Manipulados ( Engenharia de Características )
* Modelos Simples ( Rasos - Shallow ( OpenCV2  >> SVM  ( Classificador Linear Filtros  )))
* Poucos parâmetros

Aprendizado Profundo ( Deep Learning ) em outra palavras o modelo aprende sozinho apenas com a imagem crua , sem que eu tenha que pré-processar os dados.

* Atributos Complexos :  Não estruturados, Alta Dimensionalidade
* Previamente Selecionados / Manipulados ( Engenharia de Características )
*  O próprio modelo aprende como : Selecionar e extrair características , reconhece padrões de características
* Modelos Complexos : Milhares  / Milhões de Parâmetros 

Redes Neurais Convolucionais :  São redes neurais que trabalham com qualquer tipo de dado porém vamos fixar com Imagens .

* Convoluções de Dimensão 1 :  Para vetores
* Convoluções de Dimensão 2 : Matrizes =  Imagens
* Convoluções de Dimensão 3 : Matrizes de Matrizes = Volumes

Em outras palavras não estamos criando uma outra rede neural , apenas estamos adicionando camadas novas dando suas respectivas responsabilidades.

*  Bloco 1  : Redução de Dimensionalidade , Extração de Características  ( Camadas de Convoluções , por exemplo Pooling, Conv, ReLU )
*  Bloco 2 :  Reconhecimento de Classificação de Padrões

Tipos de Camadas : 

* Convolucionais
* Dimensionalidade (Pooling / Up Sampling )
* Densas / Totalmente Conectadas (MLP)
* Normalização (Dropout , Normalization, Nose )

Funções de Ativação ( Para última camada da Rede ) :

*  Família ReLU 
*  Família Softmax ( Cara Computacionalmente ) : Quanto mais saídas eu tiver mais caro será ( O(n ))

 
	Em outras palavras as CNN's elas terão muitas camadas de convulção, pooling, relu e perceptrons, com isso temos a primeira parte ou seja o Pass Forward e geralmente o output e a Classificação ou seja  essa saída e uma distribuição de probabilidade que será associada a cada classe possível . O modelo então e treinado comparando a distribuição de probabilidades com a classe verdadeira , sendo realizada pela função de custo ( otimizador ) .


Da penúltima camada para trás a gente usa a ReLU  seria o estado da arte é que funciona bem para todos os modelos, e a última camada e utilizado o Softmax ( problemas de classificação ) .


Otimizadores / Métodos de Treinamento : 


Gradiente Descendente [ Batch / Mini-Batch / Stochastic ]  ( Pelo menos e oque estamos utilizando) e um Suavizador  dos pesos do modelo,  mas em outras palavras cada otimizados da um modelo diferente. e por padrão utiliza-se o descendente. 


* Funções de Custo : 

Entropia Cruzada  e uma função de custo que tem que ser utilizada quando a função de ativação da última camada for uma sigmóide ou uma softmax .  E isso porque ela compara o quão parecidas são duas distribuições de probabilidade da sua rede.  

No caso utilizamos a Entropia Cruzada  Binária e ( 0, 1 )  e deveremos utilizar a Entropia Cruzada Multinomial.

Arquitetura do Modelo

Componentes da Camada Convolucional 

* Quantos Filtros ? 
* Qual a dimensão de cada Fitlro ?
* Qual o incremento entre os filtros (stride) ? 
* Qual e a função de ativação ?  Não e obrigatório porém podem passar valores negativos.

Camadas de Pooling  ( Redução , Amostra , Pixels Importante )

* Qual tipo do pooling ? ( Máximo , média, mínimo)
* Qual a dimensão do filtro ? 2x2 , 3x3 , 4x4  .... 
* Quanto maior a dimensão maior a redução da imagem e quanto maior a redução da imagem mais perde definição.

Camadas de Perceptrons ( Densas / Totalmente Conectadas )

* Quantas camadas ? 
* Quantos neurônios por camda ?
* Qual a função de ativação ?
*  Sabemos por padrão da penúltima  camada para trás  usamos Relu e da camada final em diante e Softmax. 

Outras escolhas arquiteturais 

* Otimizador 
* Funções de Custo
* Técnicas de Normalização 
* Dropout, Normalization, Noise

Depois de escolher você a Arquitetura Sequencial  

* Como as camadas estarão conectadas entre si ? 

Arquitetura Pararalela -> Percorre caminhos diferentes  e depois são concatenadas

Framework Keras + TensorFlow   ( Deep Learning )

Deep Learning e muto caro computacionalmente 

* Paralelos ou divididos 
* Hardware GPU NVIDIA , TPU GOOGLE

Para o Framework Keras o Modelo é Camadas e Otimizador 

keras.layers - Camadas

* Densa
* Convolucional  ( Conv2D )
* Pooling ( MaxPooling2D , AveragePooling2D )
* Regularização ( Dropout, GaussianNose, BatchNormalization )

nn.layers - Camadas 

* Input
* Flatten
* Reshape
* Concatenate

![[Pasted image 20250325214851.png]]

![[Pasted image 20250325214905.png]]


![[Pasted image 20250325215139.png]]

![[Pasted image 20250325215245.png]]


Motivações de Usar Visão Computacional no Tratamento de Feridas : 

1.  Automatização de Análise de Feridas 
2. Precisão e Consistência 
3. Análise Multimodal
4. Capacidade de Lidar com Diversidade
5. Melhorias nos Tratamentos e Resultados
6. Redução de Custos 