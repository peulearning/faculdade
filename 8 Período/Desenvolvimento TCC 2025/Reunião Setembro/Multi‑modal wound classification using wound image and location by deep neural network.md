
Classificação multimodal de feridas usando imagem e localização de feridas por rede neural profunda

**Resumo :** 

O peso global das feridas agudas e crônicas apresenta um argumento convincente para melhorar os métodos de classificação de feridas, um passo vital no diagnóstico e na determinação dos tratamentos ideais. Reconhecendo essa necessidade, apresentamos uma rede multimodal inovadora baseada em uma rede neural convolucional profunda para categorizar as feridas em quatro categorias: úlceras diabéticas, por pressão, cirúrgicas e venosas. Nossa rede multimodal usa imagens das feridas e suas localizações correspondentes no corpo para uma classificação mais precisa. Um aspecto único de nossa metodologia é a incorporação de um sistema de mapa corporal que facilita a marcação precisa da localização da ferida, melhorando as técnicas tradicionais de classificação de imagens de feridas. Uma característica distintiva de nossa abordagem é a integração de modelos como VGG16, ResNet152 e EfficientNet em uma arquitetura inovadora. Essa arquitetura inclui elementos como módulos espaciais e de canal Squeeze-and-Excitation, Axial Attention e um Adaptive Gated Multi-Layer Perceptron, fornecendo uma base robusta para a classificação. Nossa rede multimodal foi treinada e avaliada em dois conjuntos de dados distintos, compreendendo imagens relevantes e informações de localização correspondentes . Notavelmente, nossa rede proposta superou os métodos tradicionais, atingindo uma faixa de precisão de 74,79 a 100% para a região de interesse (ROI) sem classificações de localização, 73,98 a 100% para ROI com classificações de localização e 78,10 a 100% para classificações de imagem inteira. Isso representa uma melhoria significativa em relação às métricas de desempenho relatadas anteriormente na literatura. Nossos resultados indicamo potencial de nossa rede multimodal como uma ferramenta eficaz de apoio à decisão para classificação de imagens de feridas,abrindo caminho para sua aplicação em vários contextos clínicos.

**Introdução:** 

Mais de 8 milhões de pessoas sofrem de feridas, e os custos com tratamentos médicos relacionados a feridas variaram de US$ 28,1 bilhões a US$ 96,8 bilhões, de acordo com uma análise retrospectiva de 2018.
Esse número imenso nos dá uma ideia da população relacionada a feridas e seus cuidados e tratamento.
Os tipos mais comuns de feridas/são úlcera do pé diabético (DFU), úlcera venosa da perna (VLU), úlcera por pressão (PU) e ferida cirúrgica (SW).
Cerca de 34% das pessoas com diabetes têm risco ao longo da vida de desenvolver uma DFU, e mais de 50% das úlceras do pé diabético ficam infectadas.
Cerca de 0,15% a 0,3% das pessoas sofrem de VLU ativa em todo o mundo3.
A úlcera por pressão é outra ferida significativa, e 2,5 milhões de pessoas são afetadas a cada ano4.
Anualmente, cerca de 4,5% das pessoas passam por uma cirurgia que leva a uma ferida cirúrgica5.
As estatísticas acima mostram que as feridas causam um enorme ônus financeiro e podem até mesmo ser fatais para os pacientes.
Uma parte essencial do tratamento de feridas é diferenciar entre os diferentes tipos de feridas (DFU, VLU, PU, SW, etc.
) ou condições da ferida (infecção vs.
 não infecção, isquemia vs.
 não isquêmica, etc.
). Para preparar medicamentos e diretrizes de tratamento adequados, os médicos devem primeiro detectar a classe correta da ferida.
Até o recente avanço da inteligência artificial (IA), os especialistas em feridas classificavam manualmente as feridas.
A IA pode economizar tempo e custos e, em alguns casos, pode fornecer previsões melhores do que os humanos. Nos últimos anos, os algoritmos de IA evoluíram para as chamadas técnicas orientadas por dados, sem intervenção humana ou de especialistas, em comparação com as primeiras gerações de IA que eram baseadas em regras, dependendo principalmente do conhecimento de especialistas6.
Esta pesquisa se concentra na classificação do tipo de ferida usando uma técnica de IA orientada por dados chamada Deep Learning (DL). O deep learning é predominante no processamento de imagens, com um enorme sucesso na análise de imagens médicas. No campo geral do processamento e estudo de imagens, alguns algoritmos de DL amplamente utilizados são Redes Neurais Convolucionais (CNN), Redes de Crença Profunda (DBN), Máquinas de Boltzmann Profundas (DBM) e Autoencoders Empilhados (Denoising)⁷. Além disso, alguns dos métodos de DL mais comuns para análise de imagens médicas incluem LeNet, AlexNet, VGG 19, GoogleNet, ResNet, FCNN, RNNs, Autoencoders, Autoencoders Empilhados, Máquinas de Boltzmann Restritas (RBM), Autoencoders Variacionais e Redes Adversariais Generativas8. Bakator et al. 9 revisaram CNN, RBM,
![[Pasted image 20250918164640.png]]


Máquina de vetor de suporte auto-aconselhada (SA-SVM), rede neural recorrente convolucional (CRNN), DBN, Autoencoders de Desruído Empilhados (SDAE), Redes Neurais Recursivas de Gráfico Não Direcionado (UGRNN), U-NET e Rede Neural Convolucional Profunda Baseada em Estrutura de Classe (CSDCNN) como métodos de aprendizagem profunda no campo do diagnóstico médico.
Embora existam alguns modelos de aprendizado de máquina baseados em características e de aprendizado profundo de ponta a ponta para classificação de feridas com base em imagens, a precisão da classificação é limitada devido às informações incompletas consideradas nos classificadores.
A novidade da presente pesquisa é adicionar a localização da ferida como uma característica vital para obter um resultado de classificação mais preciso.
A localização da ferida é uma entrada padrão para documentos de registros eletrônicos de saúde (EHR), que muitos médicos utilizam para diagnóstico e prognóstico de feridas.
Infelizmente, essas localizações são documentadas manualmente, sem diretrizes específicas, o que leva a algumas inconsistências.
No presente trabalho, desenvolvemos um mapa corporal a partir do qual é possível selecionar a localização da ferida de forma visual e precisa.
Em seguida, para cada imagem de ferida, a localização da ferida foi definida através do mapa corporal e a localização foi indexada de acordo com o nome do arquivo de imagem.
Por fim, o classificador desenvolvido foi treinado com recursos de imagem (obtidos por convolução) e localização e produziu um desempenho de classificação superior em comparação com classificadores de feridas baseados em imagem.
Um fluxo de trabalho básico desta pesquisa é mostrado na Fig.
 1.
O classificador de feridas desenvolvido recebe tanto a imagem da ferida quanto a localização como entradas e gera a classe de ferida correspondente.
O restante do trabalho está organizado da seguinte forma.
Trabalhos relacionados à classificação de feridas são discutidos na seção “Trabalhos relacionados”.
A seção “Metodologia” discute a metodologia, onde o conjunto de dados, o mapa corporal e os modelos de classificação são descritos.
Na seção “Experimento, resultado e discussão”, a configuração experimental, os resultados e a comparação, bem como a discussão sobre os resultados, são apresentados.
Por fim, o artigo é concluído e algumas observações sobre direções futuras são feitas.


**Trabalhos Relacionados**

 A classificação de feridas inclui a classificação do tipo de ferida, a classificação do tecido da ferida, a classificação da profundidade da queimadura, etc. A classificação do tipo de ferida considera diferentes tipos de feridas e não feridas (pele normal, fundo, etc.). Fundo versus DFU, pele normal versus PU e DFU versus PU são exemplos de classificação binária do tipo de ferida. Em contrapartida, DFU versus PU versus VLU é um exemplo de classificação multiclasse do tipo de ferida. A classificação do tecido da ferida diferencia entre diferentes tipos de tecidos (granulação, esfacelação, necrose, etc. ) dentro de uma ferida específica. A classificação da profundidade da queimadura mede a profundidade (dermal superficial, dermal profunda, espessura total, etc. ) da ferida por queimadura. Como esta pesquisa se concentra na classificação do tipo de ferida, esta seção discute trabalhos existentes de classificação do tipo de ferida baseados em dados.
Aqui, apresentamos trabalhos de classificação do tipo de ferida baseados em aprendizado de máquina e aprendizado profundo. Uma abordagem de aprendizado de máquina foi proposta por Abubakar et al. 10 para diferenciar feridas por queimadura e úlceras por pressão. As características foram extraídas usando arquiteturas profundas pré-treinadas, como VGG-face, ResNet101 e ResNet152, a partir das imagens e, em seguida, alimentadas em um classificador SVM para classificar as imagens em classes de feridas por queimadura ou pressão. O conjunto de dados usado neste estudo incluiu 29 imagens de feridas por pressão e 31 de queimaduras obtidas na internet e em um hospital, respectivamente. Após o aumento, elas tinham três categorias: queimadura, pressão e pele saudável, com 990 imagens de amostra em cada classe.
Várias experiências, incluindo classificação binária (queimadura ou pressão) e classificação de 3 classes (queimadura, pressão e pele saudável), foram realizadas. Goyal et al.
11 utilizaram aprendizado de máquina tradicional, aprendizado profundo e modelos CNN ensemble para classificação binária de isquemia versus não isquemia e infecção versus não infecção em imagens de DFU. Os autores desenvolveram um conjunto de dados contendo 1459 imagens de DFU que foram rotuladas por dois profissionais de saúde. Para o aprendizado de máquina tradicional, os autores utilizaram BayesNet, Random Forest e perceptron multicamadas.

	Três redes CNN (InceptionV3, ResNet50 e InceptionResNetV2) foram usadas como abordagens de aprendizado profundo. O conjunto CNN continha um classificador SVM que recebe as características de gargalo de três redes CNN como entrada. A avaliação do teste mostrou que os métodos tradicionais de aprendizado de máquina tiveram o pior desempenho, seguidos pelas redes de aprendizado profundo, enquanto o conjunto CNN teve o melhor desempenho em ambas as classificações binárias. Os autores relataram uma precisão de 90% para a classificação de isquemia e 73% para a classificação de infecção. Uma nova arquitetura CNN chamada DFUNet foi desenvolvida por Goyal et al.12 para a classificação binária de pele saudável e pele com DFU.
	
Um conjunto de dados de 397 imagens de feridas foi apresentado, e técnicas de aumento de dados foram aplicadas para aumentar o número de imagens.
A DFUNet proposta utilizou a ideia de concatenar as saídas