## 📌 Pautas da Reunião

- Resumo
    
- Palavras-chave
    
- Abstract (Resumo em inglês)
    
- Keywords
    
- **1.1 Objetivo geral e específicos**
    
- **1.2 Justificativa**
    
- **1.3 Tema**
    
- **1.4 Problema**
    
- **1.5 Hipótese**
    
- 📊 Apresentação das métricas dos modelos:
    
    - CNN Sequencial
        
    - MobileNetV2
        
    - YOLOv3 + ResNet50
    
# Resultado Rede Sequencial 🦾

![[Pasted image 20251127155916.png]]

# Resultado Rede MobileV2

![[Pasted image 20251127124148.png]]

# Resultado Rede YoloV3 (Darknet / Resnet50)

![[Pasted image 20251127125102.png]]


# Para a Escrita  / Próxima Reunião 


1. a classe de imagens e há ,Diabete que está com mais problemas (ou seja mais erros no modelo ) , para identificar padrões e características( Analisar as imagens ou o que cada modelo está enxergando) .

2. Preciso saber quais os Padrões de cada tipo de ferida ou quando não há feridas ( Reconhecimento das feridas : Lesões Normais; Segmentar as Feridas ).

3. Acurácias com valores altos nos modelos cujo foram treinados elas estão com altos valores acima de 95% estou preocupado de ser algum erro.

4. Preparar melhor as imagens ⚠️

5. Metodologia Iniciais do Teste do Modelo, nesta etapa, eu preciso escrever no meu TCC que a metodologia inicial de teste e oque eu fiz a construção do modelo  , apontar como o eu modelo está funcionando , Oque ele está enxergando nas feridas como ele está classificando, além de apontar as tecnologias utilizadas pelo mesmo.

6. Comparar os Resultados dos Testes Acurracy, Precision, Recall, Matriz de Confusão  ( Utilizar isso na Metodologia Inicial de Testes )

7. Oque motivou utilizar os modelos ( YoloV3 , Rede Sequencial, MobileNetV2)

8. Compara o que há de comum ( F1 - Score, Accuracy , Precision)

9. Refinar técnicas futuramente


# Pesquisa

## 📊 **2. Interpretação das Matrizes de Confusão e Relatórios**

## **2.1 Classe com Mais Problemas**

Você identificou corretamente:

> **A classe "diabetic" é a que mais apresenta erros nos três modelos.**

Isso é consistente e se repete nos gráficos enviados.

### **Como escrever isso no TCC**

> A classe _diabetic_ apresentou maior taxa de erros nos três modelos avaliados. Esse comportamento indica três possíveis causas principais:  
> (i) heterogeneidade visual dessa categoria;  
> (ii) limitação dos modelos em distinguir suas características específicas;  
> (iii) possível desbalanceamento ou variação de qualidade das imagens da classe.  
> Esse resultado demanda uma análise aprofundada das imagens e da forma como cada arquitetura está extraindo padrões visuais.

---

## 🔍 **3. Padrões Visuais: o Que Precisa Ser Identificado**

Você vai precisar descrever no TCC:

### Para cada classe:

- O que caracteriza uma **ferida diabética**
    
- O que caracteriza uma **ferida normal**
    
- O que caracteriza **lesões venosas, cirúrgicas, pressão, background**
    
- Diferenças anatômicas e visuais
    
- O que pode confundir o modelo
    

E ainda:

> Relacionar **o que o modelo enxerga** com **o que é esperado clinicamente**.

Quando você me enviar o código dos modelos, eu posso produzir:  
✔ interpretações de Grad-CAM  
✔ heatmaps de ativação  
✔ mapas de regiões de interesse

Isso vai turbinar o seu capítulo da metodologia e discussão.

---

## ⚠️ **4. Sobre as Acurácias Altas (>95%)**

A sua preocupação é válida. Em TCC isso precisa ser explicado.

Trechos para usar no texto:

> As acurácias superiores a 95% obtidas pelos modelos levantaram preocupação inicial quanto à possibilidade de _overfitting_, inconsistências no dataset ou problemas de vazamento de dados (data leakage). Essa hipótese será investigada através de:
> 
> - análise de distribuição das classes
>     
> - inspeção da separação dos conjuntos de treino/validação/teste
>     
> - verificação de duplicações
>     
> - avaliação qualitativa das imagens utilizadas
>     


---

## 🖼️ **5. Preparar Melhor as Imagens**

Pontos que devem constar na metodologia:

- padronização de iluminação
    
- padronização de tamanho
    
- remoção de bordas/ruídos
    
- aumento de dataset (data augmentation)
    
- segmentação prévia das feridas (opcional)
    



---

## 🧪 **6. Metodologia Inicial de Testes (TCC)**

Aqui está a parte pronta para você colar no Overleaf:

> A metodologia inicial consistiu em treinar e avaliar três modelos de classificação de imagens de feridas cutâneas: uma rede CNN sequencial, a MobileNetV2 e o YOLOv3 associado à ResNet-50.  
> Para cada arquitetura, foram registrados e comparados os seguintes indicadores:  
> **acurácia, precisão, recall, F1-score e matriz de confusão**.  
> O objetivo dessa etapa foi determinar o desempenho geral, identificar padrões de erro e avaliar a capacidade dos modelos de reconhecer diferentes tipos de lesões.  
> Além disso, foram analisados os padrões visuais capturados pelas redes, permitindo observar como cada modelo discrimina as classes, especialmente as que apresentam maior complexidade visual, como as feridas diabéticas.

---

## 🔍 **7. Justificativa da Escolha dos Modelos**

Trecho pronto para sua metodologia:

> Os três modelos foram selecionados de acordo com diferentes níveis de complexidade e aplicação prática:
> 
> - **Rede Sequencial**: estabelece uma linha de base simples para avaliar separabilidade inicial das classes.
>     
> - **MobileNetV2**: arquitetura leve e eficiente, amplamente utilizada em aplicações móveis, alinhada ao objetivo do TCC.
>     
> - **YOLOv3 + ResNet50**: combinação robusta para cenários com detecção e classificação simultâneas, adequada para futuras etapas de segmentação e análise de regiões específicas das feridas.
>     

---

## 📊 **8. Comparação dos Modelos (Resumo Técnico)**

Todos apresentaram acurácias altas, mas:

|Modelo|Pontos Fortes|Pontos Fracos|
|---|---|---|
|**CNN Sequencial**|Extremamente precisa|Possível overfitting|
|**MobileNetV2**|Equilibrada e realista|Leve confusão entre classes diabéticas|
|**YOLOv3 + ResNet50**|Melhor em captar regiões|Maior confusão na classe _venous_|

Trecho utilizável:

> Apesar das pequenas variações nos valores de F1-score, os três modelos apresentaram comportamento semelhante, com destaque para a alta sensibilidade nas classes _background_ e _normal_, e maior dificuldade nas classes _diabetic_ e _venous_. Essa tendência aparece de forma consistente nos três relatórios e matrizes de confusão.

---

## 🔧 **9. Técnicas Futuras**

Liste como objetivos futuros:

- segmentação automática das feridas
    
- Grad-CAM para interpretação
    
- Equalização e padronização das imagens
    
- Dataset ampliado
    
- Ajustes finos de hiperparâmetros
    
- Teste em ambiente mobile real