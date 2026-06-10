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


