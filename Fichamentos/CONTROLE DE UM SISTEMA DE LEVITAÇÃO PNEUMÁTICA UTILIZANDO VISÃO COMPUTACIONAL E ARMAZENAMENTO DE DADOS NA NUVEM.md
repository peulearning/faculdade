

Página 26

**3.4 VISÃO COMPUTACIONAL** 

A Visão Computacional é uma aplicação da Inteligência Artificial que visa mimetizar a visão humana. 
Esse ramo desenvolve sistemas artificias, seja hardware ou software, que adquirem informações de imagens ou quaisquer dados com múltiplas dimensões.
Ela extrai muitas informações de imagens capturadas por câmeras, sensores, scanners e outros dispositivos. Essas informações permitem identificar, manipular e considerar os objetos que compõem a imagem (BAREELOS, 2021).
A visão computacional procura oferecer, de modo mais eficiente possível, uma vasta quantidade de informações ao sistema computacional. 
O reconhecimento de padrões está engajada no campo da visão computacional com atuações e perspectivas importantes para alcançar e realizar a máquina inteligente (SILVA, 2008) No contexto de tecnologia da visão, é correto afirmar que visão computacional é uma técnica na qual tem-se como elemento de entrada uma imagem e após processamento de alto nível obtém-se como elemento de saída uma decisão inteligente, como por exemplo a classificação de padrões da imagem (FABRE, 2019).

**3.4.1 Sistema de Visão Computacional** 

Um sistema de visão computacional necessita de diversos componentes que estão interligados e dependem um do outro pra funcionar. A figura 4, define uma estrutura geral dos componentes que envolvem um sistema de visão computacional (GALLON, 2013).

![[Pasted image 20250611121116.png]]

Os componentes e suas respectivas descrições são os seguintes:

I. Sensor de Aquisição: A função do sistema de aquisição de imagens é obter informações da cena a ser monitorada, necessárias ao processamento e consecutiva inspeção. São utilizados diferentes equipamentos como sensores, câmeras, lentes e etc. (STIVANELLO, 2004);
II. Iluminação da cena: A iluminação exerce um papel fundamental no sistema de captura, uma vez que uma câmera captura a luz refletida sobre a cena observada. Existem diversas técnicas de iluminação, que variam entre si na fonte de luz, intensidade, direção, etc. (STIVANELLO, 2004); 
III. Sistema de aquisição da imagem: Este sistema deve possuir placas de aquisição de imagens capazes de converter o sinal de vídeo proveniente das câmeras em imagens digitais (STIVANELLO, 2004);
IV. Dispositivo de Processamento: Dever dispor de um hardware de alto desempenho para poder realizar o processamento das imagens digitais. Geralmente são utilizados computadores para realizar o processamento (GALLON, 2013);

V. Software de processamento de imagens digitais: O software utilizado nos sistemas de visão é responsável pelo controle do sistema como um todo, pelo processamento e análise das imagens e pela interface com o usuário (STIVANELLO, 2004). Realiza a manipulação das imagens, reconhecimento e classificação de objetos, dentre outros processos (GALLON, 2013); 
VI. Display: Monitores ou outros dispositivos de saída de vídeo para exibição das informações geradas pelo software de processamento de imagens (GALLON, 2013); 
VII. Armazenamento: Utilizado para armazenar todas as informações coletadas com o software de processamento de imagens (GALLON 2013);

Para todo sistema de visão computacional são necessárias algumas etapas de processamento que variam de acordo com a aplicação (SILVA, 2008). Porém, basicamente todos os sistemas de visão computacional envolvem reconhecimento de objetos em imagens e transformações destes em informações que serão processadas e utilizadas por algum sistema especialista (DE MILANO e HORONATO, 2014).

3.4.1.1 Imagem Digital
3.4.1.2 Processamento Digital de Imagens
3.5.1 Internet das Coisas
**3.5.1.1 Arquitetura para IoT**

Para conectar bilhões de objetos inteligentes à Internet, deve-se ter uma arquitetura flexível. Na literatura, tem-se variedade de propostas de arquiteturas sofisticadas, que se baseiam nas necessidades da academia e da indústria (SANTOS et al., 2016). Segundo Filho (2016) de acordo com o Instituto IEEE (Insitute of Electrical and Electronic Engineers) a atual arquitetura para a IoT é constituída de três níveis, Aplicações, Rede/Comunicação de Dados e Sensoriamento, conforme ilustrado na figura 8.

![[Pasted image 20250611121439.png]]


**3.5.1.2 Cloud Computing (ThingSpeak)** https://thingspeak.mathworks.com/

Através da Internet das Coisas (IoT), uma imensa quantidade de dispositivos são conectados à internet, gerando uma grande quantidade de dados. Computação em nuvem é uma tecnologia adotada atualmente para processar, armazenar e prover controle de acesso a dados (PACHECO, 2018). Logo, torna-se imprescindível o uso de computação em nuvem (Cloud Computing) em aplicações de IoT. Para aplicações de Internet das Coisas, o conceito de nuvem se tornou um complemento importante. Os dispositivos IoT normalmente têm poder de processamento limitado, baixa confiabilidade e alta taxa de erros. Aplicações críticas não devem ficar hospedadas em dispositivos IoT. Esses dispositivos devem enviar suas informações para um elemento centralizador, com maior poder de processamento e disponibilidade. Se esse elemento estiver na nuvem, as aplicações ganham mais flexibilidade e escalabilidade. Além disso, evitam a necessidade de um servidor local de alta disponibilidade (FONSECA, 2018) Computação em nuvem pretende ser global e prover serviços para as massas que vão desde o usuário final que hospeda seus documentos pessoais na Internet até empresas que terceirizam toda infraestrutura de TI para outras empresas. Nunca uma abordagem para utilização real foi tão global e completa: não apenas recursos de computação e armazenamento são entregues sob demanda, mas toda a pilha de computação pode ser aproveitada na nuvem. A Figura 11 mostra uma visão geral de uma nuvem computacional (SOUSA, 2009).![[Pasted image 20250611121803.png]]

Dentre os serviços ofertados pela computação em nuvem, o armazenamento de dados se destaca. O armazenamento da computação em nuvem permite que os usuários finais possam guardar seus dados com um prestador de serviço da nuvem ao invés de um sistema local. Assim como acontece com outros serviços de nuvem, é possível acessar os dados armazenados através da conexão com a Internet (ANDRADE et al., 2015). Os dados, a partir desde modelo, são armazenados em múltiplos servidores em vez de servidores dedicados, tipicamente empregues em rede tradicionais de armazenamento de dados. A localização dos ficheiros pode mudar a qualquer momento, visto que o sistema gere dinamicamente o espaço disponível nos vários servidores e equilibra o armazenamento, utilizando algoritmos de otimização. Contudo, apesar desta localização variável, o utilizador vê os ficheiros numa localização “estática”, sendo-lhe permitida a gestão dos seus dados como se estivesse utilizando o seu próprio computador (BARBOSA, 2013). Uma aplicação típica de IoT é funcionar como datalog, cujo objetivo é apenas registar uma grandeza obtida por um sensor. Esse registro pode ser usado com vários objetivos, desde monitoramento ambiental e aplicações industriais. Também é possível disparar alarmes ou ações baseadas nos valores registrados. Datalog já foi um processo manual, mas passou a ser automatizado com armazenamento local, depois por um servidor em rede. O momento atual é de enviar o registro para a nuvem, que economiza toda a infraestrutura necessária para o registro local e, de quebra, facilita o acesso remoto. Diversos serviços de registro de informações IoT estão disponíveis; alguns de forma gratuita. Eles oferecem interface elaboradas, incluindo gráficos e aplicativos para acesso em smartphone (DE OLIVEIRA, 2017). Uma das plataformas muito utilizada nos projetos de internet das coisas é a ThingSpeak. Com ela é possível receber dados de sensores, analisar esses dados, plotar gráficos para melhorar a visualização, criar reações interligadas aos dados recebidos, e enviar tweeters que possibilitam a interação com outras plataformas e ações via protocolo HTTP. As interfaces desta plataforma deixam as aplicações mais intuitivas para análise do usuário (DE OLIVEIRA, 2017).