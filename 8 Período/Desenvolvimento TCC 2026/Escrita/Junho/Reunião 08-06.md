
#### 🎯 Objetivo Principal e Considerações Críticas

O foco central das próximas iterações é garantir a ausência de **overfitting**, validar o impacto real do pré-processamento e testar abordagens híbridas usando técnicas clássicas de Visão Computacional.

- **Validação de Overfitting:** É crucial monitorar e documentar as curvas de _loss_ e _accuracy_ (treino vs. validação) para provar que o modelo está generalizando bem.
    
- **Isolamento de Data Augmentation:** A orientação é separar completamente a augmentação do treino e do teste.
    
    - _Nota Arquitetural:_ Como o pipeline atual já foi migrado para utilizar o `ImageDataGenerator` exclusivamente durante o _loop_ de treinamento, **esta exigência já está sendo atendida de forma nativa e segura**. Os geradores de validação e teste devem receber apenas o reescalonamento de pixels, sem transformações extras.
        
    - _Ponto de Atenção:_ Lembre-se de manter a restrição de brilho (_brightness range_) fora das configurações do gerador, já que a remoção desse parâmetro trouxe aquele ganho sólido de 2% no desempenho recente.
        

#### 🔄 Pipeline de Treinamento (Step-by-Step)

Para organizar os dados e o processo de treinamento, a sequência oficial acordada é:

1. **Unificação Inicial:** Juntar as origens de treino e teste atuais em um único _pool_ de dados brutos.
    
2. **Split Rigoroso:** Dividir o dataset unificado em três diretórios limpos: `Train`, `Validation` e `Test`.
    
3. **Configuração de Augmentation:** Configurar o `ImageDataGenerator` para atuar dinamicamente apenas no diretório `Train`.
    
4. **Treinamento Base:** Executar o treinamento (_Play training_) e registrar as métricas.
    
5. **Fine-Tuning:** Refinar os pesos do modelo (descongelando camadas finais, se necessário) para estabilizar os resultados.  
    
6. **Encerramento das Atividades:** Salvar os pesos e gerar os artefatos de avaliação (Matriz de Confusão, Curvas ROC/AUC).
    




--- 



#### ⚪ Pipeline de Treinamento


Notebook 📚 :  [Refazendo Split DatasetOriginal Archictecture MobileNetV2 4 Classes Modify Grad-Cam Apply .ipynb - Colab](https://colab.research.google.com/drive/1194PdVXnJdZDFcPZAJlS4OZo4P5wOUyu#scrollTo=voGjGoPVIiT2) 

 #Validação de Overfitting

Para monitorar possíveis sinais de overfitting foram utilizadas:

- Curvas de Accuracy de Treino e Validação;
- Curvas de Loss de Treino e Validação;
- EarlyStopping;  
- ReduceLROnPlateau; 
- Avaliação final utilizando conjunto de teste isolado.

 #Resultado Observado

Durante o treinamento foi observada uma diferença moderada entre treino e validação.

```
Treino ≈ 79% Validação ≈ 65% ~ 70%
```

Apesar da diferença entre as curvas, a perda de validação permaneceu relativamente estável ao longo das épocas.

Não foram observados comportamentos típicos de overfitting severo, como:

```
Train Loss ↓↓↓
Validation Loss ↑↑↑
```

ou

```
Train Accuracy ↑↑↑Validation Accuracy ↓↓↓
```

 Conclusão

Até o momento não existem evidências fortes de overfitting severo.

O modelo demonstra capacidade razoável de generalização para dados não vistos.


#Isolamento da Data Augmentation

A orientação de separar completamente os conjuntos de treino, validação e teste foi atendida.

O fluxo executado foi:

```
Dataset Original       
 ↓
 Dataset Mestre        ↓Novo Split(train / validation / test)        ↓Data Augmentation apenas no treino
```

 Configuração utilizada

```
rotation_range=20
width_shift_range=0.1
height_shift_range=0.1
shear_range=0.1
zoom_range=0.1
horizontal_flip=True
```

A validação e o teste receberam apenas:

```
preprocess_input
```

sem transformações geométricas ou fotométricas.

 #Garantias obtidas

- Sem compartilhamento de imagens entre conjuntos;
- Sem compartilhamento de imagens augmentadas entre conjuntos;
- Sem evidências de Data Leakage;
- Pipeline compatível com boas práticas experimentais.


---
##### Etapa 1 — Recuperação do Dataset Original

O dataset original disponibilizado pelo autor possuía a seguinte organização:

```
Train/
Test/
```

Além disso, continha diretórios auxiliares do macOS (`__MACOSX`), os quais foram desconsiderados durante o processamento.


--- 

##### Etapa 2 — Construção do Dataset Mestre

As imagens originalmente presentes em Train e Test foram reunidas em um único repositório denominado:

```
dataset_master/
```

Objetivo:

- Eliminar dependência do split originalmente fornecido;
- Permitir a criação de uma nova divisão controlada dos dados;
- Garantir reprodutibilidade experimental.



---

##### Etapa 3 — Novo Split Estratificado

A partir do `dataset_master` foi criado um novo conjunto experimental:

```
dataset_master_split/

├── train/
├── validation/
└── test/
```

Utilizando:

```
random_state = 42
```

para garantir reprodutibilidade.

###### Distribuição obtida

|Classe|Train|Validation|Test|
|---|---|---|---|
|Background|17|4|4|
|Diabetic|129|28|28|
|Normal|70|15|15|
|Pressure|93|20|21|

Totais:

| Conjunto   | Imagens |
| ---------- | ------- |
| Train      | 309     |
| Validation | 67      |
| Test       | 68      |

  
TRAIN 
background: 17 diabetic: 129 normal: 70 pressure: 93 
TOTAL: 309 


VALIDATION
background: 4 diabetic: 28 normal: 15 pressure: 20 
TOTAL: 67 

TEST
background: 4 diabetic: 28 normal: 15 pressure: 21 
TOTAL: 68

--- 


##### Etapa 4 — Treinamento Base

O treinamento passou a utilizar:

###### Train Generator

Com Data Augmentation:

```
train_datagen = ImageDataGenerator(
    preprocessing_function=preprocess_input,
    rotation_range=20,
    width_shift_range=0.1,
    height_shift_range=0.1,
    shear_range=0.1,
    zoom_range=0.1,
    horizontal_flip=True,
    fill_mode='nearest'
)
```

###### Validation/ Test Generator

Sem Data Augmentation:

```
val_test_datagen = ImageDataGenerator(
    preprocessing_function=preprocess_input
)
```

---

##### Etapa 5 — Fine-Tuning

Foi adotada uma estratégia em duas fases.

###### Fase 1

Treinamento apenas da cabeça classificadora.

Objetivo:

- Aproveitar os pesos pré-treinados da MobileNetV2;
- Adaptar as camadas finais ao domínio de feridas.

###### Fase 2

Fine-Tuning parcial da MobileNetV2.

Configuração:

```
base_model.trainable = True

for layer in base_model.layers[:-30]:
    layer.trainable = False
```

Mantendo congeladas todas as camadas exceto as 30 últimas.

Taxa de aprendizado reduzida:

```
Adam(learning_rate=1e-5)
```

para evitar destruição dos pesos previamente aprendidos.

---



* Matriz de Confusão


![[Captura de tela 2026-06-09 151227.png]]
![[Pasted image 20260609151609.png]]

 Classification Report

| Classe     | Precision | Recall | F1-Score |
| ---------- | --------- | ------ | -------- |
| Background | 1.00      | 0.50   | 0.67     |
| Diabetic   | 0.65      | 0.86   | 0.74     |
| Normal     | 0.82      | 0.93   | 0.88     |
| Pressure   | 0.67      | 0.38   | 0.48     |


* Acúracia Treinamento / Perca Loss 

![[Captura de tela 2026-06-09 151234.png]]

 Interpretação

Observou-se crescimento consistente da acurácia de treinamento durante as primeiras épocas, indicando aprendizado efetivo das características das imagens.

A acurácia de validação acompanhou parcialmente esse crescimento, mantendo-se relativamente estável durante boa parte do treinamento.

Valores observados aproximadamente:

```
Treino ≈ 79%Validação ≈ 65%–70%
```

Embora exista uma diferença entre as curvas, não foi observada divergência extrema entre treino e validação.

 Conclusão

Os resultados sugerem que o modelo está aprendendo padrões relevantes sem apresentar comportamento típico de memorização excessiva.

---

 Curva de Loss

 Comportamento observado

A perda de treinamento apresentou queda expressiva nas primeiras épocas:

```
2.15 → 0.55
```

demonstrando convergência adequada do processo de otimização.

Já a perda de validação apresentou redução gradual seguida de estabilização:

```
1.25 → 0.60
```

mantendo comportamento relativamente controlado durante o treinamento.

 Ponto de Atenção

Próximo às últimas épocas ocorreu uma pequena oscilação na perda de treinamento.

Entretanto:

- A perda de validação não apresentou crescimento explosivo;
- O EarlyStopping interrompeu o treinamento antes que ocorresse degradação significativa;
- O ReduceLROnPlateau contribuiu para estabilizar o processo de aprendizagem.

---

 Avaliação de Overfitting

Um cenário clássico de overfitting seria caracterizado por:

```
Accuracy Treino ↑↑↑
Accuracy Validação ↓↓↓
Loss Treino ↓↓↓
Loss Validação ↑↑↑
```

Esse comportamento não foi observado no experimento atual.

 Evidências

✅ Curvas relativamente próximas.

✅ Loss de validação controlada.

✅ Accuracy de validação estável.

✅ Generalização confirmada pelo conjunto de teste.

 Conclusão

Os gráficos indicam ausência de overfitting severo. Existe uma diferença moderada entre treino e validação, esperada para datasets pequenos e desbalanceados, mas o comportamento geral das curvas sugere que o modelo manteve capacidade de generalização para dados não vistos.


* Curva AUC / ROC
![[Captura de tela 2026-06-09 151314.png]]


 AUC por Classe

|Classe|AUC|
|---|---|
|Background|1.00|
|Diabetic|0.83|
|Normal|1.00|
|Pressure|0.7|


 📌 Resultado Final do Experimento

|Métrica|Valor|
|---|---|
|Accuracy|71%|
|Macro F1|0.69|
|Weighted F1|0.69|
|Melhor Classe|Normal|
|Classe Mais Difícil|Pressure|
|Principal Confusão|Pressure → Diabetic|
|Overfitting Severo|Não observado|
|Data Leakage|Não observado|