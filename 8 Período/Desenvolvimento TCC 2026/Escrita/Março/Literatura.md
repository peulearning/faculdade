
# 1. Revisões científicas (mostram o estado da arte geral)

### 1️⃣ “Deep learning in chronic wound segmentation: a comprehensive review and meta-analysis” (2025)

Este é um dos artigos mais completos para **referencial teórico**.

Principais pontos:

- Revisão sistemática de pesquisas de **2015–2023**
    
- Analisa métodos de **Deep Learning para segmentação de feridas crônicas**
    
- Discute arquiteturas como:
    
    - **U-Net**
        
    - **SegNet**
        
    - **DeepLab**
        
    - **Mask R-CNN**
        

Conclusão do artigo:

- **CNNs são o padrão dominante na análise automática de feridas**
    
- **segmentação semântica** é a principal abordagem para monitoramento clínico.
    

 [Deep learning in chronic wound segmentation: a comprehensive review and meta-analysis | The Visual Computer | Springer Nature Link](https://link.springer.com/article/10.1007/s00371-025-04133-y?)

> “Estado da arte em detecção automática de feridas”.

---

### 2️⃣ “CNN-Based Wound Segmentation: A Review of Models and Performance Evaluation” (2025)

Esse estudo compara diversos modelos de **segmentação de feridas usando CNN**.

Modelos avaliados:

- **U-Net**
    
- **SegNet**
    
- **MobileNetV2**
    
- variantes de **U-Net**
    

O trabalho mostra que **arquiteturas encoder-decoder como U-Net apresentam melhor desempenho em segmentação médica**. https://journals.stmjournals.com/ctsp/article%3D2025/view%3D197539/

---

# 2. Artigos de estado da arte com novas arquiteturas

Esses são **papers recentes propondo novos modelos**.

---

## 3️⃣ Dual Attention U-Net (2025)

**Paper**

“Wound Segmentation with U-Net Using a Dual Attention Mechanism and Transfer Learning”

Principais contribuições:

- combina **VGG16 + U-Net**
    
- utiliza **attention mechanisms**
    
- treinado em imagens de **diabetic foot ulcer**
    

Resultados:

- **Dice Score: 94.1%**
    
- **IoU: 89.3%**

[Wound Segmentation with U-Net Using a Dual Attention Mechanism and Transfer Learning | Journal of Imaging Informatics in Medicine | Springer Nature Link](https://link.springer.com/article/10.1007/s10278-025-01386-w?)

Isso mostra que **variantes de U-Net continuam sendo estado da arte em segmentação de feridas**.

---

## 4️⃣ SwishRes-U-Net (2025)

Paper:

“SwishRes-U-Net: A deep neural architecture for chronic wound segmentation”

Contribuições:

- arquitetura baseada em **ResNet + U-Net**
    
- projetada para **úlcera diabética**
    
- melhora a segmentação de bordas irregulares.
    

O trabalho destaca que a **segmentação da área da ferida é essencial para avaliar o processo de cicatrização**.


[SwishRes-U-Net: A deep neural architecture for chronic wound segmentation - ScienceDirect](https://www.sciencedirect.com/science/article/abs/pii/S1746809424011066?)
---

## 5️⃣ RACL-U-Net (2026)

Paper recente:

“Novel wound image segmentation with enhanced global context and Adaptive Channel-Aware Normalization”

Melhorias propostas:

- **Residual connections**
    
- **attention mechanism**
    
- **ConvLSTM**
    

Objetivo:

- melhorar a **delimitação da borda da ferida em imagens complexas**.

[Novel wound image segmentation with enhanced global context and Adaptive Channel-Aware Normalization - ScienceDirect](https://www.sciencedirect.com/science/article/abs/pii/S0957417425042228?)

---

# 3. Papers que combinam detecção + segmentação

Esses são muito relevantes para sistemas reais.

---

## 6️⃣ Detect-and-Segment (2022)

Paper:

“Detect-and-Segment: A Deep Learning Approach to Automate Wound Image Segmentation”

Pipeline:

Detecção da ferida  
       ↓  
Recorte da região  
       ↓  
Segmentação da ferida

Resultados:

- melhora significativa na precisão de segmentação
    
- **MCC aumentou de 0.29 para 0.85** ao combinar detecção e segmentação.
    

Esse modelo inspirou muitos pipelines atuais.


[Detect-and-segment: A deep learning approach to automate wound image segmentation - ScienceDirect](https://www.sciencedirect.com/science/article/pii/S2352914822000375?)
---

# 4. Pesquisas mais recentes (2025) — novas tendências

## 7️⃣ WoundNet-Ensemble (2025)

Arquitetura baseada em:

- **ResNet-50**
    
- **Vision Transformer**
    
- **Swin Transformer**
    

Resultados:

- **99.9% accuracy** na classificação de tipos de ferida
    
- sistema para **monitoramento da cicatrização**.
    

Mostra a tendência atual:

➡ **CNN + Transformers**


[[2512.18528] WoundNet-Ensemble: A Novel IoMT System Integrating Self-Supervised Deep Learning and Multi-Model Fusion for Automated, High-Accuracy Wound Classification and Healing Progression Monitoring](https://arxiv.org/abs/2512.18528)

---

## 8️⃣ Zero-Shot Diffusion Model para segmentação (2025)

Paper:

“Zero-Shot Diabetic Foot Ulcer Segmentation with Diffusion Models”

Inovação:

- segmentação **sem dataset rotulado**
    
- usa **modelos de difusão + self-attention**
    

Resultados:

- **IoU ≈ 86%**
    
- alta precisão em múltiplos datasets.

[[2504.17628] Beyond Labels: Zero-Shot Diabetic Foot Ulcer Wound Segmentation with Self-attention Diffusion Models and the Potential for Text-Guided Customization](https://arxiv.org/abs/2504.17628)

---

# 5. Benchmark de modelos (2025)

Paper:

“Deep Learning for Wound Tissue Segmentation: A Comprehensive Evaluation”

Esse estudo comparou:

- **82 modelos de segmentação**
    
- incluindo:
    
    - U-Net
        
    - GAN
        
    - FPN
        
    - ResNet
        

Resultado importante:

- **FPN + VGG16 foi o melhor modelo no dataset testado**.

[[2502.10652] Deep Learning for Wound Tissue Segmentation: A Comprehensive Evaluation using A Novel Dataset](https://arxiv.org/abs/2502.10652?)

---
