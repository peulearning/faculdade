
Datasets disponíveis : 

Repositório : [uwm-bigdata/wound-segmentation: code and data for wound image segmentation](https://github.com/uwm-bigdata/wound-segmentation) 

[uwm-bigdata / Repositories](https://github.com/uwm-bigdata?tab=repositories) 
### 1. Eff-ReLU-Net: a deep learning framework for multiclass wound classification (Ullah et al., 2025) [1.1](https://consensus.app/papers/effrelunet-a-deep-learning-framework-for-multiclass-wound-ullah-javed/62d6dca67a775f62966306913a70482f/)

- **Problema**: classificação multiclass de feridas crônicas (tipos de feridas). [1.2](https://consensus.app/papers/effrelunet-a-deep-learning-framework-for-multiclass-wound-ullah-javed/62d6dca67a775f62966306913a70482f/)
- **Modelo**: EfficientNet‑B0 modificado (Eff‑ReLU‑Net, troca Swish por ReLU + 3 FC layers). [1.3](https://consensus.app/papers/effrelunet-a-deep-learning-framework-for-multiclass-wound-ullah-javed/62d6dca67a775f62966306913a70482f/)
- **Datasets + onde baixar**: AZH e **Medetec Wound Database**, declarados como “publicly available”; Medetec é conhecido por estar online, mas o artigo não dá URL explícito no trecho disponível. [1.4](https://consensus.app/papers/effrelunet-a-deep-learning-framework-for-multiclass-wound-ullah-javed/62d6dca67a775f62966306913a70482f/)
- **Métricas**:
    
    - Medetec: acurácia 92,33%, precisão 97,66%, recall 95,33%, F1 96,48%. [1.5](https://consensus.app/papers/effrelunet-a-deep-learning-framework-for-multiclass-wound-ullah-javed/62d6dca67a775f62966306913a70482f/)
    - AZH: acurácia 90%, precisão 89,45%, recall 92,19%, F1 90,84%. [1.6](https://consensus.app/papers/effrelunet-a-deep-learning-framework-for-multiclass-wound-ullah-javed/62d6dca67a775f62966306913a70482f/)
    
- **Train/val/test & pré-processamento**: o resumo indica uso de várias técnicas de augmentação (rotações fixas, rotação aleatória, translação), mas não informa splits explicitamente. [1.7](https://consensus.app/papers/effrelunet-a-deep-learning-framework-for-multiclass-wound-ullah-javed/62d6dca67a775f62966306913a70482f/)
- **Limitações (a partir do texto)**
    
    - Autores destacam necessidade de melhora em eficiência computacional, generalização entre corpora e robustez a variações de imagem. [1.8](https://consensus.app/papers/effrelunet-a-deep-learning-framework-for-multiclass-wound-ullah-javed/62d6dca67a775f62966306913a70482f/)
    
- **Observação sobre dataset**: AZH + Medetec declarados como “publicly available” (sem menção a restrição de licença no trecho). [1.9](https://consensus.app/papers/effrelunet-a-deep-learning-framework-for-multiclass-wound-ullah-javed/62d6dca67a775f62966306913a70482f/)

	Artigo : https://link.springer.com/content/pdf/10.1186/s12880-025-01785-z.pdf 

1. https://www.medetec.co.uk/files/medetec-image-databases.html 
2. https://github.com/uwm-bigdata/wound-classification-using-images-and-locations


--- 


### 2. Alexnet architecture variations with transfer learning for classification of wound images (Eldem et al., 2023) [2.1](https://consensus.app/papers/alexnet-architecture-variations-with-transfer-learning-eldem-lker/31cba64bcd1c5dff8895990d35bb3b1f/)

- **Problema**: classificação de imagens de feridas de pressão e diabéticas. [2.2](https://consensus.app/papers/alexnet-architecture-variations-with-transfer-learning-eldem-lker/31cba64bcd1c5dff8895990d35bb3b1f/)
- **Modelo**: variações de AlexNet (3, 4, 6 camadas de convolução) com Softmax vs SVM; melhor: 6Conv_SVM. [2.3](https://consensus.app/papers/alexnet-architecture-variations-with-transfer-learning-eldem-lker/31cba64bcd1c5dff8895990d35bb3b1f/)
- **Datasets + links**:
    
    - **Medetec Wound Database** explicitamente descrito como público; URL não aparece no resumo, apenas menção de que é “public medetec dataset”. [2.4](https://consensus.app/papers/alexnet-architecture-variations-with-transfer-learning-eldem-lker/31cba64bcd1c5dff8895990d35bb3b1f/)
    - Um “new original Wound Image Database” proprietário (sem indicação de acesso público). [2.5](https://consensus.app/papers/alexnet-architecture-variations-with-transfer-learning-eldem-lker/31cba64bcd1c5dff8895990d35bb3b1f/)
    
- **Métricas**:
    
    - Base própria (pressão + diabética): melhor modelo 6Conv_SVM com acurácia 98,85%, sensibilidade 98,86%, especificidade 99,42%. [2.6](https://consensus.app/papers/alexnet-architecture-variations-with-transfer-learning-eldem-lker/31cba64bcd1c5dff8895990d35bb3b1f/)
    - Medetec (diabéticas + pressão): acurácia 95,33%, sensibilidade 95,33%, especificidade 97,66%. [2.7](https://consensus.app/papers/alexnet-architecture-variations-with-transfer-learning-eldem-lker/31cba64bcd1c5dff8895990d35bb3b1f/)
    
- **Train/val/test & pré-processamento**: resumo não fornece splits nem passos detalhados de pré‑processamento.
- **Limitações (implícitas)**
    
    - Base própria é relativamente pequena (227 fotografias), o que pode limitar generalização. [3.1](https://consensus.app/papers/pressure-injury-ulcer-stage-classification-based-on-huang-hung/99c54501e54e50c98f164a683dce665f/)
    - Foco principal em feridas de pressão/diabéticas – não cobre outros tipos.
    
- **Observação sobre dataset**
    - Medetec explicitamente público; o “new original Wound Image Database” não é descrito como público.

	Artigo : https://www.sciencedirect.com/science/article/pii/S2215098623001684?via%3Dihub



