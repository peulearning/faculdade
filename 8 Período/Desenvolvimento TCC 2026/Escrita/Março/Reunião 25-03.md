# 📝 Plano de Orientação

## 🎯 Prioridades de Pesquisa (Revisão Bibliográfica)

- [x] **1. Métricas de Visão Computacional:** Mapear na literatura quais são os valores de avaliação aceitáveis e esperadas para modelos de classificação de imagens através das métricas.
    
- [x] **2. Estado da Arte (Saúde + Mobile):** Levantar as discussões atuais sobre Visão Computacional na saúde, fazendo um recorte específico sobre a aplicação em **ambientes móveis**.
    
- [x] **3. Mitigação de Erros:** Pesquisar estratégias técnicas sobre como evitar **falsos negativos** (um ponto crítico em aplicações voltadas para a saúde).
    

---

## 🧠 Definição e Justificativa do Modelo

 - [x] **4. Arquitetura:** MobileNetV2.  ( Se mostrou a melhor até o determinado momento .)
    
- [x]  **5. Meta de Desempenho:** Os resultados do modelo devem ser comparados e chegar muito próximos aos relatados nos artigos de referência. ( Tabela na Reunião 17-03 )
    
- [x] **6. Justificativa Temática:** A discussão sobre diabetes está em alta. É essencial embasar a relevância do projeto na necessidade de diferenciar com precisão a ferida diabética, a lesão por pressão e a pele normal.
    
- [x] **7.Pré-processamento:** Aplicar técnicas para remover o _background_ das imagens (dataset) , garantindo que a rede neural foque apenas na área de interesse.
    

---

## 🔬 Pipeline de Experimentos (Refinamento de Classes)

A evolução dos testes de classificação deve seguir esta ordem rigorosa:

**Fase 1: Escopo Reduzido**

- [x] **2 Classes:** `Pressure` vs. `Diabetic`
    

**Fase 2: Cenário de Teste com Ruído**

- [x] **4 Classes:** `Normal`, `Background`, `Diabetic`, `Pressure`
    

> ⚠️ **Atenção:** Analisar criticamente os resultados obtidos nesta configuração antes de validar a passagem para a etapa final.

**Fase 3: Cenário Alvo (Pós-tratamento)**

- [x] **3 Classes:** `Normal`, `Diabetic`, `Pressure`


---  

## 📌 Pesquisa Pós-Reunião

## O que eu usei para pesquisar e responder os itens do TO-DO ? 

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
    



### Artigos Fichados

## 1. Métricas Aceitáveis e Implementação Mobile

> O que foi dito: _O F1-Score ideal está acima de 0.90, e a latência (tempo de inferência) no celular precisa ser baixa para uso clínico real._

### Artigo Comprobatório:

- **Título:** _Vision Transformer-based diabetic foot ulcer classification for mobile deployment: development, validation, and implementation of an iOS clinical decision support tool_
    
- **Publicação:** ResearchGate / Pré-print (Dezembro de 2025)
    
- **O que o artigo prova:** Os autores desenvolveram um modelo _Vision Transformer_ (ViT-Small) para classificar feridas de pé diabético rodando nativamente em um aplicativo iOS (CoreML). O artigo relata um **Macro-F1 de 0.9015** e uma área sob a curva (AUC) de **0.9834**.
    
- **Sobre o Mobile:** O estudo prova a viabilidade do _Edge AI_, alcançando um tempo de inferência de **50 a 80 milissegundos** no smartphone (latência imperceptível), provando que é possível ter precisão de pesquisa no ponto de atendimento ("point-of-care") sem depender de nuvem.
    

### Artigo Comprobatório (Revisão Sistemática):

- **Título:** _Advances in Image-Based Diagnosis of Diabetic Foot Ulcers Using Deep Learning and Machine Learning: A Systematic Review_
    
- **Publicação:** PubMed Central (PMC12730623), 2024/2025.
    
- **O que o artigo prova:** Após revisar mais de 1.000 artigos recentes, os autores confirmam que os modelos de Deep Learning (como U-Net para segmentação) atingem métricas de **Acurácia entre 0.88 e 0.97**, e **Sensibilidade (Recall) entre 0.89 e 0.95**, estabelecendo o referencial de qualidade atual para a literatura.
    

---

## 2. Cenário Geral de Visão Computacional na Saúde (Mobile e Explicação)

> O que foi dito: _A discussão atual no mobile envolve IA Explicável (XAI), "overlays" para o médico e redução do tempo de medição em até 40%._

### Artigo Comprobatório:

- **Título:** _A Mobile AI-enhanced Platform for Standardized Wound Assessment and Clinical Decision Support_
    
- **Publicação:** medRxiv (Janeiro de 2026 - _DOI: 10.64898/2026.01.22.26344407_)
    
- **O que o artigo prova:** Este estudo discute exatamente o abismo entre o que é feito na pesquisa e o que chega ao paciente. O artigo cita o trabalho de autores como **Brima & Atemkeng (2024)** para enfatizar que métodos de **explicabilidade visual** ("overlays" sobre a imagem) são cruciais para aumentar a confiança do clínico.
    
- **Sobre o cenário de mercado (2024-2030):** Relatórios de mercado citados na literatura (como o _Digital Wound Measurement Devices Market_, 2024) comprovam que dispositivos não-contato via smartphone representam **61% do mercado atual**, e hospitais que os adotaram relataram uma **redução de 40% no tempo de avaliação de feridas** por paciente.
    

---

## 3. Como Evitar Falsos Negativos e Diferenciar as Lesões

> O que foi dito: _A diferenciação entre Pé Diabético, Lesão por Pressão e o uso de recursos termográficos para evitar falsos negativos em peles aparentemente normais._

### Artigo Comprobatório:

- **Título:** _Emerging technologies for the management of diabetic foot ulceration: a review_
    
- **Publicação:** Frontiers in Clinical Diabetes and Healthcare (Artigo fcdhc.2024.1440209)
    
- **O que o artigo prova:** Destaca as tecnologias emergentes no monitoramento de úlceras. A literatura de revisão de 2024 e 2025 (incluindo o artigo da PMC citado na seção 1) aponta especificamente para a **imagem infravermelha térmica** como uma técnica diagnóstica promissora acoplada aos modelos móveis.
    
- **Evitando Falsos Negativos:** Como a temperatura da pele muda antes da úlcera de pressão ou do pé diabético se romper, a fusão de imagens visuais (RGB do celular) com dados térmicos ou histórico clínico é a solução defendida pela literatura para evitar que o modelo classifique uma pele em risco inflamatório como "Pele Normal" (o perigoso falso negativo).

---

### Tabela de Referência de Busca (Português vs. Inglês)

|**Contexto**|**Termos em Português (Busca Local)**|**Termos em Inglês (Busca Global/Científica)**|
|---|---|---|
|**Geral**|Métricas de avaliação classificação de imagens|Evaluation metrics image classification deep learning|
|**Saúde**|Inteligência artificial em feridas crônicas|AI-based chronic wound classification performance|
|**Padrão**|Valores de referência visão computacional|Performance benchmarks computer vision classification|


---

#### Outras Leituras de Artigos

1. https://www.researchgate.net/profile/Guanis-Vilela-Junior/publication/359541310_METRICAS_UTILIZADAS_PARA_AVALIAR_A_EFICIENCIA_DE_CLASSIFICADORES_EM_ALGORITMOS_INTELIGENTES/links/634ec60312cbac6a3ed73448/METRICAS-UTILIZADAS-PARA-AVALIAR-A-EFICIENCIA-DE-CLASSIFICADORES-EM-ALGORITMOS-INTELIGENTES.pdf

* Basicamente mostra as métricas que são utilizadas


2. https://rosario.ufma.br/jspui/bitstream/123456789/9202/1/ANDERSONCARVALHALPIMENTA.pdf 

-  Pelo resumo dá pra saber o oque utilizar quando se tratar de classificação binária e classes desbalanceadas, e as métricas que são mais utilizadas  (AUC)

3. https://ojs.eniac.com.br/index.php/EniacPesquisa/article/view/1072/1009

 * Subentende-se que utiliza o algoritmo Random Forest, com uma base de dados não seria pra classificação de imagens. 

4. [View of Classification of Foot Wound Severity in Type 2 Diabetes Mellitus Patients Using MobileNetV2-Based Convolutional Neural Network](https://jurnal.polibatam.ac.id/index.php/JAIC/article/view/11015/3046)

*  Gostei deste artigo a sua estrutura, deixa bem claro vários pontos além das métricas obtidas.


5.  https://www.mdpi.com/2673-2688/5/3/56 

 * Avaliação de mais de 200 modelos repositório : https://github.com/krishnateja95/COVID19_Benchmarking/tree/main


6. https://pmc.ncbi.nlm.nih.gov/articles/PMC12335118/pdf/12911_2025_Article_3144.pdf

*  Descreveu o processo de treinamento  , 4000 imagens no dataset,  faz segmentação, dectecção, Máscara, verdade fundamental 




---


## 🧩  Resposta do Pipeline de Experimentos

### Arquitetura Sequencial  2 Classes

- Parâmetros da Rede


![[Pasted image 20260329152427.png]]
	Relembrar camadas ( Flatten ) ⚠️
	
*  Épocas de treinamento 

![[Pasted image 20260329161027.png]]
Treinamento , Validação ❌, Teste // Momento em que a validação está entrando 


* Acurácia 

![[Pasted image 20260329161050.png]]
Corrigir na lib do matplot épocas para serem exatas.
Verficar a interpretação de Erro por época  (Validação)


Verificar o conceito de validação com acurácia e o conceito de validação com dataset
* Matriz de Confusão

![[Pasted image 20260329161109.png]]

Observar o balanceamento 

* Resultados da Matriz

![[Pasted image 20260329161755.png]]


Link do Notebook :  [Archiceture Sequencial 2 Classes.ipynb - Colab](https://colab.research.google.com/drive/1Po3TYLkytLOaqzzR1mzIMCjxvxIT3NFi#scrollTo=JxF-LCGalwzD)

### Arquitetura Sequencial  4 Classes

* Parâmetros  das Rede

![[Pasted image 20260330005348.png]]

* Épocas de Treinamento

![[Pasted image 20260330005430.png]]

* Acurácia por Época
![[Pasted image 20260330005511.png]]

* Matriz de Confusão

![[Pasted image 20260330005619.png]]

* Resultados da Matriz

![[Pasted image 20260330005627.png]]


* Curva AUC - ROC

![[Pasted image 20260330010334.png]]

Link do Notebook : [Archiceture Sequencial 4 Classes.ipynb - Colab](https://colab.research.google.com/drive/16egOWvOuY6xhz1kSyknrVpcGt3FyE786#scrollTo=d21uBrBEl7KS)



Atenção : Até aqui posso concluir através das observações e comparação com a tabela do artigo principal, que o modelo de arquitetura sequencial atingiu acurácia superior em relação ás 4 classes e valores inferiores para 2 classes. 



### Arquitetura Sequencial 3 Classes

* Parâmetros do Modelo

![[Pasted image 20260401151605.png]]

* Treinamento do Módelo 
![[Pasted image 20260401151622.png]]

*  Acurácia & Error por Época
![[Pasted image 20260401151642.png]]

* Matriz de Confusão 

![[Pasted image 20260401151657.png]]

* AUC-ROC

![[Pasted image 20260401151729.png]]

Link do Notebook :  [Archiceture Sequencial 3 Classes.ipynb - Colab](https://colab.research.google.com/drive/1t-_Y5a3d4lvcId1C3BN_8M1P72iv77sF#scrollTo=JxF-LCGalwzD)
### Arquitetura MobileNetV2 2 Classes

*  Parâmetros da Rede 


* Treinamento ( Os pesos se dão pela Imagenet ou seja rede pré-treinada , e congela a base  começando pela cabeça, e no fine-tuning vai para as camadas da base )

![[Pasted image 20260331092616.png]]

![[Pasted image 20260331092820.png]]

* Gráfico da Acurácia
![[Pasted image 20260331092909.png]]

* Relatório de Matriz de Confusão
![[Pasted image 20260331092931.png]]

* Matriz de Confusão 
![[Pasted image 20260331092949.png]]

* Acurácia de T/V  e Perca 
![[Pasted image 20260331093433.png]]

* Curva AUC - ROC

![[Pasted image 20260331093518.png]]

Link do Notebook : [Google Colab](https://colab.research.google.com/drive/1YlaVKvoTJAH4B10OsnvZXnyHvsYByL0f#scrollTo=y0b9gs76meMW)

### Arquitetura MobileNetV2  4 Classes

* Parâmetros do Modelo

![[Pasted image 20260331114948.png]]

* Treinamento do modelo 

![[Pasted image 20260331115020.png]]

![[Pasted image 20260331115029.png]]


* Acurácia de Treinamento
*
![[Pasted image 20260331163419.png]]

* Relatório da Matriz de Confusão

![[Pasted image 20260331163442.png]]

* Matriz de Confusão

![[Pasted image 20260331163458.png]]


* Acurácia de Treinamento & Validação

![[Pasted image 20260331163519.png]]


* AUC - ROC 

![[Pasted image 20260331164049.png]]


Link do Notebook :  [Archictecture MobileNetV2 4 Classes.ipynb - Colab](https://colab.research.google.com/drive/18yvVmrL5HaAs4ya-kLaAsBHg_NDL0iF-#scrollTo=QT-4-InWXRtA)

Conclui-se então portanto que para arquitetura "MobileNetV2" o modelo conseguiu ser superior a tabela de 4 classes, cujo valor alcançou 87.50%  e nosso modelo atingiu 99,81% e quanto ao experimento de 2 Classes o nosso resultado também foi superior em comparação a tabela atingindo 99,56% de resultado.

### Arquitetura MobileNetV2 3 Classes

* Parâmetros da Rede 

![[Pasted image 20260401203709.png]]


* Treinamento da rede

![[Pasted image 20260402075039.png]]


* Evolução Acurácia ( Com épocas Ajustadas )

![[Pasted image 20260402075100.png]]


* Relatório da Matriz 

![[Pasted image 20260402075140.png]]


* Matriz de Confusão 

![[Pasted image 20260402075202.png]]


* Acurácia de Treinamento / Validação ( Épocas Ajustadas )

![[Pasted image 20260402075210.png]]


* AUC-ROC 

![[Pasted image 20260402075243.png]]

Link do Notebook : https://colab.research.google.com/drive/1vU-cAZFRS28BWuuDOTxYJULCtoPwUswj#scrollTo=gfoyf_PYATSE