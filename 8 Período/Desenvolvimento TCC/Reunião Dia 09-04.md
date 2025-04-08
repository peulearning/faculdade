1. Informação generalista ( dimensão de 224x224 ) [X]
2. Porque definiram padrão 224x224  ( Pesquisar na arquitetura Alexnet ) [X]
3. Diferenças Similaridades dos termos : Teste e Validação em IA [X] 
4. Diferenças de Rede Neural x Rede Convolucional  [X ] 
5. Qual as vantagens da utilização da rede neural convolucional ? Porque os artigos defendem .Qual a aplicação dela para visão computacional . [X] 
	1. O que e mais imporante no contexto de imagens, saúde, feridas ( TRABALHO DE CLASSIFCAÇÃO ) além da accuracy oque e mais interessante de verificar, Precision ou Recall. [ X] 


###  📌 **Informação generalista (dimensão 224x224)**

A dimensão **224x224** é uma convenção comum no **pré-processamento de imagens para redes neurais convolucionais (CNNs)**. Isso significa redimensionar todas as imagens de entrada para esse tamanho antes de alimentá-las ao modelo.

- Essa dimensão é **pequena o suficiente** para garantir **eficiência computacional**, mas **grande o suficiente** para reter **informação visual relevante**.
    
- Muitos modelos pré-treinados como **VGG16**, **ResNet** e **AlexNet** utilizam essa dimensão como padrão de entrada.
    Fonte : https://medium.com/swlh/alexnet-the-cnn-that-changed-computer-vision-9a1f4a50142d?utm_source=chatgpt.com

---

### 📌 **Por que definiram o padrão 224x224? (AlexNet)**

O padrão de 224x224 surgiu com **AlexNet**, uma das primeiras CNNs a se destacar em desafios como o **ImageNet** (2012).

- AlexNet usou 224x224 porque era um **compromisso entre qualidade da imagem e capacidade computacional** disponível na época (usavam GPUs com 2GB de RAM).
    
- **Imagens muito maiores** seriam mais pesadas para o processamento, e **imagens muito pequenas** perderiam detalhes importantes.
    
- Essa dimensão também funcionava bem para **convoluções com filtros 3x3 e 5x5**, comuns naquela arquitetura.
    Fonte : https://discuss.pytorch.org/t/alexnet-input-size-224-or-227/41272?utm_source=chatgpt.com

---

### 📌 **Diferenças e Similaridades: Teste vs Validação em IA**

|Aspecto|Validação|Teste|
|---|---|---|
|**Objetivo**|Avaliar o modelo durante o treinamento (para ajuste de hiperparâmetros)|Avaliar o desempenho final do modelo|
|**Uso**|Durante o treinamento|Após o treinamento|
|**Conjunto de dados**|Dados separados do treino, mas ainda visíveis ao modelo|Dados nunca vistos pelo modelo|
|**Importância**|Para evitar overfitting durante o desenvolvimento|Para avaliar a generalização real|

> **Similaridade**: Ambos não são usados para treinar o modelo e servem para **avaliar desempenho**.
> Fonte :  https://stats.stackexchange.com/questions/19048/what-is-the-difference-between-test-set-and-validation-set?utm_source=chatgpt.com
---

### 📌 **Diferenças: Rede Neural x Rede Neural Convolucional**

| Característica              | Rede Neural (MLP)    | Rede Neural Convolucional (CNN)      |
| --------------------------- | -------------------- | ------------------------------------ |
| Estrutura                   | Totalmente conectada | Camadas com filtros convolucionais   |
| Processamento de Imagem     | Não é eficiente      | Especializada para imagens           |
| Extração de Características | Manual               | Automática (usa filtros)             |
| Complexidade                | Mais simples         | Mais complexa                        |
| Espaço de parâmetros        | Muito maior          | Muito menor (usa peso compartilhado) |

> **Resumo**: A CNN é otimizada para trabalhar com imagens, extraindo automaticamente padrões visuais como bordas, texturas e formas.
Fonte : https://www.baeldung.com/cs/convolutional-vs-regular-nn?utm_source=chatgpt.com


### 📌 **Porque os artigos defendem .Qual a aplicação dela para visão computacional**

Fonte : https://ojs.revistacontemporanea.com/ojs/index.php/home/article/view/7190/5125

### 📌**Vantagens das Redes Convolucionais (CNNs)**

**Por que os artigos defendem o uso de CNNs?**

- **Alta acurácia** na classificação de imagens.
    
- **Aprendem sozinhas as características** (features) mais importantes da imagem.
    
- **Peso compartilhado e localidade dos filtros** reduzem a quantidade de parâmetros e aumentam a eficiência.
    
- Usadas amplamente em tarefas de **visão computacional**: detecção de objetos, segmentação, classificação, etc.
    

**Aplicação em visão computacional:**

- Diagnóstico por imagem (radiologia, dermatologia)
    
- Reconhecimento facial
    
- Análise de feridas, lesões, tumores
    
- Classificação de células ou tecidos em imagens microscópicas
    

	Fonte : https://medium.com/%40tam.tamanna18/exploring-convolutional-neural-networks-architecture-steps-use-cases-and-pros-and-cons-b0d3b7d46c71

---

### 📌 **No contexto de imagens médicas (feridas), além da Accuracy, o que é mais interessante: Precision ou Recall?**

Depende do objetivo, mas **geralmente o mais importante é o `Recall`**.

#### 🧠 Por quê?

- **Recall** (Sensibilidade) mede **quantos casos positivos reais foram corretamente identificados**.
    
- Em saúde, **errar um caso positivo (falso negativo)** pode ser grave. Ex: não detectar uma ferida infectada.
    

| Métrica       | Interpretação no contexto de saúde                                                 |
| ------------- | ---------------------------------------------------------------------------------- |
| **Accuracy**  | Boa no geral, mas pode ser enganosa em bases desbalanceadas                        |
| **Precision** | Evita falsos positivos (ex: marcar uma ferida como grave quando não é)             |
| **Recall**    | Evita falsos negativos (ex: deixar de detectar uma ferida grave) ✅ mais importante |
| **F1-Score**  | Média harmônica entre Precision e Recall (boa para equilíbrio)                     |

Fonte : https://en.wikipedia.org/wiki/Precision_and_recall

Alterações Pontuais no Código 