# METODOLOGIA

Primeiro um parágrafo introdutório.

> A pesquisa caracteriza-se como experimental, aplicada e de abordagem quantitativa, sendo conduzida por meio de sucessivos ciclos de desenvolvimento e avaliação de modelos de Aprendizado Profundo para classificação automática de imagens de feridas. O processo metodológico foi estruturado em quatro etapas principais, compreendendo desde a seleção das arquiteturas e preparação dos dados até a otimização dos modelos e análise comparativa dos resultados obtidos.



## 3.1 Desenvolvimento inicial dos modelos



> Inicialmente, foi implementada uma Rede Neural Convolucional (CNN) de arquitetura sequencial com o objetivo de estabelecer uma linha de base para comparação. Posteriormente, considerando a necessidade de reduzir o custo computacional e possibilitar futura aplicação em dispositivos móveis, a pesquisa evoluiu para a utilização da arquitetura MobileNetV2, projetada para oferecer elevada eficiência em ambientes com restrições de hardware.



## 3.2 Transfer Learning e otimização da arquitetura



> Na arquitetura MobileNetV2 foi empregada a técnica de Transfer Learning, utilizando pesos previamente treinados na base ImageNet. Posteriormente, foram realizados experimentos utilizando Fine-Tuning, com o descongelamento parcial das camadas convolucionais para adaptação do modelo ao domínio específico das imagens de feridas. Entretanto, os resultados obtidos não apresentaram desempenho satisfatório, motivando novas etapas de otimização.



## 3.3 Preparação dos dados e ajustes experimentais

Aqui entra praticamente tudo o que você falou.

> Após a avaliação inicial dos modelos, foram realizados ajustes sucessivos na preparação dos dados e na configuração experimental. Entre as estratégias adotadas destacam-se a aplicação de técnicas de aumento de dados (_data augmentation_), ajustes relacionados às características colorimétricas das imagens, preservação de informações referentes às bordas, formas geométricas e relevos das feridas, além da reorganização das classes do conjunto de dados, reduzindo gradativamente o problema de classificação de seis para cinco e posteriormente quatro classes, visando minimizar o desbalanceamento e melhorar a capacidade discriminativa do modelo.


## 3.4 Estratégias de treinamento e avaliação



> Inicialmente, adotou-se a estratégia convencional de particionamento do conjunto de dados em 70% para treinamento, 15% para validação e 15% para teste. Entretanto, devido ao número limitado de imagens disponíveis, observou-se que essa configuração reduzia significativamente a quantidade de amostras destinadas ao treinamento. Dessa forma, foram realizados novos experimentos utilizando apenas conjuntos de treinamento e teste, considerando diferentes proporções (70/30, 75/25 e 80/20). Os modelos foram comparados por meio de métricas de desempenho, incluindo acurácia, precisão, revocação (_recall_) e F1-score, buscando identificar a estratégia de treinamento mais adequada para conjuntos de dados limitados.


---


oque seria minha fase metodológica resumida : explorei a cnn sequencial , depois evolui partindo para mobilenetv2 , apliquei recursos como transfer learning ( transferência de aprendizado ) do imagenet que já tem imagens pré treinadas, porém ainda não conseguindo obter resultados satisfatórios, apliquei técnicas de fine-tunning tanto na 1 fase quanto na 2 fase porém obtive também resultados não satisfatórios, realizei alguns ajustes no código tanto para Colorimetria de algumas classes ( diabéticas, pressão ) e formas geométricas com bordas das feridas , relevos , e até chegar também no momento de reduzir as classes de 6 classes reduzindo para 5 4 e , diversificando algumas classes , além disso a preparação dos dados que antes utilzava era 70% Treino 15%Validação e 15%Teste entretanto o dataset limitado, perecebi que isso estava dificultando conseguir bons resultados , então adotei a estratégia para treino e teste somente ( 70% Treino - 30% Teste 75% Treino - 25% Teste , 80 %Treino e 20% Teste ) e fui fazendo comparativos etc tenho exemplo aqui de uma metodologia também que utilizei num resumo/artigo anterior : A abordagem caracteriza-se como experimental e incremental, em es tágio de prototipagem. Inicialmente, explorou-se o desempenho de uma Rede Neural Convolucional (CNN) de estrutura sequencial para estabelecer métricas base. 

Para otimizar a eficiência em hardware mobile, a pesquisa evoluiu para a arquitetura MobileNetV2 com transferência de aprendizado (transfer learning). Utilizaram-se as bases públicas AZH Wound Care e Medetec Wound Database, submetidas a técnicas de aumento de dados (data augmentation), como rotação e ajuste de brilho, para mitigar limitações quantitativas do dataset. Esta fase concentra-se na execução de testes experimentais para avaliação de desempenho das arquiteturas frente ao problema proposto, sem implementação final em ambiente de produção.