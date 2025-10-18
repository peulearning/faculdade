
MobileNetV2: Resíduos Invertidos e Gargalos Lineares


**Resumo**

Neste artigo, descrevemos uma nova arquitetura móvel, MobileNetV2, que melhora o desempenho de última geração de modelos móveis em múltiplas tarefas e benchmarks, bem como em um espectro de diferentes tamanhos de modelo. Também descrevemos maneiras eficientes de aplicar esses modelos móveis à detecção de objetos em uma nova estrutura
que chamamos de SSDLite. Além disso, demonstramos como construir modelos de segmentação semântica móvel por meio de uma forma reduzida do DeepLabv3, que chamamos de Mobile
DeepLabv3. baseia-se em uma estrutura residual invertida, onde as conexões de atalho estão entre as camadas finas de gargalo. A camada de expansão intermediária utiliza convoluções leves em profundidade para filtrar recursos como uma fonte de não linearidade. Além disso, descobrimos que é importante remover não linearidades nas camadas estreitas para manter o poder de representação. Demonstramos que isso melhora o desempenho e fornecemos uma intuição que levou a este projeto. Por fim, nossa abordagem permite o desacoplamento dos domínios de entrada/saída da expressividade da transformação, o que fornece uma estrutura conveniente para análises posteriores. Medimos nosso desempenho na classificação ImageNet [1], na detecção de objetos COCO [2] e na segmentação de imagens VOC [3]. Avaliamos as compensações entre precisão e número de operações medidas por multiplicação-adição (MAdd), bem como a latência real e o número de parâmetros.

1. **Introduction**

As redes neurais revolucionaram muitas áreas da inteligência das máquinas, permitindo precisão sobre-humana para tarefas desafiadoras de reconhecimento de imagens. No entanto, a busca
por melhorar a precisão geralmente tem um custo: as redes modernas de última geração exigem altos recursos computacionais além das capacidades de muitos dispositivos móveis e embarcados