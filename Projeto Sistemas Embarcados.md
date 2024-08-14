### **Projeto Simplificado de Sistema Embarcado: Monitor de Qualidade do Ar com ESP32**

#### **1. Definição do Problema e Especificações**

##### **1.1. Definição do Problema**

- **Problema a ser resolvido:**
    
    - Monitorar a qualidade do ar em ambientes fechados para garantir um ambiente saudável, detectando e alertando sobre níveis elevados de dióxido de carbono (CO2).
- **Objetivo do Sistema:**
    
    - Usar o ESP32 para criar um dispositivo que monitore o nível de CO2 e notifique o usuário quando a qualidade do ar estiver comprometida.

##### **1.2. Funcionalidades Essenciais**

- **Sensores:**
    
    - **Sensor de CO2:** Utilize um sensor de CO2 compatível, como o MH-Z19, para medir os níveis de dióxido de carbono.
- **Processamento de Dados:**
    
    - **Análise em Tempo Real:** O ESP32 processará os dados do sensor a cada segundo.
    - **Algoritmo de Alerta:** Comparação dos níveis de CO2 com um valor limiar e acionamento de alarmes se o nível for perigoso.
- **Interface de Comunicação:**
    
    - **Display OLED ou LCD:** Exibe os níveis de CO2 e mensagens de alerta.
    - **Alarmes:** Um LED acende ou um buzzer soa quando o nível de CO2 ultrapassa o limite seguro.
    - **Conectividade Wi-Fi:** O ESP32 pode enviar dados para um servidor na nuvem ou um aplicativo via Wi-Fi para monitoramento remoto.
- **Energia:**
    
    - **Alimentação USB ou Bateria:** O sistema pode ser alimentado via USB ou por uma bateria recarregável para mobilidade.

##### **1.3. Requisitos Não Funcionais**

- **Desempenho:**
    
    - **Velocidade de processamento:** Processamento de dados a cada segundo.
    - **Tempo de resposta:** O sistema deve reagir em menos de 2 segundos ao detectar níveis elevados de CO2.
- **Energia:**
    
    - **Consumo de bateria:** Considerando o uso de uma bateria de 3.7V, o consumo deve ser otimizado, desativando o Wi-Fi quando não for necessário.
    - **Autonomia:** Visa uma operação contínua de 8 a 12 horas com bateria.
- **Confiabilidade:**
    
    - **Taxa de falhas:** Baixa, com reinicialização automática em caso de falhas.
    - **Tolerância a erros:** O sistema deve alertar o usuário em caso de falhas do sensor.
- **Tamanho:**
    
    - **Dimensões físicas:** Compacto, com as dimensões do ESP32 e sensores adicionais acoplados.
- **Custo:**
    
    - **Orçamento disponível:** O custo total deve ser baixo, focando no uso do ESP32 e sensores acessíveis.
- **Ambiente:**
    
    - **Condições de operação:**
        - **Temperatura:** Operação entre 0°C e 40°C.
        - **Umidade:** Suportar níveis normais de umidade em ambientes internos.

---

### **Implementação no Wokwi:**

1. **Configuração do ESP32:**
    
    - Configure o ESP32 no Wokwi com um sensor de CO2 (como o MH-Z19).
    - Adicione um display OLED ou LCD para exibir os dados.
2. **Código:**
    
    - Escreva um código simples em C++ (Arduino) para ler os dados do sensor de CO2.
    - Adicione a lógica para acender o LED/buzzer e exibir mensagens no display quando o nível de CO2 for alto.
    - Se necessário, configure a conectividade Wi-Fi para enviar os dados para um servidor remoto.
3. **Simulação:**
    
    - Teste a simulação no Wokwi para garantir que todas as funcionalidades estejam operando como esperado.

Este projeto permite que você crie um monitor de qualidade do ar básico utilizando o ESP32 no Wokwi, com funcionalidades essenciais e focado em um protótipo funcional.