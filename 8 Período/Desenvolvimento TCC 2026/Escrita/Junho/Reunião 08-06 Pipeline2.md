#### 🔬 Experimentos de Pré-processamento (Novos Notebooks)

Para a próxima discussão, é necessário derivar o notebook principal em duas novas frentes de experimentação usando imagens em **escala de cinza**:

- [x] **Notebook 1: Edge Canny**
    
    - Converter imagens para tons de cinza.
        
    - Aplicar o filtro de Canny para detecção de bordas (focando nos contornos das estruturas/lesões).
        
    - Treinar o modelo com esses dados e extrair métricas.
        
- [ ] **Notebook 2: CLAHE (Contrast Limited Adaptive Histogram Equalization)**
    
    - Converter imagens para tons de cinza.
        
    - Aplicar o CLAHE. Essa técnica é excelente para imagens clínicas, pois realça o contraste local sem estourar o brilho global, destacando melhor a textura dos tecidos.
        
    - Treinar o modelo com esses dados e extrair métricas.
        
- [ ] **Comparativo Final:** Montar um quadro comparativo dos resultados do Canny e do CLAHE contra a _baseline_ original (RGB sem filtros).



---- 



Notebook 1. [Edge Canvy Escala Cinza Archictecture MobileNetV2 2 Classes Modify.ipynb - Colab](https://colab.research.google.com/drive/1SvFej1UgWWI2OSSBzjyUh-lDy9vSwjuM) 


#Desempenho Final no Conjunto de Teste

- **Acurácia:** 44,90%
- **Loss:** 0,7898

#### Matriz de Confusão

|Classe Real|Predito Diabetic|Predito Pressure|
|---|---|---|
|Diabetic|15|13|
|Pressure|14|7|

#### Relatório de Classificação

|Classe|Precision|Recall|F1-Score|
|---|---|---|---|
|Diabetic|0,52|0,54|0,53|
|Pressure|0,35|0,33|0,34|

**Accuracy Geral:** 45%

---

#Interpretação

A hipótese inicial era que o filtro de bordas poderia destacar características estruturais importantes das feridas, facilitando a separação entre as classes _Diabetic_ e _Pressure_. Entretanto, os resultados indicam que essa estratégia não foi eficaz.

Observa-se que:

- O modelo manteve dificuldade em distinguir as duas classes.
- A classe **Pressure** continuou apresentando baixo Recall (33%) e baixo F1-Score (34%).
- Houve elevada quantidade de falsos positivos e falsos negativos para ambas as classes.
- A acurácia final ficou próxima do acaso para um problema binário balanceado.

Além disso, durante o treinamento a acurácia de validação chegou a aproximadamente **70%**, enquanto a acurácia de teste foi de apenas **45%**, sugerindo que as características extraídas pelas bordas não generalizaram adequadamente para imagens não vistas.

---

#Possível Explicação

O detector de bordas de Canny remove grande parte das informações de textura, coloração e padrões internos da lesão, preservando apenas contornos.

No contexto de classificação de feridas, características relevantes costumam estar relacionadas a:

- textura do tecido;
- regiões necróticas;
- granulação;
- exsudato;
- padrões de coloração;
- transições suaves entre tecidos.

Ao converter as imagens para escala de cinza e posteriormente aplicar Edge Canny, muitas dessas informações discriminativas são perdidas.

Dessa forma, o modelo passa a enxergar apenas contornos, que aparentemente não são suficientes para diferenciar adequadamente as classes _Diabetic_ e _Pressure_.

---

#Conclusão do Experimento

> A aplicação da técnica Edge Canny não produziu melhorias significativas no desempenho do modelo MobileNetV2. Pelo contrário, os resultados sugerem que a extração de bordas removeu informações importantes para a caracterização das lesões, reduzindo a capacidade discriminativa da rede neural. Portanto, a utilização isolada de imagens processadas por Edge Canny não se mostrou uma estratégia adequada para este problema de classificação de feridas.


![[Pasted image 20260610132854.png]]



Notebook 2. [CLAHE Escala Cinza Archictecture MobileNetV2 2 Classes Modify.ipynb - Colab](https://colab.research.google.com/drive/1JvCKHcAfUNI5Ihd8qsA01Wu8d-eLG4ef) 



#Análise e Interpretação dos Resultados

A aplicação da técnica CLAHE teve como objetivo melhorar o contraste local das imagens, destacando características relevantes para a classificação das lesões e facilitando a extração de atributos pela rede neural.

Os resultados obtidos no conjunto de teste foram:

- **Acurácia:** 71%
- **Precisão (classe diabetic):** 71%
- **Recall (classe diabetic):** 86%
- **F1-score (classe diabetic):** 77%
- **Precisão (classe pressure):** 73%
- **Recall (classe pressure):** 52%
- **F1-score (classe pressure):** 61%
- **AUC:** 0,7874 para ambas as classes

 #Matriz de Confusão

|Classe Real|Predita Diabetic|Predita Pressure|
|---|---|---|
|Diabetic|24|4|
|Pressure|10|11|

Observa-se que o modelo apresentou melhor desempenho na identificação da classe **diabetic**, alcançando um recall de 86%, indicando elevada capacidade de detectar corretamente imagens pertencentes a essa categoria. Entretanto, para a classe **pressure**, o recall foi significativamente menor (52%), evidenciando maior dificuldade na distinção dessa classe.

O valor de **AUC = 0,7874** indica uma capacidade discriminativa considerada satisfatória, demonstrando que o modelo consegue diferenciar as duas classes melhor do que uma classificação aleatória.

Além disso, foram identificados **14 erros de classificação** entre as 49 amostras de teste, correspondendo aproximadamente a 28,6% das amostras avaliadas.

---

 #Efeito do CLAHE no Modelo

O CLAHE melhora o contraste local sem amplificar excessivamente o ruído, permitindo que estruturas importantes da imagem se tornem mais evidentes. Em imagens médicas, essa técnica frequentemente destaca bordas, texturas e padrões visuais que podem auxiliar a rede neural durante o treinamento.

Nos mapas Grad-CAM gerados, observa-se que o modelo concentra sua atenção em regiões visualmente relevantes da lesão, sugerindo que o pré-processamento foi capaz de evidenciar informações úteis para a classificação.

---

#A Melhora Foi Significativa?

A resposta depende da comparação com o modelo de referência (sem CLAHE).

### Caso a acurácia anterior tenha sido próxima de 71%

Por exemplo, entre 68% e 72%, a melhora **não pode ser considerada significativa**, pois a diferença está dentro da variação normalmente observada em treinamentos de redes neurais e pode estar relacionada à aleatoriedade do processo de treinamento.

### Caso a acurácia anterior tenha sido consideravelmente menor

Por exemplo, abaixo de 65%, então o ganho obtido com CLAHE pode ser considerado **relevante**, indicando que o aumento do contraste contribuiu positivamente para a extração de características discriminativas.

---

 #Conclusão

> A aplicação do CLAHE permitiu melhorar o contraste local das imagens e contribuiu para um desempenho global satisfatório da MobileNetV2, alcançando 71% de acurácia e AUC de 0,7874. O modelo demonstrou elevada capacidade de identificação da classe diabetic, porém ainda apresentou dificuldades na classificação da classe pressure. Embora o CLAHE tenha favorecido a extração de características relevantes e direcionado adequadamente a atenção do modelo nas regiões de interesse, a significância da melhoria depende da comparação direta com os resultados obtidos sem a aplicação da técnica. Dessa forma, os resultados sugerem que o CLAHE é uma estratégia promissora de pré-processamento, mas não suficiente, isoladamente, para eliminar as dificuldades de separação entre as duas classes analisadas.


Comparando 

|Métrica|Escala de Cinza|CLAHE + Escala de Cinza|
|---|---|---|
|Acurácia|**72%**|**71%**|
|Precisão (Diabetic)|70%|71%|
|Recall (Diabetic)|90%|86%|
|F1-Score (Diabetic)|79%|77%|
|Precisão (Pressure)|77%|73%|
|Recall (Pressure)|48%|52%|
|F1-Score (Pressure)|59%|61%|
|AUC|Não informado|0,7874|

## Interpretação Comparativa

Ao comparar os resultados obtidos com imagens em escala de cinza e aqueles obtidos após a aplicação do CLAHE, observa-se que o ganho de desempenho foi bastante limitado.

A acurácia geral apresentou uma pequena redução de **72% para 71%**, indicando que a aplicação do CLAHE não promoveu melhoria global na capacidade de classificação do modelo MobileNetV2.

Entretanto, ao analisar as métricas por classe, observa-se um comportamento interessante. A classe **pressure**, que apresentava maior dificuldade de classificação, obteve uma melhora no recall, passando de **48% para 52%**, além de um aumento no F1-score de **59% para 61%**. Isso sugere que o CLAHE ajudou o modelo a identificar um número ligeiramente maior de amostras pertencentes a essa categoria.

Por outro lado, a classe **diabetic** apresentou uma pequena queda no recall, reduzindo de **90% para 86%**, bem como uma diminuição no F1-score de **79% para 77%**. Esse comportamento indica que parte do ganho obtido para a classe pressure ocorreu à custa de uma leve perda de desempenho na identificação da classe diabetic.

A análise das matrizes de confusão reforça essa observação:

### Escala de Cinza

|Classe Real|Diabetic|Pressure|
|---|---|---|
|Diabetic|26|3|
|Pressure|11|10|

### CLAHE

|Classe Real|Diabetic|Pressure|
|---|---|---|
|Diabetic|24|4|
|Pressure|10|11|

Após a aplicação do CLAHE, houve:

- redução de 26 para 24 classificações corretas da classe diabetic;
- aumento de 10 para 11 classificações corretas da classe pressure;
- ligeira redistribuição dos erros entre as classes.

Esses resultados indicam que o CLAHE alterou a forma como o modelo percebe os padrões visuais das imagens, favorecendo discretamente a identificação da classe pressure, mas sem gerar ganhos expressivos no desempenho global.

---

## Discussão Sobre Significância

Do ponto de vista estatístico e prático, a diferença observada entre os dois experimentos é muito pequena:

- variação de apenas **1 ponto percentual na acurácia**;
- mudanças inferiores a **5 pontos percentuais** na maioria das métricas;
- comportamento semelhante das curvas de treinamento e validação.

Dessa forma, **não é possível afirmar que a aplicação do CLAHE produziu uma melhoria significativa no desempenho do modelo MobileNetV2**.

Os resultados sugerem que as imagens em escala de cinza já continham contraste suficiente para que a rede neural aprendesse características discriminativas relevantes. Embora o CLAHE tenha contribuído para uma ligeira melhora na identificação da classe pressure, esse ganho foi compensado por uma pequena redução no desempenho da classe diabetic, resultando em um desempenho global praticamente equivalente.


--- 


* ***Tabela Comparativa de Métricas Globais**

Para facilitar a visualização do impacto de cada técnica, consolidei as métricas na tabela abaixo:

|**Métrica**|**Escala de Cinza**|**CLAHE + Cinza**|**Canny Edge**|
|---|---|---|---|
|**Acurácia**|**72%**|71%|44,90%|
|**Precisão (Diabetic)**|70%|**71%**|52%|
|**Recall (Diabetic)**|**90%**|86%|54%|
|**F1-Score (Diabetic)**|**79%**|77%|53%|
|**Precisão (Pressure)**|**77%**|73%|35%|
|**Recall (Pressure)**|48%|**52%**|33%|
|**F1-Score (Pressure)**|59%|**61%**|34%|

* * **Interpretação Comparativa: O Impacto do Filtro Canny**

Ao introduzir os resultados do Canny Edge na análise, o cenário muda drasticamente. Enquanto a diferença entre Escala de Cinza e CLAHE era sutil (uma troca de vantagens entre as classes), a aplicação da detecção de bordas resultou em um **colapso no desempenho do modelo MobileNetV2**.

**1. Queda Drástica de Acurácia e Confiabilidade**

A acurácia despencou de um patamar de ~72% para apenas **44,90%**. Considerando que temos duas classes, um desempenho de 45% indica que o modelo está performando pior do que um palpite aleatório (cara ou coroa). Todas as métricas de Precisão, Recall e F1-Score sofreram quedas severas, com a classe _Pressure_ sendo a mais prejudicada (F1-Score caiu de ~60% para 34%).

**2. Análise da Matriz de Confusão**

A matriz de confusão do Canny evidencia que o modelo perdeu a capacidade de diferenciar os padrões:

- **Classe Diabetic:** O modelo acertou 15 e errou 13. Ou seja, ele confunde quase metade das amostras reais dessa classe. (No modelo em Escala de Cinza, ele acertava 26 e errava apenas 3).
    
- **Classe Pressure:** O modelo acertou apenas 7 e classificou erroneamente 14 amostras como _Diabetic_. Ele erra o dobro do que acerta nesta classe.
    

**3. Por que o Canny Edge falhou?**

O filtro Canny é excelente para destacar contornos estruturais rígidos, mas ele **elimina por completo a textura, a intensidade dos pixels e os gradientes de sombreamento**.

No contexto de imagens médicas ou teciduais (como úlceras de pressão ou retinopatia diabética), a textura, as microlesões e os padrões sutis de claro/escuro são exatamente as características discriminatórias que as Redes Neurais Convolucionais (como a MobileNetV2) utilizam para classificar a imagem. Ao deixar apenas as bordas, você removeu a informação mais valiosa que o modelo precisava para aprender.

* ***Conclusão e Recomendações**

Do ponto de vista prático e estatístico, a conclusão é clara:

1. **Canny Edge deve ser descartado** para esta tarefa específica de classificação. A remoção das texturas destrói a capacidade de predição do modelo.
    
2. **Escala de Cinza (Pura)** continua sendo o melhor modelo base (_baseline_), pois apresenta a melhor acurácia global (72%) e o modelo consegue identificar a classe _Diabetic_ com altíssima eficiência (90% de Recall).
    
3. **CLAHE + Escala de Cinza** continua sendo uma alternativa viável **apenas** se o seu objetivo de negócio/clínico exigir uma sensibilidade (Recall) um pouco maior para a classe _Pressure_, aceitando pagar o preço de errar um pouco mais na classe _Diabetic_.