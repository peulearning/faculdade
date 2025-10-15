# Pesquisa sobre o MobileNetV2

## Contexto da Descoberta

Durante o **Simpósio Internacional de IoT** e o **Congresso Latino Americano e Brasileiro de IoT**, realizado em Campinas, o professor orientador identificou uma possível solução para o meu **projeto de TCC**.  

A sugestão foi **iniciar uma pesquisa sobre a ferramenta MobileNetV2**, devido ao seu potencial de aplicação em **visão computacional móvel** e sua **eficiência em dispositivos com recursos limitados**.  

Além disso, foi levantada a possibilidade de, caso a ferramenta se mostre de fácil aprendizagem, **ofertar um minicurso sobre ela durante o Maker Day** — evento de minicursos realizado no **IFNMG Campus Januária**.

---

## Descrição Técnica — MobileNetV2

> **Resumo (por Felipe Master Mota):**  
> O **MobileNetV2** é um modelo de **rede neural convolucional (CNN)** leve, desenvolvido pelo **Google**, com **53 camadas** e **entrada de 224 × 224 pixels**.  
> Ele utiliza **convoluções separáveis em profundidade**, aplicando um filtro por canal e combinando os resultados via **convoluções pontuais**.

### Estrutura e Princípios

- Baseia-se em **convoluções separáveis em profundidade (Depthwise Separable Convolutions)**, que reduzem drasticamente o número de parâmetros.
- Utiliza **blocos Inverted Residual** e **Linear Bottlenecks**, que:
  - Melhoram a **eficiência de representação interna** da rede.
  - Reduzem **perdas de informação** ao longo das camadas.
- Possui dois tipos principais de blocos:
  - Um com **stride = [1, 1]**, que mantém o tamanho da entrada.
  - Outro com **stride = [2, 2]**, que reduz o tamanho da entrada.

---

## Aplicações na Área Médica

O MobileNetV2 tem sido amplamente utilizado em **análise de imagens médicas**, especialmente em tarefas como:

- **Classificação de lesões cutâneas;**
- **Detecção de anomalias em exames radiológicos;**
- **Segmentação de tecidos e feridas.**

Sua eficiência permite aplicação em **sistemas móveis e embarcados**, tornando-o ideal para **soluções educacionais e clínicas em campo**, como o projeto de **classificação de feridas cutâneas com apoio de tecnologia mobile**.

---

## Comparativo entre as Versões do MobileNet

| Versão | Principais Características | Vantagens |
|--------|-----------------------------|------------|
| **MobileNetV1** | Introduz convoluções separáveis em profundidade. | Reduz significativamente os parâmetros do modelo. |
| **MobileNetV2** | Adiciona blocos *Inverted Residual* e *Linear Bottleneck*. | Melhora o desempenho e a eficiência. |
| **MobileNetV3** | Otimizado via *AutoML* e módulos *SE (Squeeze-and-Excitation)*. | Equilibra precisão e velocidade, com melhor desempenho geral. |

---

## Integração com Dispositivos de Baixo Custo

> Apesar de sua complexidade, o MobileNetV2 pode ser comprimido e otimizado para rodar em microcontroladores.

Com ferramentas como **TensorFlow Lite Micro**, é possível realizar **inferências básicas no ESP32**, viabilizando:
- **Classificação de imagens simples**;
- **Detecção leve de padrões visuais**;
- **Aplicações educacionais em IoT e visão computacional**.

---

## Fontes e Leituras Recomendadas

1. [Documentação oficial — Keras (MobileNet)](https://keras.io/api/applications/mobilenet/)
2. [Artigo no Medium: *MobileNet Architectures*](https://medium-com.translate.goog/@pandrii000/mobilenet-architectures-17fe7406d794?_x_tr_sl=en&_x_tr_tl=pt&_x_tr_hl=pt&_x_tr_pto=tc&_x_tr_hist=true)
3. [Wikipedia (MobileNet)](https://en-wikipedia-org.translate.goog/wiki/MobileNet?_x_tr_sl=en&_x_tr_tl=pt&_x_tr_hl=pt&_x_tr_pto=tc)
4. [Comparativo MobileNetV2 vs V3 — LinkedIn](https://www-linkedin-com.translate.goog/pulse/mobilenet-v2-vs-mobilenet-v3-ayoub-kirouane?_x_tr_sl=en&_x_tr_tl=pt&_x_tr_hl=pt&_x_tr_pto=tc)
5. [Documentação oficial — PyTorch (MobileNetV2)](https://docs.pytorch.org/vision/main/models/mobilenetv2.html)
6. [Artigo — Viso.ai: *Efficient Deep Learning for Mobile Vision*](https://viso-ai.translate.goog/deep-learning/mobilenet-efficient-deep-learning-for-mobile-vision/?_x_tr_sl=en&_x_tr_tl=pt&_x_tr_hl=pt&_x_tr_pto=tc)

---

## Próximos Passos

- [ ] Estudar a implementação do MobileNetV2 no **TensorFlow e PyTorch**.  
- [ ] Criar um **protótipo de classificação de lesões cutâneas** com base no modelo.  
- [ ] Testar **deploy em ambiente mobile (TensorFlow Lite)** e **embarcado (ESP32)**.  
- [ ] Preparar material didático para **ofertar minicurso no Maker Day**.

---

> 📌 **Observação pessoal:**  
> O MobileNetV2 se mostra uma excelente alternativa para o projeto de **análise de lesões cutâneas** no contexto de **educação em enfermagem** e **IoT médica**, equilibrando desempenho, baixo custo computacional e aplicabilidade prática.
