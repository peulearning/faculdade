[Stress_2Class_Fine_Tunning_MobileNetV2 - Colab](https://colab.research.google.com/drive/1P2G3Avg08V3bWqPPE0amRSuA3RfoBYtV#scrollTo=ae20gvqTjCuD)


### Revisão Estratégica: 2 Classes COM Fine-Tuning

**1. A Arquitetura da "Cabeça" (Classificador Leve)**

- **A Estratégia:** No topo da MobileNetV2, utilizou-se uma estrutura mais simples: `GlobalAveragePooling2D` $\rightarrow$ `Dropout` $\rightarrow$ `Dense (128 neurónios)` $\rightarrow$ `Dropout` $\rightarrow$ `Dense (2 neurónios)`.
    
- **O Porquê:** Ao planear usar _Fine-Tuning_, o modelo assume que as próprias camadas convolucionais da MobileNetV2 se vão adaptar para aprender as texturas médicas. Portanto, não era necessário construir um classificador extremamente complexo (como o _Dual Pooling_ ou funis densos de 512/256). A rede faz o trabalho pesado na base, e a cabeça apenas toma a decisão final.
    

**2. O Treino Dividido em Duas Fases**

- **A Estratégia:** A Fase 1 treinou apenas as camadas densas por 15 épocas. A Fase 2 (Fine-Tuning) libertou uma secção da base da MobileNetV2 com uma Taxa de Aprendizado (_Learning Rate_) mais baixa.
    
- **O Porquê:** Prevenção de "Esquecimento Catastrófico". Se as camadas da base fossem descongeladas logo no início, a aleatoriedade inicial da nova camada `Dense(128)` enviaria cálculos de erro absurdos para o modelo base, destruindo os filtros visuais do ImageNet. A Fase 1 estabiliza a matemática, enquanto a Fase 2 lapida os detalhes.
    

**3. Manutenção da Resolução (320x320) e Pesos das Classes**

- **A Estratégia:** A entrada do modelo foi configurada para `320x320` e aplicou-se o `class_weight_dict` no `model.fit()`.
    
- **O Porquê:** Manter a resolução alta foi vital para garantir que, quando a Fase 2 abrisse a rede para aprender, houvesse precisão em píxeis suficiente para diferenciar as bordas de uma úlcera de pressão das de uma úlcera diabética. Os `class_weights` forçaram a rede a dar igual atenção à classe minoritária desde a primeira época.
    

### O que esperar do Comparativo com a versão "Sem Fine-Tuning"?

Ao desenvolver agora o modelo **2 Classes SEM Fine-Tuning**, preste atenção a esta diferença fundamental na filosofia do seu teste:

1. **A Filosofia do Fine-Tuning:** Confiou na capacidade da MobileNetV2 de "moldar" os seus filtros internos originais para se focar exclusivamente em lesões médicas de duas classes.
    
2. **A Filosofia do "Sem Fine-Tuning" (Feature Extraction):** O modelo vai assumir que os filtros do Google já são perfeitos. Vai congelar tudo e colocar o fardo da aprendizagem na sua nova **Cabeça Robusta** (_Dual Pooling_ + 512 + 256).
    

**A Grande Hipótese Científica:**

No cenário anterior de 6 classes, o modelo sofria um "colapso" quando tentava fazer _Fine-Tuning_ porque 6 doenças exauriam a capacidade (parâmetros) da MobileNetV2. **No entanto, num cenário de apenas 2 classes (Diabética vs. Pressão), a complexidade é drasticamente menor.** É extremamente possível que, neste caso específico de 2 classes, o _Fine-Tuning_ consiga igualar ou até mesmo superar o _Feature Extraction_ sem sofrer de _overfitting_ severo.