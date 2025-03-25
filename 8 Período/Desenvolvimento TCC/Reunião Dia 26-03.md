
Executar os Steps e Explicar [  ] 
Entender porque a dimensão 224x224 foi utilizada [ x ]  ( Possível Testar em resolução 256x256 ou 512x512 )  
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

Aprendizado Profundo ( Deep Learning )

* Atributos Complexos :  Não estruturados, Alta Dimensionalidade
* Previamente Selecionados / Manipulados ( Engenharia de Características )
*  O próprio modelo aprende como : Selecionar e extrair características , reconhece padrões de características
* Modelos Complexos : Milhares  / Milhões de Parâmetros 
* 