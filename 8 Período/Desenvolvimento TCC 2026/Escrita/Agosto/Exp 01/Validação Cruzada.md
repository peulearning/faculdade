# 🔬 Validação do Experimento — MobileNetV2

> [!question] Dúvida metodológica
> É necessário definir com a orientação se a **validação cruzada** solicitada corresponde à aplicação de **K-Fold Cross-Validation** ou à **repetição do mesmo experimento várias vezes**.

---

## 📌 Contexto

O experimento de referência utilizando a **MobileNetV2** apresentou aproximadamente **97% de acurácia**.

A ideia inicial foi verificar se esse resultado é **consistente e reproduzível**, executando novamente o mesmo experimento.

Entretanto, a professora mencionou especificamente a necessidade de realizar **validação cruzada**, sendo necessário esclarecer qual procedimento deve ser aplicado.

---

## 🔄 Possibilidade 1 — Validação Cruzada (Cross-Validation)

Na validação cruzada, o conjunto de dados é dividido em diferentes **folds**.

Exemplo: **5-Fold Stratified Cross-Validation**

Dataset de desenvolvimento
          │
          ├── Fold 1 → Treino / Validação
          ├── Fold 2 → Treino / Validação
          ├── Fold 3 → Treino / Validação
          ├── Fold 4 → Treino / Validação
          └── Fold 5 → Treino / Validação
                    │
                    ↓
             Média ± Desvio Padrão 

Cada fold é utilizado como conjunto de validação uma vez, enquanto os demais são utilizados para treinamento.

### Objetivo

Avaliar se o desempenho do modelo permanece consistente **em diferentes partições dos dados**.


## 🔁 Possibilidade 2 — Repetição do Experimento

Outra possibilidade é executar o **mesmo notebook várias vezes**, mantendo a mesma configuração experimental.

Exemplo:

```
Configuração original
        │
        ├── Execução 1
        ├── Execução 2
        ├── Execução 3
        ├── ...
        └── Execução 10
                 │
                 ↓
          Média ± Desvio Padrão
```

### Objetivo

Avaliar a **estabilidade e variabilidade do treinamento**, verificando se o modelo mantém desempenho semelhante entre diferentes execuções.

---

## 🤔 Diferença fundamental

|Procedimento|O que muda?|O que avalia?|
|---|---|---|
|**K-Fold Cross-Validation**|A partição utilizada para treinamento/validação|Robustez em diferentes subconjuntos dos dados|
|**Repetições independentes**|Seed/inicialização e/ou aleatoriedade do treinamento|Estabilidade e variabilidade do treinamento|

> **Importante:** executar o mesmo experimento 10 vezes **não é, tecnicamente, o mesmo que realizar validação cruzada**.

---

## ❓ Questão para confirmar com a orientação

> **Quando foi solicitada a "validação cruzada", devemos aplicar K-Fold/Stratified K-Fold ao conjunto de dados ou repetir o experimento original 10 vezes para avaliar a estabilidade do modelo?**

### 📝 Decisão

- [ ]  Aplicar **5-Fold Stratified Cross-Validation**
- [ ]  Aplicar **10-Fold Cross-Validation**
- [ ]  Realizar **10 execuções independentes**
- [ ]  Outra metodologia: ______________________

---
