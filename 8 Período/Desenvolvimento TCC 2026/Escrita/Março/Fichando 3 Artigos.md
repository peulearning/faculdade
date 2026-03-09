
## Fichamento de Leitura Científica

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