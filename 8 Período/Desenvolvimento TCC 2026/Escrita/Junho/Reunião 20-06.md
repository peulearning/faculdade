#RespostaConsensus

```

Questionamentos do Professor : 

Felipe:  Exato o mobilenet é o foco, por isso que falei pelo tempo. Mas antes de mexer procurar referências pra ver se tratam realmente isso do mobilenet piorar um pouco o resultado por “compactar o modelo”
 
 
 Felipe : Pq isso gera um ponto de discussão que a gente pode ter.  O fato de mobilenet piorar um pouco é uma dataset com poucas amostras.

```
 


## Não, **compactação do MobileNetV2** nem sempre **derruba a acurácia**; o efeito depende da técnica e da intensidade.

Em cenários de imagens médicas, **pruning**, quantização e arquiteturas leves frequentemente preservam grande parte do desempenho, mas perdas relevantes aparecem quando a compressão é agressiva ou mal ajustada, sobretudo com poucos dados e sem fine-tuning/augmentação [Lee2025](https://consensus.app/papers/toward-efficient-cancer-detection-on-mobile-devices-lee-park/66a735dc35285233b17e0741cefb9098/)[Zhang2025](https://consensus.app/papers/an-adaptive-model-compression-method-combining-pruning-zhang-du/104a9f1e062a5470afed10f16a4a52b2/)[Patel2021](https://consensus.app/papers/quantizing-mobilenet-models-for-classification-problem-patel-chaware/3dc72a73e2635949858c1222270cd568/).

## Impacto da Compressão no MobileNetV2

|Evidence Strength|Claim|
|---|---|
|Strong|**Pruning + quantização moderados** costumam reduzir tamanho e latência com **pequena perda** de desempenho no MobileNetV2; um estudo relatou 30% de pruning + 8-bit com apenas **1% de perda de acurácia** [Zhang2025](https://consensus.app/papers/an-adaptive-model-compression-method-combining-pruning-zhang-du/104a9f1e062a5470afed10f16a4a52b2/), e outro manteve **F1 entre 0.97–0.99** com MobileNetV2 reduzido até **2.82 MB** [Lee2025](https://consensus.app/papers/toward-efficient-cancer-detection-on-mobile-devices-lee-park/66a735dc35285233b17e0741cefb9098/).|
|Moderate|**Quantização isolada** pode causar **queda grande de acurácia** quando mal calibrada; em retinografia, MobileNetV2 quantizado caiu de **96% para 77%** [Patel2021](https://consensus.app/papers/quantizing-mobilenet-models-for-classification-problem-patel-chaware/3dc72a73e2635949858c1222270cd568/).|
|Moderate|**Modelos leves ou comprimidos** às vezes igualam ou superam o baseline quando combinados com **fine-tuning, augmentação ou redesign arquitetural**, mas isso não reflete compressão “gratuita” e sim um pipeline melhor [A.2023](https://consensus.app/papers/a-novel-endtoend-deep-convolutional-neural-network-based-a-chamola/cf2c901b48c25d5fb9c02a77315c6c06/)[Asif2024](https://consensus.app/papers/skincnet-an-efficient-lightweight-deep-learning-model-for-asif-ain/a281d07c46c652349207066c72035de6/)[Neelam2026](https://consensus.app/papers/late-feature-fusion-of-lightweight-deep-learning-neelam-verma/2fe509a9913f551e9e0808fcabd59d32/).|

Figure 1 Força da evidência sobre compressão e desempenho

## Quando a Acurácia Se Mantém

A evidência mais consistente sugere que **compressão moderada** tende a preservar desempenho quando vem com ajuste fino conjunto. Em histopatologia, MobileNetV2 com pruning e quantização manteve **F1 de 0.97–0.99** com forte redução de tamanho e tempo de inferência [Lee2025](https://consensus.app/papers/toward-efficient-cancer-detection-on-mobile-devices-lee-park/66a735dc35285233b17e0741cefb9098/). Em outro trabalho, combinar pruning não estruturado com quantização 8-bit em MobileNetV2 produziu só **1% de perda** com 30% de poda [Zhang2025](https://consensus.app/papers/an-adaptive-model-compression-method-combining-pruning-zhang-du/104a9f1e062a5470afed10f16a4a52b2/).

Também há exemplos em que a compactação coexistiu com boa performance clínica. Um MobileNetV2 otimizado para tumores hepáticos caiu de **99.6% para 94.5%**, mas reduziu bastante o tamanho e ganhou velocidade em tempo real [Ja'afaru2026](https://consensus.app/papers/quantizationaware-pruned-mobilenetv2-for-realtime-liver-jaafaru-kana/9664744862de5ca19a44b886d19a84ed/). Em tarefas de câncer em mobile, pruning com quantização foi descrito como especialmente eficaz para MobileNetV2, mantendo alta eficiência e desempenho [Lee2025](https://consensus.app/papers/toward-efficient-cancer-detection-on-mobile-devices-lee-park/66a735dc35285233b17e0741cefb9098/).

## Quando Piora

- **Quantização sozinha** pode degradar fortemente o resultado; em retinografia, MobileNetV2 caiu de 96% para **77%** [Patel2021](https://consensus.app/papers/quantizing-mobilenet-models-for-classification-problem-patel-chaware/3dc72a73e2635949858c1222270cd568/).
- **Compressão agressiva** tende a aumentar o risco de perda, como sugerem quedas observadas após poda forte em versões MobileNet [Lee2025](https://consensus.app/papers/toward-efficient-cancer-detection-on-mobile-devices-lee-park/66a735dc35285233b17e0741cefb9098/).
- Em revisão de modelos móveis para lesões, a ênfase em **compatibilidade mobile** e redes pequenas foi associada a **classificação menos precisa** em alguns estudos [Laouarem2025](https://consensus.app/papers/jildyanet-an-efficient-lightweight-multiclass-laouarem-kara-mohamed/16b44419d86f5a799f82c928a15a5808/).

## Fatores Mais Importantes Com Poucos Dados

Com **poucas amostras**, o efeito da compactação depende muito mais do pipeline do que da redução em si. Estudos em lesões de pele repetidamente associam bom desempenho de modelos leves ao uso de **data augmentation**, tratamento de desbalanceamento e tuning cuidadoso [Asif2024](https://consensus.app/papers/skincnet-an-efficient-lightweight-deep-learning-model-for-asif-ain/a281d07c46c652349207066c72035de6/)[Hu2024](https://consensus.app/papers/mobilefriendly-skin-lesion-detection-using-an-hu-yang/53bfe29afbbe5b7991ed2a17177d6169/)[Laouarem2025](https://consensus.app/papers/jildyanet-an-efficient-lightweight-multiclass-laouarem-kara-mohamed/16b44419d86f5a799f82c928a15a5808/). Há também evidência de que MobileNetV2 pode continuar competitivo em cenários pequenos e compactos quando a redução é controlada, como em MRI com **253 imagens**, onde MobileNetV2 com PCA superou VGG16 no equilíbrio entre acurácia e compacidade [Nasution2026](https://consensus.app/papers/efficiency-vs-accuracy-a-comparative-analysis-of-nasution-furqan/a6c0aa2081b85bdc9130c463eafa52e6/).

Arquiteturas leves redesenhadas às vezes superam baselines maiores ou MobileNetV2 padrão, mas isso costuma vir de **melhor desenho de rede**, atenção, fusão de features ou destilação, não apenas de “encolher” o modelo [Ding2023](https://consensus.app/papers/himvit-a-lightweight-model-for-explainable-skin-disease-ding-yi/b0021518131056d2810411ba82541a13/)[Li2025](https://consensus.app/papers/lightweight-localglobal-fusion-for-robust-multiclass-li-zhou/865af0753227501a9f4dda2abc91c6c6/)[Islam2024](https://consensus.app/papers/leveraging-knowledge-distillation-for-lightweight-skin-islam-hasib/fb39410207bf5cf4ad079ba96c87413b/).

Em resumo, em **detecção/classificação de feridas com poucos dados**, compactação do MobileNetV2 **não necessariamente piora** os resultados, mas quantização mal calibrada e compressão agressiva podem sim causar quedas substanciais. A literatura sugere que o melhor cenário é **compressão moderada + fine-tuning + augmentação + validação robusta**.

---


## 💻 Notebook Arquitetura Sequencial 2 Classes   


[2 _Classes_RefazendoSPLIT_Archiceture Sequencial 2 Classes Modify.ipynb - Colab](https://colab.research.google.com/drive/1C-WqnyNu1Gk0EGBdW9Mqw_dTXQhBLITW#scrollTo=W-DbAX6dwwHZ) 

[notebooks_tcc/2__Classes_RefazendoSPLIT_Archiceture_Sequencial_2_Classes_Modify.ipynb at main · peulearning/notebooks_tcc](https://github.com/peulearning/notebooks_tcc/blob/main/2__Classes_RefazendoSPLIT_Archiceture_Sequencial_2_Classes_Modify.ipynb)



* Sumário do Modelo 

![[Pasted image 20260622153023.png]]

* Treinamento do Modelo 
![[Pasted image 20260622153043.png]]

* Acurácia Validação / Perca

![[Pasted image 20260622153123.png]]


* Matriz de Confusão

![[Pasted image 20260622153142.png]]


* Curva ROC

![[Pasted image 20260622153156.png]]


* Grad-cam Mal Classificadas 
![[Pasted image 20260622153223.png]]


* Grad-cam Bem Classificadas 

![[Pasted image 20260622153248.png]]




## 💻 Notebook Arquitetura Sequencial 4 Classes 

[notebooks_tcc/4__Classes_RefazendoSPLIT_Archiceture_Sequencial_4_Classes_Modify.ipynb at main · peulearning/notebooks_tcc](https://github.com/peulearning/notebooks_tcc/blob/main/4__Classes_RefazendoSPLIT_Archiceture_Sequencial_4_Classes_Modify.ipynb)


[4 _Classes_RefazendoSPLIT_Archiceture Sequencial 4 Classes Modify.ipynb - Colab](https://colab.research.google.com/drive/1afoOXXJDqOxUtXTwBIO3JjAfc9f4vSTR#scrollTo=rJNPHl8nmXZ9)



* Sumário 

![[Pasted image 20260622165251.png]]


* Treinamento

![[Pasted image 20260622180031.png]]


* Acurácia / Perca

![[Pasted image 20260622180044.png]]

* Matriz de Confusão
![[Pasted image 20260622180100.png]]

* Curva ROC

![[Pasted image 20260622191046.png]]


* Grad-cam Mal-Classificados

![[Pasted image 20260622222303.png]]

 * Grad-cam Bem-Classificados

![[Pasted image 20260622222318.png]]



---

## 💻 Notebook Arquitetura Sequencial 6 Classes 


[notebooks_tcc/6__Classes_RefazendoSPLIT_Archiceture_Sequencial_Classes_Modify.ipynb at main · peulearning/notebooks_tcc](https://github.com/peulearning/notebooks_tcc/blob/main/6__Classes_RefazendoSPLIT_Archiceture_Sequencial_Classes_Modify.ipynb)


[6 _Classes_RefazendoSPLIT_Archiceture Sequencial Classes Modify.ipynb - Colab](https://colab.research.google.com/drive/1d84YQPFd8VO_mh8uzffeMRTeALQ9V-a4)



* Súmario

![[Pasted image 20260622234543.png]]


* Treinamento

![[Pasted image 20260622234604.png]]

* Acurácia Treinamento / Perca

![[Pasted image 20260622234734.png]]


* Matriz de Confusão

![[Pasted image 20260622234806.png]]


* Curva ROC

![[Pasted image 20260622234821.png]]


---


## 💻 Notebook Arquitetura Sequencial 3 Classes 


--- 

## 💻 Notebook Arquitetura MobileNetV2 6 Classes

