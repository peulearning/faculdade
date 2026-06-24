
# Prof. Felipe (23/06)

## 📚 Revisão de Literatura (Pesquisa)
- [ ] **Anomalia na Acurácia pós Fine-Tuning:** Pesquisar artigos que expliquem o seguinte cenário: o modelo sofreu perda catastrófica (*catastrophic forgetting*) durante o *fine-tuning*, porém a acurácia geral de testes ficou **maior** do que no modelo sem *fine-tuning*. O que justifica esse comportamento?
- [ ] **Estratégia de Split de Dados:** Buscar referências na literatura sobre divisão de dados em *datasets* limitados/pequenos. É mais adequado manter a divisão clássica (Treino / Validação / Teste) ou simplificar apenas para (Treino / Teste)?

## 🛠️ Ajustes de Modelo e Código
- [ ] **Baseline sem Fine-Tuning:** Verificar a necessidade de fazer ajustes nos parâmetros do modelo base (sem aplicar *fine-tuning*).
- [ ] **Adição de Novas Classes:** Analisar se a inclusão das duas novas classes (feridas cirúrgicas e venosas) exige refatoração de código, ajuste de parâmetros ou mudança na arquitetura.
- [ ] **Ajuste Fino por Tipo de Ferida:** Avaliar se as estruturas físicas e padrões visuais específicos das feridas cirúrgicas e venosas exigem adequações na rede Sequencial ou na MobileNetV2. Fatores a revisar:
  - Parâmetros da rede.
  - Hiperparâmetros de treinamento.
  - Estratégias de *Data Augmentation* (aumentação de dados).

## 📌 Observações e Insights da Reunião
> **Análise de Ruído nas Arquiteturas:**
> - **Rede Sequencial:** Os ruídos gerados/percebidos são muito baixos (pouco perceptíveis).
> - **MobileNetV2:** Os ruídos são consideravelmente mais notáveis. Avaliar o impacto disso nas predições.