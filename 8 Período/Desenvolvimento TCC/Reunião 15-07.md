
1. Escrever o Objetivo do Projeto [ X ]
2. Escrever um texto corrente organizando e relacionando os conceitos [ ]
3. Escrever a proposta do teste prático  [X]



	Objetivo do Projeto  : Desenvolver um protótipo funcional que utilize visão computacional     integrada a dispositivos móveis para detectar e monitorar feridas por meio de  imagens capturadas pela câmera do celular. ( Smartphones, i/ou dispositivos de baixa capacidade computacional .)


   Legenda : Amarelo ( Corrigir )

	Texto Corrente
	
	A evolução da **Internet das Coisas (IoT)** tem revolucionado diversos setores, e a saúde é um dos que mais se beneficia dessa conectividade e capacidade de coleta de dados. É a infraestrutura da IoT que pavimenta o caminho para estudos e aplicações que antes seriam impensáveis, especialmente em contextos onde o acesso a serviços especializados é limitado. Nesse cenário, a integração da IoT com inteligência artificial aplicada com técnicas de visão computacional ==**visão computacional** e **inteligência artificial (IA)**== emerge como uma solução poderosa para enfrentar desafios persistentes na saúde ==pública==, como o monitoramento de feridas. 

    ( Correção: A visão computacional e uma forma de aplicação de inteligência artificial , então não devo separar esses dois conceitos . )

	Feridas representam um desafio significativo e persistente, com alta incidência de complicações e dificuldade de monitoramento contínuo, especialmente em regiões afastadas dos grandes centros urbanos. A capacidade de **detecção precoce e acompanhamento remoto** é, portanto, essencial. Nosso projeto propõe um **protótipo funcional** que aproveita a facilidade do acesso e disseminação ==onipresença== dos **smartphones** para democratizar o acesso a essa avaliação, transformando a câmera de um smarthphone em uma ferramenta de diagnóstico e acompanhamento. O objetivo principal é **desenvolver um protótipo funcional que utilize visão computacional integrada a dispositivos móveis para detectar e monitorar feridas por meio de imagens capturadas pela câmera dos smarthphones  e i/ou.**

	 ( Correção : Trocar o termo Celular por Smartphone )
	
	Para a inteligência do sistema, utilizaremos **Redes Neurais Convolucionais (CNNs)**, uma técnica de **classificação de imagens** altamente eficaz comprovadas pelas taxas esperadas. A implementação técnica será realizada em **Python**, empregando **TensorFlow Keras ( Visão computacional )** para o desenvolvimento do modelo de classificação e frameworks como **Flask** ou **FastAPI** para a interface e integração. A funcionalidade será centrada no usuário: ele fará o upload da imagem da ferida, que será então processada pelo modelo treinado, retornando a classificação e, idealmente, as indicações ou medidas a serem tomadas. Serão classificadas cinco tipos de feridas: **normais, por pressão, cirúrgicas, venosas e diabéticas**.

	 Correção : ( Matriz de Confusão : VF,  FV,  VV, FF  /  Pesquisar Trabalhos com Tensorflow Keras /  Segurançã & Privacidade dos Usuários  / Definição clara dos rótulos das imagens )
	
	A inspiração para a arquitetura e os desafios a serem superados vem de projetos inovadores como o **SkinGPT-4**, um sistema de diagnóstico dermatológico interativo que utiliza um **modelo de linguagem visual avançado (MiniGPT-4)**. Este projeto, que visa mitigar a **escassez de dermatologistas**, os **desafios na interpretação de imagens** e a **demora na geração de relatórios diagnósticos amigáveis**, serve como um valioso ponto de referência.
	
	As informações obtidas de artigos como "Integrated Image and Location Analysis for Wound Classification", que nos forneceu um **dataset de imagens rotuladas** essencial para o treinamento do nosso modelo, são igualmente fundamentais.
	
	Reconhecemos os **desafios técnicos** inerentes a este projeto, como a escolha da **arquitetura (Edge, Fog, Cloud)** e a necessidade de lidar com a **qualidade variável das imagens** provenientes de diferentes dispositivos. A **privacidade dos dados** dos usuários é uma **questão ética** de suma importância, e medidas robustas serão implementadas para garantir a segurança das informações.

	 Correção : ( Edge-Fog-Cloud : Níveis de processamento  )
	 Ambiente Controlado ( Outside)
	 Edge : o meio mais próximo do usuário 
	
	Em suma, a fusão de **IoT, Visão Computacional** permite a criação de soluções acessíveis e eficazes. Este projeto de protótipo para detecção de feridas em dispositivos móveis não apenas visa otimizar o acompanhamento remoto e reduzir complicações, mas também se posiciona como um exemplo claro de como a tecnologia pode ser empregada para democratizar o acesso à saúde e melhorar a qualidade de vida, inspirados por avanços como o SkinGPT-4.

	 Proposta de Teste Prático 📌

	 Para demonstrar a viabilidade do protótipo, propomos um **teste prático focado na simulação da detecção e classificação de feridas** utilizando a arquitetura proposta (Edge-Fog-Cloud). O objetivo principal é validar a capacidade do sistema em processar imagens capturadas por um dispositivo móvel e retornar uma classificação precisa em um ambiente controlado.

	Ps : Dispositivo de Baixo Poderio Computacional