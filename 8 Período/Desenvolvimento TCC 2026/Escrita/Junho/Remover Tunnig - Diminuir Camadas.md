## Após diminuir as  últimas camadas para 20

### 🔹 Fase 1: Treinando apenas as camadas densas

- **Epoch 1/15** 7/7 — accuracy: 0.5631 — loss: 0.8976 — val_accuracy: 0.6875 — val_loss: 0.6904 — learning_rate: 0.0010
    
- **Epoch 2/15** 7/7 — accuracy: 0.7477 — loss: 0.6541 — val_accuracy: 0.8333 — val_loss: 0.4735 — learning_rate: 0.0010
    
- **Epoch 3/15** 7/7 — accuracy: 0.7523 — loss: 0.5173 — val_accuracy: 0.8542 — val_loss: 0.4720 — learning_rate: 0.0010
    
- **Epoch 4/15** 7/7 — accuracy: 0.8108 — loss: 0.4544 — val_accuracy: 0.7500 — val_loss: 0.5788 — learning_rate: 0.0010
    
- **Epoch 5/15** 7/7 — accuracy: 0.8063 — loss: 0.4417 — val_accuracy: 0.8125 — val_loss: 0.4525 — learning_rate: 0.0010
    
- **Epoch 6/15** 7/7 — accuracy: 0.7793 — loss: 0.4576 — val_accuracy: 0.7917 — val_loss: 0.5483 — learning_rate: 0.0010
    
- **Epoch 7/15** 7/7 — accuracy: 0.8288 — loss: 0.3668 — val_accuracy: 0.8333 — val_loss: 0.4333 — learning_rate: 0.0010
    
- **Epoch 8/15** 7/7 — accuracy: 0.8604 — loss: 0.3652 — val_accuracy: 0.8125 — val_loss: 0.4671 — learning_rate: 0.0010
    
- **Epoch 9/15** 7/7 — accuracy: 0.8333 — loss: 0.3184 — val_accuracy: 0.8333 — val_loss: 0.4486 — learning_rate: 0.0010
    
- **Epoch 10/15** 7/7 — accuracy: 0.8514 — loss: 0.3291 — val_accuracy: 0.7917 — val_loss: 0.4794 — learning_rate: 0.0010
    
- **Epoch 11/15**
    
    - Epoch 11: ReduceLROnPlateau → learning rate reduzido para **0.00020000000949949026**
    - accuracy: 0.8739 — loss: 0.3148 — val_accuracy: 0.8333 — val_loss: 0.4470 — learning_rate: 0.0010 _(como aparece no log)_
- **Epoch 12/15** 7/7 — accuracy: 0.8874 — loss: 0.3004 — val_accuracy: 0.8125 — val_loss: 0.4352 — learning_rate: 2.0000e-04
    
- **Epoch 13/15** 7/7 — accuracy: 0.8739 — loss: 0.2760 — val_accuracy: 0.7917 — val_loss: 0.4768 — learning_rate: 2.0000e-04
    
- **Epoch 14/15** 7/7 — accuracy: 0.8964 — loss: 0.2360 — val_accuracy: 0.7917 — val_loss: 0.5083 — learning_rate: 2.0000e-04
    
- **Epoch 15/15**
    
    - Epoch 15: ReduceLROnPlateau → learning rate reduzido para **4.0000001899898055e-05**
    - accuracy: 0.9279 — loss: 0.2108 — val_accuracy: 0.8125 — val_loss: 0.4708 — learning_rate: 2.0000e-04 _(como aparece no log)_

---

### 🔹 Fase 2: Liberando parte das camadas da base

- **Epoch 1/25** 7/7 — accuracy: 0.7613 — loss: 0.4952 — val_accuracy: 0.8125 — val_loss: 0.4316 — learning_rate: 1.0000e-05
    
- **Epoch 2/25** 7/7 — accuracy: 0.7883 — loss: 0.4852 — val_accuracy: 0.8125 — val_loss: 0.4328 — learning_rate: 1.0000e-05
    
- **Epoch 3/25** 7/7 — accuracy: 0.7477 — loss: 0.4801 — val_accuracy: 0.8125 — val_loss: 0.4350 — learning_rate: 1.0000e-05
    
- **Epoch 4/25** 7/7 — accuracy: 0.8018 — loss: 0.4447 — val_accuracy: 0.8125 — val_loss: 0.4392 — learning_rate: 1.0000e-05
    
- **Epoch 5/25**
    
    - Epoch 5: ReduceLROnPlateau → learning rate reduzido para **1.9999999494757505e-06**
    - accuracy: 0.7838 — loss: 0.4513 — val_accuracy: 0.8125 — val_loss: 0.4439 — learning_rate: 1.0000e-05 _(como aparece no log)_
- **Epoch 6/25** 7/7 — accuracy: 0.8243 — loss: 0.4168 — val_accuracy: 0.8125 — val_loss: 0.4439 — learning_rate: 2.0000e-06
    
- **Epoch 7/25** 7/7 — accuracy: 0.7838 — loss: 0.4376 — val_accuracy: 0.8125 — val_loss: 0.4440 — learning_rate: 2.0000e-06
    
- **Epoch 8/25** 7/7 — accuracy: 0.8153 — loss: 0.4303 — val_accuracy: 0.8125 — val_loss: 0.4444 — learning_rate: 2.0000e-06
    
- **Epoch 9/25**
    
    - Epoch 9: ReduceLROnPlateau → learning rate reduzido para **3.999999989900971e-07**
    - accuracy: 0.8333 — loss: 0.4050 — val_accuracy: 0.8125 — val_loss: 0.4449 — learning_rate: 2.0000e-06 _(como aparece no log)_
- **Epoch 10/25** 7/7 — accuracy: 0.8108 — loss: 0.4493 — val_accuracy: 0.8125 — val_loss: 0.4444 — learning_rate: 4.0000e-07
    
- **Epoch 11/25** 7/7 — accuracy: 0.8288 — loss: 0.4167 — val_accuracy: 0.8125 — val_loss: 0.4439 — learning_rate: 4.0000e-07
    

---

## Resultado (teste)

- **Acurácia de teste:** 0.7143
    
- **Matriz de Confusão:**
    
    ![[Pasted image 20260615234209.png]]


	 


- **Relatório de Classificação:**
    
    - **diabetic:** precision 0.75 — recall 0.75 — f1-score 0.75 — support 28
    - **pressure:** precision 0.67 — recall 0.67 — f1-score 0.67 — support 21
    - **accuracy:** 0.71 — support 49
    - **macro avg:** precision 0.71 — recall 0.71 — f1-score 0.71 — support 49
    - **weighted avg:** precision 0.71 — recall 0.71 — f1-score 0.71 — support 49

---

## Observação final (revisada)

> “Entretanto, **o resultado ainda foi** abaixo do esperado na avaliação de teste, com acurácia de **0.7143**, apesar de a validação ter ficado próxima de **0.81–0.83** durante boa parte do treinamento.”



**Diminuir de 30 para 20 camadas descongeladas melhorou a validação durante o treinamento, mas não melhorou a generalização no conjunto de teste.**

Veja os números:

### Melhor validação

Durante o treinamento você chegou a:

```
val_accuracy = 85,42%val_loss = 0,4720
```

na época 3.

Mas no teste:

```
accuracy = 71,43%
```

---

## O que isso sugere?

Seu conjunto de validação possui apenas:

```
48 imagens
```

e o teste:

```
49 imagens
```

Isso é muito pequeno.

Vamos fazer uma conta rápida:

```
85,42% de 48 imagens ≈ 41 acertos71,43% de 49 imagens ≈ 35 acertos
```

Diferença:

```
41 - 35 = 6 imagens
```

Ou seja, apenas 6 imagens mudaram completamente sua percepção do modelo.

---

# O que me chama mais atenção

A matriz de confusão está extremamente equilibrada:

|Classe|Precision|Recall|F1|
|---|---|---|---|
|Diabetic|0.75|0.75|0.75|
|Pressure|0.67|0.67|0.67|

Isso é bom.

Você não tem um problema de:

```
classe dominante
```

nem de:

```
modelo enviesado
```

O modelo está errando as duas classes de forma relativamente semelhante.

---

# O verdadeiro problema

Observe:

```
21 corretos diabetic 7 erros diabetic 14 corretos pressure 7 erros pressure
```

Total:

```
35 acertos 14 erros
```

Se o modelo acertasse apenas mais:

```
3 imagens
```

você teria:

```
38 / 49 = 77,5%
```

Se acertasse mais:

```
4 imagens
```

teria:

```
39 / 49 = 79,6%
```

Percebe?

Seu teste é tão pequeno que:

```
3 ou 4 imagens
```

mudam completamente a accuracy.

---

# O que eu faria agora?

Sinceramente?

Não continuaria fazendo ajustes aleatórios de hiperparâmetros.

Você já testou:

- CLAHE
- Canny
- Grayscale
- BatchNorm
- Dropout
- Fine Tuning

e está sempre orbitando:

```
70% a 77%
```

Isso sugere um teto do dataset.

---

# O experimento que falta para um TCC forte

### K-Fold Cross Validation

Em vez de:

```
70% treino15% validação15% teste
```

usar:

```
5-Fold
```

ou

```
10-Fold
```

Porque com:

```
319 imagens
```

o K-Fold é muito mais confiável.

Exemplo:

|Fold|Accuracy|
|---|---|
|1|75%|
|2|72%|
|3|78%|
|4|74%|
|5|76%|

Média:

```
75% ± 2%
```

Isso é muito mais defensável para banca.

---

# Outra coisa que eu testaria

Você ainda não mostrou um experimento com:

```
class_weight=class_weights
```

ativado. já testei ✅ ( não funcionou )

Você calculou os pesos mas aparentemente não os utilizou.

Embora o desbalanceamento não seja enorme:

```
0.861.19
```

vale a pena testar.

---

# Minha conclusão técnica

Pelos resultados que você mostrou:

1. O modelo não está sofrendo um overfitting grave.
2. O fine-tuning de 20 camadas foi melhor que 30 camadas.
3. A principal limitação é o tamanho do dataset (319 imagens).
4. O conjunto de teste é pequeno demais para conclusões definitivas.
5. O próximo experimento mais valioso não é mexer em Dropout ou Dense, mas sim:
    - usar `class_weight`;  Já usei  ✅
    - testar EfficientNetB0; Não negociável  ❌
	    - ou fazer validação cruzada (5-Fold). ❌


--- 


# Após diminuir as  últimas camadas para 10


![[Pasted image 20260616185234.png]]

![[Pasted image 20260616185256.png]]


![[Pasted image 20260616185241.png|697]]


![[Pasted image 20260616185313.png]]