
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

- [ ] **Relatório:** Finalizar procedimentos restantes.
    
- [ ] **Desenvolvimento:** Dar andamento no App e no TCC.
    
- [ ] **Evento:** Coletar resultados para a conferência de _Modelagem Computacional_.
    

---

## 🎯 Metas e Prazos

| **Entrega**                      | **Data Limite** | **Status**             |
| -------------------------------- | --------------- | ---------------------- |
| **Resumo Finalizado**            | 08/05 (Sexta)   | ✅ Entregue             |
| **Resultados (Escala de Cinza)** | 13/05           | 🛠️ Em Desenvolvimento |
| **Novas Metas de Trabalho**      | 13/05           | 📅 Agendado            |

---


-  Análise de Padrão de formatos de feridas por pressão e diabética ( Científica ) 
-  Pesquisar a respeito sobre as cores que são encontradas em classificação de feridas por imagens. ( Científica ) 
- Escala Cinza em Visão Computacional nas Feridas

--- 


## Resultados das Metas 📌


* **Pesquisa Cromática** 

**Nome do Artigo:_“Fully automatic wound segmentation with deep convolutional neural networks”_ (Wang et al., 2020)."**

Related studies on wound segmentation can be roughly categorized into two groups: traditional computer vision methods and deep learning methods. Studies in the first group focus on combining computer vision tech niques and traditional machine learning approaches. These studies apply manually-designed feature extraction to build a dataset that is later used to support machine learning algorithms. Song et al. described 49 features that are extracted from a wound image using K-means clustering, edge detection, thresholding, and region growing in both grayscale and RGB4. These features are filtered and prepared into a feature vector that is used to train a Multi-Layer Perceptron (MLP) and a Radial Basis Function (RBF) neural network to identify the region of a chronic wound. Ahmad et al. proposed generating a Red-Yellow-Black-White (RYKW) probability map of an input image with a modified hue-saturation-value (HSV) model5. This map then guides the segmentation process using either optimal thresholding or region growing. Hettiarachchi et al. demonstrated an energy minimizing discrete dynamic contour algorithm applied on the saturation plane of the image in its HSV color model6. The wound area is then calculated from a flood fill inside the enclosed contour. Hani et al. proposed applying an Independent Component Analysis (ICA) algorithm to the pre-processed RGB images to generate hemoglobin based images, which are used as input of K-means clustering to segment the granulation tissue from the wound images7. These segmented areas are utilized as an assessment of the early stages of ulcer healing by detecting the growth of granulation tissue on ulcer bed. Wantanajittikul et al. proposed a similar system to segment the burn wound from images8. Cr-Transformation and Luv-Transformation are applied to the input images to remove the background and highlight the wound region. The transformed images are segmented with a pixel-wise Fuzzy C-mean Clustering (FCM) algorithm. These methods suffer from at least one of the following limitations: (1) as in many computer vision systems, the hand-crafted features are affected by skin pigmentation, illumination, and image resolution, (2) they depend on manually tuned parameters and empirically handcrafted features which does not guarantee an optimal result. Additionally, they are not immune to severe pathologies and rare cases, which are very impractical from a clinical perspective, and (3) the performance is evaluated on a small biased dataset.

**Argumento:** O estudo discute o uso do modelo **RYKW (Red-Yellow-Black-White)** baseado em **HSV** para mapear tecidos de granulação e necrose. Ele prova que a extração de características de cor ajuda a guiar a segmentação em redes neurais.

**Artigo:** [s41598-020-78799-w.pdf](file:///C:/Users/peuja/Downloads/s41598-020-78799-w.pdf)

**Revista:** https://pmc.ncbi.nlm.nih.gov/articles/PMC7736585/

**Implementação:** https://github.com/uwm-bigdata/wound-segmentation

**Referência:**  Fauzi, M. F. A. et al. Computerized segmentation and measurement of chronic wound images. Comput. Biol. Med. 60, 74–85 (2015).



* **Pesquisa Padrão de Feridas**

**Nome do Artigo:** **_“The Enlightening Role of Explainable Artificial Intelligence in Chronic Wound Classification”_ (Anisuzzaman et al., 2021).**"

Artificial Intelligence (AI) has been among the most emerging research and industrial application fields, especially in the healthcare domain, but operated as a black-box model with a limited understanding of its inner working over the past decades. AI algorithms are, in large part, built on weights calculated as a result of large matrix multiplications. It is typically hard to interpret and debug the computationally intensive processes. Explainable Artificial Intelligence (XAI) aims to solve black-box and hard-to-debug approaches through the use of various techniques and tools. In this study, XAI techniques are applied to chronic wound classification. The proposed model classifies chronic wounds through the use of transfer learning and fully connected layers. Classified chronic wound images serve as input to the XAI model for an explanation. Interpretable results can help shed new perspectives to clinicians during the diagnostic phase. The proposed method successfully provides chronic wound classification and its associated explanation to extract additional knowledge that can also be interpreted by non-data-science experts, such as medical scientists and physicians. This hybrid approach is shown to aid with the interpretation and understanding of AI decision-making processes.


**Argumento:** Utiliza XAI (IA Explicável) para mostrar que modelos identificam **Pé Diabético** por padrões na região plantar e **Lesões por Pressão** por áreas maiores de dano tecidual ao redor de proeminências ósseas. Ajuda a explicar "o que" a MobileNetV2 está "olhando".

**Artigo:**[The Enlightening Role of Explainable Artificial Intelligence in Chronic Wound Classification](https://www.mdpi.com/2079-9292/10/12/1406)

