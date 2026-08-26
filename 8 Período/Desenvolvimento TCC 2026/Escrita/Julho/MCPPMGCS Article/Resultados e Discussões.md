# O que eu faria

Na minha opinião, toda a seção de Resultados deve responder **quatro perguntas**.

---

# 4.1 Impacto da configuração do conjunto de dados

Primeiro resultado.

Você começou com

- 6 classes

Depois

- 5 classes

Depois

- 4 classes.

O leitor precisa perceber isso.

Você pode fazer uma tabela única.

|Configuração|Melhor Accuracy|
|---|---|
|6 Classes|75%|
|6 Classes Ajustado|76%|
|5 Classes|84%|
|4 Classes (DP)|84%|
|4 Classes (DV)|97%|

Essa tabela já conta metade da história.

Depois discutir.

Exemplo.

> Observa-se que a redução do número de classes proporcionou aumento progressivo da acurácia. O melhor desempenho foi obtido utilizando quatro classes (Diabetic + Venous), alcançando 97% de acurácia na estratégia de particionamento 80/20 com Fine-Tuning. Esse comportamento sugere que a diminuição da complexidade do problema favoreceu a capacidade discriminativa da arquitetura.

Essa discussão é excelente.

---

# 4.2 Influência das estratégias de particionamento

Aqui entra

70/15/15

↓

70/30

↓

75/25

↓

80/20

Você tem dados suficientes.

Tabela simples.

|Split|Melhor Accuracy|
|---|---|
|70/15/15|88|
|70/30|93|
|75/25|93|
|80/20|97|

Depois discutir.

Você já sabe o motivo.

Dataset pequeno.

Mais imagens para treino.

Maior capacidade de generalização.

Esse é um dos principais resultados do TCC.

---

# 4.3 Avaliação do Fine-Tuning

Esse talvez seja o tópico mais rico.

Porque seus resultados mostram uma coisa interessante.

O Fine-Tuning

nem sempre melhorou.

Na verdade.

Vamos olhar.

Você teve

Sem Fine melhor

↓

muitas vezes.

Fine melhor

↓

algumas vezes.

Empate

↓

uma vez.

Isso merece uma tabela resumida.

|Resultado|Quantidade|
|---|---|
|Fine melhor|8|
|Sem Fine melhor|11|
|Empate|1|

Olha como isso é interessante.

Depois discutir.

Você pode escrever algo como

> Embora o Fine-Tuning tenha proporcionado melhorias em alguns cenários, principalmente na configuração com quatro classes (Diabetic + Venous) e particionamento 80/20, sua aplicação não resultou em ganhos consistentes em todos os experimentos. Esse comportamento evidencia que, para conjuntos de dados limitados, o descongelamento das camadas convolucionais pode aumentar a suscetibilidade ao sobreajuste, reduzindo a capacidade de generalização do modelo.

Essa discussão é muito forte.

---

# 4.4 Melhor modelo obtido

Agora você mostra.

Notebook 33.

Esse merece destaque.

Tabela completa.

Accuracy

Precision

Recall

F1.

Depois

Matriz de Confusão.

Depois

Comparação com Patel.

Comparação com Anisuzzaman.

Fim.

---

# Percebe uma coisa?

Você realizou 40 notebooks.

Mas o artigo pode usar somente

4 tabelas.

Talvez

3 figuras.

---

# Eu até reduziria as tabelas

Hoje você tem

mais de dez tabelas.

Eu deixaria somente estas.

---

### Tabela 1

Impacto do número de classes

|Classes|Melhor Accuracy|
|---|---|
|6|75|
|6 Ajustado|76|
|5|84|
|4 DP|84|
|4 DV|97|

---

### Tabela 2

Impacto do Split

|Split|Melhor Accuracy|
|---|---|
|70/15/15|88|
|70/30|93|
|75/25|93|
|80/20|97|

---

### Tabela 3

Fine × Sem Fine

|Resultado|Quantidade|
|---|---|
|Fine venceu|8|
|Sem Fine venceu|11|
|Empate|1|

---

### Tabela 4

Melhor Modelo

|Accuracy|Precision|Recall|F1|
|---|---|---|---|
|97|98|97|97|

---

# Uma coisa que eu descobri olhando seus resultados

Tem um padrão que talvez você mesmo ainda não tenha percebido.

Sua pesquisa mostra três evidências:

> Quanto menor o número de classes, maior foi a acurácia.

↓

> Quanto maior o conjunto destinado ao treinamento (até 80%), melhores foram os resultados.

↓

> O Fine-Tuning foi vantajoso apenas em cenários específicos, não sendo superior de forma consistente.


---

Minhas palavras 






