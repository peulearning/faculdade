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


---

# 📊 Tabela Comparativa dos Modelos

## Comparação de Arquiteturas de Classificação de Feridas  
  
| Modelo            | Framework  | Tamanho (Original) | Tempo de Inferência | Quantização FP16 | Quantização INT8 |
| ----------------- | ---------- | ------------------ | ------------------- | ---------------- | ---------------- |
| CNN Sequencial    | TensorFlow | 42.61 MB           | 0.024 s             | 21.31 MB         | 10.67 MB         |
| MobileNetV2       | TensorFlow | **9.08 MB**        | **0.013 s**         | 4.57 MB          | **2.74 MB**      |
| YOLOv3 + ResNet50 | PyTorch    | 90.98 MB           | 0.552 s             | —                | 90.23 MB         |

---

# 📌 Análise dos Resultados

## Análise Comparativa das Arquiteturas  
  
A comparação entre os modelos demonstrou diferenças significativas em termos de custo computacional, tamanho do modelo e tempo de inferência.  
  
### Arquitetura Sequencial  
  
A rede convolucional sequencial apresentou desempenho intermediário em relação aos demais modelos analisados. Apesar de possuir tamanho consideravelmente maior que o MobileNetV2, a aplicação de técnicas de quantização permitiu reduzir significativamente o tamanho do modelo, alcançando uma redução de aproximadamente 75% no formato INT8.  
  
### MobileNetV2  
  
O MobileNetV2 apresentou o melhor desempenho geral para aplicações em dispositivos móveis. O modelo possui arquitetura otimizada para dispositivos com restrições de hardware, utilizando convoluções separáveis em profundidade (Depthwise Separable Convolutions). Após a quantização, o modelo atingiu apenas 2.74 MB, mantendo alta eficiência e baixo tempo de inferência.  
  
### YOLOv3 + ResNet50  
  
O modelo baseado em YOLOv3 apresentou o maior custo computacional entre as arquiteturas testadas. Apesar de apresentar boas métricas de classificação, o tamanho elevado do modelo e o tempo de inferência significativamente maior indicam menor adequação para aplicações em dispositivos móveis.  
  
Além disso, a quantização dinâmica aplicada ao modelo resultou em redução mínima no tamanho do modelo, uma vez que grande parte das operações do YOLOv3 é composta por camadas convolucionais, que não são totalmente beneficiadas por esse tipo de otimização.  
  
### Considerações Gerais  
  
Os resultados indicam que arquiteturas projetadas especificamente para dispositivos móveis, como o MobileNetV2, apresentam desempenho superior quando comparadas a arquiteturas mais complexas. A quantização mostrou-se uma técnica eficaz para reduzir o tamanho dos modelos e melhorar a eficiência computacional, especialmente em arquiteturas leves.

# 📊 Resposta

> **Após a quantização os modelos permanecem com a mesma eficiência?**

Resposta técnica:

❌ **Nem sempre exatamente a mesma.**

✔️ Normalmente ocorre:

- grande redução de tamanho
    
- melhora na inferência
    
- **pequena perda de precisão (dependendo do modelo)**