
Objetivo: Avaliar o desempenho de modelos de Redes Neurais Convolucionais (CNNs) na classificação multiclasse de feridas crônicas e agudas, investigando se os erros de predição estão concentrados em perfis visuais específicos, como imagens com baixa luminosidade ou coloração amarelada, visando a futura implementação em dispositivos móveis de recursos limitados.


Métodos e Metodologias:  Trata-se de um estudo experimental focado no desenvolvimento e comparação de modelos de visão computacional. Utilizou-se um conjunto de dados com 
319 imagens clínicas, sendo 185 da classe Diabética e 134 da classe Pressão. O experimento comparou as arquitetura Sequencial (Customizada) e MobileNetV2 (Transfer Learning). A avaliação foi realizada através de métricas de acurácia, precisão, recall além de uma análise qualitativa da matriz de confusão voltada á identificação de padrões de erro em diferentes perfis visuais.

Resultados:  O modelo alcançou uma taxa de acerto global de aproximadamente 97%. No entanto, a análise estratificada confirmou a hipótese levantada: notou-se que a maior concentração de falsos positivos e falsos negativos ocorreu em imagens com predominância visual de tecidos amarelados, indicativos de esfacelo. Por meio de analise visuais, observou-se que o reflexo da luz (brilho) associado ao exsudato nessas lesões causava dispersão na extração de características (features) pelo algoritmo, confundindo os limites entre o leito e a borda da ferida.


Conclusão:  Evidencia-se que os erros de predição não são aleatórios, possuindo um forte viés ligado à coloração amarelada e à umidade do tecido (esfacelo). Conclui-se que, para o uso seguro do modelo como apoio à tomada de decisão, é imperativo que o modelo receba um treinamento reforçado especificamente para diferenciar reflexos luminosos de tecido desvitalizado.