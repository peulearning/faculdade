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

- [ ] Entender o comportamento do modelo, para identificar as imagens. Conhecer como ele decompõe as imagens.  cada um dos modelos, quais foram os tempos de respostas, qual e mais fidedigno,  oque vai ser melhor precisão ou sensibilidade.

- [ ] Entender também como eu fiz parametrizar ou como é parametrizado o meu modelo.
### 2. Padrões de Cada Tipo de Ferida


- **'Normal' (Background):** Busca por padrões de **pele intacta, sem lesão**; ausência de exsudato, bordas, ou tecido de granulação.
    
- **'Diabete' (Úlcera Diabética):** Foco em **úlceras profundas**, frequentemente localizadas em pontos de pressão nos pés, com **bordas arredondadas ou em saca-bocado**, e risco de necrose/infecção.
    
- **'Pressure' (Úlcera de Pressão):** Foco na **localização** (proeminências ósseas) e **profundidade/estágio** (tecidos de granulação, esfacelo, necrose).
    
- **'Sirurgical' (Ferida Cirúrgica):** Busca por **incisões lineares, bordas bem aproximadas** (suturas/grampos) e sinais de cicatrização primária.
    
- **'Venous' (Úlcera Venosa):** Foco em úlceras **superficiais**, geralmente localizadas na parte inferior da perna, com **bordas irregulares** e **pele perilesional com sinais de dermatite, edema e hiperpigmentação** (hemossiderina).
    


- [ ] Guardar termos para discussão na minha abordagem, entender termo e domínio de discussão. ( Hetereogenidade , Homegeonidade )

### 3. Acurácias Acima de 95% (Preocupação com Erro)

O receio é válido, pois valores muito altos podem indicar **Overfitting** (superajuste) ou **Vazamento de Dados (Data Leakage)**, onde informações de teste "vazam" para o treinamento.

- O modelo **MobileNetV2** (Accuracy de $0.98$) está com um desempenho excepcionalmente alto.
    

**Possíveis Causas e Ações:**

1. **Overfitting (Superajuste):** O modelo decorou os dados de treino e tem dificuldade em generalizar para dados novos. Isso é comum em Redes Sequenciais Simples.
    
    - _Ação:_ **Verifique se o conjunto de teste é realmente independente** e se as métricas de validação durante o treinamento são significativamente inferiores às métricas de teste final.
        
2. **Classes Desbalanceadas:** Não parece ser o caso, pois o **Support** (suporte) é de 135 para todas as classes.
    
3. **Vazamento de Dados:** Verifique se as imagens do conjunto de teste ou validação foram, de alguma forma, incluídas no conjunto de treinamento.
    
    - _Ação:_ **Revise o código de separação de dados** (Treino/Validação/Teste) para garantir que não haja imagens duplicadas entre os conjuntos.
        



- [ ] Posteriomente ( talvez o prazo não dê ) atacar isso futuramente para ter resultados mais confiáveis.
### 4. Preparar Melhor as Imagens

Com base nos resultados, o pré-processamento/aumento de dados (Data Augmentation) pode ser melhorado, especialmente para as classes mais confusas.

- **Ação:**
    
    - **Foco no Zoom/Corte:** Garanta que a região da ferida (o que o modelo deve ver) é o principal foco, especialmente para as classes que se confundem com o _background_.
        
    - **Variação de Iluminação e Cor:** Aumente o _Data Augmentation_ para simular diferentes ambientes de coleta de imagem, tornando o modelo mais robusto.
        

### 5. Metodologia Iniciais do Teste do Modelo


| **Seções**                        | **Conteúdo**                                                                                                                                                                                                         |
| --------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Arquitetura do Modelo**         | **YOLOv3**, a **MobileNetV2** e a **CNN Sequencial**. Explique por que a MobileNetV2 é mais leve e rápida, e por que o YOLOv3 é focado em detecção/localização.                                                      |
| **Construção do Modelo**          | **Transfer Learning** (para MobileNet/ResNet50) e como você estruturou a sua **CNN Sequencial** (número de camadas, filtros, funções de ativação, otimizador, _loss function_).                                      |
| **Funcionamento e Classificação** | **características das feridas**  para explicar o que o modelo _está enxergando_. O modelo aprendeu a mapear pixels e texturas para os padrões, mas que a ambiguidade visual resulta nos erros da Matriz de Confusão. |
| **Tecnologias**                   | (Python, TensorFlow/Keras, Colab, Overleaf) e bibliotecas (OpenCV, Scikit-learn, etc.).                                                                                                                              |

### 6. Comparando os Resultados e  O que há de Comum



- **Melhor Desempenho:** **MobileNetV2** ($\text{Accuracy} = 0.98$)
    
- **Pior Desempenho:** **CNN Sequencial** ($\text{Accuracy} = 0.90$)
    
- **Ponto de Consenso:** A classe **'Diabete'** e, em menor grau, **'Sirurgical'** e **'Venous'** são consistentemente as mais difíceis de classificar, com Recall e Precision mais baixos, indicando que a distinção entre elas é o maior desafio do seu conjunto de dados.
    



### 7. Motivo de escolha dos modelos


- **CNN Sequencial (Baseline):** Utilizada como um **ponto de partida** (baseline) para provar o conceito e mostrar que arquiteturas simples podem ser eficazes, mas que podem sofrer mais com _overfitting_ ou precisar de mais dados.
    
- **MobileNetV2 (Eficiência):** Motivação na **aplicabilidade real**. É uma arquitetura leve e otimizada para dispositivos móveis ou ambientes com restrições de processamento, ideal para um futuro protótipo clínico.
    
- **YOLOv3 + ResNet50 (Transferência de Conhecimento e Precisão):** O YOLOv3 é um sistema de **detecção de objetos**, o que implica que a motivação pode ter sido a capacidade de **localizar a ferida (segmentar)** e classificá-la, usando a ResNet-50 como um _backbone_ poderoso para extrair características ricas, visando **alta precisão**.


- [ ] O que que foi idêntico dos modelos ?  
- [ ] O que há de diferente neles ? 
- [ ] Utilizando o mesmo Dataset em todos 

## 📘 Metodologia Inicial de Testes e Resultados Preliminares

Nesta etapa do desenvolvimento, a metodologia tem como foco o teste dos modelos de visão computacional. O objetivo central é avaliar a capacidade dos modelos em extrair características (features) e classificar padrões visuais presentes nas imagens de feridas, entendendo o problema como um desafio de reconhecimento de padrões dentro da área de Visão Computacional.

---

## 🖥️ 1. Ambiente de Desenvolvimento e Ferramentas

O pipeline de testes foi estruturado em Python, utilizando bibliotecas amplamente consolidadas para processamento de imagens e dados.

### Tecnologias Utilizadas

- **Python**
    
- **OpenCV**, **NumPy**, **Pandas**
    
- **TensorFlow / Keras** para construção e avaliação dos modelos
    
- **Google Colab** com aceleração GPU
    

### Dataset

- Dataset unificado a partir das bases **MedTec** e **AZH**
    
- Total de **6 classes**:
    
    - Background
        
    - Diabetic
        
    - Normal
        
    - Pressure
        
    - Sirurgical
        
    - Venous
        

### Pré-processamento

- Redimensionamento (resize)
    
- Normalização (0–1)
    
- Data Augmentation:  
    _rotação, zoom, horizontal/vertical flip_
    
- Balanceamento de Imagens  
    
	

---

## 🧠 2. Arquiteturas Avaliadas

Três modelos foram avaliados para comparar eficiência computacional e desempenho

### 1. **CNN Sequencial (Baseline)**

- Modelo customizado criado do zero
    
- Utilizado como referência para comparar com arquiteturas pré-treinadas
    

### 2. **MobileNetV2**

- Estratégia de **Transfer Learning**
    
- Pesos da ImageNet
    
- Alta eficiência computacional, especialmente para dispositivos móveis
    

### 3. **YOLOv3 com Backbone ResNet-50**

- Adaptação para classificação
    
- Combina a profundidade da ResNet-50 com a robustez do YOLO
    
- Avaliação da extração de características em redes profundas
    

---

## 📊 3. Resultados Preliminares

Os modelos foram avaliados em um conjunto **isolado de teste**, com métricas de:

- **Acurácia**
    
- **Precisão**
    
- **Recall**
    
- **F1-Score**
    

### Tabela – Desempenho dos Modelos

|Modelo|Acurácia|Precisão|Recall|F1-Score|
|---|---|---|---|---|
|**MobileNetV2**|**0.98**|**0.98**|**0.98**|**0.98**|
|YOLOv3 (ResNet-50)|0.95|0.95|0.95|0.95|
|CNN Sequencial|0.90|0.90|0.90|0.89|

---

## 🧪 4. Análise Técnica dos Resultados

### ⭐ MobileNetV2

- Melhor desempenho geral (**98% de acurácia**)
    
- Mínima taxa de erros
    
- Transfer Learning mostrou-se extremamente eficiente
    
- Extração de features com ótima generalização
    

### 🔍 YOLOv3 + ResNet-50

- Acurácia de **95%**
    
- Alta robustez
    
- Pequena confusão na classe **Diabetic**, frequentemente classificada como _Pressure_ ou _Venous_
    
- Indica proximidade visual entre essas classes
    

### 🟦 CNN Sequencial

- Desempenho mais baixo (90%) — esperado para um baseline
    
- Maior dispersão de erros
    
- Dificuldade em distinguir nuances sutis (ex: Diabetic vs Sirurgical)
    
- Confirma a necessidade de arquiteturas mais complexas
    

---

## 🧭 5. Considerações Iniciais

- **Modelos com Transfer Learning** (MobileNetV2 e ResNet-50) superam significativamente arquiteturas construídas do zero.
    
- Acurácia acima de 95% indica que os padrões visuais entre classes são **altamente discrimináveis** por CNNs.
    
- A classe **Diabetic** ainda apresenta ambiguidade, sugerindo:
    
    - refinamento no pré-processamento,
        
    - possível reequilíbrio de classes,
        
    - ou extração de features mais específicas.






