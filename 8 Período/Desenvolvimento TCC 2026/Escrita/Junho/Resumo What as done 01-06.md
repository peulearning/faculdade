O que fazer , recapitular tudo que já temos,  matriz de confusão e resultados e os notebooks para então prosseguirmos com o pré-processamento.





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







## 🧩  Resposta do Pipeline de Experimentos (Augmentation)

### Arquitetura Sequencial  2 Classes

- Parâmetros da Rede


![[Pasted image 20260329152427.png]]
	Relembrar camadas ( Flatten ) ⚠️
	
*  Épocas de treinamento 

![[Pasted image 20260329161027.png]]
Treinamento , Validação ❌, Teste // Momento em que a validação está entrando 


* Gráficos de Acurácia e Erro Epóca  

![[Pasted image 20260329161050.png]]
Corrigir na lib do matplot épocas para serem exatas. ✅
Verficar a interpretação de Erro por época  (Validação) ❌


Verificar o conceito de validação com acurácia e o conceito de validação com dataset

* Matriz de Confusão

![[Pasted image 20260329161109.png]]

Observar o balanceamento 

* Resultados da Matriz

![[Pasted image 20260329161755.png]]


Link do Notebook :  [Archiceture Sequencial 2 Classes.ipynb - Colab](https://colab.research.google.com/drive/1Po3TYLkytLOaqzzR1mzIMCjxvxIT3NFi#scrollTo=JxF-LCGalwzD)

### Arquitetura Sequencial  4 Classes

* Parâmetros  das Rede

![[Pasted image 20260330005348.png]]

* Épocas de Treinamento

![[Pasted image 20260330005430.png]]

* Acurácia por Época
![[Pasted image 20260330005511.png]]

* Matriz de Confusão

![[Pasted image 20260330005619.png]]

* Resultados da Matriz

![[Pasted image 20260330005627.png]]


* Curva AUC - ROC

![[Pasted image 20260330010334.png]]

Link do Notebook : [Archiceture Sequencial 4 Classes.ipynb - Colab](https://colab.research.google.com/drive/16egOWvOuY6xhz1kSyknrVpcGt3FyE786#scrollTo=d21uBrBEl7KS)



Atenção : Até aqui posso concluir através das observações e comparação com a tabela do artigo principal, que o modelo de arquitetura sequencial atingiu acurácia superior em relação ás 4 classes e valores inferiores para 2 classes. 



### Arquitetura Sequencial 3 Classes

* Parâmetros do Modelo

![[Pasted image 20260401151605.png]]

* Treinamento do Módelo 
![[Pasted image 20260401151622.png]]

*  Acurácia & Error por Época
![[Pasted image 20260401151642.png]]

* Matriz de Confusão 

![[Pasted image 20260401151657.png]]

* AUC-ROC

![[Pasted image 20260401151729.png]]

Link do Notebook :  [Archiceture Sequencial 3 Classes.ipynb - Colab](https://colab.research.google.com/drive/1t-_Y5a3d4lvcId1C3BN_8M1P72iv77sF#scrollTo=JxF-LCGalwzD)
### Arquitetura MobileNetV2 2 Classes

*  Parâmetros da Rede 


* Treinamento ( Os pesos se dão pela Imagenet ou seja rede pré-treinada , e congela a base  começando pela cabeça, e no fine-tuning vai para as camadas da base )

![[Pasted image 20260331092616.png]]

![[Pasted image 20260331092820.png]]

* Gráfico da Acurácia
![[Pasted image 20260331092909.png]]

* Relatório de Matriz de Confusão
![[Pasted image 20260331092931.png]]

* Matriz de Confusão 
![[Pasted image 20260331092949.png]]

* Acurácia de T/V  e Perca 
![[Pasted image 20260331093433.png]]

* Curva AUC - ROC

![[Pasted image 20260331093518.png]]

Link do Notebook : [Google Colab](https://colab.research.google.com/drive/1YlaVKvoTJAH4B10OsnvZXnyHvsYByL0f#scrollTo=y0b9gs76meMW)

### Arquitetura MobileNetV2  4 Classes

* Parâmetros do Modelo

![[Pasted image 20260331114948.png]]

* Treinamento do modelo 

![[Pasted image 20260331115020.png]]

![[Pasted image 20260331115029.png]]


* Acurácia de Treinamento
*
![[Pasted image 20260331163419.png]]

* Relatório da Matriz de Confusão

![[Pasted image 20260331163442.png]]

* Matriz de Confusão

![[Pasted image 20260331163458.png]]


* Acurácia de Treinamento & Validação

![[Pasted image 20260331163519.png]]


* AUC - ROC 

![[Pasted image 20260331164049.png]]


Link do Notebook :  [Archictecture MobileNetV2 4 Classes.ipynb - Colab](https://colab.research.google.com/drive/18yvVmrL5HaAs4ya-kLaAsBHg_NDL0iF-#scrollTo=QT-4-InWXRtA)

Conclui-se então portanto que para arquitetura "MobileNetV2" o modelo conseguiu ser superior a tabela de 4 classes, cujo valor alcançou 87.50%  e nosso modelo atingiu 99,81% e quanto ao experimento de 2 Classes o nosso resultado também foi superior em comparação a tabela atingindo 99,56% de resultado.

### Arquitetura MobileNetV2 3 Classes

* Parâmetros da Rede 

![[Pasted image 20260401203709.png]]


* Treinamento da rede

![[Pasted image 20260402075039.png]]


* Evolução Acurácia ( Com épocas Ajustadas )

![[Pasted image 20260402075100.png]]


* Relatório da Matriz 

![[Pasted image 20260402075140.png]]


* Matriz de Confusão 

![[Pasted image 20260402075202.png]]


* Acurácia de Treinamento / Validação ( Épocas Ajustadas )

![[Pasted image 20260402075210.png]]


* AUC-ROC 

![[Pasted image 20260402075243.png]]

Link do Notebook : https://colab.research.google.com/drive/1vU-cAZFRS28BWuuDOTxYJULCtoPwUswj#scrollTo=gfoyf_PYATSE







## 🩶 Notebook em escala cinza Pré-processamento de imagem ( Sem Augmentation ) MobileNetV2  2 Classes 




![[Pasted image 20260601142632.png]]






##  🌟Notebook MobileNetV2 4 Classes Aplicando Grad-Cam

![[Pasted image 20260601142832.png|593]]


![[Pasted image 20260601142900.png]]