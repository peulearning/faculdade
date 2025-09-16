

1. Verificar no artigo o nome da lesão "normal" ✅
2. Verificar SUS, Hospital, dentre dessas lesões qual e mais comum.  ✅
3. Mais DataSets Rotulados Organizar os diretórios para se relacionarem.  ⚠️ ( Fazendo ... )
4. Elaborar o Cronograma de Entregas do TCC ✅ ( Feito )
5. Começar a escrita do TCC  ⚠️ ( A fazer )


```

#CRIAR TABELA em python: ✅
usar biblioteca glob do python
- percorrer a pasta test/diabetic, fazer isso em todas as pastas
- gerar um arquivo csv com o formato: nome_image, label
- Ex: img1.png, diabetic, salvar um csv 
- um arquivo csv, para cada classe
- juntar os arquivos csv em um único arquivo
_________________
- load do modelo
- passar todas as imagens do conjunto de testes
- gerar um arquivo csv com o formato: nome_image, predict, confianca

____________________

pegar os dois arquivos em csv: ground truth e o predict ✅

```

# **Oque foi feito :**



1.  Na verificação do artigo( Integrated Image and Location ) o rótulo da da lesão "normal" é :  ***Normal Skin (N)*

2. Verificando o DATASUS ( https://datasus.saude.gov.br/ ) através do TabNet ainda não consegui obter qual e o tipo de lesão mais comum, porém via fonte do CHATGPT,  realizei algumas pesquisas onde a por lesão por pressão seria a mais comum dentre os artigos pesquisados.

	## 🔹 Artigos/Estudos pertinentes ao tema



|Título|Ano|Local / Contexto|O que descobrem de útil|
|---|---|---|---|
|_Point prevalence and risk factors for pressure ulcers in hospitalized adult patients: a cross-sectional study_ (Einstein, São Paulo)|**2024**|Hospital universitário|Prevalência pontual de **10,71%** de úlceras por pressão. Fatores como idade, menor escolaridade, uso de fraldas, diabetes, pressão arterial, etc. [einstein (São Paulo)+1](https://journal.einstein.br/pt-br/article/point-prevalence-and-risk-factors-for-pressure-ulcers-in-hospitalized-adult-patients-a-cross-sectional-study/?utm_source=chatgpt.com)|
|_Prevalence of diabetic foot ulcers and their associated factors in patients from public hospitals in Manaus-AM_|**2022-2023**|Hospitais públicos em Manaus|Em pacientes com diabetes, prevalência de DFU foi **26,2%** entre internados. [PubMed](https://pubmed.ncbi.nlm.nih.gov/34389189/?utm_source=chatgpt.com)|
|_Diabetic Foot Ulcer and Social Determinants of Health: A Scoping Review_|**2024**|Brasil, revisão escopo|Examina fatores sociais que influenciam DFU — útil pra mostrar que o contexto pode aumentar prevalência. [Revista Estima](https://www.revistaestima.com.br/estima/article/view/1552?utm_source=chatgpt.com)|
|_Global burden of diabetic foot ulcers: a systematic review on prevalence, mortality..._|**2025**|Global (vários países)|Mostra que a prevalência de DFU varia bastante (4% a 25%) em pessoas com diabetes; custo, internação, amputação, etc. [Revista RSD](https://rsdjournal.org/rsd/article/view/49222?utm_source=chatgpt.com)|
|_Bacterial profile and antimicrobial resistance in diabetic foot ulcer infections: a 10-year retrospective cohort study_ (Minas Gerais)|**2025** (publicação em andamento)|Minas Gerais, ruptura de DFU infectados|Foca na microbiologia de infecções em DFU — ajuda se quiser falar de complicações, carga hospitalar. [RBDI](https://bjid.org.br/en-bacterial-profile-antimicrobial-resistance-in-articulo-S1413867025000716?utm_source=chatgpt.com)|

* Abaixo, são voltados para tecnologia.

| Título                                                                                                                                                                                                             | O que faz / como ajuda                                                                                                                                           | Pontos de destaque úteis para seu protótipo                                                                                                          |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------- |
| **WoundAIssist: A Patient-Centered Mobile App for AI-Assisted Wound Care** (2025) [arXiv](https://arxiv.org/abs/2506.06104?utm_source=chatgpt.com)                                                                 | App móvel com modelo leve para segmentação de feridas _on-device_, telemonitoramento com pacientes, feedback de médicos; inclui captura de imagens e uso remoto. | Mostra parte de UX/UI, usabilidade, como envolver paciente + profissional; útil pra pensar interface e arquitetura móvel.                            |
| **WoundAmbit: Bridging State-of-the-Art Semantic Segmentation and Real-World Wound Care** (2025) [arXiv](https://arxiv.org/abs/2504.06185?utm_source=chatgpt.com)                                                  | Avalia modelos de segmentação de feridas (wound segmentation) usando dados reais (“out-of-distribution”) e eficiência computacional para uso prático.            | Serve muito pra comparar modelos, ver trade-offs entre precisão vs custo computacional, ideal para mobile.                                           |
| **Wound3DAssist: A Practical Framework for 3D Wound Assessment** (2025) [arXiv](https://arxiv.org/abs/2508.17635?utm_source=chatgpt.com)                                                                           | Usa vídeo curto de smartphone para gerar modelos 3D da ferida — mede profundidade, bordas, etc.                                                                  | Se quiser ir além do plano 2D, mostra como capturar mais informação visual; bom para discussão de limitações do protótipo clássico.                  |
| **Diagnosis of Pressure Ulcer Stage Using On-Device AI** (2024) [OUCI](https://ouci.dntb.gov.ua/en/works/4YEZG064/?utm_source=chatgpt.com)                                                                         | App móvel com YOLOv8 para detectar regiões de úlceras por pressão e classificar em estágios (1-4, deep tissue, não avaliável). Tempo de resposta ~3s.            | Bom exemplo direto do que seu protótipo poderia fazer: detecção + classificação + interface móvel.                                                   |
| **Mobile Application for Diabetic Foot Ulcer Detection (ISDA 2023-2024)** [SpringerLink](https://link.springer.com/chapter/10.1007/978-3-031-64776-5_15?utm_source=chatgpt.com)                                    | Constrói app para profissionais detectarem DFU usando CNN, compara versões de YOLO; resultados bons de mAP; avaliação de usabilidade.                            | Excelente para base de como treinar, medir desempenho, validar com profissionais; muito parecida com seu escopo.                                     |
| **Artificial Intelligence in Wound Care: A Narrative Review of the Currently Available Mobile Apps for Automatic Ulcer Segmentation** (2024) [MDPI](https://www.mdpi.com/2673-7426/4/4/126?utm_source=chatgpt.com) | Revisão de apps móveis que fazem segmentação automática de úlceras; analisa quão prático é, métricas usadas, transparência, usabilidade.                         | Ótima para parte de fundamentação teórica / estado da arte; também para mostrar lacunas (por exemplo, muitos apps não são publicamente disponíveis). |
| **YOLO-Based Deep Learning Model for Pressure Ulcer Detection and Classification** (2023) [app.scinito.ai+1](https://app.scinito.ai/article/W4367189201?utm_source=chatgpt.com)                                    | Usa YOLOv5 para detectar úlceras de pressão em quatro estágios (e distinguir de não-úlcera). Obtém mAP razoável.                                                 | Serve para base de comparação de desempenho; pode inspirar escolha do modelo para o seu protótipo.                                                   |

---

## ⚠️ Limitações

- Muitos desses estudos focam **só em úlcera por pressão** ou **pé diabético**, não incluem todas as classes que  (venous, surgical, “normal”).
    
- Alguns modelos exigem equipamentos ou condições de imagem boas (boa iluminação, boa câmera), o que pode ser difícil em áreas remotas.
    
- Raramente há validação “em campo” do app (pacientes usando no dia a dia, com disparidades de luz, ângulo, fundo da imagem etc.).


## 🔧 Como usar isso ao meu favor


1. **Fundamentar a escolha do modelo e técnica** — explicar por que usar YOLO, ou segmentação, ou modelos leves, baseado nos que funcionam bem. ( Carros Autônomos )
    
2. **Inspirar métricas de avaliação** — erro, tempo de resposta, precisão, usabilidade, generalização (como fora do dataset de treino).
    
3. **Mostrar lacunas** — por exemplo, que poucos trabalhos incluem “ferida venosa” ou “ferida cirúrgica”, ou que poucos têm interface móvel bem validada.
    
4. **Comparar protótipos** — dizer “meu protótipo vai testar também X, Y, Z que faltou nesses artigos”.



5. Buscando DataSets Rótulados por lesões ( normal, pressão, cirúrgica, venosa, diabética )

	https://www.medetec.co.uk/files/medetec-image-databases.html  ( Não consegui encontrar o link do dataset no Artigo )
	https://pmc.ncbi.nlm.nih.gov/articles/PMC11186650/  ( Não consegui encontrar o link do DataSet no artigo. )
	https://www.kaggle.com/datasets/laithjj/diabetic-foot-ulcer-dfu ( Competição do Kaggle, achei interessante porém uma base pequena com as imagens todas misturadas dentro de um diretório. )


6. Cronograma de Entregas do TCC até a data da apresentação 

### **Mês: Setembro**

**Atividades principais:** Levantamento bibliográfico atualizado, revisão sobre aprendizado profundo em saúde, definição dos datasets finais, padronização das classes.

- **Lembrete Fixo:**
    
    - **Terças:** Aula de Cálculo 2 (7:30 - 9:10).
        
    - **Quintas:** Aula de Cálculo 2 (9:25 - 11:00).
        

| Semana                   | Foco Principal da Semana               | Sugestões de Tarefas                                                                                                                                                                                                             |
| ------------------------ | -------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Semana 1 (01-07/Set)** | **Levantamento Bibliográfico**         | • **Seg/Qua/Sex:** Focar na busca intensiva por artigos e teses recentes.<br>• **Ter/Qui (após a aula):** Organizar os artigos baixados, ler resumos e fichar os mais importantes.                                               |
| **Semana 2 (08-14/Set)** | **Revisão de Aprendizado Profundo**    | • **Seg/Qua/Sex:** Dedicar blocos de tempo para a leitura aprofundada dos materiais sobre Deep Learning em saúde.<br>• **Ter/Qui (após a aula):** Fazer anotações e criar mapas mentais sobre as técnicas.                       |
| **Semana 3 (15-21/Set)** | **Definição dos Datasets**             | • **Seg/Qua/Sex:** Analisar os datasets pré-selecionados, documentar suas características e bater o martelo sobre quais serão usados.<br>• **Ter/Qui (após a aula):** Começar a baixar e organizar os dados dos datasets finais. |
| **Semana 4 (22-30/Set)** | **Padronização das Classes e Revisão** | • **Seg/Qua/Sex:** Escrever os scripts para a padronização das classes dos datasets.<br>• **Ter/Qui (após a aula):** Validar a padronização e revisar o planejamento para Outubro.                                               |

Anotações : 

---

### **Mês: Outubro**

**Atividades principais:** Experimentos com modelos (3 e 5 classes), ajustes de hiperparâmetros, geração de métricas, coleta de dados do SUS.

- **Lembrete Fixo:**
    
    - **Terças:** Aula de Cálculo 2 (7:30 - 9:10).
        
    - **Quintas:** Aula de Cálculo 2 (9:25 - 11:00).
        

|Semana|Foco Principal da Semana|Sugestões de Tarefas|
|---|---|---|
|**Semana 1 (01-05/Out)**|**Estrutura dos Experimentos**|• **Seg/Qua/Sex:** Implementar o pipeline de treinamento e validação. Começar os primeiros testes com o modelo de 3 classes.<br>• **Ter/Qui (após a aula):** Analisar os primeiros resultados e planejar ajustes.|
|**Semana 2 (06-12/Out)**|**Execução e Métricas (3 Classes)**|• **Seg/Qua/Sex:** Rodar os experimentos com o modelo de 3 classes, ajustando hiperparâmetros.<br>• **Ter/Qui (após a aula):** Gerar e salvar as métricas (acurácia, F1) e matrizes de confusão.|
|**Semana 3 (13-19/Out)**|**Execução e Métricas (5 Classes)**|• **Seg/Qua/Sex:** Adaptar e rodar os experimentos para o modelo de 5 classes.<br>• **Ter/Qui (após a aula):** Gerar e salvar as métricas e matrizes de confusão para este modelo.|
|**Semana 4 (20-31/Out)**|**Coleta de Dados SUS e Consolidação**|• **Seg/Qua/Sex:** Focar na coleta e tratamento dos dados do SUS.<br>• **Ter/Qui (após a aula):** Organizar todos os resultados dos experimentos em planilhas ou documentos para facilitar a escrita.|

Anotações : 

---

### **Mês: Novembro**

**Atividades principais:** Escrita dos capítulos: Introdução, Fundamentação Teórica, Metodologia. Início dos Resultados.

- **Lembrete Fixo:**
    
    - **Terças:** Aula de Cálculo 2 (7:30 - 9:10).
        
    - **Quintas:** Aula de Cálculo 2 (9:25 - 11:00).
        

|Semana|Foco Principal da Semana|Sugestões de Tarefas|
|---|---|---|
|**Semana 1 (01-09/Nov)**|**Escrita: Introdução e Problematização**|• **Seg/Qua/Sex:** Escrever a versão inicial da Introdução, justificativa e objetivos.<br>• **Ter/Qui (após a aula):** Revisar o texto, checar referências e a clareza da escrita.|
|**Semana 2 (10-16/Nov)**|**Escrita: Fundamentação Teórica**|• **Seg/Qua/Sex:** Usar o material de Setembro para redigir a fundamentação teórica.<br>• **Ter/Qui (após a aula):** Inserir as citações e formatar as referências preliminares.|
|**Semana 3 (17-23/Nov)**|**Escrita: Metodologia**|• **Seg/Qua/Sex:** Detalhar os passos da metodologia, descrevendo os datasets, o pré-processamento e os modelos utilizados.<br>• **Ter/Qui (após a aula):** Revisar a clareza e a reprodutibilidade da seção.|
|**Semana 4 (24-30/Nov)**|**Início da Escrita: Resultados**|• **Seg/Qua/Sex:** Começar a descrever os resultados, inserindo as tabelas de métricas e as matrizes de confusão geradas em Outubro.<br>• **Ter/Qui (após a aula):** Criar os gráficos e figuras.|


Anotações : 

---

### **Mês: Dezembro**

**Atividades principais:** Finalização da escrita (Resultados, Discussão, Conclusão). Revisão geral, formatação ABNT, entrega.

- **Lembrete Fixo:**
    
    - **Terças:** Aula de Cálculo 2 (7:30 - 9:10).
        
    - **Quintas:** Aula de Cálculo 2 (9:25 - 11:00).
        

|Semana|Foco Principal da Semana|Sugestões de Tarefas|
|---|---|---|
|**Semana 1 (01-07/Dez)**|**Finalização da Escrita**|• **Seg/Qua/Sex:** Escrever a Discussão (interpretando os resultados) e a Conclusão (retomando os objetivos).<br>• **Ter/Qui (após a aula):** Escrever o resumo (abstract) e as palavras-chave.|
|**Semana 2 (08-14/Dez)**|**Revisão Geral e Coerência**|• **Seg/Qua/Sex:** Ler o documento completo do início ao fim para garantir a coesão e a fluidez do texto.<br>• **Ter/Qui (após a aula):** Fazer a revisão ortográfica e gramatical.|
|**Semana 3 (15-21/Dez)**|**Formatação ABNT**|• **Seg/Qua/Sex:** Dedicar-se exclusivamente à formatação ABNT: margens, fontes, citações, referências, sumário, etc.<br>• **Ter/Qui (após a aula):** Checar a numeração de figuras e tabelas.|
|**Semana 4 (22-31/Dez)**|**Buffer e Entrega Final**|• Esta semana serve como uma "gordura" para resolver imprevistos, fazer últimos ajustes pedidos pelo orientador e preparar a versão final para entrega.|


Anotações: 


--- 


7.  Escrita do TCC ( Preparando)

https://www.overleaf.com/project/68b8d5079e1f75ab2091ded2