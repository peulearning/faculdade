# 1. Contextualização e Referencial

### Primeiro parágrafo (Contextualização)

> O tratamento de feridas, sejam elas agudas ou crônicas, representa um desafio para os sistemas de saúde devido à necessidade de avaliação clínica precisa para definição da conduta terapêutica. Uma classificação inadequada pode comprometer o diagnóstico, retardar o início do tratamento e prolongar o processo de cicatrização, impactando diretamente a qualidade de vida dos pacientes. Nesse contexto, métodos que auxiliem os profissionais de saúde na identificação das lesões têm despertado crescente interesse na literatura. _(referenciar revisão sobre feridas)_

[s-0045-1807722.pdf](https://www.thieme-connect.de/products/ejournals/pdf/10.1055/s-0045-1807722.pdf)

---

### Segundo parágrafo (revisão)

> Nos últimos anos, a Inteligência Artificial (IA), especialmente por meio da subárea de Visão Computacional, tem sido amplamente empregada na análise de imagens médicas. Técnicas baseadas em Redes Neurais Convolucionais (CNNs) têm apresentado resultados promissores em tarefas de detecção e classificação de feridas, contribuindo como ferramentas de apoio à decisão clínica. Além do desempenho preditivo, observa-se um crescente interesse pelo desenvolvimento de modelos leves, capazes de serem executados em dispositivos com recursos computacionais limitados, como smartphones, ampliando o potencial de utilização dessas soluções em diferentes contextos de atendimento. _(referenciar artigos de CNN para classificação de feridas e MobileNetV2)_

Nesse contexto, o emprego de tecnologias baseadas em Inteligência Artificial tem se mostrado uma alternativa para auxiliar profissionais de saúde na detecção e identificação de feridas, favorecendo maior agilidade e precisão durante o processo clínico. 

---

# 2. Problematização


> Apesar dos avanços observados na literatura, o desenvolvimento de modelos para classificação automática de feridas ainda enfrenta desafios importantes. A disponibilidade de bases públicas rotuladas é limitada, o que dificulta o treinamento de modelos robustos e aumenta a necessidade de estratégias de preparação e aumento de dados. Além disso, arquiteturas profundas frequentemente demandam elevado poder computacional, restringindo sua utilização em dispositivos com hardware limitado. Nesse cenário, torna-se relevante investigar arquiteturas leves, como a MobileNetV2, capazes de equilibrar desempenho e eficiência computacional.

---

# 3. Hipótese


> Diante desse cenário, levanta-se a hipótese de que a arquitetura MobileNetV2, associada às técnicas de Transfer Learning, seja capaz de apresentar desempenho satisfatório na classificação de feridas, mesmo em um contexto caracterizado por conjunto de dados limitado e restrições computacionais.

---

# 4. Objetivo


> Diante disso, este trabalho tem como objetivo desenvolver e avaliar um modelo baseado na arquitetura MobileNetV2 para a classificação automática de feridas por meio de técnicas de Deep Learning. Além disso, busca analisar o desempenho da arquitetura em um cenário com conjunto de dados limitado, comparando diferentes estratégias de treinamento e avaliando sua viabilidade como uma solução computacional leve para apoio à identificação de feridas.



--- 

# Escrita com minhas palavras 

Introdução : 1 Tópico oque eu vou abordar em contextualização e referencial

1. Gostaria de iniciar contextualizando meu trabalho que é sobre as feridas, dizer sobre o impacto que elas tem tanto como um desafio nos sistemas de saúde quanto impacta na qualidade de vida dos pacientes ou seja merece um diagnóstico preciso.


Introdução : 2 Tópico O que abordar na problematização

2. Trabalhar com Visão Computacional na detecção e classificação de feridas com IA leve / MobileNetV2


Introdução : 3 Tópico o que irei abordar na hipótese

3. Por exemplo : "Será que o modelo mobilenet consegue se adequar bem ao contexto das feridas ( comparativo entre os experimentos etc.... ) ? "

Introdução : 4 Tópico Abordagem no Objetivo

4. Trabalhar com Visão Computacional na detecção e classificação de feridas com IA leve / MobileNetV2,Levar em consideração dataset limitado, comparação dos splits de treino e teste de 4 e 6 classes(pior cenário) para o ambiente , classes cirúrgicas e venosas / pressão e venosa adequação do código ( 6 classes ) para trabalhar com as formas (geometrica) imagens das feridas.