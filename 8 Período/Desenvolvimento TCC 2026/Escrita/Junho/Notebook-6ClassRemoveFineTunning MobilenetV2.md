[notebooks_tcc/Stress_6Class_SemFineTuning_MobileNetV2.ipynb at main · peulearning/notebooks_tcc](https://github.com/peulearning/notebooks_tcc/blob/main/Stress_6Class_SemFineTuning_MobileNetV2.ipynb)


Nesse notebook , apliquei as mesmas coisas que os notebooks anteriores. 


### 1. Dados e Pré-Processamento

- **Resolução de Entrada (320x320):** Aumentamos a resolução padrão da MobileNetV2 (que é 224x224) para 320x320.
    
    - _Por quê:_ Feridas possuem detalhes microscópicos (como bordas de fibrina ou pontos de sutura) que se perdem em resoluções baixas.
        
- **Pré-processamento Nativo:** Uso do `preprocess_input` da própria MobileNetV2 em vez do clássico `rescale=1./255`.
    
    - _Por quê:_ Garante que as imagens de feridas entrem na rede com a exata mesma distribuição matemática das imagens do ImageNet que a rede usou para aprender.
        
- **Data Augmentation Estratégico (Suave):** Rotação leve (10°), zoom quase nulo (0.05), sem distorção de cisalhamento (`shear=0`) e com espelhamento horizontal.
    
    - _Por quê:_ Reduzimos a agressividade para não desfigurar linhas retas (bisturi/suturas da classe cirúrgica) e para não cortar o "contexto" da perna ou pé, que ajuda a diferenciar úlceras venosas de diabéticas.
        

### 2. A Arquitetura da Rede Neural

- **Backbone (Base do Modelo):** `MobileNetV2` carregada com os pesos do ImageNet.
    
- **Estratégia de Transfer Learning:** Extração de Características (_Feature Extraction_ pura), com as 154 camadas da base **100% congeladas**.
    
    - _Por quê:_ Evitar o "Esquecimento Catastrófico". Como temos poucas imagens (Dataset pequeno), se treinássemos a base agora, a rede destruiria os filtros matemáticos perfeitos de bordas e formas que o Google levou semanas para treinar.
        
- **O Topo da Rede (Custom Classifier Head):**
    
    - **Dual Pooling (`Concatenate`):** Juntamos o `GlobalAveragePooling2D` (a média da imagem) com o `GlobalMaxPooling2D` (os picos de textura extrema, como pontos de necrose).
        
    - **Camadas Densas Profundas (Funil):** Uma camada de 512 neurônios seguida de uma de 256 neurônios.
        
    - **Regularização Severa:** Uso de `BatchNormalization` (para estabilizar a matemática) e `Dropout` de 0.4 e 0.3 (para "desligar" neurônios aleatoriamente e forçar a rede a não decorar as imagens).
        

### 3. Matemática de Otimização e Treinamento

- **Função de Perda (Focal Loss):** `CategoricalFocalCrossentropy(gamma=2.0)`.
    
    - _Por quê:_ A Crossentropy normal desiste das imagens difíceis e foca nas fáceis. A Focal Loss aplica uma penalidade matemática que obriga o modelo a prestar atenção nas classes que ele está errando mais (como _Pressure_ e _Sirurgical_).
        
- **Balanceamento de Classes (`class_weights`):** Uso do `compute_class_weight` do Scikit-Learn.
    
    - _Por quê:_ O seu dataset é altamente desbalanceado (ex: a classe _background_ teve peso 5.8 e a _venous_ peso 0.57). Isso ensina à rede que errar uma classe rara custa muito mais caro do que errar uma classe abundante.
        
- **Otimizador e Callbacks:**
    
    - **Adam (Learning Rate 1e-3):** Taxa padrão alta, ideal para treinar camadas Densas que começam do zero (aleatórias).
        
    - **EarlyStopping (patience=8):** Para o treino se a rede começar a decorar os dados (_overfitting_) e restaura os melhores pesos.
        
    - **ReduceLROnPlateau (patience=3):** Reduz a taxa de aprendizado se a rede travar, ajudando-a a encontrar o mínimo global da função de perda com passos menores.
        

### Onde estamos agora?

Este conjunto de técnicas permitiu que um modelo "cego" para medicina (a base da MobileNetV2 não sabe o que é uma ferida, só sabe ver formas) alcançasse **73% de acerto em 6 classes médicas hiper-complexas**, apenas interpretando formas geométricas e texturas.