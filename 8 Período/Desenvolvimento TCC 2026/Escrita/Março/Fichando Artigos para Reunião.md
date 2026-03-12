
## 1 Fichamento de Leitura Científica

**1. Referência Bibliográfica (Padrão ABNT)** SANTIAGO, Flávio Augusto Maia _et al_. Avanços no reconhecimento do Sinal de Frank com InceptionV3 e Vision Transformer: uma abordagem com fine-tuning. In: XXVIII ENCONTRO NACIONAL DE MODELAGEM COMPUTACIONAL e XVI ENCONTRO DE CIÊNCIA E TECNOLOGIA DE MATERIAIS, 2025.

**2. Tema / Assunto** Aplicação de técnicas de Aprendizado de Máquina Profundo (Deep Learning) e Visão Computacional para o reconhecimento automatizado de marcadores clínicos em imagens médicas.

**3. Objetivo do Artigo** O estudo visa avaliar técnicas de pré-processamento de imagens e definir os melhores hiperparâmetros para construir um modelo de _Deep Learning_ capaz de analisar imagens e detectar a presença do Sinal de Frank (uma prega no lóbulo da orelha associada a doenças cardiovasculares).

**4. Metodologia Relevante (Aplicável ao TCC)**

- **Pré-processamento e Região de Interesse (RoI):** As imagens foram padronizadas para 224x224 pixels e anotadas com _bounding boxes_ (caixas delimitadoras) usando a ferramenta Roboflow, focando apenas na área específica de interesse (o lóbulo da orelha), o que permitiu o recorte automático das imagens.
    
- **Aumento de Dados (Data Augmentation):** Para lidar com uma base de dados limitada de apenas 1601 imagens , os autores aplicaram técnicas de aumento de dados. Foram utilizadas transformações geométricas e no espaço de cores, como espelhamento, rotações e _Color Jitter_ (alterações de brilho, contraste, saturação e matiz).
    
- **Transferência de Aprendizado (Transfer Learning):** Em vez de treinar redes do zero, utilizaram as arquiteturas InceptionV3 e Vision Transformer (ViT) pré-treinadas no ImageNet, aplicando o conceito de _fine-tuning_ (ajuste fino) nas camadas finais.
    
- **Métricas de Avaliação:** Utilizou-se a acurácia, mas adotou-se o _Recall_ (Revocação) da classe positiva como critério de desempate, dada a sua criticidade na área da saúde.
    

**5. Principais Resultados** A aplicação conjunta das técnicas de recorte focado (pré-processamento), aumento de dados e arquiteturas pré-treinadas resultou numa acurácia de até 99,79% com a arquitetura ViT. A pesquisa concluiu que focar a análise na região de maior relevância (restringindo ruídos externos) e aplicar ajustes criteriosos potencializa significativamente a capacidade de generalização e detecção do modelo.

**6. Citações Diretas de Destaque (Para utilizar na redação do TCC)**

- **Sobre o desafio de dados limitados:** "Técnicas de Data Augmentation são amplamente utilizadas para reduzir overfitting e melhorar a capacidade de generalização dos modelos de DL, sobretudo quando há limitação de dados disponíveis".
    
- **Sobre a importância de simular a realidade (ótimo para justificar câmeras mobile):** "Essas transformações, aplicadas no carregamento dos dados, ampliaram a diversidade do conjunto, compensando a pouca quantidade de imagens e simulando variações reais...".
    
- **Sobre a escolha da métrica na saúde:** "...deu-se preferência ao modelo com maior recall da classe positiva, por se tratar de uma característica crítica na identificação da condição-alvo.".
    

**7. Reflexões e Conexões com o Meu Projeto (Detecção de Feridas via Mobile)**

- _Justificativa para o Pré-processamento:_ Assim como o artigo delimitou o lóbulo da orelha com _bounding boxes_, a minha aplicação mobile deve orientar o utilizador ou utilizar um algoritmo prévio para focar estritamente na área da ferida, ignorando o restante da pele e fundo. Isso minimiza características não relacionadas e foca a rede em padrões específicos.
    
- _Lidando com fotos de telemóvel:_ Como o meu projeto envolve captura de imagem via "Tecnologia Mobile", o utilizador final tirará fotos em diferentes condições de luminosidade. Posso citar o uso de _Color Jitter_ deste artigo como uma estratégia essencial de _Data Augmentation_ para treinar o meu modelo a ser resiliente a variações de brilho e contraste originadas pelas câmeras dos smartphones.
    
- _Métricas de Saúde:_ Devo justificar no meu TCC que a minha métrica prioritária na avaliação de detecção de feridas não será apenas a Acurácia, mas também o _Recall_, garantindo que o algoritmo minimize os casos de falsos negativos (deixar de identificar uma ferida real)


Link Local : file:///C:/Users/peuja/Downloads/1265089.pdf


## 2 Fichamento de Leitura Científica

**1. Referência Bibliográfica (Padrão ABNT)** SCARAMUSSA, Lorenzo M.; PACHECO, Andre G. C. Anotação de imagens médicas assistida por IA: um estudo sobre segmentação de lesões de pele por não especialistas. Departamento de Informática, Universidade Federal do Espírito Santo (UFES), Vitória, Brasil, 2024.

**2. Tema / Assunto** Anotação e segmentação de imagens médicas através de inteligência artificial (IA) interativa, ferramentas de _crowdsourcing_ e o uso de mão de obra não especialista para contornar o alto custo de criação de bases de dados.

**3. Objetivo do Artigo** O estudo propõe a criação de um _framework_ interativo para auxiliar não especialistas na geração de máscaras de segmentação de lesões cutâneas. O objetivo é reduzir o tempo e os custos associados à anotação manual para o treinamento de IAs, mantendo os padrões de qualidade compatíveis com as exigências clínicas.

**4. Metodologia Relevante (Aplicável ao TCC)**

- **Base Tecnológica (SAM):** O _framework_ utiliza o _Segment Anything Model_ (SAM), um modelo de visão computacional de ponta que gera segmentações a partir de _prompts_ simples (como caixas delimitadoras) sem a necessidade de ser treinado do zero para tarefas específicas.
    
- **Interação Humano-IA:** Aplicou-se o conceito _human-in-the-loop_, no qual o utilizador leigo indica onde está a lesão na imagem e a IA preenche o recorte com precisão.
    
- **Protocolo de Avaliação:** 50 voluntários sem formação médica avaliaram a ferramenta segmentando 50 lesões. Eles usaram dois modos: manual (desenhando todo o contorno) e assistido (desenhando apenas uma caixa delimitadora para a IA trabalhar).
    
- **Crowdsourcing (STAPLE):** Para melhorar a precisão, o estudo agregou as marcações feitas pelos vários leigos através de um algoritmo de consenso chamado STAPLE, criando uma máscara final de alta confiabilidade.
    

**5. Principais Resultados** A abordagem assistida por IA reduziu o tempo de segmentação em cerca de 75 segundos por imagem, mantendo a mesma precisão do modo puramente manual. Além disso, quando se combinou as anotações dos leigos via _crowdsourcing_, a qualidade das máscaras aumentou cerca de 4%, resultando em marcações muito próximas das feitas por dermatologistas especialistas (com um índice _Dice_ de 0.875). Para os utilizadores, o método assistido tornou a tarefa rápida, fácil e muito menos exaustiva.

**6. Citações Diretas de Destaque (Para utilizar na redação do TCC)**

- **Sobre o gargalo da base de dados:** "A anotação de imagens médicas é essencial para a construção de bases de dados destinadas ao treinamento de algoritmos de inteligência artificial (IA). No entanto, a dependência de especialistas torna esse processo caro e difícil de escalar.".
    
- **Sobre a viabilidade do método assistido:** "A abordagem assistida se mostrou factível, acelerando o processo de segmentação sem comprometer a qualidade dos resultados.".
    
- **Sobre o papel do utilizador:** "...consiste em uma segmentação assistida, na qual um não especialista participa do processo de criação da máscara de segmentação através da indicação de áreas de região de interesse nas imagens médicas.".
    

**7. Reflexões e Conexões com o Meu Projeto (Detecção de Feridas via Mobile)**

- _Anotação Colaborativa da Base de Dados:_ Para o meu TCC, construir um bom _dataset_ de feridas é um desafio enorme. Este artigo serve como base metodológica para justificar que eu não preciso que médicos e enfermeiros anotem cada pixel das feridas; pessoas comuns (como colegas do curso) podem usar ferramentas assistidas (como o SAM) para delinear o _dataset_ do TCC com qualidade médica.
    
- _Interface do Aplicativo Mobile:_ O artigo demonstra que desenhar na tela (modo manual) é considerado "lento e cansativo". Trazendo isto para o cenário _mobile_, a melhor forma de interação para a deteção de feridas no ecrã de um telemóvel será o "modo assistido": basta que o próprio paciente desenhe uma caixa por cima da ferida para que o algoritmo extraia apenas a lesão para análise (ignorando o fundo ou roupa).
    
- _Uso de IAs generalistas na saúde:_ O estudo comprova que modelos como o _Vision Transformer_ e o _Segment Anything Model_, mesmo não sendo treinados exclusivamente com dermatologia, têm uma excelente capacidade de adaptação para recortar bordas irregulares típicas de anomalias na pele (o que inclui feridas).

Link Local : file:///C:/Users/peuja/Downloads/35492-673-28588-1-10-20250606%20(1).pdf

## Fichamento de Leitura Científica

**1. Referência Bibliográfica (Padrão ABNT)** COSTA, Jordanna Caballero _et al_. Inteligência Artificial na Caracterização e Mensuração de Feridas: Uma Revisão Sistemática. Faculdade Zarns - Itumbiara; Instituto de Informática - Universidade Federal de Goiás (UFG), 2025.

**2. Tema / Assunto** O uso de inteligência artificial, processamento de imagens e ferramentas computacionais (incluindo dispositivos móveis) para a caracterização, mensuração e avaliação clínica de feridas.

**3. Objetivo do Artigo** Analisar, através de uma revisão sistemática da literatura, a aplicabilidade de softwares e tecnologias de IA no tratamento de feridas, identificando os tipos de lesões mais estudados, os algoritmos utilizados, os parâmetros mensurados (como tamanho e profundidade) e os desafios enfrentados na sua implementação clínica.

**4. Metodologia Relevante (Aplicável ao TCC)**

- Trata-se de um Mapeamento Sistemático da Literatura (MSL) focado na abordagem PICO, revisando 25 estudos primários publicados nos últimos 10 anos.
    
- Mapeou a utilização de Redes Neurais Convolucionais (CNNs), destacando as arquiteturas U-Net, ResNet e Mask R-CNN como as abordagens predominantes na análise de imagens médicas para este fim.
    
- Levantou o estado da arte sobre o uso de aplicações móveis (smartphones) e tecnologias complementares (como termografia e 3D) para captura de imagem e monitorização remota.


**5. Principais Resultados** A revisão concluiu que a IA reduz a subjetividade da análise clínica, oferecendo uma avaliação padronizada e muitas vezes superior aos métodos manuais tradicionais. As úlceras por pressão e feridas diabéticas são as mais pesquisadas devido ao seu elevado impacto. No entanto, a implementação destas tecnologias esbarra frequentemente em desafios relacionados com a qualidade inadequada das fotografias tiradas e a falta de padronização na recolha dos dados.

**6. Citações Diretas de Destaque (Para utilizar na redação do TCC)**

- **Sobre o uso de telemóveis e Visão Computacional:** "A estimativa da área da ferida pode ser aprimorada por softwares de segmentação de imagem, que utilizam técnicas como redes neurais convolucionais (CNNs) para identificar automaticamente as bordas da lesão a partir de imagens capturadas por câmeras de smartphones...".
    
- **Sobre a redução de erros humanos:** "...a IA pode alcançar um nível de precisão comparável ou superior ao de especialistas humanos na segmentação de feridas, reduzindo a subjetividade da análise clínica".
    
- **Sobre o grande obstáculo da captura de imagens:** "...dificuldades mais recorrentes nas aplicações de inteligência artificial e softwares para caracterização e gestão de feridas envolvem, principalmente, problemas relacionados à qualidade das imagens utilizadas para análise".


**7. Reflexões e Conexões com o Meu Projeto (Detecção de Feridas via Mobile)**

- _Validação da Arquitetura Tecnológica:_ Este artigo é uma excelente base teórica para o capítulo de revisão de literatura do TCC. Ele prova cientificamente que as Redes Neurais Convolucionais (CNNs) são o estado da arte indiscutível para identificar bordas e classificar tecidos de feridas através de imagens.
    
- _Justificativa para o Desenvolvimento Mobile:_ O artigo defende que a integração da IA com telemóveis otimiza o trabalho clínico, facilita o envio de imagens pelos pacientes e permite o monitoramento remoto. Esta é a justificativa bibliográfica perfeita para a componente "Apoio de Tecnologia Mobile" do título do seu projeto.
    
- _Prevenção de Falhas no TCC (O Problema da Qualidade da Imagem):_ Como o artigo aponta que o maior desafio atual é a qualidade inadequada das fotografias capturadas e o enquadramento, o seu TCC pode ganhar muito destaque clínico se a sua aplicação móvel propuser uma solução para isso. Por exemplo, pode descrever no seu projeto que a interface da sua app foi pensada para ter guias visuais de enquadramento ou alertas sobre baixa luminosidade para ajudar o utilizador final a capturar uma fotografia com qualidade suficiente antes de a enviar para o modelo de Visão Computacional

Link Local : file:///C:/Users/peuja/Downloads/35537-673-28633-1-10-20250606%20(1).pdf