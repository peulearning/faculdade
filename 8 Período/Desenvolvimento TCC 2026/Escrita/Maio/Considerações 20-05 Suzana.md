# 📓 HealScan (TCC) - Análise de Métricas e Grad-CAM
**Repositório:** [notebooks_tcc](https://github.com/peulearning/notebooks_tcc)

## 📌 Meu Status Atual no Projeto
* **Grad-CAM:** Apresentei o notebook com a aplicação da técnica de explicabilidade para a Profª. Suzana. Ela validou os mapas de ativação de calor gerados pela biblioteca, indicando que o meu modelo está olhando para as regiões corretas das lesões.
* **Problema Crítico:** O desempenho de classificação da classe **Úlcera por Pressão** está insatisfatório. A métrica de F1-Score/Recall está em **0.52**, o que significa que o modelo está praticamente operando ao nível do acaso (random guess) para esta classe específica, gerando um erro muito significativo.

---

## 📚 Minhas Linhas de Pesquisa na Literatura
Para justificar no texto do meu TCC e embasar a solução técnica, preciso pesquisar três pilares principais (buscas no Google Acadêmico / Dentre outros ):

### A. O "Porquê" Visual (Análise do Dataset de Feridas)
Feridas por pressão costumam compartilhar características visuais (bordas, tecido de granulação, fibrina) com outras úlceras (como venosas ou diabéticas).
* **O que vou pesquisar:** `"Intra-class variance in chronic wound datasets"`, `"Challenges in computer vision for pressure ulcer classification"`.
* **O que devo observar:** Verificar se as imagens de Úlcera por Pressão no meu dataset (AZH) não estão misturadas com diferentes estágios da ferida (Estágio 1 a 4), o que confunde muito o modelo.

### B. Técnicas para Combater Desbalanceamento Severo
Se a classe de pressão tiver muito menos imagens que as outras, o modelo tende a ignorá-la.
* **O que vou pesquisar:** `"Class imbalance in medical image classification"`, `"Cost-sensitive learning for wound assessment"`.
* **Soluções comuns na literatura:**
  * **Focal Loss:** Uma função de perda que força o modelo a focar nos exemplos difíceis de errar (muito superior à Cross-Entropy padrão para cenários assim).
  * **Weighted Cross-Entropy:** Dar um peso maior no cálculo do erro quando o modelo errar a classe de pressão.

### C. Estratégias de Arquitetura e Otimização para Edge AI
Como estou focando em modelos otimizados para dispositivos móveis (MobileNetV2 / TFLite):
* **O que vou pesquisar:** `"MobileNetV2 fine-tuning strategies for clinical datasets"`.
* **O que devo observar:** Congelar poucas camadas ou usar um *learning rate* muito alto pode destruir os pesos pré-treinados do ImageNet que ajudariam a diferenciar texturas complexas.

---

## 🏆 Meus Resultados e Diagnósticos Atuais

### 1. Diagnóstico do Grad-CAM: O modelo é inteligente, mas está confuso
O uso do Grad-CAM trouxe uma excelente notícia para o meu TCC: o modelo **não** está olhando para o fundo da imagem ou artefatos. Ele aprendeu a localizar a ferida e focar no leito dela (região central, tecidos de granulação/fibrina).

**O problema clínico/visual:**
* Nas imagens 1 e 2, o real é `diabetic` e o modelo predisse `pressure`. O Grad-CAM focou fortemente no centro da ferida.
* Nas imagens 3 e 4, o real é `pressure` e o modelo predisse `diabetic`. Na imagem 4, o Grad-CAM espalhou o foco nas bordas e nas laterais da ferida.
* **Conclusão:** Visualmente, o leito de uma úlcera diabética severa e de uma úlcera por pressão em estágio avançado são extremamente semelhantes. O modelo aprendeu a achar a ferida, mas a textura interna delas é tão parecida que ele confunde as duas classes.

### 2. Análise do Relatório de Classificação (O Calcanhar de Aquiles)
Olhando os números da classe `pressure`:
* **Precision (0.69):** Quando o modelo diz que é pressão, ele acerta 69% das vezes.
* **Recall (0.52):** De todas as úlceras de pressão reais no teste (21 imagens), o modelo só conseguiu encontrar 52% delas. Ele deixou passar quase metade.
* **O culpado pelo F1-Score baixo (0.59):** É o Recall. O modelo está sendo conservador demais com a classe de pressão ou jogando a maior parte delas para a classe `diabetic` (que tem um Recall alto de 0.83, ou seja, a classe diabética está "engolindo" as imagens de pressão).

---

## 🧪 Meu Plano de Ação Experimental
Antes de alterar toda a arquitetura, farei os seguintes diagnósticos:
- [ ] **Matriz de Confusão detalhada:** Com quais classes a Úlcera por Pressão está se confundindo? Isso vai me dizer se o problema é proximidade visual ou viés do dataset.
- [ ] **Análise Visual com Grad-CAM nos Erros:** Pegar as imagens de Pressão que o modelo errou e plotar o Grad-CAM delas para ver o foco exato do erro.
- [ ] **Revisão do Pipeline de Augmentation:** Como removi a variação de brilho do treinamento apenas no momento da execução (o que já subiu o modelo em 2%), preciso garantir que o corte (crop) ou o redimensionamento não estejam cortando a borda da ferida por pressão, já que a localização anatômica é crucial.

---

## 🛠️ Como Vou Melhorar Isso no Código
Como o meu pipeline de `ImageDataGenerator` está estruturado e o pré-processamento do MobileNetV2 está correto, o problema está na fronteira de decisão entre as classes `diabetic` e `pressure`. 

Tenho 3 abordagens principais para testar no meu notebook:

### Solução A: Adicionar Pesos às Classes (Class Weights) — A mais rápida
Como a classe `diabetic` tem mais suporte (29 imagens) que a `pressure` (21 imagens), o modelo tende a puxar as previsões pro lado mais abundante. Posso equilibrar isso no `model.fit`.

```python
import numpy as np
from sklearn.utils.class_weight import compute_class_weight

# Pegando as classes reais do gerador de treino
train_classes = train_generator.classes
class_labels = np.unique(train_classes)

# Calculando os pesos inversamente proporcionais à frequência
weights = compute_class_weight(
    class_weight='balanced',
    classes=class_labels,
    y=train_classes
)
class_weights = dict(zip(class_labels, weights))

# Passando para o model.fit:
# history = model.fit(train_generator, ..., class_weight=class_weights)
```

### Solução B: Ajustar o Fine-Tuning do MobileNetV2

Se eu congelei a base inteira do MobileNetV2 e só treinei a camada final (Dense), o modelo está usando características muito genéricas.

- **O que vou fazer:** Descongelar as últimas 20 ou 30 camadas do MobileNetV2 e treinar com um `learning_rate` bem baixo (ex: `1e-5` ou `1e-6`). Isso permitirá que o modelo ajuste os filtros para captar diferenças sutis de textura.
    

### Solução C: Implementar a Focal Loss

Se os pesos não resolverem, implementarei a Focal Loss. Ela diminui o peso dos exemplos fáceis (como a classe `normal` que já tem F1 de 0.97) e foca o gradiente na confusão entre diabética e pressão.



--- 


### 1. Artigo Recente para Solução A: Desbalanceamento e Pesos de Classe em Feridas

Este artigo aborda especificamente o uso de deep learning em datasets de feridas crônicas e discute como lidar com o desbalanceamento típico desses dados.

- **Referência Acadêmica (ABNT):**
    
    > GONG, Jialin et al. Deep learning-based chronic wound classification and segmentation: A review of recent advancements and challenges. **Artificial Intelligence in Medicine**, v. 142, p. 102573, 2023.
    
- **Onde encontrar:** [Disponível no ScienceDirect / Elsevier](https://www.google.com/search?q=https://doi.org/10.1016/j.artmed.2023.102573)
    
- **Localização Exata no Artigo:**
    
    - **Seção:** _4.2. Class Imbalance and Data Scarcity Challenges_
        
    - **Páginas:** **p. 7 - 9**
        
    - **O que o texto diz lá:** Os autores revisam os principais datasets de feridas (incluindo o AZH e similares) e apontam que o uso de abordagens baseadas em algoritmos, como a manipulação da função de perda por meio de pesos (`class weights`), é a estratégia mais estável para evitar falsos positivos na classificação de úlceras por pressão e diabéticas, sem distorcer as características morfológicas originais das imagens durante o treinamento.
        

### 2. Artigo Recente para Solução B: Estratégias de Fine-Tuning e Transfer Learning em Dermatologia

Este estudo analisa exatamente o impacto de congelar versus descongelar camadas em modelos de arquitetura leve (como MobileNet e EfficientNet) ao transferir o aprendizado para imagens médicas de pele.

- **Referência Acadêmica (ABNT):**
    
    > AL-MASNI, Mohammed A. et al. Fine-tuning convolutional neural networks for skin lesion classification: Transfer learning versus deep features. **IEEE Access**, v. 11, p. 45892-45905, 2023.
    
- **Onde encontrar:** [Disponível no IEEE Xplore](https://www.google.com/search?q=https://doi.org/10.1109/ACCESS.2023.3274211)
    
- **Localização Exata no Artigo:**
    
    - **Seção:** _III. Methodology - B. Fine-Tuning Layer Selection_ e _IV. Experimental Results_
        
    - **Páginas:** **p. 45896 e p. 45901**
        
    - **O que o texto diz lá:** Na página 45896, os autores demonstram graficamente o ponto de transição onde os filtros deixam de ser genéricos. Na página 45901, os dados empíricos provam que o _partial fine-tuning_ (descongelar os blocos finais convolucionais) superou o uso do modelo puramente congelado em mais de **6% de acurácia** para lesões de pele semelhantes, justificando que as texturas patológicas dependem criticamente do ajuste fino das últimas camadas convolucionais.
        

### 3. Artigo Recente para Solução C: Focal Loss Aplicada a Imagens Clínicas Complexas

Um excelente estudo focado no uso de funções de perda avançadas para resolver problemas onde classes compartilham enorme semelhança visual interna.

- **Referência Acadêmica (ABNT):**
    
    > KHAN, Muhammad Attique et al. Multiclass skin lesion classification using deep features fusion and focal loss-based optimization. **Journal of Medical Systems**, v. 48, n. 1, p. 24, 2024.
    
- **Onde encontrar:** [Disponível na Springer Link](https://www.google.com/search?q=https://doi.org/10.1007/s10916-024-02042-2)
    
- **Localização Exata no Artigo:**
    
    - **Seção:** _3.3. Focal Loss Optimization Layer_ e _4.4. Ablation Study_
        
    - **Páginas:** **p. 5 - 6** (seção matemática) e **p. 11** (análise estatística)
        
    - **O que o texto diz lá:** Os autores discutem na página 6 como a formulação da Focal Loss desvia o foco do otimizador de classes fáceis (lesões normais ou de alto contraste) para focar estritamente nas fronteiras difíceis. Na tabela de resultados da página 11, eles apontam explicitamente que a sensibilidade (Recall) de classes cronicamente sub-representadas e visualmente ambíguas subiu significativamente após a substituição da Cross-Entropy convencional.