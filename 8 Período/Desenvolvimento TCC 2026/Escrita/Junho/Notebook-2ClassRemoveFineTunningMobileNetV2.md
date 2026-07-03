
[Stress_2Class_SemFineTuning_MobileNetV2 - Colab](https://colab.research.google.com/drive/1Gh5ZZEXdMZhio0_acIz-xPjs3NwOQNzQ#scrollTo=gfoyf_PYATSE)

### Revisão Estratégica: 2 Classes SEM Fine-Tuning (Feature Extraction)

**1. A Arquitetura da "Cabeça" (Dual Pooling e Funil Denso)**

- **A Estratégia:** A base da MobileNetV2 foi mantida 100% congelada. No topo da rede, implementou-se o _Dual Pooling_ (concatenando a média geral da imagem, via `GlobalAveragePooling2D`, com os picos de textura extrema, via `GlobalMaxPooling2D`). A seguir, utilizou-se um "funil" profundo (ex: camadas Densas maiores com `BatchNormalization` e `Dropout`).
    
- **O Porquê:** Como a base convolucional está totalmente travada, a rede não pode aprender formas ou texturas novas. Toda a responsabilidade de "entender" a ferida recai sobre as camadas finais. O _Dual Pooling_ duplica a quantidade de informação visual que a cabeça recebe, enquanto as camadas densas profundas garantem que a rede tenha parâmetros (neurónios) suficientes para traduzir os filtros genéricos do Google em classificações médicas.
    

**2. Treinamento em Fase Única**

- **A Estratégia:** O modelo é treinado do início ao fim numa única fase, apenas ajustando os pesos da cabeça customizada, geralmente usando uma Taxa de Aprendizado (Learning Rate) mais alta no início (como `1e-3`) com `EarlyStopping`.
    
- **O Porquê:** Reduz drasticamente o custo computacional e o tempo de treino. Mais importante ainda: elimina completamente o risco de _overfitting_ severo nas camadas convolucionais. O modelo é forçado a generalizar utilizando apenas as regras de visão que a MobileNetV2 aprendeu ao processar milhões de imagens.
    

**3. Manutenção das Funções de Contenção de Viés**

- **A Estratégia:** Continuar a utilizar alta resolução (320x320), a função `CategoricalFocalCrossentropy` e o balanço de pesos (`class_weights`).
    
- **O Porquê:** Mesmo com apenas 2 classes (Diabética vs. Pressão), a assimetria na quantidade de dados (mais casos diabéticos do que de pressão) cria um forte viés. Sem estas técnicas, o modelo, mesmo congelado, tenderia a ignorar a classe minoritária.
    

### O Comparativo (O Embate Direto no TCC)

Quando as execuções terminarem, o seu capítulo de resultados focará em responder à grande questão do seu trabalho: **"Afinal, vale a pena destrancar as camadas da MobileNetV2 para classificar feridas quando se tem um dataset muito pequeno?"**

Aqui está o que deve analisar e como estruturar a comparação entre os dois modelos de 2 Classes:

**1. Estabilidade e Overfitting (Olhe para as Gráficas de Val Loss)**

- **Fine-Tuning:** Costuma mostrar uma _loss_ de validação nervosa, que pode subir de forma abrupta logo após descongelar a rede (sintoma de _overfitting_).
    
- **Sem Fine-Tuning:** Costuma apresentar uma curva de validação muito mais suave e linear, estabilizando-se sem solavancos.
    
- _O que destacar no texto:_ O modelo "Sem Fine-Tuning" é matematicamente mais seguro e previsível para o uso clínico.
    

**2. O Comportamento do Recall (Quem descobre a Lesão de Pressão?)**

- O verdadeiro teste da sua IA não é a acurácia global, mas o _Recall_ da classe `pressure` (a mais difícil).
    
- _Cenário A:_ Se o Fine-Tuning alcançar 80% na de pressão e o "Sem Fine-Tuning" alcançar 60%, significa que, com 2 classes, os filtros locais da MobileNetV2 conseguem de facto adaptar-se à pele humana.
    
- _Cenário B:_ Se o "Sem Fine-Tuning" vencer no acerto da pressão, isso é a prova irrefutável (o "prego no caixão") de que o _Esquecimento Catastrófico_ ocorre mesmo em tarefas binárias simples.
    

**3. O Custo-Benefício Computacional**

- _O que destacar no texto:_ O "Sem Fine-Tuning" treina com uma fração do tempo e esforço de hardware. Num contexto de hospitais com poucos recursos ou processamento _mobile_, um modelo leve e estático que performe tão bem ou melhor que o _Fine-Tuning_ é uma vitória da engenharia de software aplicada à saúde.



