
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

| **Entrega**                      | **Data Limite** | **Status**             |
| -------------------------------- | --------------- | ---------------------- |
| **Resumo Finalizado**            | 08/05 (Sexta)   | ✅ Entregue             |
| **Resultados (Escala de Cinza)** | 13/05           | 🛠️ Em Desenvolvimento |
| **Novas Metas de Trabalho**      | 13/05           | 📅 Agendado            |

---


-  Análise de Padrão de formatos de feridas por pressão e diabética ( Científica ) ✅
-  Pesquisar a respeito sobre as cores que são encontradas em classificação de feridas por imagens. ( Científica )  ✅
- Escala Cinza em Visão Computacional nas Feridas ✅

--- 


## Resultados das Metas 📌


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



* **Pesquisa Padrão e Formato de  Feridas Diabéticas e por Pressão**

**Nome do Artigo:** **_“The Enlightening Role of Explainable Artificial Intelligence in Chronic Wound Classification”_ (Anisuzzaman et al., 2021).**"

**Trecho do Artigo:** Artificial Intelligence (AI) has been among the most emerging research and industrial application fields, especially in the healthcare domain, but operated as a black-box model with a limited understanding of its inner working over the past decades. AI algorithms are, in large part, built on weights calculated as a result of large matrix multiplications. It is typically hard to interpret and debug the computationally intensive processes. Explainable Artificial Intelligence (XAI) aims to solve black-box and hard-to-debug approaches through the use of various techniques and tools. In this study, XAI techniques are applied to chronic wound classification. The proposed model classifies chronic wounds through the use of transfer learning and fully connected layers. Classified chronic wound images serve as input to the XAI model for an explanation. Interpretable results can help shed new perspectives to clinicians during the diagnostic phase. The proposed method successfully provides chronic wound classification and its associated explanation to extract additional knowledge that can also be interpreted by non-data-science experts, such as medical scientists and physicians. This hybrid approach is shown to aid with the interpretation and understanding of AI decision-making processes.


**Argumento:** Utiliza XAI (IA Explicável) para mostrar que modelos identificam **Pé Diabético** por padrões na região plantar e **Lesões por Pressão** por áreas maiores de dano tecidual ao redor de proeminências ósseas. Ajuda a explicar "o que" a MobileNetV2 está "olhando".

**Artigo:** 

**Revista:** [The Enlightening Role of Explainable Artificial Intelligence in Chronic Wound Classification](https://www.mdpi.com/2079-9292/10/12/1406)


**Implementação:**

**Referências:**


* **Pesquisa Escala Cinza**

**Nome do Artigo:** **_“A Systematic Investigation of Models for Color Image Processing in Wound Size Estimation”_ (Zuniga-Rodriguez et al., 2021)."**

**Trecho do Artigo:**  Propusemos que um método para medir a área da ferida
deve implementar diferentes etapas, incluindo a conversão para escala de cinza para posterior aplicação do limiar e um método de segmentação para medir a área da ferida como o número de pixels para posterior conversão para unidades métricas. Em relação aos dispositivos, a tecnologia móvel demonstrou ter atingido o nível de precisão confiável.

Os desafios de trabalhar com imagens de feridas residem em suas colorações muito heterogêneas,
relacionadas à cor da pele do paciente e a anomalias como eritema e estrias. Além disso, a complexidade do processo de segmentação de tecidos aumenta ainda mais, pois os limites
entre diferentes regiões de tecido são frequentemente imprecisos e altamente irregulares. Portanto, técnicas de processamento de imagem e inteligência computacional têm sido aplicadas em diversos estudos para abordar vários aspectos do diagnóstico de feridas.


**Argumento:** O artigo valida o uso de **Grayscale** como etapa essencial para pré-processamento e segmentação de bordas (thresholding). Ele discute como a redução de canais facilita o cálculo de área em dispositivos móveis.

**Artigo:** file:///C:/Users/peuja/Downloads/computers-10-00043.pdf

**Revista:** https://www.mdpi.com/2073-431X/10/4/43

**Implementação:**  

**Referências:**