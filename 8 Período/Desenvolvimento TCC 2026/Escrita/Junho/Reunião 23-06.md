
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

![[Pasted image 20260624181913.png]]



![[Pasted image 20260624181900.png]]



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


