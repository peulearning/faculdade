
### Estratégias Comuns e Pontos Fortes

Ambos os notebooks compartilham de uma fundação sólida e práticas avançadas de engenharia e aprendizado de máquina para a MobileNetV2:

- **Data Augmentation Dinâmica e Otimizada:** A transição da geração de imagens estáticas (trecho de código comentado) para a augmentation realizada exclusivamente em tempo de treinamento via `ImageDataGenerator` é uma excelente prática. A decisão de remover as variações de brilho (`brightness_range`) dessa pipeline é uma otimização cirúrgica, pois a constância na iluminação ajuda a rede a focar nas texturas e bordas reais dos tecidos, o que consolida o ganho de 2% no desempenho do modelo observado com essa configuração.
    
- **Arquitetura "Dual Pooling":** A criação de uma cabeça de classificação que concatena o `GlobalAveragePooling2D` (para capturar o contexto geral da imagem) com o `GlobalMaxPooling2D` (excelente para extrair picos de textura aguda, como áreas de necrose ou esfacelo) duplica a riqueza de informações enviadas às camadas densas.
    
- **Mitigação do Desbalanceamento:** O uso integrado do cálculo de `class_weight='balanced'` com a função de perda `CategoricalFocalCrossentropy` (com gamma=2.0) é uma tática de estado da arte para forçar a rede a penalizar erros nas classes mais difíceis e minoritárias do dataset.

### Comparativo dos Resultados: Fine-Tuning vs. Extração de Características

O ponto de divergência entre os arquivos está na Fase 2 do treinamento.

**1. Modelo com Fine-Tuning**

- **Estratégia:** Após treinar a nova cabeça de classificação por 15 épocas, as últimas 20 camadas da MobileNetV2 foram descongeladas e treinadas com uma taxa de aprendizado menor (Adam com `1e-5`) por mais 25 épocas.
    
- **Resultados de Teste:** O modelo obteve uma **acurácia de 70.23%** e uma perda (loss) de **0.8620**.
    
- **Análise das Classes:** A precisão caiu consideravelmente em úlceras de pressão ("pressure") com 0.50 e feridas cirúrgicas ("sirurgical") com 0.56.
    

**2. Modelo Sem Fine-Tuning (Feature Extractor)**

- **Estratégia:** A base da MobileNetV2 permaneceu 100% congelada. A cabeça de classificação foi treinada com limite de 40 épocas, mas o `EarlyStopping` encerrou o processo na época 23 para evitar sobreajuste, restaurando os melhores pesos.
    
- **Resultados de Teste:** O modelo atingiu uma **acurácia de 72.52%** e uma perda drasticamente menor de **0.1227**.
    
- **Análise das Classes:** Embora a classe "pressure" ainda seja um gargalo (precisão de 0.52), houve uma melhoria notável na classe cirúrgica ("sirurgical"), que saltou para 0.71 de precisão. A classe "normal" atingiu 0.94 de precisão e 1.00 de recall.

### Veredito da Análise

Surpreendentemente, **a estratégia Sem Fine-Tuning se saiu melhor.** Em problemas de visão computacional médica com datasets pequenos e complexos, descongelar as camadas de uma rede previamente treinada na _ImageNet_ pode destruir representações de características genéricas altamente valiosas (como a detecção de bordas e gradientes de cor) através do _catastrophic forgetting_. O aumento na função de perda (de 0.12 no modelo congelado para 0.86 no modelo com fine-tuning) indica que a rede que sofreu fine-tuning começou a decorar os dados de treino (overfitting), perdendo a capacidade de generalização nas imagens de teste inéditas.

**Recomendação:** Para o contexto de um aplicativo mobile embarcado focado em precisão e baixo tempo de inferência, o modelo **Sem Fine-Tuning** é a escolha ideal atual. Ele consome muito menos recurso computacional para treinar, apresenta uma _Loss_ incrivelmente estável (0.12) e garante a melhor acurácia (72.5%) com a generalização mantida.



--- 

Manter a pipeline de dados, a mesma separação de classes, o pré-processamento (com a remoção estratégica do brilho na augmentation) e a mesmíssima estrutura de cabeça de classificação (`Concatenate` entre `GlobalAveragePooling2D` e `GlobalMaxPooling2D`, seguido pelas camadas densas e dropout) é a única maneira correta de realizar um estudo comparativo justo. Na literatura científica, isso é conhecido como um **estudo controlado** ou **análise de ablação parcial**, onde isola-se apenas a variável de interesse: o congelamento ou descongelamento dos pesos da base.

Para enriquecer a escrita acadêmica e a discussão dos resultados para o seu Trabalho de Conclusão de Curso (TCC), existem alguns pontos cruciais que vale a pena destacar e aprofundar:

### 1. Justificativa Teórica do Fenômeno (Esquecimento Catastrófico)

Você deve explicar o motivo técnico pelo qual o modelo sem Fine-Tuning superou o modelo com Fine-Tuning.

- A MobileNetV2 foi extensivamente treinada na _ImageNet_, aprendendo extratores de características (filtros de bordas, texturas, formas geométricas e gradientes) extremamente genéricos e robustos.
    
- Quando as últimas 20 camadas foram descongeladas, mesmo utilizando uma taxa de aprendizado baixa (`1e-5`), o modelo sofreu de **Esquecimento Catastrófico** (_Catastrophic Forgetting_). Em datasets médicos e de feridas, que costumam ser menores e altamente complexos, a rede tendeu a ajustar excessivamente os filtros extratores de características globais para se adequar estritamente aos ruídos ou padrões muito específicos do conjunto de treino (como o fundo da imagem ou a iluminação específica daquela amostra), destruindo o poder de generalização que ela já possuía.
    

### 2. O Impacto Arquitetural da Fusão GAP + GMP

A escolha de concatenar o `GlobalAveragePooling2D` (GAP) com o `GlobalMaxPooling2D` (GMP) merece um parágrafo de destaque na sua metodologia:

- **GAP:** Atua como um regularizador estrutural, extraindo a informação global e o contexto espacial da lesão, reduzindo a propensão ao overfitting.
    
- **GMP:** Funciona como um detector de saliências. Em imagens dermatológicas ou clínicas, elementos críticos (como pontos de necrose, fibrina ou linhas finas de uma incisão cirúrgica) ocupam poucos pixels na imagem. O GMP garante que esses picos de ativação de alta frequência não sejam diluídos pela média do GAP.
    
- Demonstrar que essa cabeça híbrida manteve o modelo congelado estável com uma perda em teste extremamente baixa (**0.1227**) prova que a extração de características cruas da MobileNetV2 já era linearmente separável o suficiente para o problema se combinada com essa estratégia de pooling duplo.
    

### 3. Dinâmica das Curvas de Aprendizado e Early Stopping

Um ponto excelente para destacar na análise comparativa é o comportamento do treinamento:

- No modelo **Sem Fine-Tuning**, o `EarlyStopping` agiu de forma cirúrgica por volta da época 23. Isso mostra que a cabeça densa convergiu rapidamente e o algoritmo interrompeu o processo antes que a distância entre a curva de validação e de treino começasse a divergir de forma prejudicial.
    
- No modelo **Com Fine-Tuning**, o processo continuou por mais épocas após o descongelamento. A discrepância gritante entre a acurácia de treino e a acurácia de teste, somada à perda inflada de **0.8620** em teste, é a assinatura clássica de um _overfitting de representação_, onde os pesos internos das camadas convolucionais foram corrompidos.
    

### 4. Análise de Desempenho por Classe (Granularidade)

Não se limite a olhar apenas para a acurácia global. Para a banca do seu TCC, a análise das classes específicas trará o verdadeiro valor científico:

- **A Classe Cirúrgica (_Sirurgical_):** O salto de 0.56 para 0.71 de precisão no modelo sem fine-tuning indica que os filtros nativos da MobileNetV2 são excelentes para reconhecer formas geométricas lineares e padrões texturais de suturas e cortes limpos. O fine-tuning prejudicou essa capacidade.
    
- **O Gargalo da Classe de Úlceras por Pressão (_Pressure_):** Ambas as abordagens patinaram nessa classe (precisões em torno de 0.50 e 0.52). Isso evidencia que o problema aqui não é a estratégia de transfer learning em si, mas sim a similaridade visual intrínseca (alta variação intraclasse ou baixa variação interclasse) entre úlceras por pressão em estágios iniciais e outras lesões vasculares ou ulcerativas do dataset. É um excelente gancho para propor trabalhos futuros (como o uso de mecanismos de atenção ou segmentação prévia da região de interesse).

---


### 📊 Tabela 1: Comparativo por Classe

A tabela abaixo compara a **Precisão (P)**, **Recall (R)** e **F1-Score (F1)** para cada uma das 6 classes.

| **Classe**     | **Sem Fine-Tuning(Prec / Rec / F1)** | **Com Fine-Tuning(Prec / Rec / F1)** | **Impacto no F1-Score** |
| -------------- | ------------------------------------ | ------------------------------------ | ----------------------- |
| **background** | 0.57 / 1.00 / 0.73                   | 1.00 / 1.00 / 1.00                   | 📈 +0.27 (Perfeito)     |
| **diabetic**   | 0.72 / 0.75 / 0.74                   | 0.73 / 0.79 / 0.76                   | 📈 +0.02 (Melhorou)     |
| **normal**     | 0.94 / 1.00 / 0.97                   | 0.75 / 1.00 / 0.86                   | 📉 -0.11 (Piorou)       |
| **pressure**   | 0.52 / 0.52 / 0.52                   | 0.50 / 0.43 / 0.46                   | 📉 -0.06 (Piorou)       |
| **sirurgical** | 0.71 / 0.48 / 0.57                   | 0.56 / 0.40 / 0.47                   | 📉 -0.10 (Piorou)       |
| **venous**     | 0.78 / 0.84 / 0.81                   | 0.78 / 0.84 / 0.81                   | ➖ Estável               |

### 🌍 Tabela 2: Métricas Globais

Aqui comparamos o desempenho geral do modelo nos dois experimentos.

| **Métrica Global**              | **Sem Fine-Tuning** | **Com Fine-Tuning** | **Diferença** |
| ------------------------------- | ------------------- | ------------------- | ------------- |
| **Acurácia Geral**              | **0.73 (73%)**      | 0.70 (70%)          | 📉 -3%        |
| **Macro Avg** _(P / R / F1)_    | 0.71 / 0.77 / 0.72  | 0.72 / 0.74 / 0.73  | 📈 +0.01 (F1) |
| **Weighted Avg** _(P / R / F1)_ | 0.72 / 0.73 / 0.72  | 0.69 / 0.70 / 0.69  | 📉 -0.03 (F1) |

### 1. O problema dos Falsos Positivos na classe "Normal"

Com o fine-tuning, a classe `normal` sofreu um efeito colateral grave. O _Recall_ continuou em 1.00 (ou seja, ele encontra todos os casos normais reais), mas a _Precisão_ despencou de 0.94 para 0.75.

- **O que isso significa na prática:** O modelo afinado começou a chutar "normal" para coisas que não são. Ele está gerando muitos Falsos Positivos. Em um cenário médico, isso é perigoso, pois o modelo estaria dizendo que uma pessoa com uma ferida/condição está saudável.
    

### 2. As classes "Pressure" e "Sirurgical" são o calcanhar de Aquiles

Mesmo antes do fine-tuning, o modelo já tinha dificuldade com `pressure` e `sirurgical`. Com o fine-tuning, piorou. O _Recall_ dessas classes ficou em 0.43 e 0.40, respectivamente.

- **O que isso significa na prática:** De cada 10 imagens de lesões cirúrgicas, o modelo está errando 6. Ele simplesmente não aprendeu a identificar os padrões visuais dessas duas classes. É provável que visualmente elas se confundam muito entre si ou com as lesões diabéticas/venosas.
    

### 3. A ilusão da classe "Background"

O relatório com fine-tuning mostra cravados 1.00 (100%) em Precisão e Recall para `background`. Parece excelente, mas repare na coluna _Support_: **existem apenas 4 amostras dessa classe**.

- **O que isso significa na prática:** É fácil tirar nota 100 em uma prova com apenas 4 questões. Essa métrica perfeita não tem validade estatística confiável. Não podemos afirmar que o modelo é bom em "background" com base em apenas 4 imagens de validação.
    

### 4. O peso do desbalanceamento de dados

Olhe para a coluna _Support_ geral: você tem 38 casos `venous` e 28 `diabetic`, mas apenas 15 `normal` e 4 `background`.

- **O que isso significa na prática:** Seu conjunto de dados está desbalanceado. As redes neurais são "preguiçosas"; elas tendem a focar em aprender as classes que aparecem mais vezes (as majoritárias) para diminuir o erro geral. Note que a classe `venous`, que é a maior de todas, foi a única que se manteve totalmente inabalada e estável nos dois cenários (F1-Score de 0.81).
    

Resumindo: o fine-tuning confundiu o modelo, fazendo-o errar mais nas lesões difíceis e começar a classificar feridas como "normais". O modelo precisa de ajuda para entender as diferenças sutis, principalmente das lesões por pressão e cirúrgicas.