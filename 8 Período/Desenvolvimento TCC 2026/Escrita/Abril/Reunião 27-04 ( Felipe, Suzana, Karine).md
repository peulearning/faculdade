# Análise de Perfis Visuais: Úlceras Diabéticas vs. Pressão

## 📊 1. Dados Quantizados (Dataset Original vs. Treinamento)

### Dataset Original

|**Perfil Visual**|**Diabética (Qtd)**|**Diabética (%)**|**Pressão (Qtd)**|**Pressão (%)**|
|---|---|---|---|---|
|**Vermelhada**|52|28%|60|45%|
|**Amarelada**|52|28%|17|12%|
|**Branco**|57|30%|36|27%|
|**Necrosada**|24|12%|19|14%|
|**TOTAL**|**185**|**100%**|**132**|**100%**|

### Dataset de Treinamento

|**Perfil Visual**|**Diabética Treino**|**Diabética (%)**|**Pressão Treino**|**Pressão (%)**|
|---|---|---|---|---|
|**Vermelhada**|41|31%|60|45%|
|**Amarelada**|33|25%|17|12%|
|**Branco**|41|31%|36|27%|
|**Necrosada**|14|10%|19|14%|
|**TOTAL**|**129**|**100%**|**132**|**100%**|

---

## 🧠 2. Conclusões e Insights da Quantização

### 2.1. Perfil Visual Característico por Classe

Ao comparar os dois datasets originais, notam-se assinaturas visuais distintas que servem como marcadores clínicos para o modelo:

- **Dataset Diabético:** Possui uma distribuição mais equilibrada entre os perfis "Vermelhada", "Amarelada" e "Branco". O perfil **Amarelado** é significativamente mais comum (28%) do que nas feridas de pressão (12%).
    
- **Dataset de Pressão:** É fortemente dominado pelo perfil **Vermelhado** (45%).
    

> **Conclusão:** O perfil visual (tecido/coloração) atua como um forte indicador estatístico para a rede neural. O tecido de granulação ("Vermelhado") tornou-se um marcador forte para úlceras de pressão, enquanto o esfacelo ("Amarelado") ocorre proporcionalmente mais nas diabéticas.

### 2.2. A Lógica do Erro: A Síndrome do "Falso Diabético"

A discrepância na distribuição do perfil "Amarelado" explica a maior taxa de erros na classe de Pressão (falsos negativos).

- **O Paradigma do Modelo:** No dataset original, a classe Diabética tem mais que o dobro da proporção de tecidos amarelados (28% vs 12%). O modelo aprendeu a seguinte regra implícita: _"Se há predominância de tecido amarelo (esfacelo), a probabilidade de ser úlcera diabética é maior"_.
    
- **O Problema de Classificação:** Quando o modelo analisa uma úlcera de Pressão complexa, com muito esfacelo (amarela), ele se baseia nessa estatística enviesada e a classifica incorretamente como Diabética.
    

> **Conclusão Central:** O modelo sofre de um **viés de prevalência cromática**. Ele associou a cor amarelada à patologia diabética devido à maior amostragem desse perfil no treino, falhando ao lidar com variações clínicas semelhantes na classe de pressão.

### 2.3. O Desafio da Classe "Necrosada"

Em ambas as patologias, a categoria indicativa de necrose representa a minoria dos dados:

- **Diabético:** 12% a 10% (Treino)
    
- **Pressão:** 14%
    

> **Conclusão:** Por ser a classe menos representada, o modelo possui uma capacidade de generalização inferior para reconhecer tecidos necróticos (escuros). Isso exige atenção especial, pois mascara uma fraqueza do algoritmo em casos clínicos de maior gravidade.

### 2.4. Alerta Metodológico: Divisão de Treinamento

- **Classe Diabética:** O _split_ foi adequado (~70% para treino, 129 de 185). A proporção interna dos perfis se manteve fiel ao dataset original, garantindo uma amostragem representativa.
    
- **Classe Pressão (Risco de Overfitting):** A planilha de treinamento consumiu **100%** do dataset original (132 imagens).
    

> **Conclusão:** Sem uma reserva de imagens inéditas de úlceras de pressão para validação e teste, é impossível garantir que o modelo não está sofrendo de _overfitting_ (memorizando as imagens ao invés de aprender os padrões). A avaliação de métricas nesta classe requer um conjunto de teste externo para validação.

---

## 🛠️ 3. Resumo para Documentação e Soluções Propostas

Com base nos dados estruturados, os próximos passos do estudo devem considerar:

1. **Validação da Diferenciação Clínica:** A quantização matemática corrobora clinicamente que úlceras de pressão e diabéticas da base de dados possuem assinaturas teciduais distintas.
    
2. **Correção de Viés:** Necessidade de aplicação de _Data Augmentation_ direcionado **exclusivamente** para imagens de "Pressão Amarelada" e "Necrosadas" em geral, visando quebrar a associação exclusiva do amarelo com o diabetes e reforçar o reconhecimento de necroses.
    
3. **Revisão do Dataset de Pressão:** Urgência em reestruturar a divisão (Treino/Validação/Teste) da classe de pressão para validar as métricas reais do model