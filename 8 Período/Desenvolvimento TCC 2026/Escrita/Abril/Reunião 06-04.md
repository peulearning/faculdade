
# 📅 Reunião TCC - Suzana (06/04/2026)

## 📌 Tópicos Discutidos
* **Imagens Parecidas:** Necessidade de avaliar a similaridade entre as imagens do dataset.
* **Data Augmentation:** * As técnicas de augmentation estão impactando negativamente **2 classes específicas (D, P)**, causando mais confusão do que ajudando o modelo a generalizar.
  * Ferramenta de apoio sugerida: [Albumentations Explore](https://explore.albumentations.ai).
* **Perfis das Imagens:** Foram identificados diferentes perfis visuais:
  * Vermelhas
  * Amareladas
  * Necrosadas
  * Variações de iluminação: Algumas mais claras, outras mais escuras.
* ⚠️ **Atenção (Desbalanceamento):** Existe uma quantidade muito pequena de imagens do perfil *Necrosadas*.

---

## 🔬 Hipótese Principal
> Será que os erros de predição que o modelo está cometendo estão concentrados em uma dessas subclasses de perfis visuais específicos (ex: apenas nas amareladas ou apenas nas mais escuras)?


* Selecionado para o Artigo do Congresso de Enfermagem  ( MOC )
---

## ✅ Checklist de Próximos Passos

- [x] Analisar os resultados das imagens antes e depois do *Data Augmentation* (usar o [Albumentations Explore](https://explore.albumentations.ai) como referência).
- [ ] Investigar a fundo o motivo do *Augmentation* estar confundindo as classes **D** e **P**. (Talvez testar outras transformações ou remover as atuais para essas classes).
- [ ] Validar a hipótese: mapear se os erros do modelo se concentram em imagens vermelhas, amareladas, necrosadas, claras ou escuras.
- [x] Implementar script para exibir lado a lado a imagem, o que foi predito e o rótulo real (análise a olho nu), com foco especial no cenário de 2 Classes.
- [ ] Avaliar uma estratégia para lidar com a escassez de imagens da classe/perfil *Necrosadas* (ex: coleta de novos dados, pesos nas classes, oversampling).

---

## 💻 Códigos Úteis

### Avaliação detalhada com matriz de confusão
Script para plotar a matriz de confusão e o relatório de classificação do modelo atual:

```python

from sklearn.metrics import classification_report, confusion_matrix, ConfusionMatrixDisplay
import matplotlib.pyplot as plt
import numpy as np

# ==========================================
# 1. PREDIÇÕES E AVALIAÇÃO GLOBAL
# ==========================================
y_true = test_generator.classes
y_pred_probs = modelo.predict(test_generator)
y_pred = np.argmax(y_pred_probs, axis=1)
class_names = list(test_generator.class_indices.keys())

# Matriz de Confusão
cm = confusion_matrix(y_true, y_pred)
disp = ConfusionMatrixDisplay(confusion_matrix=cm, display_labels=class_names)
disp.plot(cmap=plt.cm.Blues)
plt.title("Matriz de Confusão - Desempenho Geral")
plt.xticks(rotation=45)
plt.savefig("matriz_confusao.png", bbox_inches='tight') # Salva a imagem para o TCC
plt.show()

# Classification Report
report = classification_report(y_true, y_pred, target_names=class_names)
print("\n📊 Relatório de Classificação:\n")
print(report)

# ==========================================
# 2. ANÁLISE A OLHO NU (Filtro de Erros)
# ==========================================
print("\n👁️ Extraindo imagens classificadas incorretamente para validação da hipótese...\n")

# Encontrar os índices onde o modelo errou
erros_indices = np.where(y_pred != y_true)[0]

if len(erros_indices) == 0:
    print("O modelo acertou todas as predições no conjunto de teste!")
else:
    # Pegar até 10 erros para visualizar
    num_erros_mostrar = min(10, len(erros_indices))
    indices_para_mostrar = erros_indices[:num_erros_mostrar]
    
    plt.figure(figsize=(15, 6))
    plt.suptitle("Análise de Erros: Real vs Predito (Avaliando Perfis de Cor/Necrose)", fontsize=16, y=1.05)
    
    # Como test_generator pode não dar acesso direto à imagem por índice facilmente,
    # vamos buscar os caminhos dos arquivos de teste que falharam
    filepaths = test_generator.filepaths
    
    for i, idx in enumerate(indices_para_mostrar):
        plt.subplot(2, 5, i + 1)
        
        # Carrega a imagem original que o modelo errou
        img_path = filepaths[idx]
        img = plt.imread(img_path)
        plt.imshow(img)
        
        true_label = class_names[y_true[idx]]
        pred_label = class_names[y_pred[idx]]
        
        plt.title(f"Real: {true_label}\nPred: {pred_label}", color="red", fontweight='bold')
        plt.axis("off")

    plt.tight_layout()
    plt.savefig("analise_erros_visuais.png", bbox_inches='tight') # Salva a imagem para o TCC
    plt.show()
    ```
``` 

---

## 🧩  Resposta do Pipeline de Experimentos (Sem Augmentation)

### Arquitetura Sequencial  2 Classes

* Parâmetros da Rade 
![[Pasted image 20260408101718.png]]

* Treinamento 
![[Pasted image 20260408101736.png]]

* Acurácia / Perca
![[Pasted image 20260408101750.png]]

* Matriz 

![[Pasted image 20260408101805.png]]

* Relatório da Matriz

![[Pasted image 20260408101819.png]]

* Curva ROC
![[Pasted image 20260408101835.png]]


### Arquitetura Sequencial  4 Classes

* Parâmetros da Rede
![[Pasted image 20260408102143.png]]

* Treinamento da Rede

![[Pasted image 20260408102156.png]]

* Acurácia / Perca
![[Pasted image 20260408102206.png]]

* Matriz

![[Pasted image 20260408102218.png]]

* Relatório 

![[Pasted image 20260408102227.png]]

* Curva ROC
![[Pasted image 20260408102247.png]]
### Arquitetura Sequencial 3 Classes

* Parâmetros da Rede

![[Pasted image 20260408102328.png]]


* Treinamento da Rede

![[Pasted image 20260408102344.png]]


* Acurácia / Perca

![[Pasted image 20260408102356.png]]


* Matriz

![[Pasted image 20260408102408.png]]


* Relatório 

![[Pasted image 20260408102418.png]]

* Curva ROC
![[Pasted image 20260408102435.png]]


### Arquitetura MobileNetV2 2 Classes

* Parâmetros da Rede

![[Pasted image 20260408094646.png]]




* Treinamento 

![[Pasted image 20260408094617.png]]


* Relatório da Rede
![[Pasted image 20260408094927.png]]

* Matriz de Confusão

![[Pasted image 20260408094949.png]]

* Análise 

![[Pasted image 20260408095355.png]]


* Acurácia / Perca

![[Pasted image 20260408095413.png]]

*  Curva ROC

![[Pasted image 20260408095428.png]]


### Arquitetura MobileNetV2  4 Classes

* Parâmetros da Rede

![[Pasted image 20260408080556.png]]

* Treinamento da rede 

![[Pasted image 20260408080625.png]]

* Matriz de Confusão

![[Pasted image 20260408080656.png]]


* Relatório de Classificação

![[Pasted image 20260408080713.png]]

*  Análise das Imagens ( Diferencial ) 

![[Pasted image 20260408080736.png]]


* Acurácia / Perca

![[Pasted image 20260408083838.png]]


* Curva ROC

![[Pasted image 20260408083858.png]]

### Arquitetura MobileNetV2 3 Classes

* Parâmetros da Rede

![[Pasted image 20260408091155.png]]

* Treinamento da Rede 

![[Pasted image 20260408091320.png]]


* Relatório da Matriz
![[Pasted image 20260408091311.png]]

* Matriz 

![[Pasted image 20260408091344.png]]

* Análises de Imagens

![[Pasted image 20260408091403.png]]


* Acurácia e Perca

![[Pasted image 20260408091416.png]]

*  Curva ROC 
![[Pasted image 20260408091449.png]]


--- 

## Estudo de Caso sobre a Augmentation




![[Pasted image 20260408133745.png]]

e essa :


![[Pasted image 20260408135740.png]] 


```

E possível Notar : Classes que estão sendo impactadas pela Augmentation ( Diabetic , Pressure) ou seja está ajudando confudir mais que esclarecer.

  

  

Para valida oque foi notado eu removi a augmentation e o modelo caiu em relação a acurácia porém ficou mais coerente pelo gráfico. Oque me gerou a hipótese.

  

Hipótese :

  

Será que os erros que o modelo possui são de uma dessas sub classe em específico ?   

  

Perfil das Imagens identificadas diferentes perfis visuais :

  

Vermelhas, amareladas/ Esbranquiçadas , Necrosadas, Variações de Iluminação.

  

Desbalanceamento : Há mais imagens Necrosadas ? Ou mais Amareladas ? Ou mais vermelhadas ?

  

O que eu preciso fazer ?

  

- [✅] Quantificar os sub grupos que a gente observou.

  

- [✅] Do tipo, qual a porcentagem de erros são da feridas ( Necrosadas, Avermelhadas, Amareladas / Esbranquiçadas )

  

- [✅] Para que possamos verificar a quantidade dessas variedades se tem alguma que é maior que a outra.

  

  

LEMBRANDO QUE ESTOU FALANDO DO GOOGLE COLAB , e estou utilizando o drive para visualizar as pastas após o treinamento de toda a a rede.

```


--- 



## Lembrando que essa é uma análise baseada nas imagens de erro exibidas:

### 1. Amareladas / Esbranquiçadas (Esfacelo/Exsudato)

Este é o grupo mais frequente entre os erros. O modelo parece ter uma dificuldade enorme aqui, muitas vezes confundindo _Pressure_ com _Diabetic_ quando há esse tom.

- **Quantidade estimada:** **10 a 11 imagens**.
    
- **Exemplos claros:** As imagens com IDs `119_0`, `11_0`, `27_0`, `145_0`, `89_0` e as três últimas da terceira fileira.
    
- **Insight:** O esfacelo (tecido amarelo) é muito comum em ambos os tipos de ferida, o que explica por que a Augmentation de cor pode estar "melando" a distinção que o modelo tenta fazer.
    

### 2. Avermelhadas (Tecido de Granulação / Sangue)

Imagens onde o vermelho vivo predomina, indicando tecido vivo ou sangramento recente.

- **Quantidade estimada:** **5 a 6 imagens**.
    
- **Exemplos claros:** `10_0`, `35_0`, `83_0` e a primeira da segunda fileira.
    
- **Insight:** Note que algumas feridas bem vermelhas (granulação limpa) estão sendo classificadas como _Pressure_ indevidamente.
    

### 3. Necrosadas (Escuras / Pretas)

Tecido morto, geralmente preto ou marrom muito escuro.

- **Quantidade estimada:** **3 imagens**.
    
- **Exemplos claros:** `125_0` (claramente necrose no calcanhar), `101_0` (ponto central necrótico) e `138_0` (necrose parcial lateral).
    
- **Insight:** A necrose é um marcador forte. Se o seu dataset de treino tiver muito mais necrose em uma classe do que em outra, o modelo "vicia" nessa cor.
    

### 4. Variações de Iluminação / Background

- **Quantidade estimada:** **1 a 2 imagens**.
    
- **Exemplos claros:** A primeira imagem da última fileira (fundo muito escuro e pele negra, o que altera o contraste) e a `25_3` (com flash forte).
    

---

### 5.  A planilha (`HealScan_Analise_Erros.csv`):

Para o seu TCC, você pode usar esses números para montar um gráfico de barras. Com base no que vi, sua tabela ficaria mais ou menos assim:

| **Perfil Visual**             | **Qtd de Erros** | **% Aproximada** |
| ----------------------------- | ---------------- | ---------------- |
| **Amareladas (Esfacelo)**     | 11               | 55%              |
| **Avermelhadas (Granulação)** | 6                | 30%              |
| **Necrosadas (Tecido Morto)** | 3                | 15%              |

**Conclusão:**

Os dados visuais confirmam que o **perfil amarelado** é o maior vilão. Se o _Data Augmentation_ que você usou estava alterando matiz (_hue_) ou saturação, ele provavelmente estava transformando feridas vermelhas em amareladas artificialmente durante o treino. Isso explica por que, ao remover o augmentation, o gráfico ficou mais "coerente": o modelo parou de ver "fantasmas amarelados" onde eles não existiam na vida real.


-- -

### 6. A Prova do Desbalanceamento e da Complexidade



- No modelo Sequencial, o F1-Score (que equilibra Precisão e Recall) de `diabetic` foi **0.68**, contra **0.54** de `pressure`.
    
- No MobileNetV2, o F1-Score de `diabetic` subiu para **0.75**, contra **0.63** de `pressure`.
    

Isso prova categoricamente ao seu orientador que o problema não é _apenas_ a arquitetura da rede, mas a **natureza dos dados**. Independentemente do modelo ser simples ou complexo, ele sempre tem um desempenho cerca de 12 a 14 pontos pior na classe de pressão. Isso valida a nossa tese: a classe `pressure` tem menos imagens e uma variabilidade visual muito maior, tornando-a estatisticamente e visualmente mais difícil de ser aprendida.

### 7. Dissecando a Classe de Pressão (O Calcanhar de Aquiles)

Vamos olhar especificamente para o que a MobileNetV2 fez pela classe mais difícil (`pressure`):

- **Recall (Sensibilidade) de 0.62:** O modelo conseguiu encontrar 62% de todas as úlceras de pressão reais do teste. O modelo Sequencial estava com **0.52** (praticamente jogando uma moeda, errando metade).
    
- **Precision (Precisão) de 0.65:** Quando a MobileNetV2 aponta e diz "Isso é uma úlcera de pressão!", ela está correta 65% das vezes. O modelo Sequencial acertava apenas 55%.

![[Pasted image 20260409194618.png]]

MobileNetV2 tirou o modelo do "chute cego" e começou a realmente traçar uma fronteira de decisão geométrica para as úlceras de pressão, graças aos pesos pré-treinados (Transfer Learning) que extraem texturas melhores.

### 3. O Salto de Desempenho Geral (A Vitória do Transfer Learning)


- **Acurácia e F1-Score:** O modelo pulou de uma acurácia de **62% para 70%**. O `macro avg` do F1-score (que calcula a média tratando as duas classes com o mesmo peso, ignorando o desbalanceamento) pulou de **0.61 para 0.69**.

- **A Realidade:**  Uma acurácia de 70% no contexto da saúde  ainda não é ideal para um diagnóstico clínico final (onde esperaríamos algo acima de 85-90%). **No entanto**, para um dataset pequeno de apenas cerca de 50 imagens de teste e sem metadados clínicos (idade do paciente, local da ferida, diabetes confirmada), é um resultado **muito promissor**.

## Analisando antes do SPLIT as imagens

Dentre os problemas encontrados alguns listados :

- Primeiro modelo (CNN) sofreu com variação de luz artificial.
    
- O segundo modelo melhorou, mas você descobriu que as imagens estavam sofrendo distorção geométrica no pré-processamento.
    
-  Implementei o `ImageOps.pad` para preservar o formato real da lesão, o que refinou o aprendizado das características de borda (que é o que você testará na próxima rodada).

