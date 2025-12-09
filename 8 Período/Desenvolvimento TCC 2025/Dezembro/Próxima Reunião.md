## 1. 🖼️ O Processo de Decomposição (Extração de Características)

O coração de qualquer CNN é a **Camada Convolucional**, que atua como um detector de _features_ (características) por meio de **filtros** (ou _kernels_).

### A. Decomposição em Níveis (Hierarquia de Características)

Os modelos não veem a imagem como um todo de uma vez, mas a decompõem em uma hierarquia de complexidade:

| **Camada da CNN**          | **O que o Modelo "Vê" (Decompõe)**                                       | **Aplicação em Imagens de Feridas**                                                                                                                                         |
| -------------------------- | ------------------------------------------------------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Camadas Iniciais**       | **Características Simples:** Bordas, linhas, curvas, gradientes de cor.  | Detecção da **borda da ferida** (forma), separação entre o tecido necrótico (cor escura) e o tecido de granulação (cor vermelha viva).                                      |
| **Camadas Intermediárias** | **Características Complexas:** Texturas, padrões, combinações de formas. | Detecção de **tecidos específicos** (esfacelo, necrose, granulação), padrões de **hiperpigmentação perilesional** (venosa), ou a **linearidade de uma sutura** (cirúrgica). |
| **Camadas Finais**         | **Características Semânticas:** Partes do objeto (ferida completa).      | Reconhecimento do **padrão de uma úlcera de pressão** em proeminência óssea ou o **formato de saca-bocado** da úlcera diabética.                                            |
### B. O Papel do _Feature Map_

A cada camada convolucional, a imagem de entrada é transformada em um **mapa de características** (_Feature Map_). Esse mapa é a representação numérica da intensidade com que um _feature_ específico (por exemplo, uma borda vertical) está presente em diferentes regiões da imagem. O modelo está, literalmente, **decompondo** a imagem em milhares de mapas de características.

## 2. 🎯 Como Cada Modelo Decompõe de Forma Diferente

Enquanto a **CNN Sequencial** é rasa e aprende _features_ mais específicas do seu _dataset_ local, as arquiteturas pré-treinadas (MobileNetV2 e ResNet50) usam decomposições mais robustas.

## 🧠 Como os Modelos Decompõem e Classificam as Imagens de Feridas

Seus modelos (CNN Sequencial, MobileNetV2 e YOLOv3+ResNet50) são Redes Neurais Convolucionais (CNNs). Eles decompõem e classificam as imagens de feridas em um processo de múltiplas etapas, que vai da extração de características visuais simples até a decisão final da classe.

---

## 1. 🖼️ O Processo de Decomposição (Extração de Características)

O coração de qualquer CNN é a **Camada Convolucional**, que atua como um detector de _features_ (características) por meio de **filtros** (ou _kernels_).

### A. Decomposição em Níveis (Hierarquia de Características)

Os modelos não veem a imagem como um todo de uma vez, mas a decompõem em uma hierarquia de complexidade:

|**Camada da CNN**|**O que o Modelo "Vê" (Decompõe)**|**Aplicação em Imagens de Feridas**|
|---|---|---|
|**Camadas Iniciais**|**Características Simples:** Bordas, linhas, curvas, gradientes de cor.|Detecção da **borda da ferida** (forma), separação entre o tecido necrótico (cor escura) e o tecido de granulação (cor vermelha viva).|
|**Camadas Intermediárias**|**Características Complexas:** Texturas, padrões, combinações de formas.|Detecção de **tecidos específicos** (esfacelo, necrose, granulação), padrões de **hiperpigmentação perilesional** (venosa), ou a **linearidade de uma sutura** (cirúrgica).|
|**Camadas Finais**|**Características Semânticas:** Partes do objeto (ferida completa).|Reconhecimento do **padrão de uma úlcera de pressão** em proeminência óssea ou o **formato de saca-bocado** da úlcera diabética.|

### B. O Papel do _Feature Map_

A cada camada convolucional, a imagem de entrada é transformada em um **mapa de características** (_Feature Map_). Esse mapa é a representação numérica da intensidade com que um _feature_ específico (por exemplo, uma borda vertical) está presente em diferentes regiões da imagem. O modelo está, literalmente, **decompondo** a imagem em milhares de mapas de características.

---

## 2. 🎯 Como Cada Modelo Decompõe de Forma Diferente

Enquanto a **CNN Sequencial** é rasa e aprende _features_ mais específicas do seu _dataset_ local, as arquiteturas pré-treinadas (MobileNetV2 e ResNet50) usam decomposições mais robustas.

### A. MobileNetV2 (Eficiência no Detalhamento)

- **Técnica Chave:** **Convoluções Separáveis em Profundidade** (_Depthwise Separable Convolutions_).
    
- **Decomposição:** O MobileNetV2 decompõe a imagem em **duas etapas** separadas:
    
    1. **Depthwise:** Aplica filtros em **cada canal de cor individualmente** (separando a decomposição de forma e cor).
        
    2. **Pointwise (1x1):** Combina os resultados dos canais, otimizando a combinação das _features_.
        
- **Vantagem:** Essa decomposição é muito mais **eficiente** e **rápida** do que a convolução padrão, mantendo alta precisão. É por isso que obteve $0.98$ de acurácia, extraindo _features_ complexas com o mínimo de processamento.

### B. YOLOv3 + ResNet50 (Profundidade e Robustez)

- **Técnica Chave:** **Conexões Residuais** (ResNet-50) e **Detecção em Grades** (YOLOv3).
    
- **Decomposição (ResNet-50):** A ResNet-50 é uma rede **muito profunda** que usa **conexões _bypass_** (_skip connections_ ou _residual blocks_). Isso permite que ela continue a se aprofundar na decomposição (extração de _features_) sem que o aprendizado se degrade. Ela é mestre em extrair _features_ de **texturas e padrões muito sutis**.
    
- **YOLOv3:** Embora você a tenha usado para classificação, o YOLOv3 é, fundamentalmente, um modelo de **Detecção**. Isso significa que ele decompõe a imagem e a divide em **células de grade**, tentando identificar se a ferida está presente em cada célula. Essa metodologia de decomposição é robusta e ajuda a focar na **região de interesse**, o que explica a alta acurácia ($0.95$).

## 3. ⚖️ O Processo de Classificação (Decisão)

Após a decomposição, os modelos passam para a fase de tomada de decisão.

### A. Achatamento (_Flatten_) e Camadas Densa (_Dense_)

Os **Mapas de Características** finais (a decomposição da imagem) são **achatados** (_flattened_) em um único vetor longo. Este vetor, que contém as _features_ mais importantes da ferida, é alimentado nas **Camadas Densa** (ou _Fully Connected_).

### B. Geração de Probabilidades (Softmax)

A última camada Densa do seu modelo tem 6 neurônios (um para cada classe de ferida: Diabetic, Pressure, Venous, etc.).

- A função de ativação **Softmax** é aplicada a esta camada, que transforma as pontuações brutas dos neurônios em **probabilidades**, onde a soma de todas as probabilidades é igual a $1.0$ (ou 100%).
    
- **Exemplo de Decisão:**
    
    - Diabetic: $0.01$
        
    - Pressure: $0.05$
        
    - Sirurgical: $0.02$
        
    - Venous: $0.85$
        
    - Normal: $0.03$
        
    - Background: $0.04$
        
    - **Classificação Final:** O modelo escolhe a classe com a maior probabilidade, neste caso, **'Venous'** ($0.85$).
        

### C. Por que 'Diabete' Apresenta Erros (Ambiguidade)

Os erros na classe 'Diabete' ocorrem porque as _features_ extraídas (textura, profundidade, bordas) são muito próximas das _features_ de 'Pressure' e 'Venous'.

Se uma imagem de úlcera diabética (Real: Diabete) tiver _features_ que se parecem muito com um padrão aprendido de úlcera venosa, o neurônio de 'Venous' pode ter uma probabilidade ligeiramente maior (ex: $0.45$) do que o de 'Diabete' ($0.40$), resultando em um erro de classificação (Falso Negativo para Diabete e Falso Positivo para Venous).


### ⚠️ Parametrização do Modelo

- **MobileNetV2 e YOLOv3 + ResNet50:** Utilizaram o conceito de **Transfer Learning**. Isso significa que a maior parte dos parâmetros (pesos) foi **inicialmente parametrizada** com o conhecimento adquirido na base de dados **ImageNet** (visão geral de objetos) e depois **ajustada** (fine-tuning) para as suas imagens de feridas.
    
- **CNN Sequencial:** Foi parametrizada a partir do zero (randomização), aprendendo todos os seus pesos apenas com o seu _dataset_ de feridas.
    
- **Comum a todos (Configurações da Camada de Saída):** O parâmetro mais importante que você ajustou para a sua tarefa é a **camada de saída** (classificação). Ela foi parametrizada com **6 unidades** (uma para cada classe) e, provavelmente, a função de ativação **Softmax** para gerar a probabilidade de cada classe.

- **Qual é o mais fidedigno?**
    
    - O **MobileNetV2** é o mais fidedigno em termos de métricas ($0.98$), indicando a melhor capacidade de generalização e menor taxa de erro no seu conjunto de testes.
        
- **O que será melhor: Precisão ou Sensibilidade (Recall)?**
    
    - Em diagnóstico médico (como classificação de feridas), geralmente o **Recall (Sensibilidade)** é preferível, especialmente para as classes de doenças (Diabete, Pressure, Venous). Um alto Recall significa que o modelo **não deixa de detectar** um caso positivo (baixo Falso Negativo).
        
    - No seu caso, como o Recall está muito alto em todas as classes no MobileNetV2 ($0.98$), o modelo consegue ser **preciso e sensível**. Se as classes estivessem mais desbalanceadas ou os erros fossem mais críticos, seria necessário um ajuste que priorizasse o Recall.
    
#### 🎯 Idêntico (O que foi mantido igual)

- **Dataset:** Todos os modelos foram treinados e testados utilizando o **mesmo Dataset unificado** (MedTec + AZH).
    
- **Classes de Saída:** Todos classificam nas **6 mesmas classes** (Background, Diabetic, Normal, Pressure, Sirurgical, Venous).
    
- **Pré-processamento Básico:** Compartilharam as mesmas etapas de **Redimensionamento, Normalização (0-1)** e a mesma estratégia de **Data Augmentation**.
    
- **Função de Perda (_Loss Function_):** Embora não detalhado, é altamente provável que todos tenham usado a mesma função de perda para a classificação multi-classe, como **Categorical Cross-Entropy**.
### ⚔️ Atacar Futuramente o Risco de Overfitting / Vazamento de Dados

Você está correto em identificar que acurácias acima de 95% levantam uma bandeira vermelha 🚩. Para atacar o problema futuramente e ter resultados mais confiáveis, as seguintes ações são essenciais:

1. **Validação Cruzada (Cross-Validation):** Em vez de uma única divisão Treino/Teste, use a K-Fold Cross-Validation. Isso garante que cada imagem tenha a chance de estar no conjunto de teste, dando uma estimativa mais robusta do desempenho.
    
2. **Análise de Desempenho no Conjunto de Validação:**
    
    - **Ação:** Plote a curva de **Loss/Acurácia** do conjunto de **Treinamento** versus o conjunto de **Validação** ao longo das épocas.
        
    - **Resultado Esperado:** Se a **Loss de Treinamento continuar a cair**, mas a **Loss de Validação começar a subir** em um determinado ponto, isso é a **evidência clássica de Overfitting** (superajuste).
        
3. **Inspeção de Imagens Confusas:** Use técnicas de **Interpretabilidade** como **Grad-CAM** (Gradient-weighted Class Activation Mapping) para visualizar **quais pixels** o modelo MobileNetV2 está realmente usando para tomar decisões. Se o mapa de calor mostrar que ele está focando em _pixels de background_ ou _marcas d'água_ específicas, isso pode indicar que o modelo está aprendendo um padrão acidental (vazamento de dados ou viés de coleta) em vez da própria ferida.

### 🥊 Guardar Termos para Discussão (Heterogeneidade vs. Homogeneidade)

Estes termos são excelentes para a discussão da natureza dos seus dados e dos erros.

- **Heterogeneidade:** Refere-se à **diversidade visual** dentro de uma mesma classe ou entre classes que causam confusão.
    
    - **Discussão:** A confusão do modelo na classe **'Diabete'** com **'Pressure'** e **'Venous'** pode ser atribuída à **alta heterogeneidade visual** dessas úlceras. Uma úlcera diabética pode parecer muito diferente dependendo da infecção, estágio e localização, o que a torna visualmente ambígua para o modelo. A heterogeneidade é o que a sua etapa de **Data Augmentation** tenta mitigar (tornar o modelo robusto a variações).
        
- **Homogeneidade:** Refere-se à **similaridade visual** das imagens.
    
    - **Discussão:** A alta acurácia do MobileNetV2 ($0.98$) sugere que, apesar das ambiguidades, as classes de feridas possuem **padrões visuais altamente homogêneos e discrimináveis** (características únicas) que a CNN conseguiu extrair com sucesso. O 'Normal' (background) e o 'Sirurgical' (incisão linear) provavelmente têm alta homogeneidade dentro de si, facilitando a classificação.