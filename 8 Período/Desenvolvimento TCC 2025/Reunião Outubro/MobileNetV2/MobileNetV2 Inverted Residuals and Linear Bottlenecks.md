
MobileNetV2: Resíduos Invertidos e Gargalos Lineares


**Resumo**

Neste artigo, descrevemos uma nova arquitetura móvel, MobileNetV2, que melhora o desempenho de última geração de modelos móveis em múltiplas tarefas e benchmarks, bem como em um espectro de diferentes tamanhos de modelo. Também descrevemos maneiras eficientes de aplicar esses modelos móveis à detecção de objetos em uma nova estrutura
que chamamos de SSDLite. Além disso, demonstramos como construir modelos de segmentação semântica móvel por meio de uma forma reduzida do DeepLabv3, que chamamos de Mobile
DeepLabv3. baseia-se em uma estrutura residual invertida, onde as conexões de atalho estão entre as camadas finas de gargalo. A camada de expansão intermediária utiliza convoluções leves em profundidade para filtrar recursos como uma fonte de não linearidade. Além disso, descobrimos que é importante remover não linearidades nas camadas estreitas para manter o poder de representação. Demonstramos que isso melhora o desempenho e fornecemos uma intuição que levou a este projeto. Por fim, nossa abordagem permite o desacoplamento dos domínios de entrada/saída da expressividade da transformação, o que fornece uma estrutura conveniente para análises posteriores. Medimos nosso desempenho na classificação ImageNet [1], na detecção de objetos COCO [2] e na segmentação de imagens VOC [3]. Avaliamos as compensações entre precisão e número de operações medidas por multiplicação-adição (MAdd), bem como a latência real e o número de parâmetros.

1. **Introduction**

As redes neurais revolucionaram muitas áreas da inteligência das máquinas, permitindo precisão sobre-humana para tarefas desafiadoras de reconhecimento de imagens. No entanto, a busca
por melhorar a precisão geralmente tem um custo: as redes modernas de última geração exigem altos recursos computacionais além das capacidades de muitos dispositivos móveis e embarcados aplicações. Este artigo apresenta uma nova arquitetura de rede neural especificamente adaptada para ambientes móveis e com recursos limitados. Nossa rede impulsiona o estado da arte em modelos de visão computacional adaptados para dispositivos móveis, reduzindo significativamente o número de operações e a memória necessária, mantendo a mesma precisão.
Nossa principal contribuição é um novo módulo de camada: o resíduo invertido com gargalo linear. Este módulo recebe como entrada uma representação compactada de baixa dimensão
que é primeiro expandida para alta dimensão e filtrada com uma convolução leve em profundidade. Os recursos são posteriormente projetados de volta para uma representação de baixa dimensão com uma convolução linear. A implementação oficial está disponível como parte da biblioteca de modelos TensorFlow-Slim em [4]. Este módulo pode ser implementado eficientemente usando operações padrão em qualquer estrutura moderna e permite que nossos modelos superem o estado da arte em vários pontos de desempenho usando benchmarks padrão. Além disso, este módulo convolucional é particularmente adequado para projetos móveis, pois permite reduzir significativamente o consumo de memória necessário durante a inferência, nunca materializando totalmente grandes tensores intermediários. Isso reduz a necessidade de acesso à memória principal em muitos projetos de hardware embarcado, que fornecem pequenas
quantidades de memória cache controlada por software muito rápida.

2. **Trabalhos Correlatos**

Ajustar arquiteturas neurais profundas para atingir um equilíbrio ideal entre precisão e desempenho tem sido uma área de pesquisa ativa nos últimos anos. Tanto a busca manual de arquitetura quanto as melhorias em algoritmos de treinamento, realizadas por diversas equipes, levaram a melhorias drásticas em relação a projetos iniciais, como AlexNet [5], VGGNet [6], GoogLeNet [7] e ResNet [8]. Recentemente, houve muito progresso na exploração de arquitetura algorítmica, incluindo otimização de hiperparâmetros [9, 10, 11], bem como vários métodos de poda de rede [12, 13, 14, 15, 16, 17] e aprendizagem de conectividade [18, 19]. Uma quantidade substancial de trabalho também foi dedicada à alteração da estrutura de conectividade dos blocos convolucionais internos, como no ShuffleNet [20] ou à introdução de dispersão [21] e outros [22]. Recentemente, [23, 24, 25, 26] inauguraram uma nova direção, trazendo métodos de otimização, incluindo algoritmos genéticos e aprendizado por reforço para a busca arquitetônica. No entanto, uma desvantagem é que as redes resultantes acabam sendo muito complexas. Neste artigo, buscamos o objetivo de desenvolver uma melhor intuição sobre como as redes neurais operam e usar isso para orientar o projeto de rede mais simples possível. Nossa abordagem deve ser vista como complementar à descrita em [23] e trabalhos relacionados. Nesse sentido, nossa abordagem é semelhante à adotada por [20, 22] e permite aprimorar ainda mais o desempenho, ao mesmo tempo em que fornece uma visão geral de sua operação interna. Nosso projeto de rede é baseado no MobileNetV1 [27]. Ele mantém sua simplicidade e não requer operadores especiais, ao mesmo tempo em que melhora significativamente sua precisão, alcançando o estado da arte em múltiplas tarefas de classificação e detecção de imagens para aplicativos móveis.

3. **Preliminares, discussão e intuição**

**3.1. Convoluções Separáveis ​​em Profundidade**
Convoluções Separáveis ​​em Profundidade são um bloco de construção fundamental para muitas arquiteturas de redes neurais eficientes[27, 28, 20] e também as utilizamos neste trabalho.
A ideia básica é substituir um operador convolucional completo por uma versão fatorada que divide a convolução em duas camadas separadas. A primeira camada é chamada de convolução em profundidade e realiza uma filtragem leve aplicando um único filtro convolucional por canal de entrada. A segunda camada é uma convolução 1 × 1, chamada de convolução pontual que é responsável por construir novos recursos por meio do cálculo de combinações lineares dos canais de entrada. 

A convolução padrão utiliza um tensor de entrada hi × wi × di . Li, e aplica o kernel convolucional K ∈ Rk×k×di×dj para produzir um tensor de saída hi × wi × dj Lj. Camadas convolucionais padrão têm o custo computacional de hi · wi · di · dj · k · k.
Convoluções separáveis ​​em profundidade são uma substituição imediata para camadas convolucionais padrão. Empiricamente, elas funcionam quase tão bem quanto as convoluções regulares mas apenas custam hi · wi · di(k 2 + dj ) que é a soma da profundidade e 1 × 1 no ponto Convoluções. A convolução separável em profundidade reduz efetivamente a computação em comparação com camadas tradicionais por quase um fator de k^21 . O MobileNetV2 utiliza k = 3
(3 × 3 convoluções separáveis ​​em profundidade), de modo que o custo computacional é de 8 a 9 vezes menor do que o das convoluções padrão, com apenas uma pequena redução na precisão [27]

**3.2. Gargalos Lineares**

Considere uma rede neural profunda composta por n camadas Li, cada uma das quais possui um tensor de ativação de dimensões hi × wi × di. Ao longo desta seção, discutiremos as propriedades básicas desses tensores de ativação, que trataremos como contêineres de "pixels" hi × wi
com dimensões di. Informalmente, para um conjunto de entrada de imagens reais,
dizemos que o conjunto de ativações de camadas (para qualquer camada Li) forma uma "variedade de interesse". Há muito tempo se supõe que variedades de interesse em redes neurais
podem estar inseridas em subespaços de baixa dimensão. Em outras palavras, quando observamos todos os pixels individuais do canal-d de uma camada convolucional profunda, a informaçãocodificada nesses valores, na verdade, reside em alguma variedade, que, por sua vez, pode ser inserida em um subespaço de baixa dimensão2. À primeira vista, tal fato poderia ser capturado e explorado simplesmente reduzindo a dimensionalidade de uma camada, reduzindo assim a dimensionalidade do espaço operacional. Isso foi explorado com sucesso pelo MobileNetV1 [27] para efetivamente equilibrar computação e precisão por meio de um parâmetro multiplicador de largura, e foi incorporado em projetos de modelos eficientes
de outras redes também [20]. Seguindo essa intuição, a abordagem do multiplicador de largura permite reduzir adimensionalidade do espaço de ativação até que a variedade de interesse abranja todo esse espaço. No entanto, essa intuição se desfaz quando lembramos que redes neurais convolucionais profundas, na verdade, possuem transformações não lineares por coordenada, como ReLU. Por exemplo, ReLU aplicado a uma linha no espaço 1D produz um 'raio',
enquanto que, como no espaço Rn, geralmente resulta em uma curva linear por partes com n-junções. É fácil perceber que, em geral, se o resultado de uma transformação de camada
ReLU(Bx) tiver um volume S diferente de zero, os pontos mapeados para o interior S são obtidos por meio de uma transformação linear B da entrada, indicando assim que a parte do espaço de entrada correspondente à saída dimensional completa está limitada a uma transformação linear.
Em outras palavras, redes profundas têm apenas o poder de um classificador linear na parte de volume diferente de zero do 

![[Pasted image 20251019131821.png]]

Figura 1: Exemplos de transformações ReLU de
variedades de baixa dimensão imersas em espaços de alta dimensão.
Nestes exemplos, a espiral inicial é imersa emum espaço n-dimensional usando a matriz aleatória T, seguida por ReLU, e então projetada de volta para o espaço 2D usando T
−1. Nos exemplos acima, n = 2 e 3 resultam em perda de informação, onde
certos pontos da variedade colapsam uns nos outros, enquanto para n = 15 a 30 a transformação é altamente não convexa.

![[Pasted image 20251019132630.png]]

Figura 2: Evolução de blocos de convolução separáveis. A textura hachurada diagonalmente indica camadas que não contêm não linearidades. A última camada (claramente colorida) indica o início do próximo bloco. Observação: 2d e 2c são blocos equivalentes quando empilhados. Melhor visualizado em cores.

Domínio de saída. Consultamos material suplementar para uma declaração mais formal.
Por outro lado, quando ReLU colapsa o canal, ele inevitavelmente perde informações nesse canal. No entanto, se tivermos muitos canais e houver uma estrutura na variedade de ativação, essa informação ainda pode ser preservada nos outros canais. Em materiais suplementares, mostramos que, se a variedade de entrada puder ser incorporada em um subespaço de dimensão significativamente inferior do espaço de ativação, então a transformação ReLU preserva a informação enquanto introduz a complexidade necessária ao conjunto de funções expressáveis.
Para resumir, destacamos duas propriedades que são indicativas da exigência de que a variedade
de interesse esteja em um subespaço de baixa dimensão do espaço de ativação de dimensão superior:

1. Se a variedade de interesse permanecer com volume diferente de zero após a transformação ReLU, isso corresponde a uma transformação linear.

![[Pasted image 20251019133052.png]]

Figura 3: A diferença entre o bloco residual [8, 30] e o residual invertido. Camadas hachuradas diagonalmente não utilizam não linearidades. Usamos a espessura de cada bloco para
indicar seu número relativo de canais. Observe como os resíduos clássicos conectam as camadas com alto número de canais, enquanto os resíduos invertidos conectam os gargalos. Melhor visualizado em cores.

2. ReLU é capaz de preservar informações completas sobre a variedade de entrada, mas somente se a variedade de entrada estiver em um subespaço de baixa dimensão do espaço de entrada.

Essas duas percepções nos fornecem uma dica empírica para otimizar as arquiteturas neurais existentes: assumindo que a variedade de interesse é de baixa dimensão, podemos capturar isso inserindo camadas de gargalo linear nos blocos convolucionais. Evidências experimentais sugerem
que o uso de camadas lineares é crucial, pois evita que não linearidades destruam muita informação. NaSeção 6, mostramos empiricamente que o uso de camadas não lineares
em gargalos de fato prejudica o desempenho em vários por cento, validando ainda mais nossa hipótese³. Notamos que relatórios semelhantes onde a não linearidade foi auxiliada foram relatados em [29], onde a não linearidade foi removida da entrada do bloco residual tradicional e que levaram a um melhor desempenho no conjunto de dados CIFAR. No restante deste artigo, utilizaremos convoluções de gargalo. Chamaremos a razão entre o tamanho do gargalo de entrada e o tamanho interno de razão de expansão. 

**3.3. Resíduos invertidos**

Os blocos de gargalo parecem semelhantes ao bloco de resíduos, onde cada bloco contém uma entrada seguida por vários gargalos, seguidos por uma expansão [8]. No entanto, inspirados pela intuição de que os gargalos na verdade contêm todas as informações necessárias, enquanto uma
camada de expansão atua meramente como um detalhe de implementação que acompanha uma transformação não linear do tensor, usamos atalhos diretamente entre os gargalos.A Figura 3 fornece uma visualização esquemática da diferença nos designs. A motivação para inserir atalhos é semelhante à das conexões residuais clássicas: queremos melhorar a capacidade de propagação de um gradiente através das camadas multiplicadoras. No entanto, o design invertido é consideravelmente mais eficiente em termos de memória (consulte a Seção 5 para
detalhes), além de funcionar um pouco melhor em nossos experimentos.

**Tempo de execução e contagem de parâmetros para convolução de gargalo.**

A estrutura básica de implementação é ilustrada na Tabela 1. Para um bloco de tamanho h × w, fator de expansão t e tamanho de kernel k com d'.canais de entrada e d'' canais de saída, o número total de multiplicações e somas necessárias é h · w · d'· t(d' + k² + d''). Comparada com (1), esta expressão tem um termo extra, pois de fato temos uma convolução 1 × 1 extra; no entanto, a natureza de nossas redes nos permite utilizar dimensões de entrada e saída muito menores. Na Tabela 3, comparamos os tamanhos necessários para cada resolução entre MobileNetV1,MobileNetV2 e ShuffleNet.

**3.4. Interpretação do fluxo de informações**

Uma propriedade interessante da nossa arquitetura é que ela
fornece uma separação natural entre os domínios de entrada/saída dos blocos de construção (camadas de gargalo) e a transformação da camada – que é uma função não linear que converte a entrada em saída. A primeira pode ser vista como a capacidade da rede em cada camada, enquanto a última como a expressividade. Isso contrasta com os blocos convolucionais tradicionais, tanto regulares quanto separáveis, onde tanto a expressividade quanto a capacidade estão entrelaçadas e são funções da profundidade da camada de saída. Em particular, no nosso caso, quando a profundidade da camada interna é 0, a convolução subjacente é a função identidade graças à conexão de atalho. Quando a taxa de expansão é menor que 1, este é um bloco convolucional residual clássico [8, 30]. No entanto, para os nossos propósitos,
mostramos que uma taxa de expansão maior que 1 é a mais útil. Essa interpretação nos permite estudar a expressividade da rede separadamente de sua capacidade e acreditamos que uma exploração mais aprofundada dessa separação é necessária para fornecer uma melhor compreensão das propriedades da rede.

4. **Arquitetura do Modelo**

Agora, descrevemos nossa arquitetura em detalhes. Conforme discutido na seção anterior, o bloco de construção básico é uma convolução separável em profundidade com gargalo e resíduos. A estrutura detalhada deste bloco é mostrada em

![[Pasted image 20251019144246.png]]

Tabela 1: Bloco residual de gargalo transformando de k para k' canais, com passo s e fator de expansão t.

Tabela 1. A arquitetura do MobileNetV2 contém a camada inicial de convolução completa com 32 filtros, seguida por 19 camadas de gargalo residual descritas na Tabela 2. Utilizamos ReLU6 como não linearidade devido à sua robustez quando usado com computação de baixa precisão [27]. Sempre utilizamos o tamanho de kernel 3 × 3, como padrão para redes modernas, e utilizamos dropout e normalização em lote durante o treinamento.

Com exceção da primeira camada, usamos uma taxa de expansão constante em toda a rede. Em nossos experimentos, descobrimos que taxas de expansão entre 5 e 10 resultam em curvas de desempenho quase idênticas, com redes menores apresentando melhor desempenho com taxas de expansão ligeiramente menores e redes maiores apresentando desempenho ligeiramente melhor com taxas de expansão maiores.

Para todos os nossos experimentos principais, usamos um fator de expansão de 6 aplicado ao tamanho do tensor de entrada. Por exemplo, para uma camada de gargalo que utiliza um tensor de entrada de 64 canais e produz um tensor com 128 canais, a camada de expansão intermediária é então 64 · 6 = 384 canais.

**Parâmetros de compensação:**

Como em [27], adaptamos nossa arquitetura a diferentes pontos de desempenho, usando
a resolução da imagem de entrada e o multiplicador de largura como hiperparâmetros ajustáveis, que podem ser ajustados dependendo das compensações desejadas entre precisão/desempenho. Nossa rede primária (multiplicador de largura 1, 224 × 224) tem um custo computacional de 300 milhões de multiplicações-adições e usa 3,4 milhões de parâmetros. Exploramos as compensações de desempenho para resoluções de entrada de 96 a 224 e multiplicadores de largura de 0,35 a 1,4. O custo computacional da rede varia de 7 multiplicações-adições a 585 milhões de MAdições, enquanto o tamanho do modelo varia entre 1,7 milhões e 6,9 milhões de parâmetros.
Uma pequena diferença de implementação com [27] é que, para multiplicadores menores que um, aplicamos o multiplicador de largura a todas as camadas, exceto à última camada convolucional.
Isso melhora o desempenho de modelos menores.

![[Pasted image 20251019151013.png]]

Tabela 2: MobileNetV2: Cada linha descreve uma sequência de 1 ou mais camadas idênticas (módulo stride), repetidas n vezes. Todas as camadas na mesma sequência têm o mesmo
número c de canais de saída. A primeira camada de cada sequência tem um stride s e todas as outras usam o stride 1. Todas as convoluções espaciais usam kernels 3 × 3. O fator de expansão
t é sempre aplicado ao tamanho da entrada, conforme descrito na Tabela 1.

![[Pasted image 20251019232416.png]]


Tabela 3: Número máximo de canais/memória (em Kb) que precisam ser materializados em cada resolução espacial para diferentes arquiteturas. Assumimos floats de 16 bits para ativações. Para ShuffleNet, usamos 2x, g = 3, que corresponde ao desempenho de MobileNetV1 e MobileNetV2. Para a primeira camada de MobileNetV2 e ShuffleNet, podemos empregar o truque descrito na Seção 5 para reduzir a necessidade de memória. Embora ShuffleNet empregue gargalos em outros lugares, os tensores não gargalos ainda precisam ser materializados devido à
presença de atalhos entre os tensores não gargalos.

5. **Notas de Implementação**

5.1. Inferência com eficiência de memória
As camadas de gargalo residual invertidas permitem uma implementação particularmente eficiente em termos de memória, o que é muito
importante para aplicações móveis. Um padrão de eficiência A influência no grafo de computação G pode ser simplificada:

![[Pasted image 20251019232842.png]]

Ou, para reformular, a quantidade de memória é simplesmente o tamanho total máximo de entradas e saídas combinadas em todas as operações. A seguir, mostramos que, se tratarmos
um bloco residual de gargalo como uma única operação (e tratarmos a convolução interna como um tensor descartável), a quantidade total de memória seria dominada pelo tamanho dos
tensores de gargalo, em vez do tamanho dos tensores internos ao gargalo (e muito maiores).

Bloco Residual de Gargalo Um operador de bloco de gargalo F(x) mostrado na Figura 3b pode ser expresso como uma composição de três operadores F(x) = [A ◦ N ◦ B]x,
onde A é uma transformação linear A : Rs×s×k → Rs×s×n, N é uma transformação não linear por canal: N : Rs×s×n → Rs'×s'×n, e B é novamente uma transformação linear para o domínio de saída: B : R^s'×^s'×^n  → Rs'×s'×k'.
Para nossas redes N = ReLU6 ◦ dwise ◦ ReLU6, mas os resultados se aplicam a qualquer transformação por canal. Suponha que o tamanho do domínio de entrada seja |x| e o tamanho
do domínio de saída seja |y|, então a memória necessária para calcular F(X) pode ser tão baixa quanto |s²k| + |s'²k'|2k'| + O(max(s^2, s'^2)). O algoritmo é baseado no fato de que o tensor interno I pode ser representado como uma concatenação de t tensores, de tamanho n/t cada, e nossa função pode então ser representada como

![[Pasted image 20251019233249.png]]


Ao acumular a soma, precisamos manter apenas um bloco intermediário de tamanho n/t na memória o tempo todo. Usando n = t, acabamos tendo que manter apenas um único
canal da representação intermediária o tempo todo. As duas restrições que nos permitiram usar esse truque são (a) o fato de que a transformação interna (que inclui não linearidade e profundidade) é por canal, e (b) os operadores não por canal consecutivos têm uma proporção significativa entre o tamanho da entrada e o tamanho da saída. Para a maioria das redes neurais tradicionais, esse truque não produziria uma melhoria significativa. We note that, the number of multiply-adds operators needed to compute F(X) using t-way split is independent of t, however in existing implementations we find that replacing one matrix multiplication with several smaller ones hurts runtime performance due to in

![[Pasted image 20251019233444.png]]

![[Pasted image 20251019233455.png]]

Implementação eficiente de inferência que utiliza, por exemplo, TensorFlow[31] ou Caffe [32], constrói um hipergrafo de computação acíclico direcionado G, consistindo de arestas representando as operações e nós representando tensores da computação intermediária. A computação é escalonada para minimizar o número total de tensores que precisa ser armazenado na memória. No caso mais geral, ele busca todas as ordens de computação plausíveis Σ(G) e escolhe aquela que minimiza

![[Pasted image 20251019233525.png]]

onde R(i, π, G) é a lista de tensores intermediários que
estão conectados a qualquer um dos πi . . . πn nós, |A| representa
o tamanho do tensor A e tamanho(i) é a quantidade total de memória necessária para armazenamento interno durante a operação i.

Para gráficos que possuem apenas estrutura paralela trivial
(como conexão residual), há apenas uma ordem de computação viável não trivial e, portanto, a quantidade total
e um limite na memória necessária para inferir

![[Pasted image 20251019233618.png]]

Figura 5: Curva de desempenho do MobileNetV2 vs. MobileNetV1, ShuffleNet, NAS. Para nossas redes, usamos multiplicadores de 0,35, 0,5, 0,75 e 1,0 para todas as resoluções, e um adicional de 1,4 para 224. Melhor visualizado em cores.

![[Pasted image 20251019233649.png]]

Figura 6: O impacto das não linearidades e vários tipos de conexões de atalho (residuais).

Aumento de perdas de cache. Descobrimos que essa abordagem é a mais útil para ser usada com t sendo uma pequena constante entre 2 e 5. Ela reduz significativamente o requisito de memória,
mas ainda permite utilizar a maior parte da eficiência obtida com o uso de operadores de multiplicação e convolução de matrizes altamente otimizados, fornecidos por frameworks de
aprendizagem profunda. Resta saber se a otimização em nível de framework especial pode levar a melhorias adicionais em tempo de execução.

Link : https://arxiv.org/pdf/1801.04381