## 📊 Análise das Métricas dos Modelos

Abaixo estão as métricas consolidadas de **Acurácia (Accuracy)**, **Precisão (Precision)**, **Recall** e **F1-Score** (Weighted Average) para cada modelo, obtidas das imagens da Matriz de Confusão e do Relatório de Classificação.

|**Modelo**|**Accuracy**|**Precision (WA)**|**Recall (WA)**|**F1-Score (WA)**|
|---|---|---|---|---|
|**MobileNetV2**|$\mathbf{0.98}$|$\mathbf{0.98}$|$\mathbf{0.98}$|$\mathbf{0.98}$|
|**YOLOv3 + ResNet50**|$0.95$|$0.95$|$0.95$|$0.95$|
|**CNN Sequencial**|$0.90$|$0.90$|$0.90$|$0.89$|

_(WA = Weighted Average, Média Ponderada)_

---

## 🧐 Respostas aos Questionamentos


### 1. A Classe 'Diabete' com Mais Problemas (Erros)

A classe **'Diabete'** realmente é uma das que apresenta mais erros de classificação em todos os modelos, mas principalmente na **CNN Sequencial** e **YOLOv3 + ResNet50**.

- **CNN Sequencial:**
    
    - **Recall (Diabete):** $0.92$ (124 corretos de 135)
        
    - **Principais Erros:** Errou 8 como 'pressure', 4 como 'sirurgical' e 4 como 'venous'.
        
- **YOLOv3 + ResNet50:**
    
    - **Recall (Diabete):** $0.94$ (127 corretos de 135)
        
    - **Principais Erros:** Errou 3 como 'pressure', 1 como 'sirurgical' e 3 como 'venous'.
        
- **MobileNetV2:**
    
    - **Recall (Diabete):** $0.98$ (132 corretos de 135)
        
    - **Principais Erros:** Errou 1 como 'pressure' e 2 como 'venous'.
        


feridas diabéticas podem ser visualmente ambíguas, apresentando características que se assemelham a estágios de pressão (úlceras de pressão) ou a feridas cirúrgicas/venosas, dependendo do estágio, infecção, e presença de tecido necrótico. O modelo pode estar se confundindo com características como profundidade, presença de bordas irregulares ou exsudato, que são comuns em diferentes tipos de úlceras.

### 2. Padrões de Cada Tipo de Ferida


- **'Normal' (Background):** Busca por padrões de **pele intacta, sem lesão**; ausência de exsudato, bordas, ou tecido de granulação.
    
- **'Diabete' (Úlcera Diabética):** Foco em **úlceras profundas**, frequentemente localizadas em pontos de pressão nos pés, com **bordas arredondadas ou em saca-bocado**, e risco de necrose/infecção.
    
- **'Pressure' (Úlcera de Pressão):** Foco na **localização** (proeminências ósseas) e **profundidade/estágio** (tecidos de granulação, esfacelo, necrose).
    
- **'Sirurgical' (Ferida Cirúrgica):** Busca por **incisões lineares, bordas bem aproximadas** (suturas/grampos) e sinais de cicatrização primária.
    
- **'Venous' (Úlcera Venosa):** Foco em úlceras **superficiais**, geralmente localizadas na parte inferior da perna, com **bordas irregulares** e **pele perilesional com sinais de dermatite, edema e hiperpigmentação** (hemossiderina).
    


Metodologia/Discussão para descrever esses padrões e, em seguida, discutir se o desempenho do modelo reflete essa distinção. Use a confusão entre 'Diabete' e 'Pressure'/'Venous' como evidência de que a distinção visual nem sempre é clara.

### 3. Acurácias Acima de 95% (Preocupação com Erro)

O receio é válido, pois valores muito altos podem indicar **Overfitting** (superajuste) ou **Vazamento de Dados (Data Leakage)**, onde informações de teste "vazam" para o treinamento.

- O modelo **MobileNetV2** (Accuracy de $0.98$) está com um desempenho excepcionalmente alto.
    

**Possíveis Causas e Ações:**

1. **Overfitting (Superajuste):** O modelo decorou os dados de treino e tem dificuldade em generalizar para dados novos. Isso é comum em Redes Sequenciais Simples.
    
    - _Ação:_ **Verifique se o conjunto de teste é realmente independente** e se as métricas de validação durante o treinamento são significativamente inferiores às métricas de teste final.
        
2. **Classes Desbalanceadas:** Não parece ser o caso, pois o **Support** (suporte) é de 135 para todas as classes.
    
3. **Vazamento de Dados:** Verifique se as imagens do conjunto de teste ou validação foram, de alguma forma, incluídas no conjunto de treinamento.
    
    - _Ação:_ **Revise o código de separação de dados** (Treino/Validação/Teste) para garantir que não haja imagens duplicadas entre os conjuntos.
        


Mencione a alta acurácia como um ponto forte, mas também como um ponto de cautela. Na Metodologia, afirme que foram tomadas as devidas precauções (como separação rigorosa de conjuntos de dados e técnicas de Regularização/Dropout) para mitigar o overfitting.

### 4. Preparar Melhor as Imagens

Com base nos resultados, o pré-processamento/aumento de dados (Data Augmentation) pode ser melhorado, especialmente para as classes mais confusas.

- **Ação:**
    
    - **Foco no Zoom/Corte:** Garanta que a região da ferida (o que o modelo deve ver) é o principal foco, especialmente para as classes que se confundem com o _background_.
        
    - **Variação de Iluminação e Cor:** Aumente o _Data Augmentation_ para simular diferentes ambientes de coleta de imagem, tornando o modelo mais robusto.
        

### 5. Metodologia Iniciais do Teste do Modelo

Esta seção é o cerne do seu TCC. Ela deve ser descritiva e justificar suas escolhas.

|**Seção do TCC**|**Conteúdo**|
|---|---|
|**Arquitetura do Modelo**|Descreva o **YOLOv3**, a **MobileNetV2** e a **CNN Sequencial**. Explique por que a MobileNetV2 é mais leve e rápida, e por que o YOLOv3 é focado em detecção/localização.|
|**Construção do Modelo**|Detalhe o uso de **Transfer Learning** (para MobileNet/ResNet50) e como você estruturou a sua **CNN Sequencial** (número de camadas, filtros, funções de ativação, otimizador, _loss function_).|
|**Funcionamento e Classificação**|Use as **características das feridas** (ponto 2) para explicar o que o modelo _está enxergando_. Afirme que o modelo aprendeu a mapear pixels e texturas para os padrões que você descreveu, mas que a ambiguidade visual resulta nos erros da Matriz de Confusão.|
|**Tecnologias**|Liste as tecnologias (Python, TensorFlow/Keras, Colab, Overleaf) e bibliotecas (OpenCV, Scikit-learn, etc.).|

### 6. Comparar os Resultados e 8. O que há de Comum

A comparação deve ser feita na seção de **Resultados e Discussão**, usando a tabela que montei no início.

- **Melhor Desempenho:** **MobileNetV2** ($\text{Accuracy} = 0.98$)
    
- **Pior Desempenho:** **CNN Sequencial** ($\text{Accuracy} = 0.90$)
    
- **Ponto de Consenso:** A classe **'Diabete'** e, em menor grau, **'Sirurgical'** e **'Venous'** são consistentemente as mais difíceis de classificar, com Recall e Precision mais baixos, indicando que a distinção entre elas é o maior desafio do seu conjunto de dados.
    

Ação para a Escrita:

Use as métricas (Precision, Recall, F1-Score) para quantificar o desempenho e a Matriz de Confusão para visualizar e interpretar os erros.

### 7. O que motivou a utilizar os modelos

Justifique a escolha de cada arquitetura:

- **CNN Sequencial (Baseline):** Utilizada como um **ponto de partida** (baseline) para provar o conceito e mostrar que arquiteturas simples podem ser eficazes, mas que podem sofrer mais com _overfitting_ ou precisar de mais dados.
    
- **MobileNetV2 (Eficiência):** Motivação na **aplicabilidade real**. É uma arquitetura leve e otimizada para dispositivos móveis ou ambientes com restrições de processamento, ideal para um futuro protótipo clínico.
    
- **YOLOv3 + ResNet50 (Transferência de Conhecimento e Precisão):** O YOLOv3 é um sistema de **detecção de objetos**, o que implica que a motivação pode ter sido a capacidade de **localizar a ferida (segmentar)** e classificá-la, usando a ResNet-50 como um _backbone_ poderoso para extrair características ricas, visando **alta precisão**.