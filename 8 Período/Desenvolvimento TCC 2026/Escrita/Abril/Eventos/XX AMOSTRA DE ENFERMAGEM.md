
 Título : **ANÁLISE DE ERROS EM PERFIS VISUAIS NA CLASSIFICAÇÃO DE ÚLCERAS DIABÉTICAS E POR PRESSÃO** ✅ ❌  ( Deixar por último )
 
 * Suzana : ( Utilizar Palavras Chaves como Visão Computacional , Inteligência Artificial, Machine Learning, Deep Learning ) 
 * Felipe : Palavras como Visão Computacional e MobileNetV2 deve conter no título.

**Objetivos:** 


Objetivo: Esta pesquisa tem como objetivo desenvolver um modelo de Visão Computacional baseado em Inteligência Artificial para detecção e classificação de feridas. Utilizando-se a arquitetura MobileNetV2. O foco do estudo reside na otimização do modelo para execução em ambiente móvel para apoio a tomada de decisão. ❌

Objetivo:   Desenvolver e prototipar um modelo de Visão Computacional leve, utilizando a arquitetura MobileNetV2, para a detecção e classificação de feridas em dispositivos móveis. O foco reside na viabilização da tecnologia para ambientes de recursos computacionais limitados como ferramenta de apoio à decisão aos profissionais da saúde. ❌

Objetivo: Investigar e desenvolver um protótipo de modelo de Visão Computacional de arquitetura leve, fundamentado na rede MobileNetV2, voltado para a detecção e classificação de feridas cutâneas em dispositivos de recursos limitados. A pesquisa busca analisar a viabilidade técnica dessa tecnologia em cenários de recursos computacionais limitados, visando fundamentar uma futura ferramenta de apoio à decisão clínica que auxilie profissionais da saúde na padronização e monitoramento de lesões, como úlceras diabéticas e por pressão.




Dúvidas que surgiram durante a escrita dos Objetivos  : 

Quais objetivos do contexto geral ?  Sendo que eu fiz foi fazer um estudo experimental, para testar a arquitetura sequencial através de uma base de dados limitadas com poucas imagens e variedades ?  Até o amadurecimento da ideia e construção do aplicativo mobile. 

---
**Métodos e Metodologia :** 


Metodologia:  A metodologia envolveu o treinamento da arquitetura MobileNetV2 utilizando as bases de dados públicas _AZH Wound Care_ e _Medetec Wound Database_. Foram aplicadas técnicas de _transfer learning_ e _data augmentation_ para a classificação das feridas. Paralelamente, desenvolveu-se um protótipo de aplicativo móvel projetado para integrar o modelo e operar de forma offline, garantindo o suporte à tomada de decisão em dispositivos com recursos computacionais limitados.



Metodologia: A metodologia adotada seguiu uma abordagem experimental e incremental, dividida em etapas de treinamento e teste de modelos. Inicialmente, explorou-se o desempenho de uma Rede Neural Convolucional (CNN) de estrutura sequencial para estabelecer parâmetros iniciais de classificação. Como base de dados, foram utilizadas as bibliotecas públicas AZH Wound Care e Medetec Wound Database, que passaram por processos de pré-processamento e aumento de dados (data augmentation). Técnicas como rotação, ajuste de brilho e zoom foram aplicadas para diversificar as amostras e compensar as limitações quantitativas do dataset original. Visando otimizar a eficiência e adequar o sistema para dispositivos com hardware restrito, a pesquisa evoluiu para a implementação da arquitetura MobileNetV2. Esta transição permitiu explorar o aprendizado por transferência (transfer learning) e as camadas de convolução separáveis em profundidade, características que tornam o modelo mais leve e rápido. Paralelamente aos testes de arquitetura, iniciou-se a construção de um protótipo de aplicativo móvel para testar a integração dos modelos e avaliar a viabilidade do processamento local e offline, simulando condições reais de uso.


Dúvidas que surgiram durante a escrita dos métodos  : 


Tenho que colocar incialmente pela base da dados ? Porque se for seria seguindo a ordem cronológica de pesquisa ao qual iniciei por ela, em seguida experimentando as redes neurais convolucionais como a rede sequencial, em seguida a mobilenetv2 e por fim a yolov3.

E até qual ponto eu vou ? Trabalhar com o comparativo entre os resultados das arquiteturas ?  

--- 


**Resultados:**  


Resultados: Os resultados indicaram que o modelo baseado na arquitetura MobileNetV2 alcançou uma acurácia de aproximadamente 97%, demonstrando o potencial da visão computacional para a classificação de feridas. Todavia, é imperativo destacar que o sistema atua como ferramenta de suporte e não substitui o julgamento clínico do profissional de saúde. A confiabilidade dos resultados é influenciada pelas limitações quantitativas e de diversidade do _dataset_ utilizado, o que reforça a necessidade de uma interpretação clínica soberana e complementar aos dados fornecidos pela inteligência artificial. ❌

Resultados: Os testes comparativos realizados demonstraram que a migração da CNN sequencial para a arquitetura MobileNetV2 resultou em uma melhora significativa no desempenho computacional e na precisão. Nos experimentos controlados, o modelo alcançou uma acurácia aproximada de 97%. Durante a avaliação das métricas, priorizou-se o Recall (sensibilidade), visando garantir que o sistema minimize a ocorrência de falsos negativos, o que é crítico no contexto clínico para evitar que lesões reais passem despercebidas. Embora esses índices iniciais sejam promissores, eles foram obtidos dentro das limitações de diversidade e tamanho da base de dados atual. Portanto, tais dados não devem ser vistos como comprovações diagnósticas definitivas, mas sim como evidências de que os modelos estão apresentando um comportamento satisfatório no reconhecimento de padrões de úlceras diabéticas e por pressão sob condições experimentais.



Dúvidas que surgiram durante a escrita dos Resultados  :

Os resultados que eu trago são todos ? os que tenho até o momento ? Pois eu tenho diversos notebooks cujo trabalho com as diversas classes, e com diversos resultados e métricas diferentes além de inúmeras metodologias aplicadas, quais os  materiais necessariamente vu utilizar ?  

--- 


**Conclusões:**



Conclusão ou Considerações Finais: O estudo evidenciou a viabilidade técnica da arquitetura MobileNetV2 para operação _offline_ em dispositivos com restrições de hardware. Ressalta-se que, embora o estudo de caso tenha sido conduzido no domínio da saúde, a pesquisa priorizou aspectos tecnológicos, de modo que os resultados representam evidências de aplicabilidade técnica e não conclusões diagnósticas definitivas. Como limitações, apontam-se o tamanho do conjunto de dados e a ausência de testes de usabilidade em ambiente real. Para trabalhos futuros, propõe-se o desenvolvimento pleno do aplicativo móvel para implementação em larga escala, destacando sua importância fundamental na prática clínica de profissionais de saúde. A continuidade deste projeto visa consolidar uma ferramenta de suporte à decisão que promova maior agilidade, segurança e precisão no tratamento de lesões cutâneas no ponto de atendimento.  ❌
  

  

\textbf{Considerações Finais:} Conclui-se que o trabalho encontra-se em estágio de desenvolvimento de protótipo, não estando ainda finalizado. A pesquisa cumpriu a etapa de demonstrar a viabilidade técnica preliminar de modelos leves para o domínio das feridas cutâneas. Ressalta-se que o sistema é concebido como uma ferramenta de suporte e não possui caráter comprobatório ou substitutivo à visão do profissional. Para trabalhos futuros, visa-se a construção efetiva do aplicativo móvel e a integração completa com o modelo de visão computacional otimizado, além da expansão da base de dados e a realização de testes de usabilidade com usuários finais. Este esforço contínuo busca consolidar uma solução que, no futuro, promova maior agilidade e precisão no suporte à decisão clínica no ponto de atendimento. 

Dúvidas que surgiram durante a escrita das Conclusões :  

Por fim a conclusão quais conclusões tenho que trazer ? O que vai ficar para trabalhos futuros ? Ou o que está em desenvolvimento até o momento ?  Quais abordagens conclusivas eles vão querer ? 

---


**Palavras-chave:** Inteligência Artificial. Classificação de Feridas. MobileNetV2. Visão Computacional.



--- 


#Escrita

\textbf{Objetivo:} Investigar e desenvolver um protótipo de modelo de Visão Computacional de arquitetura leve, fundamentado na rede MobileNetV2, voltado para a triagem e classificação de feridas cutâneas em dispositivos móveis. A pesquisa analisa a viabilidade técnica dessa tecnologia em cenários de recursos computacionais limitados, visando fundamentar uma futura ferramenta de apoio à decisão clínica que reforce a precisão técnica no monitoramento de lesões, como úlceras diabéticas e por pressão. \textbf{Metodologia:} A abordagem caracteriza-se como experimental e incremental, em estágio de prototipagem. Inicialmente, explorou-se o desempenho de uma Rede Neural Convolucional (CNN) de estrutura sequencial para estabelecer métricas base. Para otimizar a eficiência em hardware mobile, a pesquisa evoluiu para a arquitetura MobileNetV2 com transferência de aprendizado (\textit{transfer learning}). Utilizaram-se as bases públicas AZH Wound Care e Medetec Wound Database, submetidas a técnicas de aumento de dados (\textit{data augmentation}), como rotação e ajuste de brilho, para mitigar limitações quantitativas do \textit{dataset}. Esta fase concentra-se na execução de testes experimentais para avaliação de desempenho das arquiteturas frente ao problema proposto, sem implementação final em ambiente de produção \textbf{Resultados:} Os testes comparativos indicaram que a transição para a MobileNetV2 apresentou resultados iniciais promissores, atingindo acurácia aproximada de 97\% em ambiente controlado. Priorizou-se a métrica de \textit{Recall} (sensibilidade) para minimizar falsos negativos, fator crítico para a segurança do paciente. Ressalta-se que tais índices não configuram comprovações diagnósticas, mas evidências do potencial técnico do modelo, que permanece em estágio de aprimoramento e ajuste de hiperparâmetros. \textbf{Conclusão:} O trabalho demonstra a viabilidade técnica preliminar de modelos leves no domínio das feridas, embora ainda não esteja finalizado. O modelo é concebido estritamente como suporte técnico e não substitui a avaliação profissional. Para trabalhos futuros, planeja-se a construção efetiva do aplicativo móvel e a integração completa com o modelo de visão computacional otimizado, além da expansão da base de dados e realização de testes de usabilidade em ambiente real, buscando consolidar uma ferramenta que promova agilidade e precisão no suporte à decisão clínica no ponto de atendimento.