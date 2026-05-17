# 📚 Reunião / Discussões de Pesquisa — Pré-processamento e Interpretabilidade do Modelo

> **Contexto:** Discussões e direcionamentos levantados pela Suzana sobre detecção, identificação, pré-processamento de imagens e interpretabilidade do modelo utilizando Grad-CAM.

---

# 🧠 Questões Conceituais Importantes

## ❓ Detecção vs Identificação

> “A detecção é um problema e também é importante na etapa da construção do modelo. Porém, qual seria mais importante: a detecção ou a identificação?”  
> — Suzana

### 🔎 Reflexão

É necessário avaliar qual abordagem possui maior impacto no problema proposto:

|Aspecto|Detecção|Identificação / Classificação|
|---|---|---|
|Objetivo|Encontrar a região da lesão|Classificar o tipo da lesão|
|Complexidade|Mais alta|Moderada|
|Dependência|Pode melhorar classificação|Depende da qualidade da região analisada|
|Aplicação|Localização da área relevante|Diagnóstico final|

### 💡 Ponto Importante

Talvez o modelo esteja classificando corretamente sem necessariamente “olhar” para a região correta.

Por isso, a interpretabilidade utilizando **Grad-CAM** se torna essencial.

---

# ⚠️ Observações Sobre Pré-processamento

## 🔬 Testes Realizados

Foi realizado:

- Conversão simples de imagens
- Testes rápidos de pré-processamento
- Aplicação básica de escala de cinza

### 🚨 Problema Identificado

Ainda **não foram aplicadas técnicas de luminância e realce de contraste**.

---

# ⚠️ Técnica Importante Ainda Não Aplicada

## 🧪 CLAHE — Equalização Adaptativa Limitada por Contraste

### Nome Completo

**Contrast Limited Adaptive Histogram Equalization (CLAHE)**

### Objetivo

Melhorar contraste local da imagem sem amplificar excessivamente ruídos.

### Possíveis Benefícios

- Melhor evidência das regiões da lesão
- Realce de texturas
- Melhor separação visual das estruturas cutâneas
- Potencial melhoria nas métricas do modelo

### ⚠️ Status

- ❌ Ainda não implementado
- 🔥 Considerado prioritário

---

# 🧩 Extração de Bordas e Estruturas

## 🖼️ Edge Detection — Canny (OpenCV)

> “Conjunto de pixels importante para identificação do modelo levando em consideração as outras áreas.”  
> — Suzana

### Objetivo

Avaliar se o modelo pode se beneficiar de:

- Estruturas das bordas
- Contornos da lesão
- Informações geométricas
- Distribuição dos pixels

### Técnica Sugerida

- **Canny Edge Detection (OpenCV)**

### Hipótese

As bordas podem conter informações discriminativas relevantes para classificação.

---

# 🎨 Considerações Sobre Espaço de Cor

## 🔍 Comparação Necessária

### Avaliar:

- Modelo colorido (RGB)
- Escala de cinza

### Objetivo

Verificar:

> As cores realmente possuem relevância para classificação?

### Possível Cenário

Talvez:

- Textura seja mais importante que cor
- Bordas sejam mais importantes que tonalidade
- Informações cromáticas estejam adicionando ruído

---

# 🔥 Próximos Passos (Estratégia de Pesquisa)

## 📌 Grad-CAM

### Objetivo Principal

Verificar se o modelo está analisando as regiões corretas da imagem.

---

# ✅ Checklist Geral de Atividades

## 🧪 Etapa 1 — Interpretabilidade do Modelo

### Grad-CAM

- [ ]  Implementar Grad-CAM no modelo RGB atual
- [ ]  Gerar mapas de ativação para todas as classes
- [ ]  Verificar se o modelo está olhando para a lesão
- [ ]  Identificar padrões incorretos de atenção
- [ ]  Comparar imagens corretas vs incorretas
- [ ]  Salvar exemplos relevantes para documentação
- [ ]  Criar análise visual qualitativa dos resultados

---

## 📊 Etapa 2 — Análise das Métricas

- [ ]  Identificar classes com menor desempenho
- [ ]  Verificar precision, recall e F1-score por classe
- [ ]  Relacionar erros com regiões analisadas no Grad-CAM
- [ ]  Investigar possíveis padrões de confusão entre classes

---

## 🖼️ Etapa 3 — Pré-processamentos

### Escala de Cinza

- [ ]  Testar conversão para grayscale
- [ ]  Replicar grayscale nos 3 canais
- [ ]  Treinar novamente o modelo
- [ ]  Comparar métricas com RGB original

---

### CLAHE

- [ ]  Implementar CLAHE utilizando OpenCV
- [ ]  Testar diferentes parâmetros de contraste
- [ ]  Aplicar CLAHE antes do resize
- [ ]  Comparar impacto nas métricas
- [ ]  Verificar impacto visual nas lesões

---

### Edge Detection (Canny)

- [ ]  Aplicar Canny Edge Detection
- [ ]  Gerar imagens de borda
- [ ]  Testar treinamento utilizando bordas
- [ ]  Avaliar se bordas ajudam na classificação
- [ ]  Comparar com RGB e grayscale

---

## 🔬 Etapa 4 — Análise Qualitativa / Empírica

- [ ]  Revisar manualmente imagens mal classificadas
- [ ]  Avaliar localização da atenção do modelo
- [ ]  Verificar inconsistências visuais
- [ ]  Analisar imagens difíceis entre classes semelhantes
- [ ]  Criar documentação visual dos casos problemáticos

---

# 📈 Possíveis Experimentos Futuros

## 🧠 Experimentos Comparativos

- [ ]  RGB vs Grayscale
- [ ]  RGB vs CLAHE
- [ ]  RGB vs Edge Detection
- [ ]  CLAHE + Grayscale
- [ ]  CLAHE + Canny
- [ ]  Ensemble de pré-processamentos

---

# 💡 Hipóteses de Pesquisa

## Hipótese 1

O modelo pode estar utilizando regiões irrelevantes da imagem para classificação.

## Hipótese 2

As cores podem não possuir relevância significativa para identificação das lesões.

## Hipótese 3

O realce de contraste utilizando CLAHE pode melhorar a capacidade discriminativa do modelo.

## Hipótese 4

Informações estruturais (bordas e contornos) podem auxiliar na classificação das lesões.

---

# 📝 Observações Finais

## ⚠️ Pontos Críticos

- Não focar apenas em métricas
- Verificar interpretabilidade do modelo
- Entender _como_ o modelo está aprendendo
- Garantir que o modelo analise regiões clinicamente relevantes

---

# 🚀 Prioridade Recomendada

|Prioridade|Atividade|
|---|---|
|🔥 Alta|Grad-CAM|
|🔥 Alta|CLAHE|
|🔥 Alta|Análise qualitativa manual|
|🟡 Média|Grayscale|
|🟡 Média|Edge Detection|
|🟢 Baixa|Combinações avançadas de pré-processamento|