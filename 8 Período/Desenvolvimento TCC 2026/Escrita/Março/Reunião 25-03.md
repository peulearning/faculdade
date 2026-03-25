# 📝 Plano de Orientação

## 🎯 Prioridades de Pesquisa (Revisão Bibliográfica)

- [ ] **Métricas de Visão Computacional:** Mapear na literatura quais são as métricas de avaliação aceitáveis e esperadas para modelos de classificação de imagens.
    
- [ ] **Estado da Arte (Saúde + Mobile):** Levantar as discussões atuais sobre Visão Computacional na saúde, fazendo um recorte específico sobre a aplicação em **ambientes móveis**.
    
- [ ] **Mitigação de Erros:** Pesquisar estratégias técnicas sobre como evitar **falsos negativos** (um ponto crítico em aplicações voltadas para a saúde).
    

---

## 🧠 Definição e Justificativa do Modelo

 - [x] **Arquitetura:** MobileNetV2.  ( Se mostrou a melhor até o determinado momento .)
    
- [ ]  **Meta de Desempenho:** Os resultados do modelo devem ser comparados e chegar muito próximos aos relatados nos artigos de referência. ( Tabela na Reunião 17-03 )
    
- [ ] **Justificativa Temática:** A discussão sobre diabetes está em alta. É essencial embasar a relevância do projeto na necessidade de diferenciar com precisão a ferida diabética, a lesão por pressão e a pele normal.
    
- [ ] **Pré-processamento:** Aplicar técnicas para remover o _background_ das imagens (dataset) , garantindo que a rede neural foque apenas na área de interesse.
    

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