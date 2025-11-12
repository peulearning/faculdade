# 🧠 1. Contexto geral do TCC

```
"Aplicação de Visão Computacional com Apoio de Tecnologia Mobile para Detecção de Feridas"
```

| Modelo                                | Tipo                                  | Características principais                                                         | Possível aplicação                               |
| ------------------------------------- | ------------------------------------- | ---------------------------------------------------------------------------------- | ------------------------------------------------ |
| **CNN Sequencial (ConvNet clássica)** | Rede convolucional simples            | Estrutura manual, ideal para experimentos controlados e compreensão de fundamentos | Benchmark inicial (base comparativa)             |
| **MobileNetV2**                       | Rede pré-treinada (transfer learning) | Leve, eficiente e otimizada para dispositivos móveis                               | Implementação no app Android (execução local)    |
| **YOLOv3**                            | Detector de objetos                   | Realiza **detecção + classificação** simultânea                                    | Reconhecimento de múltiplas lesões em tempo real |
# 📱 2. Integração com ESP32 e Aplicativo Mobile


### 🔹 Abordagem A — Processamento **em nuvem**

- A **ESP32** apenas captura a imagem (via câmera ou módulo).
    
- A imagem é enviada via Wi-Fi para uma **API** (Flask, FastAPI, etc.) hospedada, por exemplo, em um servidor ou na nuvem (Google Cloud, AWS ou servidor institucional).
    
- O modelo (CNN, MobileNet ou YOLOv3) roda no servidor e devolve o **resultado da classificação** para o app.
    
- O **aplicativo mobile** exibe o diagnóstico ou feedback.

### 🔹 Abordagem B — Processamento **no dispositivo**

- Exporta o modelo do Colab em formato **TensorFlow Lite (.tflite)**.
    
- O app (feito em Flutter, React Native ou Android nativo) roda o modelo localmente.
    
- Assim, pode funcionar **offline** e com latência mínima.

### 🔹 Abordagem C — Comparação dos **Modelos e Artigos**


# 💬 3. Perguntas

### 🔸 Sobre os modelos:

1. **“Acha válido manter os três modelos (CNN simples, MobileNetV2 e YOLOv3) para comparação no artigo, ou devo focar em um e aprofundar mais a parte de desempenho e aplicabilidade?”**
    
2. **“Deveríamos incluir métricas específicas de detecção como _mAP_ e IoU para o YOLOv3, além da acurácia e F1-score usadas na classificação?”**
    
3. **“Na sua visão, seria mais interessante treinar o MobileNetV2 totalmente com nosso dataset ou usar apenas fine-tuning parcial?”**
### 🔸 Sobre a integração (ESP32 + App):

4. **“Você acha mais viável rodar o modelo no app (via TensorFlow Lite) ou enviar as imagens da ESP32 para uma API e processar na nuvem?”**
    
5. **“Seria interessante usar o ESP32 apenas como coletor de imagens, ou também como elemento educacional (ex: feedback visual via LED quando o modelo detecta algo)?”**
    
6. **“Como podemos garantir privacidade e segurança dos dados das imagens capturadas por pacientes/alunos?”**
### 🔸 Sobre a pesquisa e metodologia:

7. **“Seria válido realizar um estudo comparativo entre os modelos (tempo de inferência, precisão e aplicabilidade prática) e usar isso como base da análise de resultados?”**
    
8. **“Podemos incluir uma etapa de validação com estudantes de enfermagem usando o app para medir impacto educacional?”**
    
9. **“Você sugere priorizar o foco técnico (modelos e desempenho) ou educacional (usabilidade e impacto no aprendizado) na redação final?”**

## 🚀 Sugestão de próximos passos 

1. **Consolidar os resultados** dos modelos (gráficos de precisão, perda e matriz de confusão).
    
2. **Escolher um modelo principal** (provavelmente MobileNetV2 ou YOLOv3).
    
3. **Planejar o app mobile** (definir stack e integração).
    
4. **Prototipar o fluxo com ESP32 + servidor/API ou app local.**
    
5. **Preparar o capítulo de resultados e discussão** com foco nas métricas e aplicabilidade.