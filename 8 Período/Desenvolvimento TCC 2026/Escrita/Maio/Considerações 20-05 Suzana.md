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