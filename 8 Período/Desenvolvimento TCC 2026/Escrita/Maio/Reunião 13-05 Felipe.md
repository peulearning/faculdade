# 📔 Registro de Reunião e Pesquisa: Visão Computacional (13/05)


``` 

Q1: Quando trabalho com escala de cinza na visão computacional, oque estou querendo observar nas imagens ? 

Q2: Qual a qualidade de resultado que tenho envolvendo a escala cinza e o mapa de calor ?

Q3: Qual a alteração na qualidade eu tenho nesse ponto quando trabalho com heatmaps ?

Q4: Na escala de Cinza eu consigo analisar bem os mapas de calor ?

Q5: Na visão computacional como vou detectar uma ferida e identificar ?  Qual padrão ?

Q6: Como consigo diferenciar , o tipo de ferida ? (ex : Diabetes , Pressão)

Q7: No dataset cujo estou utilizando, não consigo identificar a base do corpo onde está, apenas o leito ferida.


Q8: Está correto utilizar a escala de cinza ? Se estiver quais técnicas eu devo utilizar para melhorar o meu pré-processamento.

```

## 🔍 Discussão Teórica e Conceitual

Esta seção detalha os pontos levantados via pesquisa integrada (Google + Gemini) sobre os fundamentos do projeto.

### O Papel da Escala de Cinza

- **O que observar:** Ao trabalhar com escala de cinza, o foco principal é a análise de **texturas, formas e gradientes de intensidade**. Busca-se identificar padrões morfológicos que a variação de cores pode ocultar ou confundir.
    
- **Qualidade e Heatmaps:** * A qualidade dos resultados ao cruzar escala de cinza com mapas de calor (heatmaps) reside na capacidade da IA de destacar áreas de alta relevância baseadas na densidade da textura.
    
    - **Alteração na qualidade:** Ao introduzir heatmaps, ganha-se uma interpretação visual de onde o modelo está focando, porém, na escala de cinza pura, essa análise pode ser limitada pela falta de contraste térmico/cromático que ajudaria a diferenciar tipos de tecidos.
        
    - **Análise de Mapas de Calor:** É possível analisá-los bem em escala de cinza, desde que o pré-processamento garanta que as bordas e profundidades estejam matematicamente bem definidas.
        

### Identificação e Padrões de Feridas

- **Detecção de Padrão:** Na visão computacional, a ferida é detectada através da identificação de descontinuidades na textura da pele e mudanças abruptas nos níveis de cinza (bordas).
    
- **Diferenciação (Diabetes vs. Pressão):** A diferenciação ocorre pelo padrão de borda (geralmente mais calosa e circular na diabética) e pela localização anatômica, embora o tipo de tecido no leito da ferida seja o principal descritor.
    
- **Validação da Escala de Cinza:** É correto utilizá-la, mas para elevar o nível do trabalho científico, não deve ser uma conversão simples, mas sim baseada em técnicas que preservem a luminância (como o canal L do CIELab).
    

---

## 📊 Análise do Dataset e Desafios Científicos

### Limitações do Dataset

- **Ausência de Contexto:** No dataset atual, não é possível identificar a base do corpo (mapa corporal). A imagem apresenta exclusivamente o **leito da ferida**.
    
- **Desafio Clássico:** A classificação automática torna-se um desafio clássico de Visão Computacional devido à falta de contexto espacial/anatômico.
    

### Definições Essenciais para o Trabalho

- **ROI (Region of Interest):** Conceito obrigatório na pesquisa científica. Refere-se à delimitação exata da área da ferida para processamento isolado.
    

### Testes de Identificação de Mapa Corporal (Escala de Cinza)

Abaixo, o levantamento manual realizado para tentar identificar a localização anatômica:

#### Dataset: Úlcera Diabética

|**Amostragem**|**Distribuição de Achados**|
|---|---|
|**Linha 1**|6 NDA / 2 Calcanhar|
|**Linha 2**|1 Sola / 1 NDA / 2 Calcanhar / 2 Dedos|
|**Linha 3**|3 NDA / 2 Dedos / 1 Calcanhar|
|**Linha 4**|5 Dedos / 1 Sola|
|**Linha 5**|4 NDA / 1 Dedo|

#### Dataset: Úlcera de Pressão

|**Amostragem**|**Distribuição de Achados**|
|---|---|
|**Linha 1**|4 NDA / 2 Calcanhar|
|**Linha 2**|5 NDA / 1 Calcanhar|
|**Linha 3**|6 NDA|
|**Linha 4**|3 NDA|

---

## 🔄 Fluxo de Processamento e Decisões de Segmento

### Hierarquia de Atenção

A prioridade do modelo deve seguir o fluxo:

1. **Identificação:** É Ferida ou Não Ferida?
    
2. **Classificação:** Qual ferida é? (Especifidade da patologia).
    
3. **Segmento de Fluxo:** Retomar o ponto exato de quando a imagem é recebida para garantir que o pipeline trate a imagem bruta corretamente desde o início.
    

---

## 🛠️ Pipeline de Pré-processamento Recomendado

### Estrutura do Pipeline

Para extrair a máxima qualidade focando no leito da ferida, o fluxo deve combinar dados cromáticos e de luminância:

1. **[Imagem RGB Bruta]** ➡️ **[Correção de Iluminação]**
    
2. **[Divisão em 2 Caminhos]**:
    
    - **Caminho 1:** Canal L (CIELab) ➡️ Realce de Bordas (Escala de Cinza Perceptual).
        
    - **Caminho 2:** Canais a* e b* ➡️ Análise de Tecido (Informação de Cor).
        

### Detalhamento das 4 Técnicas Estruturadas

#### 1. Correção de Iluminação e Brilho (Indispensável)

- **Problema:** Fotos clínicas possuem sombras e reflexos de flash que geram falsos gradientes.
    
- **Técnica:** **CLAHE** (Contrast Limited Adaptive Histogram Equalization).
    
- **Aplicação:** Atua em pequenas regiões para aumentar o contraste local sem "estourar" o brilho, evidenciando microtexturas do leito e da borda.
    

#### 2. Extração Isolada da Luminância (Espaço CIELab)

- **Problema:** Conversão simples para cinza (RGB average) destrói dados matemáticos cruciais.
    
- **Técnica:** Isolamento do **Canal L (Lightness)**.
    
- **Aplicação:** Funciona como uma escala de cinza fiel à percepção humana. O Canal L detecta profundidade e calosidade, enquanto os canais **a** e **b** mantêm a informação de cor para infecções.
    

#### 3. Redução de Ruído com Preservação de Bordas

- **Problema:** Filtros comuns (Gaussiano) destroem bordas calosas essenciais na úlcera diabética.
    
- **Técnica:** **Filtro Bilateral** ou **Anisotropia**.
    
- **Aplicação:** Suaviza o ruído interno dos tecidos, mas mantém a nitidez perfeita nas linhas de transição da pele lesada.
    

#### 4. Normalização de Tamanho e Intensidade de Pixel

- **Problema:** Variações de câmera e distância confundem a IA.
    
- **Técnica:** **Padding** (manter aspect ratio) + **Normalização**.
    
- **Aplicação:** Redimensionar preenchendo bordas com zero e dividir os valores por 255 (escala 0 a 1) ou centralizar média/desvio padrão conforme padrões da ImageNet.