[Stress_2Class_Fine_Tunning_MobileNetV2 - Colab](https://colab.research.google.com/drive/1P2G3Avg08V3bWqPPE0amRSuA3RfoBYtV#scrollTo=ggR-734W3EaX)
[Stress_2Class_SemFineTuning_MobileNetV2 - Colab](https://colab.research.google.com/drive/1Gh5ZZEXdMZhio0_acIz-xPjs3NwOQNzQ#scrollTo=t_YTJBlNJSBm)


### 1. Estratégias Abordadas

**Estratégia 1: Sem Fine-Tuning (Apenas Extração de Características)**

- Nesta abordagem, o modelo MobileNetV2 é carregado com pesos pré-treinados (provavelmente da base ImageNet).
    
- A base convolucional da rede é "congelada" (seus pesos não são atualizados durante o treinamento).
    
- Apenas a nova "cabeça" do modelo (as camadas densas finais, também chamadas de classificadores) é treinada com o seu conjunto de dados de imagens de feridas.
    
- **Vantagem**: O treinamento é muito mais rápido e o risco de overfitting grave (sobreajuste) no início é reduzido, pois a rede apenas reaproveita extratores de bordas, cores e texturas genéricas já aprendidas.
    

**Estratégia 2: Com Fine-Tuning (Ajuste Fino)**

- Nesta abordagem, você dá um passo além. Após treinar o topo da rede (ou logo em seguida), você "descongela" algumas (ou todas) as camadas convolucionais do MobileNetV2.
    
- O modelo é retreinado com uma taxa de aprendizado bem baixa (ex: 1e-5 usando o otimizador Adam).
    
- **Vantagem**: Permite que as camadas mais profundas da rede neural adaptem seus mapas de características para identificar padrões altamente específicos de tecidos de feridas (como necrose, esfacelo e granulação) em vez de objetos genéricos.
    

### 2. Comparativo de Métricas

De acordo com os dados obtidos nas saídas dos seus notebooks, observamos o seguinte comportamento:

- **Resultados do Modelo Com Fine-Tuning**:
    
    - **Acurácia**: Alcançou 0.69 (69%) no conjunto de testes para 49 amostras avaliadas.
        
    - **Métricas por classe**: O modelo se saiu melhor na classe de feridas diabéticas (Precisão de 0.69, Recall de 0.86 e F1-Score de 0.76).
        
    - No entanto, teve maior dificuldade com as lesões de pressão, obtendo um Recall mais baixo (0.48) e F1-Score de 0.57. Isso significa que ele perdeu muitas lesões de pressão, possivelmente confundindo-as com outras classes.
        
- **Resultados do Modelo Sem Fine-Tuning**:
    
    - Embora a matriz de confusão final completa tenha sido suprimida, os registros de treinamento (logs de época) mostraram uma **Acurácia de Validação (val_accuracy) de aproximadamente 0.7292 (72.9%)** e uma taxa de perda de validação bastante controlada.
        

**Análise do Comparativo:**

1. **Possível Overfitting no Fine-Tuning**: Muitas vezes espera-se que o Fine-Tuning supere a versão estática. Porém, se a acurácia de validação do modelo _Sem_ Fine-Tuning (~73%) se sustentou um pouco melhor ou de forma equivalente à acurácia de teste do _Com_ Fine-Tuning (69%), pode indicar que o descongelamento dos pesos fez o modelo decorar um pouco do conjunto de treino. Feridas possuem alta variabilidade visual e datasets pequenos sofrem rapidamente com o ajuste fino.
    
2. **Uso de pesos das classes**: No Fine-Tuning, vi que você introduziu `class_weight` (pesos de classe) para lidar com o desbalanceamento das imagens. Apesar disso, o recall para feridas de pressão se manteve baixo.
    
3. **Análise Grad-CAM**: Ambos os notebooks possuem código implementado para gerar os mapas de calor (Grad-CAM), ajudando a verificar visualmente em qual parte do tecido (ex: regiões necróticas ou bordas) a rede está focando para tomar a decisão. Esta é uma excelente estratégia clínica para justificar falsos positivos.

--- 


### 1. Indícios de Overfitting (Sobreajuste)

Em ambos os testes, é possível notar um descolamento entre a métrica de treino e a de validação.

- A **acurácia de treino (`accuracy`)** continua subindo e passa dos **90% a 93%**.
    
- A **acurácia de validação (`val_accuracy`)** atinge um teto e estaciona na casa dos **75% a 77%** (tendo um pico isolado de 81% na Fase 1 do segundo log).
    
- **O que isso significa?** O modelo está aprendendo a "decorar" as imagens de treino, mas perde um pouco da capacidade de generalizar quando vê imagens novas (dados de validação).
    

### 2. O Impacto Real da Fase 2 (Fine-Tuning)

Se olharmos para o segundo bloco de logs (Fase 1 seguida da Fase 2), o Fine-Tuning não trouxe a melhoria esperada:

- No final da **Fase 1** (época 14), seu modelo atingiu `val_accuracy: 0.8125` e `val_loss: 0.0952`.
    
- Durante a **Fase 2** (após liberar as camadas da base convolucional), a `val_accuracy` caiu ligeiramente e estabilizou em torno de **0.75 a 0.77**, enquanto o `val_loss` ficou travado na casa de **0.08X**.
    
- **Conclusão:** Descongelar os pesos da base não ajudou o modelo a aprender características mais ricas; na verdade, apenas facilitou para que a rede memorizasse ainda mais o conjunto de treinamento. Isso é muito comum em datasets médicos ou de imagens muito específicas quando o volume de dados é pequeno.
    

### 3. Instabilidade Inicial e Tamanho do Dataset

Nos primeiros passos da Fase 1 do segundo log, a acurácia de validação salta erraticamente: `0.4167` ➡️ `0.6667` ➡️ `0.5417` ➡️ `0.6042`. O `val_loss` também começa muito alto (`2.1005`).

- O fato de você ter apenas **14 steps (passos)** por época (ex: `14/14 ━ 34s`) indica que o seu dataset é bem pequeno.
    
- Em datasets pequenos, cada lote (batch) de imagens de validação tem um peso enorme na métrica. Se o modelo errar duas imagens a mais em uma época, a métrica despenca, causando essa grande instabilidade.
    

### 4. Excelente Estratégia de Learning Rate (Taxa de Aprendizado)

Um ponto muito positivo da sua abordagem foi o gerenciamento da Taxa de Aprendizado (LR):

- **ReduceLROnPlateau:** Na Fase 1, o sistema percebeu quando o modelo parou de evoluir e cortou o LR pela metade repetidas vezes (de `0.001` até `8.00e-06`). Isso ajudou a estabilizar a perda (`loss`).
    
- **LearningRateScheduler na Fase 2:** Você utilizou LRs minúsculas (como `1.00e-06` e `1.00e-05`) ao descongelar a rede. Essa é exatamente a **abordagem correta**. Se você usasse uma taxa alta na Fase 2, destruiria os pesos pré-treinados valiosos da MobileNetV2 (fenômeno conhecido como _catastrophic forgetting_).


---


### Recomendações para os Próximos Passos:

Para tentar quebrar essa barreira dos 77% e reduzir o overfitting, você pode testar o seguinte:

1. **Aumentar o Data Augmentation:** Rotacionar mais as imagens de feridas, aplicar zoom, alterar brilho e contraste, ou virá-las horizontalmente/verticalmente para forçar a rede a aprender de formas variadas. ( Verificar )
    
2. **Early Stopping:** O treinamento da Fase 2 poderia ter sido interrompido mais cedo. Implementar um `EarlyStopping` monitorando o `val_loss` com um _patience_ de 5 a 10 épocas pouparia tempo e salvaria o modelo no seu melhor momento (possivelmente no final da Fase 1). ✅ ( Já fiz isso )
    
3. **Mais Dropout:** Aumentar a taxa de Dropout nas camadas densas (ex: de 0.2 para 0.4 ou 0.5) pode forçar a rede a não depender de neurônios específicos, melhorando a generalização. ( Verificar )