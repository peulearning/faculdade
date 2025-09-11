
* Retomando as Pesquisas relacionadas ao TCC [ X ]
* Pesquisar mais sobre visões computacional CNN ( Propostas de trabalho com poucos recursos computacionais, comportamento das tecnologias em nuvens, processamento distribuído, níveis de processamento. ) [X] 
*  Pesquisar Edge Fog Cloud [ X ] 


# 📌**Oque é Edge Fog Cloud ?**

"Edge Computing ou computação de borda, é uma tecnologia que coleta e processa dados perto da fonte de onde eles são gerados, como câmeras de segurança e sensores em máquinas industriais. Isso só não acelera a resposta aos dados coletados, permitindo ações quase instantâneas, como também reduz a necessidade de enviar grandes quantidades de dados para a Cloud, economizando largura de banda.  Um exemplo prático é o uso em cidades inteligentes, onde o processamento de dados e tráfego em tempo real pode melhorar o fluxo veicular e segurança pública."

"Já o Fog Computing, ou computação de névoa, por sua vez, atua como uma extensão da Edge Computing, criando uma rede de nós intermediários, que processam dados entre os dispositivos Edge e a Cloud. Isso permite um processamento mais eficiente e distribuído, ideal para aplicações que exigem processamento de dados complexos ou análise em tempo real em grande escala. Um exemplo de sua aplicação é na agricultura, onde é possível gerenciar dados de diversos sensores espalhados por uma fazenda, podendo otimizar a irrigação e o uso de fertilizantes, aumentando a eficiência e a produção."

"Cloud Computing, também conhecida como computação em nuvem, é uma tecnologia que permite o acesso remoto a recursos de computação, como servidores, armazenamento e banco de dados, por meio da internet. Com aplicações desde serviços de armazenamento até plataformas de computação em nuvem, possibilitando revolucionar a maneira como armazenamos dados e executamos aplicações, oferecendo escalabilidade, flexibilidade e eficiência para empresas e consumidores."

2.1.2 ( Página 21 ) Arquitetura Edge-Fog-Cloud

"A arquitetura Edge-Fog-Cloud" destaca-se pela sua capacidade de reduzir a latência e manter a funcionalidade mesmo na ausência de conexão com a internet, diferenciando-se significativamente da abordagem tradicional Edge-Cloud. Enquanto a Edge-Cloud permite o processamento de dados na borda, reduzindo a necessidade de envio constante para a nuvem, a inclusão da camada de Fog amplia essa capacidade, processando dados mais perto da fonte e permitindo respostas rápidas mesmo sem conexão. Além disso, a estrutura Edge-Fog-Cloud mantém a flexibilidade de acessar a Cloud para análises mais complexa, proporcionando um equilíbrio ideal entre processamento local eficiente e capacidades analíticas avançadas na nuvem."

![[Pasted image 20250609145305.png]]

![[Pasted image 20250609151529.png]]

2.1.3 HDF5

2.1.4 Processamento e Armazenamento paralelo e distribuído



# 📌 **Visão Computacional** 



Oque é a visão computacional ? 
```
A visão computacional em aprendizado de máquina é o uso da técnica de aprendizado de máquina para permitir que os computadores "vejam" e compreendam o mundo ao seu redor (SHAPIRO; STOCKMAN,2001).

Isso é alcançado através de análises de imagens e vídeos para extrair informações úteis e tomar decisões baseadas em seu treinamento.

As estruturas mais comuns de aprendizado de máquinas para visão computacional incluem redes neurais, profundas do inglês Deep Learning (DP), como a rede neural convolucional (CNN), que é especialmente eficaz para reconhecimento de imagem.
```

* Proposta de trabalho com poucos recursos computacionais


 [[Fichamentos/A method for embedding a computer vision application into a wearable device ( Um método para incorporar uma aplicação de visão computacional em um dispositivo vestível )]]


* Comportamento das Tecnologias em Nuvem
``` 
As tecnologias em nuvem (cloud computing) são uma forma de disponibilizar recursos computacionais — como servidores, armazenamento, bancos de dados, redes, software e inteligência artificial — pela internet, sob demanda e com pagamento conforme o uso.

Como funciona ?

a computação em nuvem funciona através de data centers(centros de processamento de dados) distribuídos globalmente por empresa como Amazon(AWS), Microsoft(Azure), Google(GCP), entre outas. Esses datas centers fornecem recursos virtualizados aos usuários por meio de:

1. Virtualização: Servidores físicos são particionados em várias máquinas virtuais.
2. Automação: Permite escalar recursos automaticamente conforme a necessidade.
3. Gerenciamento Remoto: Os usuários acessam e gerenciam recursos via navegador ou APIs.
4. Redes seguras: Comunicação entre cleintes e data centers por meio de redes criptografadas e seguras.

Características :

1. Elasticidade : se adapta automaticamente á demanda : aumenta ou diminui recursos conforme necesário.

2. Escalabilidade : pode escalar verticalmente ( mais potência ) ou horizontalmente ( mais instânicas);

3. Alta disponibilidade : Serviços são projetados para continuar funcionando mesmo com falhas em partes da infraestrutura.

Redundância geográfica e comum ( dados replicaods em várias regiões).

4. Pay-as-you-go (Pague pelo uso) : Recursos utilizados =  CPU, GPU GB de armazenamento ou quantidade de acessos.

5. Self-service sob demanda : Usuários podem criar, configurar e gerenciar seus própios recursos sem depender de intervenção humana.

6. Acesso amplo á rede: Recursos estão acessíveis por meioda internet em qualquer dispositivo autorizado.

7. Multitenancy (multiusuário) : vários usuários compartilham os mesmo recursos físicos, mas com isolamento lógico.



```

[[CONTROLE DE UM SISTEMA DE LEVITAÇÃO PNEUMÁTICA UTILIZANDO VISÃO COMPUTACIONAL E ARMAZENAMENTO DE DADOS NA NUVEM]]

* Processamentos Distribuídos 

```
O processamento distribuído refere-se à execução de tarefas computacionais em múltiplos computadores interconectados, permitindo lidar com grandes volumes de dados e demandas de processamento que superam a capacidade de uma única máquina. Essa abordagem é fundamental em ambientes de computação em nuvem e na borda, sendo aplicada em áreas como Internet das Coisas, inteligência artificial e redes veiculares, onde a descentralização traz vantagens de escalabilidade, resiliência e eficiência 1065. O processamento distribuído pode ser realizado por meio de diferentes arquiteturas e frameworks, como Flink, Kafka Streams e Spark Streaming, que facilitam a distribuição e o balanceamento de carga entre os nós do sistema 3. Fatores como o design do pipeline, alocação de recursos e características dos dados influenciam diretamente a eficiência e a escalabilidade dessas soluções 3. Estratégias modernas também incluem técnicas de aprendizado distribuído e computação codificada, que otimizam o uso de recursos e minimizam atrasos, especialmente em sistemas heterogêneos e dinâmicos 275. Apesar dos avanços, desafios persistem, como a padronização de métricas de desempenho, a complexidade de integração e a necessidade de políticas de gestão adequadas para evitar armadilhas comuns 694. O futuro do processamento distribuído aponta para soluções cada vez mais autônomas, econômicas e adaptáveis, capazes de atender às crescentes demandas de dados e aplicações em tempo real 8102.
```



[[SEGURANÇA DO AMBIENTE USANDO DISPOSITIVO IOT COM PROCESSAMENTO DISTRIBUÍDO]]


* Níveis de Processamento

```
 os “níveis de processamento” referem-se a diferentes camadas ou etapas pelas quais os dados passam dentro de um sistema computacional, cada uma com funções e complexidades distintas. Uma classificação comum divide o processamento em quatro níveis: processamento de dados (operações básicas como cálculos e manipulação de dados brutos), processamento de informações (organização e interpretação dos dados para gerar informações úteis), processamento de conhecimento (aplicação de regras e inferências para gerar conhecimento a partir das informações) e processamento de inteligência (capacidade de tomar decisões autônomas e aprender com experiências, como em sistemas de inteligência artificial) 46. Essa hierarquia reflete a evolução dos sistemas computacionais, desde tarefas simples até operações complexas que simulam aspectos do raciocínio humano. Além disso, abordagens como a de David Marr propõem três níveis para analisar sistemas de processamento de informação: o nível computacional (o que o sistema faz e por quê), o nível algorítmico (como o sistema faz) e o nível de implementação (em que meio físico o sistema é realizado), sendo amplamente utilizados para entender tanto cérebros quanto computadores 2. Esses modelos ajudam a estruturar o desenvolvimento de hardware, software e algoritmos, além de orientar pesquisas em inteligência artificial e ciência cognitiva 26.

```




## ☁️ 1. Comportamento das Tecnologias em Nuvem

### Definição

Computação em nuvem é a entrega de recursos de TI—como computação, armazenamento e serviços—sob demanda via internet, com pagamento conforme o uso [pt.wikipedia.org+2ciasc.sc.gov.br+2starsoft.com.br+2](https://www.ciasc.sc.gov.br/computacao-em-nuvem-o-que-precisamos-saber-sobre-a-tecnologia/?utm_source=chatgpt.com)[redalyc.org+15aws.amazon.com+15atlassian.com+15](https://aws.amazon.com/pt/what-is-cloud-computing/?utm_source=chatgpt.com).

### Características essenciais (segundo o NIST) :

1. **Autoatendimento sob demanda** – o usuário provisiona recursos sem interação humana com o provedor.
    
2. **Amplo acesso por rede** – acessível via padrões e dispositivos diversos.
    
3. **Pool de recursos** – recursos físicos e virtuais são compartilhados entre clientes.
    
4. **Elasticidade rápida** – escalonamento automático de recursos.
    
5. **Serviço mensurável** – monitoramento, controle e cobrança baseados em métricas.
    

### Modelos de implantação [catalogcdns3.ulife.com.br+2atlassian.com+2pt.wikipedia.org+2](https://www.atlassian.com/br/microservices/cloud-computing?)[catalogcdns3.ulife.com.br+2esr.rnp.br+2pt.wikipedia.org+2](https://esr.rnp.br/computacao-em-nuvem/o-que-e-computacao-em-nuvem/)[ciasc.sc.gov.br+2teleone.com.br+2pt.wikipedia.org+2](https://www.teleone.com.br/recursos/nuvem/)[esr.rnp.br+2pt.wikipedia.org+2catalogcdns3.ulife.com.br+2](https://pt.wikipedia.org/wiki/Computa%C3%A7%C3%A3o_em_nuvem?)[catalogcdns3.ulife.com.br](https://catalogcdns3.ulife.com.br/content-cli/CTI_SECLOU_20/unidade_1/ebook/index.html?):

- **Nuvem Pública** – recursos compartilhados via internet.
    
- **Nuvem Privada** – dedicada a uma organização, maior controle e segurança.
    
- **Nuvem Híbrida / Multicloud** – combinação de nuvens públicas e privadas, permitindo flexibilidade.
    

### Comportamento típico:

- **Elasticidade**: ajusta recursos à demanda automaticamente [pt.wikipedia.org+15pt.wikipedia.org+15atlassian.com+15](https://pt.wikipedia.org/wiki/Elasticidade_%28computa%C3%A7%C3%A3o_em_nuvem%29?utm_source=chatgpt.com)[catalogcdns3.ulife.com.br+15atlassian.com+15teleone.com.br+15](https://www.atlassian.com/br/microservices/cloud-computing?utm_source=chatgpt.com)[pt.wikipedia.org+4teleone.com.br+4atlassian.com+4](https://www.teleone.com.br/recursos/nuvem/?utm_source=chatgpt.com).
    
- **Alta confiabilidade**: falhas pontuais não afetam todo o serviço [medium.com](https://medium.com/internet-das-coisas/cloud-02-highlights-das-tecnologias-conceitos-e-desafios-da-computa%C3%A7%C3%A3o-nas-nuvens-22ceff63b6c4?utm_source=chatgpt.com).
    
- **Redução de custo**: menos investimento inicial; modelo pay-per-use [ciasc.sc.gov.br](https://www.ciasc.sc.gov.br/computacao-em-nuvem-o-que-precisamos-saber-sobre-a-tecnologia/?utm_source=chatgpt.com).
    

---

## 2. Processamentos Distribuídos

### Definição

Processamento distribuído é uma arquitetura que conecta vários nós (computadores) para executar juntos tarefas grandes, dividindo-as e melhorando desempenho e escalabilidade [ciasc.sc.gov.br+9pt.wikipedia.org+9medium.com+9](https://pt.wikipedia.org/wiki/Sistema_de_processamento_distribu%C3%ADdo?utm_source=chatgpt.com).

### Diferenciação:

- **SMP (Symmetric Multiprocessing)**: múltiplas CPUs em um único hardware.
    
- **Distribuído**: CPUs em máquinas fisicamente distintas [reddit.com+2pt.wikipedia.org+2reddit.com+2](https://pt.wikipedia.org/wiki/Sistema_de_processamento_distribu%C3%ADdo?utm_source=chatgpt.com).
    

### Componentes-chave:

- **Nó**: unidade computacional participante.
    
- **IPC distribuída**: comunicação entre nós.
    
- **Padrões**: como MPI e PVM permitem portabilidade e paralelismo [reddit.com+3pt.wikipedia.org+3aws.amazon.com+3](https://pt.wikipedia.org/wiki/Sistema_de_processamento_distribu%C3%ADdo?utm_source=chatgpt.com).
    

### Vantagens:

- Paralelismo de tarefas divisíveis.
    
- Utilização de hardware heterogêneo.
    
- Custo-benefício (uso de máquinas simples em rede).
    

---

## 🧠 3. Níveis de Processamento

Em arquitetura de sistemas ou de dados, “níveis de processamento” (camadas) ajudam a organizar funcionalidades:

### Exemplos clássicos:

#### a) Modelo em três camadas (3-tier) [reddit.com](https://www.reddit.com/r/brdev/comments/ujc3xp?utm_source=chatgpt.com):

1. **Camada de apresentação** – interface com o usuário (GUI).
    
2. **Camada de negócio** – lógica funcional, regras de negócio.
    
3. **Camada de dados** – armazenamento e acesso a dados (banco de dados).
    

#### b) Em nuvem e microsserviços (computação nativa em nuvem) [pt.wikipedia.org](https://pt.wikipedia.org/wiki/Modelo_em_tr%C3%AAs_camadas?utm_source=chatgpt.com):

- **Infraestrutura** (IaaS) – VMs, rede, armazenamento.
    
- **Plataforma** (PaaS) – runtime, contêineres, frameworks.
    
- **Aplicação** (SaaS/FaaS) – software e funções executadas pelo usuário.
    

### Benefícios:

- **Modularidade**: mudanças numa camada não afetam as outras.
    
- **Escalabilidade independente**: cada camada pode crescer conforme a necessidade.
    
- **Manutenção facilitada**: responsabilidades separadas melhoram desenvolvimento e operação.
    

---

### 📌 Resumo Comparativo

| Tema                      | Objetivo                                       | Benefícios principais                                    |
| ------------------------- | ---------------------------------------------- | -------------------------------------------------------- |
| Tecnologias em nuvem      | Fornecer TI como serviço justo sob demanda     | Elasticidade, confiabilidade, redução de custos          |
| Processamento distribuído | Dividir tarefas para execução paralela em rede | Paralelismo, resiliência e economia com hardware simples |
| Níveis de processamento   | Organizar arquitetura em camadas separadas     | Modularidade, escalabilidade e manutenção independente   |
