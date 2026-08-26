# 1. "Me apresente o desafio ."


> "Desenvolvi uma API REST em Java 21 utilizando Spring Boot para simular um conector de reservas de hotéis. A ideia foi reproduzir um cenário semelhante ao de integrações com parceiros como Omnibees ou Booking.
> 
> A aplicação suporta duas formas de recebimento de reservas: através de um processo de polling, executado periodicamente por um Scheduler, e através de um endpoint REST que simula um webhook enviado por um parceiro.
> 
> Internamente utilizei uma arquitetura em camadas, separando controller, service, repository, DTOs, entidades, mappers, configuração de segurança e integração externa. Para persistência utilizei PostgreSQL com Spring Data JPA e implementei autenticação Basic Authentication utilizando Spring Security."

---

# 2. "Por que você escolheu essa arquitetura?"


> "Eu procurei seguir uma arquitetura em camadas porque ela facilita manutenção, testes e evolução do projeto.
> 
> O Controller recebe a requisição.
> 
> A Service concentra toda a regra de negócio.
> 
> O Repository fica responsável apenas pelo acesso aos dados.
> 
> Os DTOs evitam expor diretamente as entidades.
> 
> E utilizei MapStruct para reduzir código manual na conversão entre DTOs e entidades."

---

# 3. "Como funciona o polling?"



```
Scheduler
     │
     ▼
OmnibeesClient
     │
GET /mock/omnibees/reservations
     │
Lista de reservas
     │
ReservationService
     │
Banco
```



> "A cada 30 segundos o Scheduler executa uma tarefa programada.
> 
> Essa tarefa chama um cliente HTTP responsável por consumir um endpoint que simula um parceiro externo.
> 
> Cada reserva retornada é enviada para a camada de serviço, que decide se deve inserir, atualizar ou cancelar a reserva."

---

# 4. "Como você evitou duplicidade?"



> "Utilizei o campo reservationId como identificador de negócio.
> 
> Antes de inserir uma reserva faço uma consulta pelo reservationId.
> 
> Se ela não existir, insiro.
> 
> Se existir, atualizo os dados.
> 
> Dessa forma a operação se torna idempotente e não são criados registros duplicados."

Esse termo ("idempotência") chama atenção positivamente.

---

# 5. "Como funciona o cancelamento?"

> "O requisito solicitava cancelamento lógico.
> 
> Portanto eu não removo o registro do banco.
> 
> Apenas altero o status para CANCELLED, atualizo a data de cancelamento e preservo todo o histórico da reserva."

---

# 6. "Por que MapStruct?"

> "Poderia fazer a conversão manualmente, porém o MapStruct gera esse código em tempo de compilação.
> 
> Isso reduz código repetitivo, melhora a legibilidade e evita erros de mapeamento."

---

# 7. "Por que DTO?"

Outra pergunta clássica.

> "Para desacoplar a API da entidade do banco.
> 
> Assim posso alterar a estrutura interna sem impactar os consumidores da API.
> 
> Também facilita validações e impede exposição de campos que não deveriam ser enviados ao cliente."

---

# 8. "Como está a segurança?"

> "Todos os endpoints da API estão protegidos por Basic Authentication utilizando Spring Security.
> 
> Apenas o endpoint interno de mock foi liberado para permitir que o Scheduler simulasse um parceiro externo sem necessidade de autenticação.
> 
> Em um ambiente real esse endpoint não existiria dentro da própria aplicação."



---

# 9. "Por que você criou um mock interno?"



> "Como o desafio não disponibilizava um parceiro real, optei por criar um endpoint interno que simula a resposta de uma OTA.
> 
> Dessa forma consegui validar toda a lógica de polling sem depender de serviços externos."

---

# 10. "Por que PostgreSQL?"

> "Porque é um banco relacional bastante utilizado em produção, possui boa integração com Spring Data JPA e atende perfeitamente aos requisitos do desafio."

---

# 11. "Como você tratou exceções?"

> "Implementei um tratamento global utilizando @ControllerAdvice.
> 
> Dessa forma todas as exceções da aplicação são centralizadas e retornam respostas padronizadas para a API."

---

# 12. "Como você faria isso em produção?"



> "Eu substituiria o mock interno pela URL do parceiro.
> 
> Utilizaria Flyway para versionamento do banco.
> 
> Adicionaria testes unitários e de integração.
> 
> Configuraria Prometheus e Grafana para observabilidade.
> 
> Também colocaria a aplicação em containers Docker e utilizaria variáveis de ambiente para as configurações sensíveis."

---

# 13. "O que você faria diferente?"



> "Uma evolução interessante seria implementar uma arquitetura baseada em interfaces para suportar múltiplos parceiros.
> 
> Cada parceiro implementaria uma interface ReservationIntegration, permitindo adicionar novas integrações sem alterar a lógica existente, seguindo o princípio Open/Closed do SOLID."



---

# 14. "Por que não implementou o desafio extra?"



> "Priorizei entregar uma solução funcional, organizada e aderente aos requisitos obrigatórios. Depois disso eu começaria a evoluir para múltiplos parceiros e testes automatizados."


---

# 15. Se perguntarem sobre SOLID

Você já utilizou alguns princípios.

**SRP**

> Cada classe possui uma responsabilidade.

Controller

↓

Service

↓

Repository

↓

Mapper

↓

Scheduler

↓

Integration

---

**OCP**




> "Hoje o projeto possui apenas uma integração, mas eu evoluiria para uma interface ReservationIntegration permitindo adicionar Booking, Expedia e outros parceiros sem alterar a lógica existente."

---

# 16. Pergunta técnica difícil

> "Por que você escolheu polling ao invés de mensageria?"

Resposta:

> "Porque o desafio exigia polling. Em produção eu avaliaria o uso de filas como RabbitMQ ou Kafka quando fosse necessário maior escalabilidade ou processamento assíncrono."

---

# 17. Pergunta final

> "Tem algo que você gostaria de melhorar no projeto?"

Resposta:

> "Sim. Implementaria Flyway para migrações versionadas, testes automatizados com JUnit e Mockito, integração contínua, observabilidade com Prometheus e Grafana e uma arquitetura para múltiplos parceiros baseada em interfaces."


---


# 1. Arquitetura em Camadas

### Problema

> "Era necessário organizar a aplicação para evitar que regras de negócio ficassem misturadas com código HTTP ou acesso ao banco."

### Solução

> "Separei a aplicação em Controller, Service, Repository, DTO, Mapper e Config."

### Benefício

> "Isso facilita manutenção, reutilização de código e torna o projeto mais simples de evoluir."

---

# 2. DTO

### Problema

> "Não queria expor diretamente as entidades do banco."

### Solução

> "Criei DTOs específicos para entrada e saída da API."

### Benefício

> "A API fica desacoplada da estrutura do banco e posso evoluir a entidade sem quebrar os consumidores."

---

# 3. MapStruct

### Problema

> "A conversão manual entre DTO e entidade gera muito código repetitivo."

### Solução

> "Utilizei MapStruct para gerar os mapeamentos automaticamente."

### Benefício

> "O código fica menor, mais legível e com menos chance de erro."

---

# 4. Scheduler

### Problema

> "O desafio exigia buscar reservas periodicamente."

### Solução

> "Implementei um Scheduler executado a cada 30 segundos."

### Benefício

> "A aplicação passa a consumir automaticamente reservas sem necessidade de intervenção manual."

---

# 5. Polling

### Problema

> "As reservas poderiam chegar de parceiros que não enviam webhooks."

### Solução

> "Criei um cliente HTTP que consulta periodicamente um endpoint."

### Benefício

> "O sistema consegue integrar parceiros que trabalham apenas com consulta periódica."

---

# 6. Basic Authentication

### Problema

> "Os endpoints não poderiam ficar públicos."

### Solução

> "Configurei Spring Security utilizando Basic Authentication."

### Benefício

> "Apenas clientes autenticados conseguem acessar a API."

---

# 7. Idempotência

Essa é uma das respostas mais fortes.

### Problema

> "Uma mesma reserva poderia ser enviada diversas vezes."

### Solução

> "Antes de inserir verifico se já existe uma reserva com o mesmo reservationId."

### Benefício

> "Evito registros duplicados e garanto uma operação idempotente."

---

# 8. Cancelamento Lógico

### Problema

> "O desafio exigia manter o histórico das reservas."

### Solução

> "Ao invés de excluir, altero o status para CANCELLED e registro a data."

### Benefício

> "Preservo o histórico para auditoria e futuras consultas."

---

# 9. Mock Interno

Essa pode aparecer.

### Problema

> "O desafio não disponibilizava um parceiro real."

### Solução

> "Implementei um endpoint interno simulando a resposta da Omnibees."

### Benefício

> "Consegui validar toda a integração de ponta a ponta sem depender de um sistema externo."

---

# 10. PostgreSQL

### Problema

> "Era necessário persistir as reservas."

### Solução

> "Utilizei PostgreSQL integrado ao Spring Data JPA."

### Benefício

> "Obtive persistência confiável utilizando um banco amplamente adotado em produção."

---

# 11. Tratamento Global de Exceções

### Problema

> "Sem tratamento centralizado, cada controller precisaria repetir código."

### Solução

> "Implementei um @ControllerAdvice."

### Benefício

> "As respostas de erro ficam padronizadas e o código permanece mais limpo."

---

# 12. Swagger

### Problema

> "Era necessário facilitar os testes da API."

### Solução

> "Adicionei documentação automática utilizando OpenAPI."

### Benefício

> "Qualquer desenvolvedor consegue testar os endpoints diretamente pelo navegador."

---

# 13. Logs

### Problema

> "Era importante acompanhar a execução do polling."

### Solução

> "Adicionei logs no início e ao final do processamento."

### Benefício

> "Facilita monitoramento e diagnóstico de problemas."

---

# 14. Se perguntarem "por que Spring Boot?"

Uma resposta muito boa seria:

**Problema**

> "Era necessário desenvolver uma API rapidamente."

**Solução**

> "Utilizei Spring Boot."

**Benefício**

> "Ele reduz configurações, possui integração nativa com JPA, Security, Validation e acelera bastante o desenvolvimento."

---

# A resposta que mais impressiona

Imagine que o entrevistador pergunte:

> "Por que você usou MapStruct?"



> "Na aplicação eu precisava converter objetos entre DTOs e entidades. Fazer isso manualmente aumentaria a quantidade de código repetitivo e a chance de inconsistências. Por isso utilizei o MapStruct, que gera esses mapeamentos em tempo de compilação, deixando o código mais limpo, reduzindo erros e facilitando a manutenção."