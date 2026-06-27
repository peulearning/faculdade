
# Prof. Felipe (23/06)

## 📚 Revisão de Literatura (Pesquisa)
- [x] **Anomalia na Acurácia pós Fine-Tuning:** Pesquisar artigos que expliquem o seguinte cenário: o modelo sofreu perda catastrófica (*catastrophic forgetting*) durante o *fine-tuning*, porém a acurácia geral de testes ficou **maior** do que no modelo sem *fine-tuning*. O que justifica esse comportamento?
- [x] **Estratégia de Split de Dados:** Buscar referências na literatura sobre divisão de dados em *datasets* limitados/pequenos. É mais adequado manter a divisão clássica (Treino / Validação / Teste) ou simplificar apenas para (Treino / Teste)?

## 🛠️ Ajustes de Modelo e Código
- [ ] **Baseline sem Fine-Tuning:** Verificar a necessidade de fazer ajustes nos parâmetros do modelo base (sem aplicar *fine-tuning*).
- [ ] **Adição de Novas Classes:** Analisar se a inclusão das duas novas classes (feridas cirúrgicas e venosas) exige refatoração de código, ajuste de parâmetros ou mudança na arquitetura.
- [ ] **Ajuste Fino por Tipo de Ferida:** Avaliar se as estruturas físicas e padrões visuais específicos das feridas cirúrgicas e venosas exigem adequações na rede Sequencial ou na MobileNetV2. Fatores a revisar:
  - Parâmetros da rede.
  - Hiperparâmetros de treinamento.
  - Estratégias de *Data Augmentation* (aumentação de dados).

## 📌 Observações e Insights da Reunião
> **Análise de Ruído nas Arquiteturas:**
> - **Rede Sequencial:** Os ruídos gerados/percebidos são muito baixos (pouco perceptíveis).
> - **MobileNetV2:** Os ruídos são consideravelmente mais notáveis. Avaliar o impacto disso nas predições.


--- 


## Resposta da Anomalia  📚

Com Fine Tunning 4 Classes  [4_Classes_Refazendo Split DatasetOriginal Archictecture MobileNetV2 Modify Grad-Cam Apply .ipynb - Colab](https://colab.research.google.com/drive/1194PdVXnJdZDFcPZAJlS4OZo4P5wOUyu#scrollTo=h0HmlC1ZmHnr) 

![[Pasted image 20260624181913.png]]



![[Pasted image 20260624181900.png]]



Sem Fine Tunning 4 Classes  [Cópia de RF_FT_4_Classes_Refazendo Split DatasetOriginal Archictecture MobileNetV2 Modify Grad-Cam Apply .ipynb - Colab](https://colab.research.google.com/drive/1NWSBfrQDf4JZSDahWIudGHKbDCv-lpXl#scrollTo=yqNWmkf9H0mK)


![[Pasted image 20260625195904.png]]


![[Pasted image 20260625200000.png]]

Ao Analisar os gráficos e os logs de época lado a lado revela que o que ocorreu aqui não é exatamente o que a literatura define como _Catastrophic Forgetting_, mas sim uma combinação de dois outros fenômenos muito bem documentados em Deep Learning: **Destruição de Representação (Representation Destruction)** e **Deslocamento Induzido por Augmentation (Augmentation-Induced Shift)**.

Aqui está a literatura e a explicação técnica do que causou esse comportamento na sua "Fase 2".

### 1. O Choque Inicial: Destruição de Representação

O termo _Catastrophic Forgetting_ (Esquecimento Catastrófico) na literatura clássica (como em trabalhos de French, 1999, ou Kirkpatrick et al., 2017) refere-se especificamente ao **Aprendizado Contínuo (Continual Learning)**: o modelo aprende a Tarefa A, depois aprende a Tarefa B, e esquece a Tarefa A.

O que aconteceu no seu log (a acurácia de treino desabar de 0.91 para 0.59 na Epoch 1/25 ao liberar as camadas) ocorre dentro de uma única tarefa durante o _Transfer Learning_. A literatura chama isso de **Feature Co-adaptation Breakdown** ou **Representation Destruction**.

- **A Literatura:** O artigo seminal **"How transferable are features in deep neural networks?" (Yosinski et al., 2014)** explica que as camadas de uma rede neural co-adaptam seus pesos umas com as outras de forma frágil. Outro artigo recente e muito relevante é **"Fine-Tuning can Distort Pretrained Features and Underperform Out-of-Distribution" (Kumar et al., 2022)**.
    
- **O que ocorreu no seu modelo:** Quando você iniciou a Fase 2 ("Liberando parte das camadas da base"), os gradientes calculados pela sua camada de classificação (que ainda estava se ajustando) fluíram com muita força para as camadas convolucionais recém-descongeladas. Isso "quebrou" os pesos ótimos que vieram pré-treinados. Em problemas de classificação com apenas 2 classes, uma queda para a casa dos 0.57~0.59 significa que os filtros da rede foram tão bagunçados pelo gradiente inicial que ela voltou a praticamente "chutar" o resultado (quase 50/50).
    

### 2. Por que a Validação ficou maior (e travada)?

Olhando seus logs, a acurácia de treino desaba e sofre para subir, mas a `val_accuracy` fica perfeitamente travada em `0.8657` por mais de 12 épocas seguidas, mesmo com a _loss_ flutuando levemente. Isso é o indicativo claro de duas características da sua arquitetura de treino:

- **O Efeito do Data Augmentation Dinâmico:** Quando a estratégia adota transformações de imagem (rotações, zooms, espelhamentos) de forma dinâmica usando geradores inseridos exclusivamente dentro do loop de treino, a rede é forçada a classificar imagens distorcidas, difíceis e inéditas a cada _step_. Enquanto isso, o conjunto de validação passa pela rede de forma "limpa" e estática. O artigo **"Fixing the train-test resolution discrepancy" (Touvron et al., 2019)** aborda exatamente como o pipeline de treino moderno frequentemente gera métricas de treino piores que as de teste/validação porque o treino atua sob forte perturbação e regularização. A rede avalia imagens limpas muito melhor do que imagens sob _augmentation_.
    
- **A "Reta" de Validação:** Uma acurácia de validação cravada repetidamente em 0.8657 com variações na casa decimal (exatamente a mesma porcentagem de acerto) sugere matematicamente que o conjunto de validação é bastante enxuto. O modelo pode estar errando e acertando exatamente as mesmas poucas imagens a cada época. As pequenas mudanças nos pesos na Fase 2 não foram suficientes para cruzar o limite de decisão (_decision boundary_) de nenhuma imagem de validação, alterando apenas o grau de confiança (a _loss_), mas não o veredito final.
    

### Como a literatura resolve essa anomalia na Fase 2?

Para evitar essa destruição de pesos ao descongelar as camadas (e essa queda abrupta no gráfico), a literatura acadêmica padrão recomenda metodologias de **Gradual Unfreezing** e **Slanted Triangular Learning Rates**, popularizadas pelo artigo **"Universal Language Model Fine-tuning for Text Classification" (Howard & Ruder, 2018)** — que, apesar do título focado em texto, tornou-se o padrão ouro para Visão Computacional.

A prática consiste em:

1. Usar um _Learning Rate_ (taxa de aprendizado) severamente menor ao descongelar a base (algo como 10x ou 100x menor do que o usado na Fase 1). Seus logs mostram que o `ReduceLROnPlateau` reduziu a taxa drasticamente mais para o final, mas o impacto inicial já havia ocorrido na Época 1.
    
2. Descongelar um bloco convolucional por vez, de cima para baixo, em vez de liberar "parte das camadas" todas de uma vez.




Código Sugerido para Tentar Recuperar através do Fine-Tunnig uma Possibilidade


```
# ----------------------------------------------------
    # FASE 1: Aquecimento da Cabeça (Transfer Learning)
    # ----------------------------------------------------
    base_model = tf.keras.applications.MobileNetV2(
        input_shape=(224, 224, 3),
        include_top=False,
        weights='imagenet'
    )
    
    # Congela TUDO
    base_model.trainable = False
    
    # Constrói a cabeça para 4 classes
    inputs = tf.keras.Input(shape=(224, 224, 3))
    x = base_model(inputs, training=False) # training=False garante que o BatchNorm não desconfigure
    x = tf.keras.layers.GlobalAveragePooling2D()(x)
    x = tf.keras.layers.Dropout(0.3)(x)
    outputs = tf.keras.layers.Dense(4, activation='softmax')(x)
    
    model = tf.keras.Model(inputs, outputs)
    
    # Compila com LR "normal"
    model.compile(optimizer=tf.keras.optimizers.Adam(learning_rate=1e-3),
                  loss='categorical_crossentropy',
                  metrics=['accuracy'])
                  
    # Treina apenas a cabeça densa (ex: 10 a 15 épocas)
    model.fit(train_generator, validation_data=val_generator, epochs=15)
    
    # ----------------------------------------------------
    # FASE 2: Gradual Unfreezing (O segredo para não quebrar os pesos)
    # ----------------------------------------------------
    base_model.trainable = True
    
    # Quantas camadas o MobileNetV2 tem? Geralmente ~154.
    # Vamos congelar as primeiras 100 camadas (que detectam bordas e cores genéricas)
    # e descongelar apenas os últimos blocos convolucionais.
    fine_tune_at = 100 
    
    for layer in base_model.layers[:fine_tune_at]:
        layer.trainable = False
        
    # CRÍTICO: Recompilar o modelo com um Learning Rate MUITO menor (ex: 1e-5 ou 1e-6)
    model.compile(optimizer=tf.keras.optimizers.Adam(learning_rate=1e-5),
                  loss='categorical_crossentropy',
                  metrics=['accuracy'])
                  
    # Treina o fine-tuning
    model.fit(train_generator, validation_data=val_generator, epochs=25)
```



--- 

## Resposta da Estratégia de Split 🛠️

É perfeitamente compreensível que você esteja usando a divisão 70/15/15. Esse é o padrão "de livro" ensinado em 99% dos cursos de Machine Learning. No entanto, essa regra de ouro foi criada para a era do _Big Data_ (como a base ImageNet, que tem mais de 1 milhão de imagens).

Quando você aplica a divisão 70/15/15 em um contexto de prototipagem com imagens clínicas reais — como a classificação de lesões que você está desenvolvendo no HealScan, onde o volume total de dados ($N$) é severamente restrito —, essa divisão tripla cria duas armadilhas estatísticas fatais.

Aqui está o motivo pelo qual a literatura recomenda abandonar o 70/15/15 e simplificar para apenas **Treino / Teste (ex: 80/20 ou 85/15)** em datasets pequenos:

### 1. O Problema da Fome de Dados (_Data Starvation_)

Modelos profundos, como a arquitetura que você está ajustando, são extremamente "famintos" por dados para conseguir mapear as texturas complexas de 4 classes clínicas diferentes.

- **No split 70/15/15:** Você está jogando fora **30%** do seu poder de aprendizado apenas para poder avaliar o modelo. Em um dataset pequeno, perder 30% das imagens de treino significa perder variações cruciais de cor, borda e iluminação. O modelo não atinge o seu potencial máximo de generalização.
    
- **Na literatura (Guyon, 1997):** A lei de escala matemática prova que, quanto menor a sua base total, mais dados você precisa alocar para o Treinamento. Mudar para uma divisão de Treino/Teste de 80/20 devolve uma carga vital de imagens para o modelo aprender.
    

### 2. A Alta Variância da Validação (O "Efeito Loteria")

Imagine que seu dataset tenha, por exemplo, 400 imagens (100 de cada classe).

- Se você separa 15% para validação, você tem apenas **15 imagens por classe** nesse conjunto.
    
- Se o modelo errar ou acertar apenas **uma** única imagem durante uma época de treino, a acurácia de validação daquela classe salta ou despenca quase 7% de uma vez.
    
- **O resultado prático:** O seu gráfico de `val_accuracy` vai ficar caótico, cheio de picos e vales. Isso destrói o funcionamento de ferramentas como o _Early Stopping_ ou o _ReduceLROnPlateau_, porque o Keras não vai conseguir entender se o modelo está realmente melhorando ou se apenas deu "sorte" no lote daquela época.
    

### Por que simplificar para apenas Treino / Teste é mais adequado?

A literatura (como a revisão de Arlot & Celisse) reconhece que, quando os dados são escassos, você precisa tomar uma decisão pragmática: **é melhor ter um modelo bem treinado avaliado de forma um pouco mais simples, do que um modelo mal treinado avaliado de forma rigorosa.**

Ao adotar um split de **80/20 (Treino/Teste)**, a dinâmica funciona assim:

1. **Maximização do Aprendizado:** 80% dos dados vão para a sua estratégia de _data augmentation_ dinâmica, garantindo que o modelo veja o máximo de diversidade de tecido possível.
    
2. **Avaliação Consolidada:** Os 20% restantes (que obrigatoriamente devem ser estratificados para manter a proporção exata das 4 classes) atuam de forma dupla. No código do Keras, você passa esses 20% no parâmetro `validation_data` para monitorar o treinamento e evitar o _overfitting_. Como você não tem dados suficientes para uma terceira base, a métrica final de acurácia, recall e a matriz de confusão do seu TCC serão extraídas do comportamento do modelo nesses mesmos 20% no final do treinamento.
    

**Em resumo:** O split 70/15/15 em datasets limitados "asfixia" o aprendizado do modelo e gera métricas mascaradas por falta de volume. O uso do Hold-out simples (Treino/Teste a 80/20 ou 85/15) é a defesa estatística mais sólida para garantir que a rede neural tenha exemplos suficientes para aprender os limites de decisão da sua tarefa de Visão Computacional


Para fundamentar essa troca de estratégia e construir uma defesa sólida para a sua metodologia, você precisa citar autores que investigaram matematicamente o comportamento de redes neurais e algoritmos sob **escassez de dados**. A literatura estatística e de aprendizado de máquina prova que a divisão tradicional (70/15/15) não é um dogma inquebrável, mas sim uma heurística que falha em cenários limitados.

Aqui estão os artigos científicos e as justificativas técnicas para comprovar os dois fenômenos discutidos (Fome de Dados e Alta Variância), focando em dados de alta dimensionalidade como imagens clínicas.

### 3. A Prova contra a Divisão Tripla (O Problema da "Fome de Dados")

Quando o tamanho da amostra (N) é pequeno, retirar 30% das imagens para validação e teste degrada irreversivelmente a capacidade do modelo de aprender as fronteiras de decisão das classes.

- **O Estudo Biomédico Definitivo:**
    
    > **Dobbin, K. K., & Simon, R. M. (2011).** _Optimally splitting cases for training and testing high dimensional classifiers._ Journal of Machine Learning Research, 12(12).
    
    - **A Defesa:** Os autores focaram especificamente em dados médicos e biológicos de alta dimensionalidade (onde há muitas características e poucos exemplos). Eles provaram matematicamente que, quando o dataset é restrito, **alocar uma fração excessiva para avaliação compromete a construção do modelo**. A recomendação deles é maximizar os dados de treino, justificando a consolidação em uma partição de avaliação única (Hold-out simples) em vez de fatiar o dataset em três.
        
- **A Lei de Escala Estatística:**
    
    > **Guyon, I. (1997).** _A scaling law for the validation-set training-set size ratio._ AT&T Bell Laboratories.
    
    - **A Defesa:** Isabelle Guyon demonstra que a proporção ideal entre o conjunto de treinamento e o conjunto de validação não é estática (como a regra do 70/15/15 pressupõe). A lei de escala prova que **conforme o tamanho total do dataset diminui, a fração de dados que deve ser destinada à validação tende a zero**. A prioridade estatística deve ser sempre garantir que a rede alcance sua capacidade de generalização máxima no treino.
        

### 4. A Prova sobre a Alta Variância (O "Efeito Loteria")

Ter um conjunto de validação muito pequeno (como 15% de uma base já enxuta) gera ruído na avaliação. O modelo parece estar sofrendo _overfitting_ ou oscilando, quando na verdade a culpa é da amostragem instável.

- **A Ilusão da Acurácia em Amostras Pequenas:**
    
    > **Vabalas, A., Gowen, E., Poliakoff, E., & Casson, A. J. (2019).** _Machine learning algorithm validation with a limited sample size._ PLOS ONE, 14(11).
    
    - **A Defesa:** Este artigo investiga o viés de validação em datasets com tamanho restrito ($N < 1000$). Eles concluem que metodologias de validação complexas em amostras pequenas resultam em estimativas de erro altamente variáveis (alta variância). Simplificar para uma divisão conservadora de Treino/Teste, associada a medidas robustas de treinamento (como _data augmentation_ dinâmica sem distorções severas), produz uma estimativa de desempenho muito mais confiável e realista para o mundo real.
        

### 5. O Requisito Obrigatório: Estratificação

Ao decidir utilizar apenas a estratégia Treino / Teste (ex: 80/20 ou 85/15), a literatura exige uma condição inegociável para validar essa escolha.

- **A Regra de Ouro do Split:**
    
    > **Kohavi, R. (1995).** _A study of cross-validation and bootstrap for accuracy estimation and model selection._ International Joint Conference on Artificial Intelligence (IJCAI).
    
    - **A Defesa:** O estudo fundamental de Ron Kohavi prova que a divisão aleatória simples (_random split_) é destrutiva para bases de dados pequenas ou desbalanceadas. A **estratificação** (forçar que a proporção de cada classe no conjunto de treino seja idêntica à proporção no conjunto de teste) é a única técnica que estabiliza a variância da métrica de avaliação quando se abandona a validação cruzada.
        

**Sugestão de Redação para a Metodologia Científica:**


> _"Diferente de grandes bancos de dados (como a ImageNet), a escassez inerente de imagens clínicas neste escopo impõe limitações estatísticas severas. O uso da divisão convencional tripla (Treinamento, Validação e Teste) foi descartado, pois, de acordo com Dobbin & Simon (2011) e a Lei de Escala de Guyon (1997), fracionar excessivamente bases restritas induz à privação de dados (data starvation), impedindo a convergência dos gradientes do modelo. Ademais, partições de validação diminutas geram alta variância nas métricas de avaliação durante o treinamento, mascarando a real capacidade de generalização da arquitetura (Vabalas et al., 2019). Portanto, optou-se por uma estratégia de divisão do tipo Hold-out consolidado (Treinamento e Teste), aplicando-se rigorosamente o particionamento estratificado recomendado por Kohavi (1995), o que garante a representatividade espacial idêntica das 4 classes tanto no ajuste dos pesos quanto na avaliação final das métricas de desempenho."_


---

## 🧬 Resposta  aos Ajustes no Código

### 1. Baseline sem Fine-Tuning (Ajustes no Modelo Base)

Como o seu modelo é uma CNN Sequencial construída do zero (utilizando camadas `Conv2D`, `MaxPooling2D`, `Flatten` e `Dense`), o conceito clássico de _fine-tuning_ (congelar/descongelar camadas de um modelo pré-treinado) não se aplica diretamente. Qualquer "ajuste" no modelo base envolverá a alteração direta da sua arquitetura e hiperparâmetros.

- **Necessidade de ajuste:** Sim, é altamente recomendável ajustar os parâmetros base. Como você está lidando com um problema de visão computacional na área médica, a arquitetura inicial pode ser "simples demais" para capturar as nuances entre diferentes tipos de lesões. Você precisará iterar sobre o número de camadas convolucionais, a quantidade de filtros e as taxas de _Dropout_ para encontrar o equilíbrio ideal entre _underfitting_ e _overfitting_.

### 2. Adição de Novas Classes (Cirúrgicas e Venosas)

A inclusão destas duas novas classes (aumentando a complexidade do problema) exigirá as seguintes modificações obrigatórias:

- **Refatoração da Arquitetura (Camada de Saída):** A mudança mais crítica é na última camada `Dense` da sua rede. O número de neurônios desta camada deve ser alterado para corresponder ao novo número total de classes (por exemplo, se antes eram 4, agora devem ser 6), mantendo a função de ativação `softmax` para classificação multiclasse. ✅
    
- **Capacidade da Rede:** Ao adicionar mais classes, a fronteira de decisão do modelo se torna mais complexa. É provável que você precise adicionar mais camadas `Conv2D` ou aumentar o número de filtros (ex: passar de 32 $\rightarrow$ 64 $\rightarrow$ 128) para garantir que a rede tenha capacidade de aprendizado suficiente para distinguir 6 categorias.
    
- **Balanceamento:** Como visto nos logs do notebook, a classe `venous` possui 247 imagens e a `sirurgical` 164. Será crucial garantir que essas classes não desbalanceiem o treinamento em relação às classes com menos imagens (como `normal` que possui 100).
    

### 3. Ajuste Fino por Tipo de Ferida (Características Visuais)

As feridas cirúrgicas e venosas possuem padrões visuais muito distintos:

- **Feridas Cirúrgicas:** Geralmente apresentam bordas retas, incisões limpas, presença de suturas (pontos) e formatos mais lineares.
    
- **Feridas Venosas:** Costumam ter bordas irregulares, leito raso, exsudato, e estão frequentemente associadas a alterações de cor na pele ao redor (dermatite ocre, hiperpigmentação).
    

Para que a rede Sequencial aprenda essas distinções, as seguintes adequações são sugeridas:

**A. Parâmetros da Rede:**

- **Tamanho do Kernel (Filtros):** Para capturar características finas e lineares (como suturas de feridas cirúrgicas), filtros menores como `(3, 3)` são eficientes. Você pode até experimentar uma camada inicial com kernel `(5, 5)` para capturar padrões mais amplos e de cor nas feridas venosas.
    

**B. Hiperparâmetros de Treinamento:**

- **Taxa de Aprendizado (Learning Rate):** Com um problema mais complexo, a rede pode ter dificuldade em convergir. O uso de _callbacks_ como o `ReduceLROnPlateau` (que já parece estar diminuindo a taxa nos seus logs, passando de `2.0000e-04` para `4.0000e-05`) é essencial para refinar os pesos nas épocas finais.
    
- **Épocas (Epochs):** O treinamento atual está configurado para 25 épocas. Com a adição de novas classes, a rede precisará de mais tempo para aprender. Considere aumentar as épocas (ex: 50 a 100) utilizando o _callback_ `EarlyStopping` para interromper o treinamento caso a rede comece a decorar os dados (_overfitting_).
    

**C. Estratégias de Data Augmentation:**

Para garantir que o modelo generalize bem as novas classes, reforce o gerador de dados (_train_generator_):

- **Rotação e Espelhamento (`rotation_range`, `horizontal_flip`, `vertical_flip`):** Extremamente úteis para feridas venosas, que não possuem uma orientação "certa" (podem ser vistas de qualquer ângulo).
    
- **Variação de Brilho e Contraste (`brightness_range`):** Importante para feridas cirúrgicas, cujas fotos muitas vezes são tiradas em ambientes hospitalares com iluminação forte (como luzes de centro cirúrgico), ajudando o modelo a não se confundir com os reflexos na pele.
    
- **Zoom (`zoom_range`):** Ajuda a focar nos detalhes internos da ferida (como o tipo de tecido ou a presença de suturas), isolando o fundo (background).


--- 

## Códigos a serem Aplicados 6 Classes Sequencial


### 1. Atualização do _Data Augmentation_ (Aumentação de Dados)

Este código ajusta o `ImageDataGenerator` para incluir variações de brilho, zoom e rotações, que são cruciais para as feridas cirúrgicas e venosas.

Python

```
from tensorflow.keras.preprocessing.image import ImageDataGenerator

# Configuração do gerador para os dados de treino com as novas estratégias
train_datagen = ImageDataGenerator(
    rescale=1./255,                 # Normalização
    rotation_range=40,              # Útil para feridas venosas (sem orientação fixa)
    width_shift_range=0.2,
    height_shift_range=0.2,
    shear_range=0.2,
    zoom_range=0.3,                 # Foco nos detalhes internos (suturas, tecidos)
    horizontal_flip=True,           # Espelhamento horizontal
    vertical_flip=True,             # Espelhamento vertical
    brightness_range=[0.8, 1.2],    # Variação de brilho (simula diferentes iluminações cirúrgicas/clínicas)
    fill_mode='nearest'
)

# Para validação e teste, aplicamos APENAS a normalização
val_datagen = ImageDataGenerator(rescale=1./255)

# Exemplo de criação dos geradores (ajuste o 'target_size' e o 'batch_size' conforme o seu código original)
train_generator = train_datagen.flow_from_directory(
    'caminho/para/diretorio/train', # Substitua pelo seu caminho de treino
    target_size=(224, 224),
    batch_size=32,
    class_mode='categorical'        # Importante para multiclasse
)

validation_generator = val_datagen.flow_from_directory(
    'caminho/para/diretorio/val',   # Substitua pelo seu caminho de validação
    target_size=(224, 224),
    batch_size=32,
    class_mode='categorical'
)
```

### 2. Refatoração da Arquitetura Sequencial (Modelo Base e Novas Classes)

Nesta etapa, adicionamos mais capacidade à rede (mais filtros) e atualizamos a camada de saída para 6 classes (assumindo que passou de 4 para 6 ao adicionar as feridas cirúrgicas e venosas).

Python

```
from tensorflow.keras.models import Sequential
from tensorflow.keras.layers import Conv2D, MaxPooling2D, Flatten, Dense, Dropout

# Definição da Arquitetura da CNN
model = Sequential([
    # 1ª Camada Convolucional: Kernel (3,3) para captar padrões finos iniciais
    Conv2D(32, (3, 3), activation='relu', input_shape=(224, 224, 3)),
    MaxPooling2D(pool_size=(2, 2)),
    
    # 2ª Camada Convolucional: Aumento progressivo de filtros
    Conv2D(64, (3, 3), activation='relu'),
    MaxPooling2D(pool_size=(2, 2)),
    
    # 3ª Camada Convolucional: Extração de características mais complexas
    Conv2D(128, (3, 3), activation='relu'),
    MaxPooling2D(pool_size=(2, 2)),
    
    # 4ª Camada Convolucional (Opcional, mas recomendada para 6 classes)
    Conv2D(128, (3, 3), activation='relu'),
    MaxPooling2D(pool_size=(2, 2)),
    
    Flatten(),
    
    # Camada Oculta Densa
    Dense(256, activation='relu'),
    Dropout(0.5),  # Dropout de 50% para mitigar o overfitting
    
    # Camada de Saída: Substituir o valor '6' pelo número exato de classes finais no seu dataset
    Dense(6, activation='softmax') # Softmax garante probabilidades para cada classe
])

model.summary()
```

### 3. Hiperparâmetros de Treino e _Callbacks_

Aqui adicionamos os controlos de _EarlyStopping_ e _ReduceLROnPlateau_ para otimizar o treino e evitar que o modelo decore os dados (overfitting), permitindo treinar por mais épocas.

Python

```
from tensorflow.keras.callbacks import EarlyStopping, ReduceLROnPlateau
from tensorflow.keras.optimizers import Adam

# Callbacks
early_stopping = EarlyStopping(
    monitor='val_loss', 
    patience=10,                 # Espera 10 épocas sem melhoria antes de parar
    restore_best_weights=True    # Restaura os pesos da melhor época
)

reduce_lr = ReduceLROnPlateau(
    monitor='val_loss', 
    factor=0.2,                  # Reduz a taxa de aprendizagem a 20% do valor atual
    patience=5,                  # Se em 5 épocas não melhorar, reduz a taxa
    min_lr=1e-6                  # Limite mínimo da taxa de aprendizagem
)

# Compilação do modelo
model.compile(
    optimizer=Adam(learning_rate=0.001), # Taxa de aprendizagem inicial
    loss='categorical_crossentropy',     # Função de perda para problemas multiclasse
    metrics=['accuracy']
)

# Treino do modelo (aumentando as épocas para permitir a convergência)
history = model.fit(
    train_generator,
    epochs=50,                           # Como temos EarlyStopping, podemos aumentar as épocas com segurança
    validation_data=validation_generator,
    callbacks=[early_stopping, reduce_lr]
)
```


---


## Códigos a serem Aplicados 6 Classes MobileNetV2 

