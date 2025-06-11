
**Resumo** 

Aplicações de classificação de padrões podem ser encontradas em todos os lugares, especialmente aquelas que utilizam visão computacional.
O que as torna difíceis de incorporar é o fato de frequentemente exigirem muitos recursos computacionais.
A visão computacional embarcada tem sido aplicada em diversos contextos, como automação industrial ou residencial, robótica e tecnologias assistivas. Este trabalho realiza uma exploração do espaço de projeto em um sistema de classificação de imagens e incorpora uma aplicação de visão computacional em uma plataforma de recursos mínimos, visando
dispositivos vestíveis. O extrator de características e o classificador são avaliados quanto ao uso de memória e tempo de computação. Um método é proposto para otimizar tais características, levando a uma redução de mais de 99% no tempo de computação e 92% no uso de memória, em relação a uma implementação padrão. Resultados experimentais em uma plataforma ARM Cortex-M(https://pt.wikipedia.org/wiki/ARM_Cortex-M) mostraram um tempo total de classificação de 0,3 s, mantendo a mesma precisão da simulação realizada. Além disso, foram necessários menos de 20 KB de memória de dados,
que é o recurso mais limitado disponível em microcontroladores de baixo custo e baixo consumo de energia. A aplicação alvo, utilizada para a avaliação experimental, é um detector de faixa de pedestres usado para auxiliar pessoas com deficiência visual.


**Introdução** 

Para aumentar a eficiência e a produtividade em diversos contextos, sistemas de visão computacional têm sido desenvolvidos e utilizados para classificar padrões e detectar objetos em produtos e processos. No entanto, muitas vezes esses sistemas exigem alta capacidade de processamento e grande quantidade de memória de dados, o que impõe um desafio extra para a incorporação dessas aplicações. A plataforma da aplicação alvo frequentemente apresenta restrições de recursos, como tamanhos de memória reduzidos e baixo desempenho da CPU, que devem ser considerados nas etapas de projeto.O desenvolvimento de soluções de aprendizado de máquina para problemas práticos em estações de trabalho, onde não há limitações de recursos de hardware, é comum, mas a exploração das reais restrições para a incorporação dessas aplicações de aprendizado de máquina é frequentemente deixada para o futuro.
Um subconjunto interessante dessas aplicações visa atender às necessidades de pessoas com deficiência visual, e o dispositivo geralmente é um dispositivo vestível colocado no corpo do usuário. Esses dispositivos vestíveis têm certas restrições, pois exigem peso leve e Baterias de longa duração. Este trabalho visa criar um dispositivo vestível de visão computacional
para pessoas com deficiência visual.
Este trabalho apresenta uma exploração do espaço de projeto em um classificador de padrões composto por uma GLCM (Matriz de Concorrência de Níveis de Cinza) combinada
com uma SVM (Máquina de Vetores de Suporte) para colaborar com este dispositivo. Esta combinação provou ser eficaz para muitas aplicações de identificação de texturas [1–3].
Todo o processo foi testado em uma estação de trabalho e em uma plataforma embarcada, onde uma implementação final foi avaliada.
A principal contribuição deste trabalho é avaliar e reduzir a memória necessária no sistema classificador para que ele se encaixe em uma plataforma de microcontrolador
de baixo consumo, preservando a precisão obtida
em estações de trabalho com recursos muito maiores.
Este artigo está organizado da seguinte forma: A Seção 2 descreve alguns
dos sistemas de última geração que apresentam trabalhos relacionados,
bem como diferentes tentativas de embarcar sistemas de visão computacional.
A Seção 3 descreve a exploração deste trabalho e os resultados obtidos utilizando uma estação de trabalho. A Seção 4 mostra os resultados do classificador em uma plataforma embarcada. A Seção 5 apresenta a conclusão
do artigo e discute os resultados obtidos.


2.2. Propostas para auxiliar pessoas com deficiência visual
Existem muitas aplicações de visão computacional que utilizam classificação de padrões
e são adequadas para esta investigação. Neste artigo,
uma aplicação automática de Detecção de Faixa de Pedestres foi escolhida.
Na literatura, diversas estratégias têm sido utilizadas para detectar
faixas de pedestres. Em [14], o processamento de imagens morfológicas de multirresolução foi utilizado para detectar faixas de pedestres e semáforos. Utilizando imagens de
(640 × 360) pixels, obteve-se uma precisão de 98,33% para
as faixas de pedestres e 89,47% para a detecção de semáforos de pedestres. Os algoritmos foram implementados em um dispositivo portátil,
baseado em um Intel Atom Z3735F operando a 1,33 GHz e atingindo
um tempo de computação de 218,1 ms. Nenhuma avaliação de memória foi realizada. O autor não utilizou plataformas embarcadas nem aprendizado de máquina
para detectar padrões. Com base na plataforma escolhida, pode-se
inferir que a solução consome muita energia.
Poggi [15] utilizou uma CNN (Rede Neural Convolucional) com uma arquitetura MLP (Perceptron Multicamadas) de 2 camadas para detectar faixas de pedestres. O dispositivo utilizou imagens capturadas por uma câmera 3D e as classificou em 5 classes diferentes: vertical, horizontal, diagonal esquerda, diagonal direita e outras (sem faixas de pedestres). O método proposto alcançou uma precisão entre 88% e 94% para as 5 classes treinadas, com tempo de computação de 200 ms em um dispositivo Exynos 4412 Prime de 1,7 GHz. Esta é outra implementação de CPU de alto desempenho, mais focada na precisão do classificador do que na economia de energia. Um classificador SVM usando padrões binários locais (LBP), GLCM e uma combinação de ambos como extratores de características foi utilizado em [16]. As imagens foram obtidas de um satélite e a computação foi realizada por um smartphone. O método proposto obteve
uma precisão de 96,6% usando LBP, 90,9% usando LBP + GLCM e 90,3%
usando GLCM. Outros métodos que utilizam aprendizado profundo, como AlexNet,
VGG e GoogLeNet, são aplicados em imagens de satélite para detectar faixas de pedestres em [17], obtendo uma precisão média de 97,11%.
Normalmente, a investigação com uma plataforma embarcada é deixada para
trabalhos futuros, ou as plataformas escolhidas para validação possuem recursos computacionais e energéticos "abundantes", como smartphones, e
os recursos consumidos pelo dispositivo não são avaliados.
A presente investigação está mais focada em plataformas muito restritas que podem ser facilmente usadas por pessoas com deficiência visual,
propondo também estratégias para otimização de recursos computacionais.