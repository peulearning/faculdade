
🔬 Hipótese Principal 
> Será que os erros de predição que o modelo está cometendo estão concentrados em uma dessas subclasses de perfis visuais específicos (ex: apenas nas amareladas ou apenas nas mais escuras)?

 Título : **ANÁLISE DE ERROS EM PERFIS VISUAIS NA CLASSIFICAÇÃO DE ÚLCERAS DIABÉTICAS E POR PRESSÃO** ✅ ❌  ( Utilizar Palavras Chaves como Visão Computacional , Inteligência Artificial, Machine Learning, Deep Learning )

**Resumo:**

**Objetivo:** Avaliar a influência de perfis teciduais específicos no desempenho de modelos de Redes Neurais Convolucionais (CNNs) MobileNetV2 para a classificação de úlceras, investigando se a concentração de erros está vinculada a características das lesões.

**Métodos:** Estudo experimental quantitativo utilizando 317 imagens clínicas (185 Diabéticas; 132 de Pressão). As imagens foram estratificadas em quatro perfis visuais: Vermelhado, Amarelado, Branco e Necrosado. O experimento utilizou a arquitetura MobileNetV2 via _Transfer Learning_. O treinamento manteve a distribuição original dos perfis, suprimindo variações de brilho no pré-processamento para isolar a resposta do modelo às características dos tecidos. 

**Resultados:** O modelo alcançou acurácia global de 97%. Entretanto, a análise estratificada confirmou a hipótese central: os erros de predição concentraram-se na classe de **Pressão**, especificamente no perfil **Amarelado** (esfacelo). Identificou-se que a rede neural desenvolveu um viés estatístico, associando tecidos amarelados à classe Diabética devido à sua maior prevalência (28% nesta classe contra 12% na de Pressão). O brilho  nessas lesões potencializou a dispersão na extração de características, induzindo a falhas de classificação sistemáticas (falsos negativos para pressão).

**Conclusão:** Evidencia-se que a alta acurácia global mascara vulnerabilidades críticas relacionadas à composição tecidual. Os erros não são aleatórios, mas fruto de um **viés de prevalência cromática** em tecidos desvitalizados. Conclui-se que, para a tomada de decisão, é imperativo que o treinamento seja reforçado com o balanceamento dos perfis visuais entre as classes, garantindo que o modelo aprenda a diferenciar patologias independentemente da predominância de esfacelo.

**Palavras-chave:** Inteligência Artificial. Classificação de Feridas. MobileNetV2. Visão Computacional.