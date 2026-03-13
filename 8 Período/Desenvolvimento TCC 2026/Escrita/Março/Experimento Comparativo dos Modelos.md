# 🤖 Modelo 1 — YOLOv3 + ResNet

## Métricas

|Classe|Precisão|Recall|F1-score|
|---|---|---|---|
|Background|0.99|1.00|0.99|
|Diabetic|0.94|0.94|0.94|
|Normal|0.99|1.00|1.00|
|Pressure|0.95|0.90|0.92|
|Sirurgical|0.91|0.93|0.92|
|Venous|0.92|0.94|0.93|

### Resultados Gerais

- **Accuracy:** 0.95
    
- **Macro Avg:** 0.95
    
- **Weighted Avg:** 0.95
    

### Performance Computacional

- **Tempo de inferência:** 0.451 s
    
- **Tamanho do modelo:** 90.98 MB
    

---

# 🤖 Modelo 2 — MobileNetV2

## Métricas

|Classe|Precisão|Recall|F1-score|
|---|---|---|---|
|Background|1.00|1.00|1.00|
|Diabetic|0.94|0.98|0.96|
|Normal|1.00|1.00|1.00|
|Pressure|0.97|0.96|0.96|
|Sirurgical|0.99|0.98|0.99|
|Venous|0.98|0.97|0.97|

### Resultados Gerais

- **Accuracy:** 0.98
    
- **Macro Avg:** 0.98
    
- **Weighted Avg:** 0.98
    

### Performance Computacional

- **Tempo de inferência:** 0.011 s
    
- **Tamanho do modelo:** 9.08 MB
    

---

# 🤖 Modelo 3 — CNN Sequencial (Treinada do Zero)

## Métricas

|Classe|Precisão|Recall|F1-score|
|---|---|---|---|
|Diabetic|0.81|0.90|0.85|
|Normal|1.00|0.99|1.00|
|Pressure|0.91|0.87|0.89|
|Sirurgical|0.87|0.89|0.88|
|Venous|0.86|0.80|0.83|

### Resultados Gerais

- **Accuracy:** 0.89
    
- **Macro Avg:** 0.89
    
- **Weighted Avg:** 0.89
    

### Performance Computacional

- **Tempo de inferência:** 0.018 s
    
- **Tamanho do modelo:** 42.61 MB
    

---

# 📊 Comparação Geral dos Modelos

|Modelo|Accuracy|Inferência|Tamanho|
|---|---|---|---|
|YOLOv3 + ResNet|0.95|0.451 s|90.98 MB|
|MobileNetV2|**0.98**|**0.011 s**|**9.08 MB**|
|CNN Sequencial|0.89|0.018 s|42.61 MB|

---

# 📌 Observações

- **MobileNetV2 apresentou o melhor desempenho geral**, combinando alta acurácia com baixo tempo de inferência.
    
- **YOLOv3 + ResNet apresentou maior custo computacional**, com modelo significativamente maior.
    
- **CNN Sequencial treinada do zero apresentou desempenho inferior**, possivelmente devido à ausência de pré-treinamento.