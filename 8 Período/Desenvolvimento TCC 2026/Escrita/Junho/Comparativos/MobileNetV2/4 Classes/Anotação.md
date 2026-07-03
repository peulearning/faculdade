[Stress_RemoveFineTunning_4Class_Mobilenet - Colab](https://colab.research.google.com/drive/1OtR9JkRj2rOq4TWRVsG56mLtwSO1ebzI#scrollTo=CuhRF5_hGX4M)

[Stress_FineTunning_4Class_Mobilenet - Colab](https://colab.research.google.com/drive/1YSE-AUoYNJNY4_lEy_BchhXvQh5SRh_3#scrollTo=yqNWmkf9H0mK)


### 1. Estratégias Aplicadas

Ambos os notebooks utilizam a arquitetura **MobileNetV2**, que é uma rede neural leve e eficiente, ideal para dispositivos móveis e visão computacional. A diferença fundamental entre eles reside na forma como a rede pré-treinada foi adaptada para o seu conjunto de dados de feridas:

- **Com Fine-Tuning (`Final_Stress_FineTunning_4Class_Mobilenet.ipynb`)**: Nesta abordagem, além de treinar a nova camada de classificação (cabeça da rede), algumas ou todas as camadas mais profundas do modelo base do MobileNetV2 são "descongeladas". Isso permite que os pesos da rede sejam ajustados especificamente para aprender características detalhadas das feridas.
    
- **Sem Fine-Tuning / Extração de Características (`Final_Stress_RemoveFineTunning_4Class_Mobilenet.ipynb`)**: Nesta estratégia, a base convolucional do MobileNetV2 permanece "congelada". Os pesos pré-treinados (geralmente na base ImageNet) são mantidos intactos, e a rede atua apenas como um extrator de características. Apenas a camada final densa de classificação é treinada para separar as classes de feridas.
    

### 2. Comparativo Geral das Métricas

De acordo com os relatórios de classificação gerados em seus conjuntos de testes (composto por 68 imagens), o modelo **sem Fine-Tuning apresentou um desempenho superior**.

|**Métrica Global**|**Com Fine-Tuning**|**Sem Fine-Tuning**|
|---|---|---|
|**Acurácia (Accuracy)**|79% (0.79)|84% (0.84)|
|**Precisão (Weighted Avg)**|80% (0.80)|84% (0.84)|
|**Recall (Weighted Avg)**|79% (0.79)|84% (0.84)|
|**F1-Score (Weighted Avg)**|79% (0.79)|84% (0.84)|
|**Quantidade de Erros**|14 erros|11 erros|

### 3. Análise Detalhada por Classes

Ao observar o comportamento do modelo em cada categoria específica de ferida, notamos onde a diferença de desempenho realmente ocorreu:

- **Classe `diabetic` (28 amostras):**
    
    - **Com Fine-Tuning**: Precisão de 0.71 e Recall de 0.86 (F1-score de 0.77). O modelo encontra a maioria dos casos diabéticos, mas tem muitos falsos positivos.
        
    - **Sem Fine-Tuning**: Precisão de 0.81 e Recall de 0.79 (F1-score de 0.80). O modelo se tornou mais equilibrado e preciso em suas predições para esta classe.
        
- **Classe `pressure` (21 amostras):**
    
    - **Com Fine-Tuning**: Precisão de 0.79 e Recall de 0.52 (F1-score de 0.63). _Este é o calcanhar de Aquiles do modelo_. O recall muito baixo indica que o modelo deixou de identificar 48% das feridas de pressão, confundindo-as com outras classes.
        
    - **Sem Fine-Tuning**: Precisão de 0.76 e Recall de 0.76 (F1-score de 0.76). Ocorreu uma **melhoria drástica**. O modelo conseguiu aprender a identificar a classe de pressão de maneira muito mais consistente, sem perder tanta informação.
        
- **Classes `normal` (15 amostras) e `background` (4 amostras):**
    
    - Ambos os modelos tiveram desempenhos quase perfeitos (F1-Score de 0.97 para normal e 1.00 para background), indicando que estas são classes visualmente fáceis de separar para o MobileNetV2.
        

### 4. Conclusões e Recomendações

Embora seja contraintuitivo, pois o _Fine-Tuning_ geralmente melhora resultados, neste cenário **o modelo sem Fine-Tuning generalizou melhor**.

**Por que isso aconteceu?**

O _Fine-Tuning_ em conjuntos de dados médicos pequenos ou com classes muito desbalanceadas pode facilmente levar ao **overfitting** (sobreajuste). Ao descongelar as camadas profundas, o modelo pode ter começado a "decorar" o ruído do conjunto de treino (prejudicando a classe de feridas de pressão, que teve um recall de apenas 52%).

Manter a rede congelada forçou o uso das características genéricas e robustas que o MobileNetV2 já havia aprendido, o que resultou em uma capacidade preditiva mais estável para os dados de teste (diminuindo os erros totais de 14 para 11).

**Recomendação:** Para este volume de dados específico, a estratégia sem _Fine-Tuning_ é mais segura e performática. Se desejar tentar o _Fine-Tuning_ novamente no futuro, recomendo usar uma taxa de aprendizado (learning rate) extremamente baixa e descongelar apenas as últimas 2 ou 3 camadas convolucionais, e não a rede inteira.


---


### 1. Comparativo de Desempenho Global (Conjunto de Teste)

Aqui estão as métricas finais de avaliação no conjunto de teste (68 imagens):

|**Métrica**|**Modelo 1: SEM Fine-Tuning**|**Modelo 2: COM Fine-Tuning**|**Comparativo**|
|---|---|---|---|
|**Acurácia (Teste)**|**83.82%** (0.8382)|79.41% (0.7941)|Modelo 1 foi ~4,4% melhor|
|**Loss (Perda no Teste)**|**0.0381**|0.0526|Modelo 1 errou com menor margem (loss menor)|
|**F1-Score (Macro)**|**0.88**|0.84|Modelo 1 lidou melhor com as classes em geral|
|**F1-Score (Ponderado)**|**0.84**|0.79|Modelo 1 manteve a consistência com o desbalanceamento|

### 2. Análise Comportamental por Classes (Onde o modelo errou?)

As classes `background` (fundo) e `normal` (pele normal) foram perfeitamente ou quase perfeitamente classificadas por ambos os modelos. O grande diferencial ocorreu nas feridas patológicas:

- **Feridas de Pressão (`pressure`):**
    
    - **Sem Fine-Tuning:** Conseguiu um equilíbrio muito bom, acertando 76% (Recall de 0.76) das feridas de pressão, com F1-Score de **0.76**.
        
    - **Com Fine-Tuning:** O desempenho caiu drasticamente. O modelo só conseguiu encontrar 52% (Recall de 0.52) das feridas de pressão reais (deixou passar quase metade).
        
- **Feridas Diabéticas (`diabetic`):**
    
    - **Com Fine-Tuning:** Como ele falhou em reconhecer as feridas de pressão, ele acabou "chutando" que a maioria era diabética. Isso explica o Recall alto (0.86) mas a Precisão baixa (0.71) nesta classe. Ele gerou muitos "Falsos Positivos" para diabética.
        
    - **Sem Fine-Tuning:** Muito mais equilibrado, com F1-Score de **0.80**.
        

### 3. O que aconteceu no Treinamento? (A "Armadilha" do Fine-Tuning)

Os seus logs de época revelam exatamente o momento em que o modelo com Fine-Tuning começou a piorar.

**No Modelo Sem Fine-Tuning:**

Durante as 40 épocas, a perda de validação (`val_loss`) foi caindo progressivamente (de 0.8439 até estabilizar em torno de **0.0501**). O modelo aprendeu bem e consolidou o conhecimento.

**No Modelo Com Fine-Tuning:**

A Fase 1 (só a camada densa) foi muito bem, chegando a uma `val_loss` de **0.0457** na época 27. **Porém, na Fase 2 (quando você liberou as camadas da base):**

- Na Época 1: `val_loss` = 0.0478
    
- Na Época 5: `val_loss` = 0.0724
    
- Na Época 10: `val_loss` = **0.0887**
    

**Diagnóstico:** Ao liberar as camadas da base na Fase 2, ocorreu um fenômeno chamado **"Catastrophic Forgetting" (Esquecimento Catastrófico)** ou **Overfitting severo**. A `val_loss` começou a subir época após época. Isso significa que, ao invés de refinar as características, os novos pesos acabaram "bagunçando" as excelentes características genéricas que o MobileNetV2 já tinha pré-treinadas da ImageNet, prejudicando sua capacidade de generalizar em imagens novas.

### Conclusão e Próximos Passos

A estratégia de **Feature Extraction (Modelo 1 - Sem Fine Tuning)** provou ser a vencedora isolada para o seu projeto atual. O MobileNetV2 original já possui uma base convolucional fantástica para extrair texturas, cores e bordas (elementos essenciais para analisar feridas).

**O que eu recomendo que você faça no seu projeto/TCC:**

1. **Adote o Modelo 1:** Utilize os resultados do modelo sem Fine-Tuning para o seu trabalho final. Acurácia de quase 84% em um problema médico com classes complexas como essa usando uma rede Mobile (leve) é um excelente resultado!
    
2. **Explique o Fenômeno:** No seu texto do trabalho, use a "Fase 2" do Fine-Tuning para enriquecer sua discussão. Mostre aos avaliadores que você tentou o Fine-Tuning, mas demonstre através do aumento da `val_loss` (overfitting) que o conjunto de dados provavelmente era pequeno demais para ajustar as camadas profundas, justificando tecnicamente a escolha do modelo com as camadas congeladas.
    
3. _(Opcional)_ **Se quiser forçar o Fine-Tuning futuramente:** Se for tentar a Fase 2 de novo, aplique a técnica de _Early Stopping_ (Parada Antecipada) monitorando a `val_loss`. Assim que ela começar a subir (por volta da época 2 ou 3 da Fase 2), o treinamento é interrompido e os melhores pesos são restaurados, evitando que o modelo piore.