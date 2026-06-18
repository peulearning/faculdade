Mostrei pra coorientadora professora suzana, todo meu raciocínio e pensamento crítico em relação a construção do notebook.  [4_Classes_Refazendo Split DatasetOriginal Archictecture MobileNetV2 Modify Grad-Cam Apply .ipynb - Colab](https://colab.research.google.com/drive/1194PdVXnJdZDFcPZAJlS4OZo4P5wOUyu#scrollTo=CuhRF5_hGX4M) sendo mais específico esse ai.


então de acordo com as considerações dela e para adiantar a efetivação do trabalho pois o prazo se encontra curto, elaborei uma cópia do mesmo [Cópia_RF_FINETUNNING 4_Classes_Refazendo Split DatasetOriginal Archictecture MobileNetV2 Modify Grad-Cam Apply .ipynb - Colab](https://colab.research.google.com/drive/18THE0ZNAsxZIm1VLCF-t2JH2gEvMYbor#scrollTo=1xtm1JVtNvAH)

para então fazer as seguintes modificações,  remover o fine-tunning pois parece que está ocasionando problema de acordo com os gráficos. e aumentar a quantidade de épocas de treinamento de 15 para 25 há 30.  E então encaminhar pra ela os prints da matriz de confusão, relatório, gráficos de acurácia e perca e de curva auc.




AS NOVAS MODIFICAÇÕES GERARAM ISSO :

Treinamento de 25 épocas 

![[Pasted image 20260618000240.png]]

Evolução da Acurácia de Teste e Perca 

![[Pasted image 20260618001907.png]]


Parâmetros

![[Pasted image 20260618001934.png]]


Matriz de Confusão

![[Pasted image 20260618002106.png]]



Relatório de Classificação

![[Pasted image 20260618001949.png]]


Curva AUC

![[Pasted image 20260618002148.png]]



Grad-CAM Predição Errada


![[Pasted image 20260618002207.png]]



Grad-CAM Predição Correta

![[Pasted image 20260618002244.png]]