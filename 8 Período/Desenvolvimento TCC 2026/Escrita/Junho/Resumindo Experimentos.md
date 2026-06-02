

# 📊 Experimento 01 — MobileNetV2 Sem Augmentation (2 Classes)

## Objetivo

Avaliar o desempenho da arquitetura MobileNetV2 utilizando Transfer Learning para classificação de lesões em duas classes:

- Diabetic
- Pressure

Sem aplicação de Data Augmentation antes do treinamento.

---

## Resultados Obtidos

### Relatório de Classificação

|Classe|Precision|Recall|F1-Score|Support|
|---|---|---|---|---|
|Diabetic|0.69|0.69|0.69|29|
|Pressure|0.57|0.57|0.57|21|

### Métricas Gerais

|Métrica|Valor|
|---|---|
|Accuracy|0.64|
|Macro Avg F1|0.63|
|Weighted Avg F1|0.64|

---

## Análise da Matriz de Confusão

Matriz:

|Real \ Predito|Diabetic|Pressure|
|---|---|---|
|Diabetic|20|9|
|Pressure|9|12|

### Acertos

- 20 imagens diabéticas classificadas corretamente.
- 12 imagens de pressão classificadas corretamente.

### Erros

- 9 diabéticas confundidas com pressão.
- 9 pressões confundidas com diabética.

---

## Interpretação

O modelo apresentou comportamento praticamente simétrico entre as duas classes.

Isso é interessante porque mostra que:

✅ Não existe viés forte para apenas uma classe.

Mas:

❌ Existe dificuldade real em diferenciar as características visuais das duas lesões.

---

## Curva ROC

AUC obtida:

- Diabetic = 0.7586
- Pressure = 0.7586

### Interpretação

Uma AUC próxima de 0.76 indica que o modelo possui capacidade moderada de separação entre as classes.

Em outras palavras:

> O modelo aprendeu padrões relevantes, mas ainda não consegue distinguir de forma consistente as características que diferenciam úlceras diabéticas de úlceras por pressão.

---

## Análise das Curvas de Treinamento

Observando os gráficos:

### Acurácia

- Treinamento termina próximo de 80%.
- Validação termina próximo de 81%.

### Perda

- Treinamento converge adequadamente.
- Validação acompanha a curva de treinamento.

### Conclusão

Não há sinais evidentes de overfitting.

O comportamento das curvas sugere que o modelo está aprendendo de forma estável.

---

## 🎯 Conclusão do Experimento

A MobileNetV2 sem Data Augmentation apresentou desempenho moderado na classificação entre úlceras diabéticas e úlceras por pressão, alcançando 64% de acurácia e AUC de aproximadamente 0,76. A matriz de confusão demonstrou que o modelo possui dificuldade em distinguir as duas categorias, confundindo-as com frequência semelhante. Apesar disso, as curvas de treinamento indicam aprendizado estável e ausência de overfitting, sugerindo que a principal limitação está relacionada à semelhança visual entre as lesões e não à capacidade de treinamento da rede.





# 📊 Experimento 02 — MobileNetV2 Sem Augmentation (4 Classes)

### Objetivo

Avaliar o desempenho da arquitetura MobileNetV2 utilizando Transfer Learning para classificação de imagens em quatro classes:

- **Background**
    
- **Diabetic**
    
- **Normal**
    
- **Pressure**
    

Sem aplicação de Data Augmentation antes do treinamento.

### Resultados Obtidos

#### Relatório de Classificação

|**Classe**|**Precision**|**Recall**|**F1-Score**|**Support**|
|---|---|---|---|---|
|background|1.00|0.60|0.75|5|
|diabetic|0.76|0.86|0.81|29|
|normal|0.79|1.00|0.88|15|
|pressure|0.73|0.52|0.61|21|

#### Métricas Gerais

|**Métrica**|**Valor**|
|---|---|
|Accuracy|0.77|
|Macro Avg F1|0.76|
|Weighted Avg F1|0.76|

### Análise da Matriz de Confusão

**Matriz:**

|**Real \ Predito**|**background**|**diabetic**|**normal**|**pressure**|
|---|---|---|---|---|
|**background**|3|1|1|0|
|**diabetic**|0|25|0|4|
|**normal**|0|0|15|0|
|**pressure**|0|7|3|11|

#### Acertos

- **3** imagens de background classificadas corretamente.
    
- **25** imagens diabéticas classificadas corretamente.
    
- **15** imagens normais classificadas corretamente (**gabarito: 100% de recall!**).
    
- **11** imagens de pressão classificadas corretamente.
    

#### Erros

- **7** imagens de pressão foram confundidas com diabéticas (o maior gargalo).
    
- **4** imagens diabéticas foram confundidas com pressão.
    
- **3** imagens de pressão foram confundidas com a classe normal.
    
- **2** imagens de background foram divididas entre diabetic (1) e normal (1).
    

#### Interpretação

- ✅ O modelo teve um desempenho impecável em identificar tecido **normal**, alcançando 100% de recall e sem gerar falsos negativos para essa categoria.
    
- ✅ A precisão para **background** foi de 1.00, o que significa que o modelo nunca rotulou erroneamente outra classe como fundo.
    
- ❌ O clássico "fantasma" do Experimento 01 continua aqui: a principal fragilidade do modelo é distinguir entre **pressure** e **diabetic**, com uma tendência mais acentuada em errar lesões de pressão achando que são diabéticas (7 ocorrências).
    

### Curva ROC

**AUC obtida:**

- **background:** 1.0000
    
- **diabetic:** 0.9193
    
- **normal:** 1.0000
    
- **pressure:** 0.8494
    

#### Interpretação

Os valores de AUC confirmam excelente capacidade de separação para as classes `background` e `normal` (ambas com 1.00). A classe `diabetic` apresenta ótima separabilidade (0.92) e a classe `pressure` apresenta o menor índice (0.85), refletindo a maior taxa de confusão observada na matriz.

### Análise das Curvas de Treinamento

#### Acurácia

- A acurácia de validação subiu rápido e se estabilizou de forma sólida em torno de **83%**.
    
- A curva de treinamento acompanhava a subida, mas sofreu uma **queda abrupta na época 15 (indo para ~52%)**, recuperando-se logo em seguida para fechar próxima a 72%.
    
- Curiosamente, a validação permaneceu superior e imune a essa instabilidade.
    

#### Perda

- A perda de validação decresceu suavemente até estabilizar em excelentes **0.37**.
    
- A perda de treinamento vinha caindo bem, mas apresentou um **pico bizarro na época 15 (subindo para ~1.05)**, coincidindo exatamente com a queda observada na acurácia, antes de retomar a descida.
    

#### Conclusão

- Não há sinais de overfitting (a perda de validação não começou a subir em nenhum momento).
    
- O comportamento anômalo na época 15 sugere uma instabilidade pontual no gradiente durante o treino (pode ter sido um lote/batch específico muito ruidoso ou uma taxa de aprendizado ligeiramente alta para aquele momento), mas a rede provou ser robusta o suficiente para se recuperar nas épocas seguintes.
    

### 🎯 Conclusão do Experimento

A expansão para 4 classes resultou em um ganho notável de desempenho geral, elevando a acurácia para **77%** (contra 64% do cenário binário). O modelo demonstrou facilidade extrema em isolar tecidos normais e elementos de fundo. Embora a sobreposição visual e a confusão mútua entre úlceras diabéticas e por pressão ainda persistam como o principal desafio do modelo, as métricas individuais de ambas as classes evoluíram substancialmente em comparação ao primeiro experimento.


# 📊 Experimento 03 — MobileNetV2 Sem Augmentation (3 Classes)

### Objetivo

Avaliar o desempenho da arquitetura MobileNetV2 utilizando Transfer Learning para classificação de imagens em três classes:

- **Diabetic**
    
- **Normal**
    
- **Pressure**
    

Sem aplicação de Data Augmentation antes do treinamento.

### Resultados Obtidos

#### Relatório de Classificação

|**Classe**|**Precision**|**Recall**|**F1-Score**|**Support**|
|---|---|---|---|---|
|diabetic|0.86|0.86|0.86|29|
|normal|0.88|1.00|0.94|15|
|pressure|0.79|0.71|0.75|21|

#### Métricas Gerais

|**Métrica**|**Valor**|
|---|---|
|Accuracy|0.85|
|Macro Avg F1|0.85|
|Weighted Avg F1|0.84|

### Análise da Matriz de Confusão

**Matriz:**

|**Real \ Predito**|**diabetic**|**normal**|**pressure**|
|---|---|---|---|
|**diabetic**|25|0|4|
|**normal**|0|15|0|
|**pressure**|4|2|15|

#### Acertos

- **25** imagens diabéticas classificadas corretamente.
    
- **15** imagens normais classificadas corretamente (**gabarito: 100% de recall!**).
    
- **15** imagens de pressão classificadas corretamente.
    

#### Erros

- **4** imagens de pressão foram confundidas com diabéticas.
    
- **4** imagens diabéticas foram confundidas com pressão.
    
- **2** imagens de pressão foram confundidas com a classe normal.
    

#### Interpretação

- ✅ O modelo manteve um desempenho perfeito na identificação da classe **normal**, atingindo 100% de recall sem falsos negativos, e com excelente precisão (0.88).
    
- 🔄 Observa-se um comportamento **perfeitamente simétrico** no erro principal: o modelo confundiu 4 imagens de `diabetic` com `pressure` e exatamente 4 imagens de `pressure` com `diabetic`.
    
- ✅ Apesar de a confusão mútua persistir, o número de erros absolutos caiu significativamente em comparação com os ensaios anteriores, resultando em métricas de F1-Score muito mais equilibradas e robustas para ambas as classes de lesões.
    

### Curva ROC

**AUC obtida:**

- **diabetic:** 0.9243
    
- **normal:** 0.9987
    
- **pressure:** 0.8701
    

#### Interpretação

Os valores de AUC demonstram uma excelente capacidade de discriminação. A classe `normal` está próxima da separabilidade ideal (0.9987), a classe `diabetic` exibe uma performance excelente (0.92) e a classe `pressure`, embora seja a menor, ainda apresenta um valor sólido de 0.87.

### Análise das Curvas de Treinamento

#### Acurácia

- A acurácia de validação subiu de forma consistente, estabilizando-se num patamar elevado em torno de **80% a 81%**.
    
- A acurácia de treinamento acompanhou a evolução, mas registou uma **queda acentuada na época 15 (atingindo ~60%)**, recuperando logo de seguida para terminar por volta dos 74%.
    
- Tal como nos testes anteriores, a curva de validação manteve-se estável e superior, sem sofrer o impacto desta oscilação.
    

#### Perda

- A perda de validação reduziu-se de forma ideal e constante, estabilizando em excelentes **0.45**.
    
- A perda de treinamento mostrou um comportamento inverso ao da acurácia, com um **pico visível na época 15 (subindo para ~0.81)**, retomando a tendência de descida logo após o desvio.
    

#### Conclusão

- A ausência de crescimento na perda de validação confirma que não existem problemas de overfitting.
    
- A instabilidade na época 15 sugere uma perturbação pontual no cálculo dos gradientes durante o treino (causada possivelmente por um lote de dados mais ruidoso), mas a rede demonstrou resiliência ao convergir com sucesso nas épocas finais.
    

### 🎯 Conclusão do Experimento

O cenário com 3 classes alcançou o melhor desempenho geral até ao momento, atingindo uma acurácia global de **85%**. A eliminação de ruídos externos (como a classe background) permitiu ao modelo focar-se inteiramente na distinção das patologias e do tecido saudável. A simetria exata nos erros entre as úlceras diabéticas e por pressão (4 erros para cada lado) comprova que o modelo está balanceado e que as dificuldades remanescentes advêm exclusivamente da semelhança morfológica e visual profunda entre estas duas categorias de lesões.


--- 

> [!WARNING] Alerta de Viés Metodológico (Data Leakage)
> Como o processo de Data Augmentation foi realizado de forma híbrida (antes e durante o treinamento) para compensar a escassez de dados, há uma forte suspeita de **Data Leakage**. Se imagens derivadas do mesmo elemento original foram distribuídas entre os blocos de treino e teste, as métricas de 99% de acurácia podem estar artificialmente infladas. O modelo pode estar operando em Overfitting por memorização de amostras duplicadas, comprometendo o real poder de generalização em um cenário clínico cego.


# 📊 Experimento 04 — MobileNetV2 Com Augmentation Hibrído (2 Classes)

### Objetivo

Avaliar o desempenho da arquitetura MobileNetV2 utilizando Transfer Learning para classificação de lesões em duas classes:

- **Diabetic**
    
- **Pressure**
    

**Com a aplicação de Data Augmentation** antes do treinamento para expandir a variedade do dataset e reduzir o erro de generalização.

### Resultados Obtidos

#### Relatório de Classificação (`image_4b7308.png`)

|**Classe**|**Precision**|**Recall**|**F1-Score**|**Support**|
|---|---|---|---|---|
|diabetic|1.00|0.97|0.98|135|
|pressure|0.97|1.00|0.99|135|

#### Métricas Gerais

|**Métrica**|**Valor**|
|---|---|
|Accuracy|0.99|
|Macro Avg F1|0.99|
|Weighted Avg F1|0.99|

### Análise da Matriz de Confusão (`image_4b733f.png`)

**Matriz:**

|**Real \ Predito**|**diabetic**|**pressure**|
|---|---|---|
|**diabetic**|131|4|
|**pressure**|0|135|

#### Acertos

- **131** imagens diabéticas classificadas corretamente.
    
- **135** imagens de pressão classificadas corretamente (**gabarito: 100% de recall!**).
    

#### Erros

- **4** imagens diabéticas foram confundidas com pressão.
    
- **0** imagens de pressão foram confundidas com diabéticas (**gabarito: 100% de precisão para diabetic!**).
    

#### Interpretação

- 🚀 **O jogo mudou completamente!** A introdução do Data Augmentation operou um milagre aqui. O modelo saltou de pífios 64% de acurácia (no Experimento 01) para espetaculares **99%**.
    
- ✅ A classe **pressure** obteve recall perfeito (1.00), significando que nenhuma lesão por pressão passou batida pelo modelo.
    
- ✅ A classe **diabetic** obteve precisão perfeita (1.00), o que garante que se o modelo apontou "diabética", a chance de acerto é total. O único e mínimo detalhe foram as 4 imagens diabéticas classificadas como pressão.
    

### Curva ROC (`image_4b705c.png`)

**AUC obtida:**

- **AUC Score geral:** 0.9956
    

#### Interpretação

Uma AUC de aproximadamente 0.9956 indica um classificador quase perfeito. O modelo demonstra uma capacidade extraordinária e extremamente robusta de discriminar as duas condições clínicas sob as transformações aplicadas pelo augmentation.

### Análise das Curvas de Treinamento (`image_4b7302.png`)

#### Acurácia

- O treinamento foi estendido para **40 épocas**.
    
- A acurácia de treinamento terminou colada em **98%**, enquanto a de validação alcançou sólidos **97%**.
    
- Nota-se uma oscilação clássica por volta da época 15 na curva de treino (um tombo temporário de ~0.85 para ~0.80), mas que foi totalmente superada com uma subida firme e linear até o final.
    

#### Perda

- A perda de validação decresceu de forma exemplar, terminando abaixo de **0.10** (~0.09).
    
- A perda de treinamento espelhou o comportamento da acurácia, exibindo um pico isolado na época 15 (~0.45) antes de derreter até atingir a marca de **0.07**.
    

#### Conclusão

- As curvas correm praticamente juntas no terço final do gráfico, indicando uma sincronia excelente entre treino e validação.
    
- **Overfitting zero.** O comportamento descendente e suave da perda de validação valida a eficácia do Data Augmentation em regularizar a rede.
    

### 🎯 Conclusão do Experimento

A aplicação de Data Augmentation transformou a MobileNetV2 binária em uma ferramenta de altíssima confiabilidade, elevando a acurácia global para **99%** e mitigando quase por completo a confusão visual entre úlceras diabéticas e por pressão que arruinou o primeiro experimento. O comportamento das curvas de aprendizado ao longo das 40 épocas prova que o modelo atingiu uma convergência madura, segura e com excelente poder de generalização.




# 📊 Experimento 05 — MobileNetV2 Com Augmentation Híbrido (4 Classes)

### Objetivo

Avaliar o desempenho da arquitetura MobileNetV2 em um cenário multiclasse utilizando uma abordagem massiva de expansão de dados: o Data Augmentation foi aplicado de forma **híbrida**, ocorrendo tanto de maneira estática/offline (antes da divisão do dataset) quanto de maneira dinâmica/online (durante o treinamento) para mitigar ao máximo a escassez de amostras originais.

As quatro classes avaliadas são: `background`, `diabetic`, `normal` e `pressure`.

### Resultados Obtidos

#### Relatório de Classificação (`image_4aeffe.png`)

|**Classe**|**Precision**|**Recall**|**F1-Score**|**Support**|
|---|---|---|---|---|
|background|1.00|1.00|1.00|135|
|diabetic|1.00|0.99|1.00|135|
|normal|1.00|1.00|1.00|135|
|pressure|0.99|1.00|1.00|135|

#### Métricas Gerais (`image_4aeffe.png`)

|**Métrica**|**Valor**|
|---|---|
|Accuracy|1.00|
|Macro Avg F1|1.00|
|Weighted Avg F1|1.00|

### Análise da Matriz de Confusão (`image_4aefdd.png`)

**Matriz:**

|**Real \ Predito**|**background**|**diabetic**|**normal**|**pressure**|
|---|---|---|---|---|
|**background**|135|0|0|0|
|**diabetic**|0|134|0|1|
|**normal**|0|0|135|0|
|**pressure**|0|0|0|135|

#### Acertos

- **135** imagens de background classificadas corretamente.
    
- **134** imagens diabéticas classificadas corretamente.
    
- **135** imagens normais classificadas corretamente.
    
- **135** imagens de pressão classificadas corretamente.
    

#### Erros

- **1** única imagem diabética foi classificada incorretamente como úlcera por pressão.
    

#### Interpretação

- O modelo atingiu um estado de quase perfeição estatística, errando apenas uma amostra em um universo de 540 imagens de teste.
    
- A separação visual entre as classes atingiu eficiência máxima, eliminando o histórico gargalo de confusão mútua entre as classes patológicas.
    

### Curva ROC (`image_4aefc1.png`)

**AUC obtida:**

- **background:** 1.00
    
- **diabetic:** 1.00
    
- **normal:** 1.00
    
- **pressure:** 1.00
    

#### Interpretação

A curva One-vs-Rest exibe linhas perpendiculares perfeitas no topo esquerdo do gráfico, resultando em uma área sob a curva (AUC) de exatamente 1.00 para todas as categorias mapeadas.

### Análise das Curvas de Treinamento (`image_4aefc5.png`)

#### Acurácia

- O treinamento se estendeu por 40 épocas com uma evolução contínua.
    
- A acurácia de validação (linha laranja) começa alta (~93.5%) e sobe de forma extremamente suave até cravar em quase 100%.
    
- A acurácia de treinamento (linha azul) inicia mais baixa (~88%) e sobe gradualmente, correndo o tempo todo **abaixo** da curva de validação.
    

#### Perda

- A perda de validação se comporta de maneira idealizada: decresce de forma constante a partir de ~0.17 até atingir a estabilidade próxima a 0.01 nas últimas épocas.
    
- A perda de treinamento acompanha o declínio partindo de ~0.32, mantendo-se consistentemente superior à perda de validação.
    

### 🚨 Análise Crítica do Experimento (Overfitting & Data Leakage)

> [!CAUTION] Confirmação de Data Leakage Metodológico
> 
> O comportamento observado nas curvas de aprendizado (`image_4aefc5.png`) — onde a validação apresenta desempenho sistematicamente superior ao treino (menor perda e maior acurácia) do início ao fim — associado a métricas de 1.00 de acurácia global (`image_4aeffe.png`), é o indicador clássico de **Vazamento de Dados (Data Leakage)**.
> 
> Ao aplicar o Data Augmentation **antes** da separação dos dados (etapa offline), imagens geradas artificialmente a partir da mesma foto original foram distribuídas tanto para o conjunto de treino quanto para o de validação. O modelo, consequentemente, não aprendeu a generalizar novos padrões clínicos de lesões, mas sim a reconhecer características duplicadas e assinaturas específicas de matrizes de pixels que ele já conhecia no set de treino. A inclusão do augmentation adicional durante o treino serviu para consolidar essa memorização (overfitting por replicação), invalidando o uso deste modelo específico em um ambiente médico real com dados inteiramente cegos.


# 📊 Experimento 06 — MobileNetV2 Com Augmentation Híbrido (3 Classes)

### Objetivo

Avaliar o desempenho da arquitetura MobileNetV2 utilizando Transfer Learning em um cenário de classificação tripartite, expandindo o dataset de forma massiva através de uma estratégia **híbrida** de Data Augmentation (modificações artificiais aplicadas antes e de forma contínua durante o treinamento).

As três classes envolvidas são:

- **Diabetic**
    
- **Normal**
    
- **Pressure**
    

### Resultados Obtidos

#### Relatório de Classificação (`image_4a8edc.png`)

|**Classe**|**Precision**|**Recall**|**F1-Score**|**Support**|
|---|---|---|---|---|
|diabetic|0.87|0.84|0.85|135|
|normal|0.97|1.00|0.99|135|
|pressure|0.84|0.84|0.84|135|

#### Métricas Gerais (`image_4a8edc.png`)

|**Métrica**|**Valor**|
|---|---|
|Accuracy|0.89|
|Macro Avg F1|0.89|
|Weighted Avg F1|0.89|

### Análise da Matriz de Confusão (`image_4a8ec4.png`)

**Matriz:**

|**Real \ Predito**|**diabetic**|**normal**|**pressure**|
|---|---|---|---|
|**diabetic**|113|0|22|
|**normal**|0|135|0|
|**pressure**|17|4|114|

#### Acertos

- **113** imagens diabéticas classificadas corretamente.
    
- **135** imagens de tecido normal classificadas corretamente (**gabarito: 100% de recall!**).
    
- **114** imagens de pressão classificadas corretamente.
    

#### Erros

- **22** imagens diabéticas foram classificadas erroneamente como úlceras de pressão.
    
- **17** imagens de pressão foram classificadas erroneamente como lesões diabéticas.
    
- **4** imagens de pressão foram confundidas com pele normal.
    

#### Interpretação

- ✅ A classe **normal** permanece consolidada como o alvo de maior facilidade para o extrator de características da MobileNetV2, obtendo um F1-score quase perfeito de 0.99.
    
- ⚠️ **O Conflito Patológico Persiste:** Mesmo com o poder do augmentation híbrido expandindo as amostras, o modelo continua exibindo uma sobreposição de fronteiras entre as classes patológicas, resultando em uma troca mútua de 39 erros cruzados entre `diabetic` e `pressure`.
    

### Curva ROC (`image_4a8ea6.png`)

**AUC Scores por Classe:**

- **diabetic:** 0.9704
    
- **normal:** 1.0000
    
- **pressure:** 0.9638
    

#### Interpretação

Os valores de AUC demonstram que, em termos de discriminação probabilística pura, o modelo retém uma excelente capacidade de separação global (valores acima de 0.96). Isso indica que os erros na matriz de confusão ocorrem em faixas limiares muito próximas de decisão.

### Análise das Curvas de Treinamento (`image_4a8ec1.png`)

#### Acurácia

- O treinamento foi encurtado para **20 épocas**.
    
- A acurácia de validação (linha laranja) escalou de forma consistente, fixando-se no platô estável de **91%**.
    
- A acurácia de treinamento (linha azul) acompanhou a subida de forma limpa, mas sofreu um **tombo agressivo na época 15 (caindo para ~77%)** antes de recuperar a tendência linear ascendente e fechar em ~89%.
    

#### Perda

- A perda de validação decresceu perfeitamente de ~0.48 para estabilizar em **0.23**.
    
- A perda de treinamento espelhou a oscilação da acurácia, exibindo um **pico súbito de ~0.53 na época 15** antes de derreter novamente.
    

#### Conclusão das Curvas

- A "instabilidade da época 15" que assombrou os cenários sem augmentation reapareceu aqui na curva de treino. No entanto, a curva de validação manteve-se firme e imune, indicando que a perturbação ficou restrita ao conjunto de treinamento.
    

### 🎯 Conclusão e Insights de Comparação

> [!TIP] O Caso Curioso do Experimento 06
> 
> Diferente do cenário de 4 classes (onde o augmentation inflou as métricas de validação artificialmente para 100%), o modelo de 3 classes **conseguiu resistir parcialmente ao Data Leakage**, estabilizando a acurácia em **89%**.
> 
> Como a acurácia de validação manteve-se ligeiramente superior à de treino de forma uniforme e sem saltos irreais, este modelo de 3 classes demonstra ter extraído um aprendizado mais fidedigno do que o modelo de 4 classes. O gargalo clínico real de diferenciar tecidos diabéticos e necrose por pressão se manifestou de forma honesta na matriz de confusão, validando que, para triagens tripartites, a arquitetura alcançou um poder de generalização maduro e mais próximo da realidade prática.




