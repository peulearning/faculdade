
---

# 📅 Reunião | 06/05/2026

> [!ABSTRACT] Objetivo Principal
> 
> **Cadastrar o Aplicativo no INPI** (Processo futuro).

---

## 📝 To-Do List (Pesquisa Científica)

- [x] **Pesquisa Cromática:** Cores encontradas na classificação de feridas por imagens.
    
- [x] **Análise de Padrão:** Formatos de feridas por pressão e pé diabético.
    
- [x] **Análise Exploratória:** Realizar pesquisa de métodos para os dados coletados.
    

---

## 👥 Responsabilidades

### 👩‍🔬 Karine

- [x] **Atualizar Resumo:** Com base no último relatório entregue.
    
- [x] **Ajuste de Título:** Deve conter explicitamente "Visão Computacional" e "MobileNetV2".
    
- [x] **Contextualização:** Refinar o contexto geral do trabalho.
    

### 👨‍💻 Meu Backlog

- [x] **Relatório:** Finalizar procedimentos restantes.
    
- [x] **Desenvolvimento:** Dar andamento no App e no TCC.
    
- [x] **Evento:** Coletar resultados para a conferência de _Modelagem Computacional_.
    

---

## 🎯 Metas e Prazos

| **Entrega**                      | **Data Limite** | **Status**  |
| -------------------------------- | --------------- | ----------- |
| **Resumo Finalizado**            | 08/05 (Sexta)   | ✅ Entregue  |
| **Resultados (Escala de Cinza)** | 13/05           | ✅Entregue   |
| **Novas Metas de Trabalho**      | 13/05           | 📅 Agendado |

---


-  Análise de Padrão de formatos de feridas por pressão e diabética ( Científica ) ✅
-  Pesquisar a respeito sobre as cores que são encontradas em classificação de feridas por imagens. ( Científica )  ✅
- Escala Cinza em Visão Computacional nas Feridas ✅

--- 


## Resultados das Metas 📌

## Artigo sobre as Cores 🔴🟡⚪⚫
* **Pesquisa Cromática** 

**Nome do Artigo:_“Fully automatic wound segmentation with deep convolutional neural networks”_ (Wang et al., 2020)."**

**Trecho do Artigo:** Estudos relacionados à segmentação de feridas podem ser categorizados em dois grupos principais: métodos tradicionais de visão computacional e métodos de aprendizado profundo. Os estudos do primeiro grupo se concentram em combinar técnicas de visão computacional com abordagens tradicionais de aprendizado de máquina. Esses estudos aplicam extração de características projetada manualmente para construir um conjunto de dados que é posteriormente usado para dar suporte a algoritmos de aprendizado de máquina. Song et al. descreveram 49 características que são extraídas de uma imagem de ferida usando agrupamento K-means, detecção de bordas, limiarização e crescimento de região, tanto em escala de cinza quanto em RGB4. Essas características são filtradas e preparadas em um vetor de características que é usado para treinar uma rede neural Perceptron Multicamadas (MLP) e uma rede neural de Função de Base Radial (RBF) para identificar a região de uma ferida crônica. Ahmad et al. propuseram gerar um mapa de probabilidade Vermelho-Amarelo-Preto-Branco (RYKW) de uma imagem de entrada com um modelo de matiz-saturação-valor (HSV) modificado. Esse mapa então orienta o processo de segmentação usando limiarização ideal ou crescimento de região. Hettiarachchi et al. demonstraram um algoritmo de contorno dinâmico discreto de minimização de energia aplicado ao plano de saturação da imagem em seu modelo de cores HSV6. A área da ferida é então calculada a partir de um preenchimento por inundação dentro do contorno delimitado. Hani et al. propuseram a aplicação de um algoritmo de Análise de Componentes Independentes (ICA) às imagens RGB pré-processadas para gerar imagens baseadas em hemoglobina, que são usadas como entrada para o agrupamento K-means para segmentar o tecido de granulação das imagens da ferida.

Essas áreas segmentadas são utilizadas como uma avaliação dos estágios iniciais da cicatrização da úlcera, detectando o crescimento do tecido de granulação no leito da úlcera. Wantanajittikul et al. propuseram um sistema semelhante para segmentar a ferida de queimadura a partir de imagens.

As transformações Cr e Luv são aplicadas às imagens de entrada para remover o fundo e destacar a região da ferida. As imagens transformadas são segmentadas com um algoritmo de Agrupamento Fuzzy C-means (FCM) pixel a pixel. Esses métodos sofrem de pelo menos uma das seguintes limitações: (1)como em muitos sistemas de visão computacional, as características extraídas manualmente são afetadas pela pigmentação da pele, iluminação, e resolução da imagem, (2) dependem de parâmetros ajustados manualmente e características extraídas empiricamente, o que não garante um resultado ideal. Além disso, não são imunes a patologias graves e casos raros, o que é muito impraticável do ponto de vista clínico, e (3) o desempenho é avaliado em um pequeno conjunto de dados enviesado.

**Argumento:** O estudo discute o uso do modelo **RYKW (Red-Yellow-Black-White)** baseado em **HSV** para mapear tecidos de granulação e necrose. Ele prova que a extração de características de cor ajuda a guiar a segmentação em redes neurais. Os métodos clássicos baseados apenas em limiares de cor (como o de Ahmad et al.) falham em dispositivos móveis devido à iluminação e pigmentação da pele. Por isso, estou utilizando a **MobileNetV2**, que, conforme propõe Wang et al. (2020), supera essas limitações ao aprender padrões complexos de forma automatizada.

**Artigo:** [s41598-020-78799-w.pdf](file:///C:/Users/peuja/Downloads/s41598-020-78799-w.pdf)

**Revista:** https://pmc.ncbi.nlm.nih.gov/articles/PMC7736585/

**Implementação:** https://github.com/uwm-bigdata/wound-segmentation

**Referência:**  Fauzi, M. F. A. et al. Computerized segmentation and measurement of chronic wound images. Comput. Biol. Med. 60, 74–85 (2015).


## Pesquisa sobre os Formato de Feridas ⭕
* **Pesquisa Padrão e Formato de  Feridas Diabéticas e por Pressão**

**Nome do Artigo:** **_“The Enlightening Role of Explainable Artificial Intelligence in Chronic Wound Classification”_ (Anisuzzaman et al., 2021).**"

**Trecho do Artigo:** Artificial Intelligence (AI) has been among the most emerging research and industrial application fields, especially in the healthcare domain, but operated as a black-box model with a limited understanding of its inner working over the past decades. AI algorithms are, in large part, built on weights calculated as a result of large matrix multiplications. It is typically hard to interpret and debug the computationally intensive processes. Explainable Artificial Intelligence (XAI) aims to solve black-box and hard-to-debug approaches through the use of various techniques and tools. In this study, XAI techniques are applied to chronic wound classification. The proposed model classifies chronic wounds through the use of transfer learning and fully connected layers. Classified chronic wound images serve as input to the XAI model for an explanation. Interpretable results can help shed new perspectives to clinicians during the diagnostic phase. The proposed method successfully provides chronic wound classification and its associated explanation to extract additional knowledge that can also be interpreted by non-data-science experts, such as medical scientists and physicians. This hybrid approach is shown to aid with the interpretation and understanding of AI decision-making processes.


**Argumento:** Utiliza XAI (IA Explicável) para mostrar que modelos identificam **Pé Diabético** por padrões na região plantar e **Lesões por Pressão** por áreas maiores de dano tecidual ao redor de proeminências ósseas. Ajuda a explicar "o que" a MobileNetV2 está "olhando".

**Artigo:** 

**Revista:** [The Enlightening Role of Explainable Artificial Intelligence in Chronic Wound Classification](https://www.mdpi.com/2079-9292/10/12/1406)


**Implementação:**

**Referências:**

## Pesquisa Escala Cinza 🩶
* **Pesquisa Escala Cinza**

**Nome do Artigo:** **_“A Systematic Investigation of Models for Color Image Processing in Wound Size Estimation”_ (Zuniga-Rodriguez et al., 2021)."**

**Trecho do Artigo:**  Propusemos que um método para medir a área da ferida
deve implementar diferentes etapas, incluindo a conversão para escala de cinza para posterior aplicação do limiar e um método de segmentação para medir a área da ferida como o número de pixels para posterior conversão para unidades métricas. Em relação aos dispositivos, a tecnologia móvel demonstrou ter atingido o nível de precisão confiável.

Os desafios de trabalhar com imagens de feridas residem em suas colorações muito heterogêneas,
relacionadas à cor da pele do paciente e a anomalias como eritema e estrias. Além disso, a complexidade do processo de segmentação de tecidos aumenta ainda mais, pois os limites
entre diferentes regiões de tecido são frequentemente imprecisos e altamente irregulares. Portanto, técnicas de processamento de imagem e inteligência computacional têm sido aplicadas em diversos estudos para abordar vários aspectos do diagnóstico de feridas.


**Argumento:** O artigo valida o uso de **Grayscale** como etapa essencial para pré-processamento e segmentação de bordas (thresholding). Ele discute sobre a redução de canais para o cálculo de área em dispositivos móveis.

**Artigo:** file:///C:/Users/peuja/Downloads/computers-10-00043.pdf

**Revista:** https://www.mdpi.com/2073-431X/10/4/43

**Implementação:**  

**Referências:**



--- 


## Notebook GrayScale + HeatMap : 

*  **Link : [Cópia de Archictecture MobileNetV2 2 Classes Modify.ipynb - Colab](https://colab.research.google.com/drive/1JvCKHcAfUNI5Ihd8qsA01Wu8d-eLG4ef#scrollTo=LC5kihhpGkL0)**


* **Comentários dos Resultados:**  

 O Ponto Forte: Sensibilidade em Pé Diabético (`diabetic`)

- **Recall de 0.90:** O modelo é um "rastreador" excelente para feridas diabéticas. De 29 imagens reais, ele acertou 26.
    
- **O que isso significa:** Ele raramente deixa passar uma ferida diabética sem percebê-la. Se o objetivo fosse triagem, seria ótimo.


 O Ponto Crítico: O "Apagão" das Lesões por Pressão (`pressure`)

- **Recall de 0.48:** Isso é preocupante. O modelo errou **mais da metade** das lesões por pressão (11 de 21).
    
- **A Confusão:** Olhando a matriz, esses 11 erros foram classificados como "diabetic".
    
- **Conclusão Clínica/Técnica:** Em escala de cinza, as características de textura de uma Lesão por Pressão estão sendo confundidas com as de um Pé Diabético.


 O impacto da Escala de Cinza na Classe `pressure`

> **Pergunta para a Profª:** _"Professora, o recall de Lesão por Pressão caiu para 48%. Será que a cor (o eritema/vermelhidão ou a coloração da necrose) era a característica principal que o modelo usava para diferenciar essas feridas?"_


**Contexto:** Lesões por pressão variam muito de tom (do rosa claro ao escuro/preto). Sem a cor, o modelo pode estar se perdendo na semelhança de texturas granulares que ambas as feridas possuem.

 Viés do Modelo para a classe majoritária

> **Pergunta para o Prof:** _"O modelo está 'chutando' muito em Diabetic (26 acertos, mas também 11 erros vindos da outra classe). Como podemos penalizar mais o erro na classe Pressure?"_

Análise Visual com Grad-CAM

> **Pergunta para o Prof:** _"Já que temos os resultados, quero usar o Grad-CAM nas 11 imagens de Pressure que foram classificadas como Diabetic. Quero ver se o modelo está focando na borda da ferida ou no fundo da imagem."_

**Por que isso é importante:** Se o Grad-CAM mostrar que o modelo olha para o "lençol" ou para a "pele ao redor" em vez da ferida, o problema não é a cor, mas o conjunto de dados.

 MobileNetV2 e Transfer Learning em Cinza

> **Pergunta para o Prof:** _"O MobileNetV2 foi treinado na ImageNet (fotos coloridas de objetos). Faz sentido tentarmos um 'Fine-tuning' mais profundo, já que a distribuição de dados (agora em cinza) mudou radicalmente em relação ao peso original?"_

 * **Gráfico da Acurácia**
	
	![[Captura de tela 2026-05-12 190330.png]]



* **Gráficos de Acurácia & Perca de Treinamento e Validação ***

![[Captura de tela 2026-05-12 190357.png]]


* **Treinamento por Épocas***

![[Pasted image 20260512204324.png]]


* **Sumário do Modelo**

![[Pasted image 20260512204348.png]]

* **Matriz de Confusão**
![[Pasted image 20260512204415.png]]



* **Análise de Erros***
![[Pasted image 20260512204435.png]]


* **Curva ROC **

![[Captura de tela 2026-05-12 190411.png]]

* **GradCam Erros**


![[Pasted image 20260512204625.png]]



* **GradCam Acertos***

![[Pasted image 20260512204650.png]]


* Depois de Analisar as imagens, oque eu pensei :  " O Modelo está de fato extraindo características ? "

 **A Prova Estatística: 72% não é "chute"**

Se o modelo não estivesse extraindo característica nenhuma, a acurácia em um problema de 2 classes (binário) seria próxima de **50%** (aleatório).

- Com **72% de acurácia**, o modelo provou que encontrou padrões matemáticos (texturas, bordas e formas) que se repetem.
    
- O **Recall de 0.90 em Diabetic** mostra que as características dessa classe são muito marcantes mesmo em preto e branco. O modelo "aprendeu" a assinatura visual do pé diabético (provavelmente a forma da ferida ou o padrão da pele ao redor).

 **O que o modelo está "enxergando" (Textura vs. Cor)**

- **Gradientes de Intensidade:** Diferença entre áreas claras (exsudato, fibrina) e escuras (necrose).
    
- **Bordas e Contornos:** A profundidade da ferida e a irregularidade das margens.
    
- **Textura Local:** A rugosidade do tecido.
    

**O Problema com a Classe `Pressure`:** O recall baixo (0.48) em Lesões por Pressão sugere que, para essa classe, a **cor era uma característica fundamental**.

- **Hipótese:** Muitas lesões por pressão em estágio inicial são identificadas pelo "eritema" (vermelhidão). Em escala de cinza, o vermelho vira um tom de cinza médio que se mistura perfeitamente com o tom de pele normal. O modelo perdeu o "alvo".

**Comparativo**

- **Comparação com RGB:** Rodar o mesmo modelo com imagens coloridas. Se o recall de `pressure` subir de 0.48 para 0.80, por exemplo, você prova que a cor é essencial para essa patologia.
    
- **Grad-CAM:** Gerar o mapa de calor. Se o Grad-CAM mostrar o modelo focando na borda da ferida, ele está extraindo **forma**. Se ele focar no centro granuloso, ele está extraindo **textura**.