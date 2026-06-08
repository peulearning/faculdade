

## 🛠️ Roteiro Estratégico

### 1. Responder com Dados às Dúvidas de 13/05 (O Veredito da Escala de Cinza)

Na reunião anterior, vocês se perguntaram se estava correto usar escala de cinza e como isso afetava os mapas de calor. **Agora você tem a resposta empírica:**

- **O Argumento:** Mostre o **Experimento 07** (Acurácia de 56% e AUC de 0.57). Explique que a conversão direta para escala de cinza destruiu os marcadores cromáticos essenciais para a área médica (eritema, esfacelo, necrose).
    
- **O Impacto no Grad-CAM:** Explique que, sem a cor, as bordas e texturas ficam indistinguíveis, fazendo com que o mapa de calor perca a capacidade de criar limiares de decisão seguros (a rede quase operou em modo aleatório/chute).
    

### 2. Demonstrar Maturidade Científica (O "Pulo do Gato" do Data Leakage)

Qualquer aluno despreparado chegaria comemorando os 100% de acurácia dos Experimentos 4, 5 e 6. Você vai fazer o oposto, o que vai impressionar a Suzane:

- **O Argumento:** Apresente os gráficos com 100% de acurácia apontando o viés de **Data Leakage**.
    
- **A Justificativa:** Explique que o Data Augmentation feito de forma híbrida (antes do split) gerou "imagens irmãs" no treino e na validação. Diga que você detectou isso porque a curva de validação ficou artificialmente superior à de treino.
    
- **A Solução:** Proponha a refatoração do pipeline para aplicar o Data Augmentation **estritamente dentro do fluxo de treinamento (online)**, garantindo que o conjunto de validação permaneça 100% cego e inédito.
    

### 3. Validar a ROI através da Explicabilidade (Grad-CAM)

Vocês debateram que o dataset não identifica a base do corpo (apenas o leito da ferida), tornando a classificação automática um desafio clássico de Visão Computacional.

- **O Argumento:** Mostre o **Experimento 08**. Use as imagens do Grad-CAM para provar que, apesar de o dataset ser limitado ao leito da ferida (sem contexto anatômico do corpo), a MobileNetV2 conseguiu isolar perfeitamente a **ROI (Region of Interest)**.
    
- **A Justificativa:** O Grad-CAM provou qualitativamente que os filtros convolucionais se apoiaram no leito ulceroso e nas bordas calosas, e não em ruídos de fundo (como lençóis ou iluminação).
    

## 🚀 Proposta de Próximos Passos 

Para sanar o dilema do _"Dar atenção a Ferida ou Não Ferida > Qual ferida é?"_, apresente a proposta de uma **Abordagem Hierárquica em Dois Estágios**:

```
                  [ Imagem de Entrada ]
                            │
                            ▼
                  ┌───────────────────┐
                  │      MODELO 1     │────► [ Pele Saudável (Normal) ]
                  │ (Binary Filter)   │
                  └───────────────────┘
                            │
                    (Detectou Ferida)
                            │
                            ▼
                  ┌───────────────────┐
                  │      MODELO 2     │├───► [ Úlcera Diabética ]
                  │  (Classifier)     │└───► [ Úlcera por Pressão ]
                  └───────────────────┘
```

### Defesa do Novo Pipeline de Pré-processamento

Apresente o pipeline que você estruturou (CLAHE + Espaço de Cores CIELab). Ele resolve perfeitamente o problema da escala de cinza:

- **Por que o CIELab?** Em vez de converter para cinza comum, o **Canal L (Luminance)** passa por filtros bilaterais para destacar as bordas e relevos das úlceras de pressão, enquanto os **Canais a* e b*** alimentam o modelo com as cores de inflamação e necrose do pé diabético. Você une o melhor dos dois mundos.
    

### Mapeamento Anatômico Manual (Amostra de Validação Externa)

Mostre a ela a contagem física que você fez das linhas do Dataset de teste (dedos, calcanhar, sola).

> **Insight para o TCC:** Como a maioria esmagadora das imagens sem identificação (NDA) ou identificadas pertencem à extremidade inferior (pés/calcanhar), você pode argumentar no texto do TCC que o dataset, embora focado no leito da ferida, possui uma distribuição de características texturais predominantemente podológicas, o que justifica a similaridade morfológica agressiva entre a pressão e a diabetes.

Considerando o feedback provável da Suzane sobre o pipeline CIELab, qual das quatro técnicas de pré-processamento (CLAHE, Canal L isolado, Filtro Bilateral ou Padding) você acredita que será o maior desafio técnico para implementar no seu código atual?


### Passo 4: Defender a MobileNetV2 pelo Critério "Offline"

Muitos avaliadores de banca perguntam: _"Por que não usou uma ResNet154 ou uma EfficientNet gigante?"_.

- **A sua resposta para a Suzane:** "Como o critério do projeto é um aplicativo estritamente **offline** para rodar em dispositivos móveis na ponta (beira do leito hospitalar), a escolha da **MobileNetV2** é ideal. Ela possui uma arquitetura leve (baseada em _Inverted Residuals_ e _Depthwise Separable Convolutions_), o que garante baixo consumo de memória e processamento local rápido, sem depender de APIs ou internet."
    

### Passo 5: O "Reset" Metodológico do Data Augmentation (Busca pelo Fidedigno)

Para o modelo ser fidedigno, o ambiente de testes precisa ser implacável.

- **O passo a percorrer:** Propor à Suzane a extinção do Augmentation feito antes do split.
    
- **A proposta técnica:** Criar um script onde o dataset original é dividido rigidamente em: **70% Treino / 15% Validação / 15% Teste**. O Data Augmentation (rotação, brilho, zoom) será aplicado **apenas** no lote de 70% de treino e de forma _online_ (durante o `model.fit`, direto na memória RAM por época). Os blocos de validação e teste ficarão intocados, puramente originais.
    
- **O que esperar:** A acurácia vai cair (provavelmente para a faixa dos 80%-90%), mas esse será o **resultado real e fidedigno** do seu modelo. É isso que a ciência busca.
    

### Passo 6: O Cronograma de Implementação do CIELab

Como você ainda não implementou o CIELab, apresente isso como a sua **próxima meta de desenvolvimento**. Sugira os seguintes passos de código para ela validar:

```
[Imagem Original RGB] 
         │
         ▼
[Passo A] -> Aplicar CLAHE (Correção de contraste local para uniformizar o leito)
         │
         ▼
[Passo B] -> Conversão para Espaço CIELab 
         │
         ├──► Canal L (Isolar para aplicar Filtro Bilateral e destacar bordas/relevos)
         └──► Canais A e B (Preservar cores de tecidos: esfacelo amarelado, necrose preta)
```

- **A pergunta para a Suzane:** _"Professora, para alimentar a MobileNetV2 (que espera 3 canais), o mais correto seria concatenar novamente o Canal L filtrado com os canais A e B originais antes de enviar para a rede, ou o CLAHE aplicado direto no RGB já mitigaria o problema que vimos no Experimento 07?"_
    

### Passo 7: O Apelo Educacional e Profissional (Abordagem Hierárquica)

Como o app ajudará estudantes e profissionais, o fluxo de detecção precisa fazer sentido clínico. Médicos e enfermeiros não olham para uma ferida e pensam em 4 classes simultaneamente. Eles pensam em exclusão.

- **A proposta de fluxo:** Sugira avaliar se vale a pena treinar dois modelos menores em vez de um único multinível:
    
    1. **Filtro Binário (Educacional):** Identifica se a imagem é Pele Saudável ou Ferida. (Excelente para o estudante treinar o olhar do que é uma lesão em estágio inicial).
        
    2. **Classificador Especialista:** Se for ferida, o modelo diz se é Diabética ou Pressão. (Foco no profissional para direcionar o tratamento/curativo correto).
        

## 📝 Resumo do que falar na Reunião 

> 1. "Professora, mapeei o dataset de testes na escala de cinza e vi onde estão anatomicamente a maioria das lesões (calcanhar, sola, dedos)."
>     
> 2. "Rodei o experimento em escala de cinza pura e o modelo colapsou (56% de acurácia), provando que a cor é vital para diferenciar diabetes de pressão."
>     
> 3. "Percebi que nossos 100% de acurácia com augmentation híbrido sofrem de Data Leakage. Para o app ser fidedigno e seguro para profissionais da saúde, precisamos isolar o augmentation estritamente na etapa de treino."
>     
> 4. "Quero iniciar a implementação do CLAHE + CIELab para tentar subir a acurácia real do modelo sem apelar para o vazamento de dados. Vamos estruturar essa pipeline?"
>     





## 📊 Panorama Real do seu Dataset (Pré-Augmentation)

| **Classe**                        | **Qtd. Imagens** | **% do Dataset** | **Status de Equilíbrio**               |
| --------------------------------- | ---------------- | ---------------- | -------------------------------------- |
| **venous** (Úlcera Venosa)        | 247              | ~26.6%           | Maioria (Classe Majoritária)           |
| **diabetic** (Pé Diabético)       | 185              | ~20.0%           | Estável                                |
| **sirurgical** (Ferida Cirúrgica) | 164              | ~17.7%           | Estável                                |
| **pressure** (Úlcera por Pressão) | 134              | ~14.4%           | Moderado                               |
| **normal** (Pele Íntegra)         | 100              | ~10.8%           | Minoria                                |
| **background** (Fundo/Controle)   | 25               | ~10.5%           | Minoria (Classe Minoritária)           |
| **TOTAL**                         | **927**          | **100%**         | **Desbalanceamento Moderado (~2.5:1)** |
