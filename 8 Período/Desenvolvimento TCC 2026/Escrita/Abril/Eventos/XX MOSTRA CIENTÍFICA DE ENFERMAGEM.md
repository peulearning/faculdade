
🔬 Hipótese Principal 
> Será que os erros de predição que o modelo, que está cometendo estão concentrados em uma dessas subclasses de perfis visuais específicos (ex: apenas nas amareladas ou apenas nas mais escuras)?

 Título : **ANÁLISE DE ERROS EM PERFIS VISUAIS NA CLASSIFICAÇÃO DE ÚLCERAS DIABÉTICAS E POR PRESSÃO**

**Resumo:**

**Objetivo:** Investigar se os erros de predição em modelos de Redes Neurais Convolucionais (CNNs) para a classificação entre úlceras diabéticas e por pressão estão concentrados em subclasses de perfis visuais específicos, como lesões com coloração amarelada ou variações de luminosidade. 

**Métodos:** Trata-se de um estudo experimental focado no desenvolvimento e avaliação de modelos de aprendizado profundo para apoio ao diagnóstico diferencial. Utilizou-se um conjunto de dados com 319 imagens clínicas, sendo 185 da classe Diabética e 134 da classe Pressão. O experimento comparou o desempenho de uma arquitetura sequencial customizada e o modelo MobileNetV2 via Transfer Learning. O treinamento utilizou aumento de dados dinâmico por meio da ferramenta ImageDataGenerator, com a supressão estratégica da variação de brilho no pré-processamento para preservar a integridade morfológica original das lesões. A avaliação baseou-se em métricas de acurácia, precisão e recall, acompanhadas de uma rigorosa análise qualitativa da matriz de confusão para correlacionar falhas algorítmicas com características visuais das feridas. 

**Resultados:** O modelo alcançou uma acurácia global de aproximadamente 97%, demonstrando a eficácia da arquitetura MobileNetV2. No entanto, a análise estratificada confirmou a hipótese central do estudo: observou-se que a maior concentração de falsos positivos e falsos negativos ocorreu em imagens com predominância visual de tecidos amarelados, indicativos de esfacelo. Através da inspeção visual das predições, identificou-se que o reflexo da luz (brilho) associado ao exsudato nessas lesões causava dispersão na extração de características (_features_) pelo algoritmo. Esse fenômeno comprometeu a nitidez dos limites entre o leito e a borda da ferida, confundindo a rede em padrões cromáticos específicos e induzindo ao erro de classificação. 

**Conclusão:** Evidencia-se que os erros de predição não são aleatórios, possuindo um forte viés ligado à coloração amarelada e à umidade do tecido (esfacelo). Conclui-se que, para o uso seguro do sistema como apoio à tomada de decisão clínica, é imperativo que o modelo receba um treinamento reforçado especificamente voltado para a diferenciação de reflexos luminosos em tecidos desvitalizados, garantindo a robustez necessária frente a variáveis ópticas do ambiente clínico.

**Palavras-chave:** Inteligência Artificial. Classificação de Feridas. MobileNetV2. Esfacelo. Visão Computacional.