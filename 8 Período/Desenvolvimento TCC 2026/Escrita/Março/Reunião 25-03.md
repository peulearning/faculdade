+# 📝 Plano de Orientação

## 🎯 Prioridades de Pesquisa (Revisão Bibliográfica)

- [x] **1. Métricas de Visão Computacional:** Mapear na literatura quais são os valores de avaliação aceitáveis e esperadas para modelos de classificação de imagens através das métricas.
    
- [ ] **2. Estado da Arte (Saúde + Mobile):** Levantar as discussões atuais sobre Visão Computacional na saúde, fazendo um recorte específico sobre a aplicação em **ambientes móveis**.
    
- [ ] **3. Mitigação de Erros:** Pesquisar estratégias técnicas sobre como evitar **falsos negativos** (um ponto crítico em aplicações voltadas para a saúde).
    

---

## 🧠 Definição e Justificativa do Modelo

 - [x] **4. Arquitetura:** MobileNetV2.  ( Se mostrou a melhor até o determinado momento .)
    
- [ ]  **5. Meta de Desempenho:** Os resultados do modelo devem ser comparados e chegar muito próximos aos relatados nos artigos de referência. ( Tabela na Reunião 17-03 )
    
- [ ] **6. Justificativa Temática:** A discussão sobre diabetes está em alta. É essencial embasar a relevância do projeto na necessidade de diferenciar com precisão a ferida diabética, a lesão por pressão e a pele normal.
    
- [ ] **7.Pré-processamento:** Aplicar técnicas para remover o _background_ das imagens (dataset) , garantindo que a rede neural foque apenas na área de interesse.
    

---

## 🔬 Pipeline de Experimentos (Refinamento de Classes)

A evolução dos testes de classificação deve seguir esta ordem rigorosa:

**Fase 1: Escopo Reduzido**

- [ ] **2 Classes:** `Pressure` vs. `Diabetic`
    

**Fase 2: Cenário de Teste com Ruído**

- [ ] **4 Classes:** `Normal`, `Background`, `Diabetic`, `Pressure`
    

> ⚠️ **Atenção:** Analisar criticamente os resultados obtidos nesta configuração antes de validar a passagem para a etapa final.

**Fase 3: Cenário Alvo (Pós-tratamento)**

- [ ] **3 Classes:** `Normal`, `Diabetic`, `Pressure`


---  

## 📌 Pesquisa Pós-Reunião

## O que eu usei pra resolver o item 1 do To-Do  ? 

#### 1. Visão Computacional Geral (Benchmarks)

Estes termos ajudam a encontrar o "estado da arte" (SOTA) e o que a comunidade acadêmica aceita como um modelo bem-sucedido.

- **Termos-Chave:** `Computer vision classification benchmarks`, `state-of-the-art image classification performance`, `evaluation metrics for deep learning classifiers`.
    
- **Métricas Específicas:** `"Accuracy"`, `"F1-score"`, `"ROC-AUC"`, `"Precision-Recall curve"`.
    
- **Exemplo de busca:** `benchmark values for image classification "state-of-the-art" 2024..2026`
    

#### 2. Contexto de Saúde e Feridas (Wound Care)

Na saúde, a tolerância a erro é menor, então os termos mudam para focar em sensibilidade e especificidade.

- **Termos-Chave:** `Deep learning wound classification`, `computer-aided diagnosis wound healing`, `automated chronic wound assessment performance`.
    
- **Métricas Clínicas:** `"Sensitivity"`, `"Specificity"`, `"Dice Coefficient"` (se houver segmentação), `"Cohen's Kappa"` (para concordância com especialistas).
    
- **Exemplo de busca:** `performance metrics "wound classification" deep learning clinical validation`
    

---

### Tabela de Referência de Busca (Português vs. Inglês)

|**Contexto**|**Termos em Português (Busca Local)**|**Termos em Inglês (Busca Global/Científica)**|
|---|---|---|
|**Geral**|Métricas de avaliação classificação de imagens|Evaluation metrics image classification deep learning|
|**Saúde**|Inteligência artificial em feridas crônicas|AI-based chronic wound classification performance|
|**Padrão**|Valores de referência visão computacional|Performance benchmarks computer vision classification|

---

#### Artigos Lidos

1. https://www.researchgate.net/profile/Guanis-Vilela-Junior/publication/359541310_METRICAS_UTILIZADAS_PARA_AVALIAR_A_EFICIENCIA_DE_CLASSIFICADORES_EM_ALGORITMOS_INTELIGENTES/links/634ec60312cbac6a3ed73448/METRICAS-UTILIZADAS-PARA-AVALIAR-A-EFICIENCIA-DE-CLASSIFICADORES-EM-ALGORITMOS-INTELIGENTES.pdf

* Basicamente mostra as métricas que são utilizadas


2. https://rosario.ufma.br/jspui/bitstream/123456789/9202/1/ANDERSONCARVALHALPIMENTA.pdf 

-  Pelo resumo dá pra saber o oque utilizar quando se tratar de classificação binária e classes desbalanceadas, e as métricas que são mais utilizadas  (AUC)

3. https://ojs.eniac.com.br/index.php/EniacPesquisa/article/view/1072/1009

 * Subentende-se que utiliza o algoritmo Random Forest, com uma base de dados não seria pra classificação de imagens. 

4. [View of Classification of Foot Wound Severity in Type 2 Diabetes Mellitus Patients Using MobileNetV2-Based Convolutional Neural Network](https://jurnal.polibatam.ac.id/index.php/JAIC/article/view/11015/3046)

*  Gostei deste artigo a sua estrutura, deixa bem claro vários pontos além das métricas obtidas.

---


## 🧩  Resposta do Pipeline de Experimentos 2 Classes


### Arquitetura Sequencial 

- Parâmetros da Rede


![[Pasted image 20260329152427.png]]

*  Épocas de treinamento 

![[Pasted image 20260329161027.png]]

* Acurácia 

![[Pasted image 20260329161050.png]]




* Matriz de Confusão

![[Pasted image 20260329161109.png]]


* Resultados da Matriz

![[Pasted image 20260329161755.png]]


Link do Notebook :  [Archiceture Sequencial 2 Classes.ipynb - Colab](https://colab.research.google.com/drive/1Po3TYLkytLOaqzzR1mzIMCjxvxIT3NFi#scrollTo=JxF-LCGalwzD)