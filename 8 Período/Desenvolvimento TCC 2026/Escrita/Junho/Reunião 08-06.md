
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
    

#### 🔬 Experimentos de Pré-processamento (Novos Notebooks)

Para a próxima discussão, é necessário derivar o notebook principal em duas novas frentes de experimentação usando imagens em **escala de cinza**:

- [ ] **Notebook 1: Edge Canny**
    
    - Converter imagens para tons de cinza.
        
    - Aplicar o filtro de Canny para detecção de bordas (focando nos contornos das estruturas/lesões).
        
    - Treinar o modelo com esses dados e extrair métricas.
        
- [ ] **Notebook 2: CLAHE (Contrast Limited Adaptive Histogram Equalization)**
    
    - Converter imagens para tons de cinza.
        
    - Aplicar o CLAHE. Essa técnica é excelente para imagens clínicas, pois realça o contraste local sem estourar o brilho global, destacando melhor a textura dos tecidos.
        
    - Treinar o modelo com esses dados e extrair métricas.
        
- [ ] **Comparativo Final:** Montar um quadro comparativo dos resultados do Canny e do CLAHE contra a _baseline_ original (RGB sem filtros).



--- 



#### ⚪ Pipeline Analisado e Feito


Notebook 📚 :  [Refazendo Split DatasetOriginal Archictecture MobileNetV2 4 Classes Modify Grad-Cam Apply .ipynb - Colab](https://colab.research.google.com/drive/1194PdVXnJdZDFcPZAJlS4OZo4P5wOUyu#scrollTo=voGjGoPVIiT2) 


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

#### Etapa 3 — Novo Split Estratificado

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

##### Distribuição obtida

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

##### Train Generator

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

##### Validation/ Test Generator

Sem Data Augmentation:

```
val_test_datagen = ImageDataGenerator(
    preprocessing_function=preprocess_input
)
```

---

##### Etapa 5 — Fine-Tuning

Foi adotada uma estratégia em duas fases.

##### Fase 1

Treinamento apenas da cabeça classificadora.

Objetivo:

- Aproveitar os pesos pré-treinados da MobileNetV2;
- Adaptar as camadas finais ao domínio de feridas.

##### Fase 2

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