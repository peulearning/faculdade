
# 📝 Estrutura para Futuros Artigos

## Identificação e Autoria

- **Campos Obrigatórios:** Autores, Instituições envolvidas e E-mails.
    
- **Local da Pesquisa:** A escrita deve sempre detalhar o "Local onde a pesquisa foi realizada".
    
- **Ordem dos Autores:** Sempre respeitar a hierarquia estrutural:
    
    1. Autor principal
        
    2. Colaboradores
        
    3. Orientador
        

## Título

- **Regra:** O Título deve ser sempre coerente com o conteúdo e a pesquisa desenvolvida.
    

## Resumo (Abstract)

O resumo deve contextualizar toda a pesquisa de forma concisa. Deve conter obrigatoriamente:

- Objetivo geral da pesquisa.
    
- Noção da Metodologia utilizada.
    
- Noção dos Resultados (dar destaque aos principais achados).
    
- Conclusão singela/direta acompanhada de possíveis propostas para trabalhos futuros.
    

## Introdução

- **Parágrafo Inicial:** Iniciar a seção transformando o _objetivo geral_ do trabalho no primeiro parágrafo.
    
- **Clareza:** O texto da introdução deve deixar extremamente claro o que o artigo busca apresentar e resolver.
    

---

# 📌 To-Do List do Projeto

## 1️⃣ Exploração e testes do modelo

-  Continuar **testando e explorando o modelo de classificação de feridas**. ❌
    
-  Verificar se o modelo realmente está **gerando a melhor resposta possível**. ❌
    
-  Testar **variações do modelo** para verificar se há melhoria nos resultados. ❌
    
-  Avaliar se **reduzir o tamanho do modelo** afeta a performance (pensando no uso em mobile). ❌
    
-  Realizar testes comparativos entre versões do modelo. ✅
     

---

## 2️⃣ Documentação dos experimentos

-  **Documentar todos os experimentos realizados**.
    
-  Registrar:
    
    - Arquitetura do modelo ✅
        
    - Dataset utilizado ✅
        
    - Métricas obtidas (accuracy, precision, recall, etc.) ✅
        
-  Criar **tabelas comparativas entre experimentos**. ⚠️
    

---

## 3️⃣ Comparação com a literatura

-  Analisar **artigos científicos sobre classificação de feridas**.
    
-  Identificar:
    
    - Quais **modelos de Deep Learning** são mais utilizados.
        
    - Quais **métricas e métodos de avaliação** são aplicados.
        
-  Comparar seu modelo com os **modelos utilizados nos artigos**.
    

---

## 4️⃣ Análise do ambiente de execução

-  Estudar **limitações de execução em dispositivos móveis**. ❌
    
-  Verificar: ❌
    
    - consumo de memória
        
    - tamanho do modelo
        
    - tempo de inferência
        
-  Pesquisar **frameworks ou recomendações para IA em mobile**. ❌
    

---

## 5️⃣ Definição do problema de classificação

Definir claramente qual abordagem será usada:

-  Classificação entre **tipos de feridas**  
    Exemplo:
    
    - Úlcera diabética
        
    - Úlcera venosa
        

OU

-  Classificação **binária**  
    Exemplo:
    
    - Ferida diabética **(sim ou não)**
        

---

## 6️⃣ Tratamento de dados e classes

-  Garantir que o dataset tenha também **imagens de background**. ✅
    
-  Incluir imagens: 
    
    - sem feridas ✅
        
    - com ambiente clínico ✅
        
-  Explicar no TCC por que essas imagens ajudam no treinamento. ❌
    

---

## 7️⃣ Diferencial do trabalho

-  Destacar que o seu trabalho é voltado para **aplicação mobile**. ✅
    
-  Comparar com trabalhos que usam **ambientes computacionais tradicionais**. ❌
    
-  Mostrar os **desafios de rodar IA em dispositivos móveis**. ❌
    

---

## 8️⃣ Produção científica

-  Reunir resultados experimentais. 
    ✅
-  Organizar dados para:
    
    - TCC 
        
    - possíveis **artigos científicos ou publicações**. ❌


--- 

# 📌 To-Do List  2 do Projeto

- [ ] **Otimização e Compreensão do Modelo:**
    
    - Entender a fundo o comportamento e como o modelo atual funciona.
        
    - Identificar os parâmetros que estão sendo utilizados e os que podem ser modificados para trazer melhorias.
        
    - Testar pré-processamento nas imagens (ex: mudar escala para cinza, alterar cores) e verificar se afeta o modelo positivamente.    

- [x] **Comparativo de Arquiteturas:**
    
    - Esboçar um comparativo estruturado entre as arquiteturas.
        
    - Comprovar, dentre os modelos testados, qual apresenta a melhor performance para o cenário do projeto.
        
- [x] **Validação da Coleta de Dados (Ética):**
    
    - Validar a ideia de _apenas coletar_ imagens de bases prontas em vez de realizar a captura diretamente (fotografar pacientes), removendo assim a funcionalidade de captura para não esbarrar em burocracias do conselho de ética.
        
- [ ] **Análise de Características (Feridas):**
    
    - Explicar tecnicamente o que diferencia uma "ferida" de uma "não ferida" para a rede.
        
    - Documentar as semelhanças encontradas nas feridas que o modelo já classifica/identifica corretamente.
        
- [ ] **Teste de Combinação de Modelos (YOLO + MobileNetV2):**
    
    - Investigar se é possível/viável utilizar o **YOLO** para a detecção (gerando as _bounding boxes_) e passar o recorte para a **MobileNetV2** fazer a classificação (identificação).
	    
	 - Buscar em literaturas,  qual método mais utilizado e mais indicado na questão de identificação e detecção de feridas. 

