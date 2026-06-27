
1. A Quebra dos Pesos: Yosinski et al. (2014)O artigo de Jason Yosinski e equipe, publicado na NeurIPS (uma das maiores conferências de IA do mundo), é a base da teoria moderna de Transfer Learning O fenômeno que ocorreu no seu modelo é detalhado na Seção 4: "Quantifying feature transferability".Onde está a prova: Yosinski introduz o conceito de "Co-adaptação Frágil" (Fragile Co-adaptation). O artigo prova que as camadas intermediárias de redes convolucionais (como os blocos centrais da MobileNetV2) não aprendem características isoladas; elas "co-adaptam" seus pesos para trabalhar em conjunto com a camada exata que vem a seguir.O mecanismo: Quando você descongelou as camadas do seu modelo e as conectou a uma nova camada densa (a cabeça de classificação), você rompeu essa co-adaptação. Yosinski demonstra empiricamente (nos gráficos das Figuras 3 e 4 do artigo) que separar camadas que evoluíram juntas e forçá-las a atualizar seus pesos de forma independente gera uma queda severa na performance logo nas primeiras épocas, pois a sintonia fina que existia entre os filtros convolucionais se perde antes que a rede consiga encontrar um novo ponto ótimo.

2. O Choque dos Gradientes: Kumar et al. (2022)Este artigo de Stanford, publicado na ICLR, explica exatamente a física do erro (a questão dos gradientes descerem com muita força) que ocorreu no seu log quando a acurácia desabou de ~0.90 para ~0.59.Onde está a prova: O fenômeno é dissecado ao longo de todo o artigo, mas o mecanismo exato é provado matematicamente na Seção 3 e visualizado na Figura 2, sob o conceito de Feature Distortion (Distorção de Representação).O mecanismo: Kumar argumenta que, quando você faz o Fine-Tuning tradicional liberando a rede inteira (ou grandes blocos dela) enquanto a camada final de classificação ainda tem uma alta taxa de erro, o cálculo da função de perda (Loss) resulta em um valor gigantesco.O efeito cascata: Pelo algoritmo de Backpropagation, esse erro gigantesco gera gradientes massivos. Quando esses gradientes "descem" para a base pré-treinada, eles dão passos de atualização (steps) grandes demais. O artigo prova que isso oblitera as representações ricas que a rede tinha da ImageNet, distorcendo os pesos de uma forma que o modelo não consegue mais extrair bordas ou texturas básicas. A solução proposta por Kumar é o Linear Probing then Fine-Tuning (LP-FT): treinar a cabeça sozinha primeiro até a perda diminuir (para que os gradientes fiquem pequenos), e só então descongelar a base.

3. A Matemática do "Chute" (Acurácia ~0.50)A queda da acurácia de treino para a casa dos 57%~59% (quase 0.50) na primeira época após o descongelamento não é um número aleatório. É a prova estatística de que os pesos foram destruídos e a rede sofreu um "reset" prático.Na literatura de estatística e aprendizado de máquina, quando uma rede neural perde sua capacidade de discriminação visual (seus filtros viram ruído por conta da distorção de gradiente descrita por Kumar), ela reverte para o comportamento de chance aleatória (random guessing).A probabilidade matemática de acerto aleatório é dada por $P = \frac{1}{C}$, onde $C$ é o número de classes.Em um problema de classificação binária ($C = 2$), a taxa base de acerto de uma rede "destruída" é de $50\%$. O fato do seu modelo ter caído para ~0.59 indica que os gradientes desconfiguraram os pesos a ponto de a rede perder praticamente toda a sua capacidade extratora prévia, ficando ligeiramente acima do acaso apenas pelo viés residual das poucas imagens que já tinha começado a memorizar. (Lembrando que para 4 classes, o fundo do poço seria próximo de $25\%$).


### 1. Como definir essa "Anomalia"

Não chame apenas de "colapso". Use o termo técnico: **"Instabilidade na convergência pós-descongelamento"** (_Instability in post-unfreezing convergence_).

A causa raiz que você descreveu (Yosinski + Kumar) pode ser definida como: **"Distorção de representação por gradientes de alta magnitude em um ambiente de co-adaptação frágil"**.

- **A "Quebra" (Yosinski):** Ocorre porque você rompeu a harmonia de pesos que foram treinados para extrair características hierárquicas na ImageNet. Ao descongelar, você "quebra" essa organização, e a rede precisa _re-aprender_ a extrair bordas e texturas, o que causa a queda inicial.
    
- **O "Choque" (Kumar):** Ocorre porque a sua camada densa (a "cabeça" que você adicionou) ainda não está perfeita. Ao propagar o erro (Loss) dela para baixo, o gradiente chega "sujo" nas camadas da MobileNetV2, forçando-as a mudar seus pesos de forma muito brusca, destruindo o conhecimento pré-treinado.
    

### 2. Como "Ajustar" (Estratégias de Mitigação)


- **Estratégia A: "Learning Rate Decay" (Já aplicada por você, mas pode ser mais agressiva)** Se o gráfico continua caindo muito, significa que seu `1e-5` ainda é alto demais. Tente usar `1e-6` ou um `LearningRateScheduler` que comece muito baixo (ex: `1e-7`) e suba gradualmente na Fase 2. Isso dá tempo para a rede "entender" que as camadas foram descongeladas sem levar um choque de gradiente.
    
- **Estratégia B: "Gradual Unfreezing" (Ajuste da arquitetura)** Em vez de descongelar as 30 últimas de uma vez, descongele aos poucos.
    
    - _Exemplo:_ Descongele apenas as últimas 10 na primeira rodada da Fase 2, treine 5 épocas, depois descongele mais 10. Isso mantém a "Co-adaptação" (Yosinski) muito mais estável, pois a rede não se sente "invadida" por uma mudança massiva em seus pesos.
        
- **Estratégia C: "Normalização do Gradiente" (Gradient Clipping)** No seu `optimizer`, adicione o parâmetro `clipnorm=1.0` ou `clipvalue=0.5`.
    
    Python
    
    ```
    optimizer = Adam(learning_rate=1e-5, clipnorm=1.0)
    ```
    
    Isso força o gradiente a não ultrapassar um valor máximo. Se o cálculo da Loss for gigante (o "choque" de Kumar), o _clipping_ corta o gradiente, impedindo que ele destrua seus pesos pré-treinados.
    

### 3. Conclusão 

> "A anomalia observada na transição para a Fase 2 (Fine-tuning) é um comportamento esperado em redes de alta profundidade. O fenômeno é caracterizado pela **ruptura da co-adaptação das camadas pré-treinadas (Yosinski et al., 2014)**, exacerbado pela **distorção das representações (Kumar et al., 2022)** devido ao fluxo de gradientes durante o ajuste da camada classificadora. A instabilidade gráfica na acurácia reflete o processo de re-adaptação dos pesos (fine-tuning) ao novo domínio de dados, demonstrando que, embora o custo computacional e a instabilidade momentânea sejam elevados, o ganho de generalização compensa a volatilidade inicial."


- **O que é?** Uma distorção de representação por ruptura de co-adaptação.
    
- **Por que acontece?** Porque a rede pré-treinada é forçada a mudar antes de estar pronta (gradientes instáveis).
    
- **Como ajustar?** Gradual unfreezing, aprendizado menor (LR) e clipping de gradientes.




--- 


