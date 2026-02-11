 O que foi discutido na pauta de hoje e que será necessário trazer para o trabalho de forma coesa e bem escrita. Dentre os pontos levantados vem:
 
  "Artigos para entender sobre os hardwares dos smartphones, cujo objetivo é saber quais são os dispositivos ideais para obter imagens para utilizar como teste de um cenário real da aplicação."
  
  "Explicar qual modelo dentre os avaliados irá melhor atender a necessidade do aplicativo,  e deixar claro o porque se é eficiência, tempo de resposta ( latência ), capacidade ou tamanho do modelo".

Será necessário enfatizar todo o processo que foi percorrido, desde a problemática onde o início foi trabalhar com tratamento de feridas alinhado a tecnologia.  Então este feito partiu da  exploração as práticas convencionais integradas  a tecnologias. Durante a pesquisa de artigos encontrei referências tais como SkinGPT-4 e Integrated image and location analysis for wound classification: a deep learning approach, dentre elas suas referências e outros artigos cujo foram explroados. O meu primeiro desafio era encontrar uma base de dados cujo me fornecesse as imagens de feridas sendo elas rotulada para melhor compreensão e aplicação. 

Esse foi meu ponto de partida, durante a leitura e compreensão dos artigo encontrei as bases de dados cujo foram utilizada  nos trabalhos relacionados https://github.com/uwm-bigdata/Multi-modal-wound-classification-using-images-and-locations  e https://www.medetec.co.uk/files/medetec-image-databases.html? . Além das tecnologias mencionadas no campo da Inteligência Artificial ( Visão Computacional ) Python com framework Keras / TensorFlow . A ideia inicial foi um modelo de arquitetura sequencial treinado do zero em seguida ao pesquisar mais artigos parti para modelos de aprendizagem por transferência.

A construção dos modelos partiu da ideia de explorar as tecnologias e aplicar em busca de melhores resultados em comparação com os artigos, utilizando treinamento, validação e teste ( foi como a base de dados foi particionada) , e métricas para avaliação dos modelos Acurácia, Precisao, F1-Score e  Recall. Partindo deste ponto, surgiu a ideia de integrar o modelo cujo obtivesse a melhor performance para integrar com a smartphones ( ou dispositivos de baixo poderio computacional ), visando um aplicativo protótipo que funcione de forma off-line. Durante esse processo foi encontrado desafios e limitações como variação de iluminação, perspectiva e ruídos limites de memória ou apresentar comportamento instável durante sua execução., tais quais me fizeram indagar as seguintes questões : 

Q1 . "Caso não tenha internet como vai funcionar?", 
Q2. "Se os smartphones não tiverem uma qualidade de imagem ou resolução adequada ?" ,
Q3. "Qual arquitetura utilizar para que o tempo de resposta seja rápido ? ", 
Q.4 "Quais tecnologias são utilizadas para classificação de imagem ?", 
Q5. " Como que funciona a parte matemática dos algoritmos a serem implementados ?" 
Q6. "Quais parâmetros são utilizados  e quais ajustar ?".
Q7."Oque os algoritmos enxergam nas imagens ?",
Q8."O que são padrões de feridas ou quais padrões o modelo interpreta  ?"
Q9."O que difere a classe  "normal" das outras ?"



Com esses dois artigos a busca de uma base de dados cujo me pudesse me fornecer imagens de feridas foi concluída.  E assim pude dar início ao desenvolvimento de modelos de visão computacional com objetivo em classificação de feridas.  Dentre as classes eram Background ( não feridas ),  Normal ( pele sem ferida ) , Diabetic ( feridas diabéticas ), Pressure ( feridas por pressão ), Venous  ( feridas venosas ), Sirurgical ( feridas cirúrgicas ).

Entretanto e necessário a compreensão e domínio do tema,  descrever como cada modelo avaliado funciona, quais seus parâmetros, como as imagens que foram preditas oque modelo enxerga, utilizando algumas bibliotecas do python como Grad-CAM , mapa de calor para ver oque os modelos interpretavam, tornando assim algo visível também para uma pessoa cujo não é especialista enxergar oque o modelo está interpretando e utilizando para classificar .
