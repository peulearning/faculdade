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

- Exporta o modelo do Colab em formato **Notebook Colab  (.colab)**.
    
- Capturar os resultados mais importantes para rede convulsionais.

- Storytelling 
# 💬 3. Perguntas

### 🔸 Sobre os modelos:

1. **“Acha válido manter os três modelos (CNN simples, MobileNetV2 e YOLOv3) para comparação no artigo, ou devo focar em um e aprofundar mais a parte de desempenho e aplicabilidade?”**
     Sim.

### 🔸 Sobre a integração (ESP32 + App):

4. **“Você acha mais viável rodar o modelo no app (via TensorFlow Lite) ou enviar as imagens da ESP32 para uma API e processar na nuvem?”**
	 Salvar o modelo e usa-lo no Tensor Flow Lite.

### 🔸 Sobre a pesquisa e metodologia:

7. **“Seria válido realizar um estudo comparativo entre os modelos (tempo de inferência, precisão e aplicabilidade prática) e usar isso como base da análise de resultados?”**
 Sim , interessantíssimo.

9. **“Você sugere priorizar o foco técnico (modelos e desempenho) ou  (usabilidade e impacto no aprendizado) na redação final?”**
Abraçar a trajetória e também a sua usabilidade.

# 🦾 4. Dicas para Futuras Reuniões

## 1. Salvar Modelos
- **Formato**: Salve os modelos ao término da execução em formatos como `.h5`, TensorFlow Lite, etc.
- **Métricas**: Exiba as métricas através de um carregamento para comparação entre os modelos:
  - Precisão
  - Recall
  - F1-Score
- **Visualização de Erros**: Tente visualizar imagens de erros e acertos, se possível.

## 2. Obter Métricas dos Modelos
- **Dados de Teste**: Coletar métricas com base nos dados de teste (embasamento através das características observáveis):
  - Comparar o que foi identificado ou predito pelo modelo.
  - Analisar acertos e erros.

## 3.Decisão com Orientador
- **Consulta**: Decidir, em conjunto com o orientador, quais próximos passos serão seguidos.

## 4. Escrita do Projeto
- **Reflexão**: Pense na escrita do projeto e no desenvolvimento do aplicativo mobile.