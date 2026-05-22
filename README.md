# CP-2---O-Caso-da-Vinheria-Agnello-1ESPV-

Sistema de Monitoramento e Alerta Ambiental Automatizado — Vinheria (Fase 2)
Este projeto consiste em uma solução robusta de computação física desenvolvida em plataforma Arduino para a Vinheria. O objetivo principal é monitorar de forma rigorosa as condições ambientais de armazenamento de vinhos finos (especialmente brancos e espumantes, altamente sensíveis a variações climáticas e luminosas), garantindo a integridade dos vedantes, rótulos e as propriedades orgânicas do líquido.

O sistema faz a leitura em tempo real de Temperatura, Umidade Relativa do Ar e Luminosidade, processa a média matemática móvel dessas variáveis para evitar falsos positivos e aciona um ecossistema de alertas visuais (LEDs), sonoros (Buzzer) e textuais (Display LCD 16x2 I2C).

---

* Arquitetura da Lógica e Desafios de Engenharia
Ao idealizar este sistema para a Fase 2, precisei resolver três grandes desafios de engenharia de software embarcado para atender às demandas dos proprietários:

1. O Desafio da Média Estável vs. Tempo de Resposta
Os proprietários exigiram que os valores na tela fossem a média de pelo menos 5 leituras a cada 5 segundos.
A armadilha comum: Ler o sensor dentro de um bloco de 5 segundos faria com que o sistema demorasse 25 segundos para calcular a primeira média.
A nossa solução: Criamos uma estrutura de dupla temporização com millis() (multitarefa sem travamento). O Arduino colhe uma amostra instantânea a cada 1 segundo e acumula esses dados. Exatamente no quinto segundo, o sistema divide a soma por 5, calcula a média perfeita e atualiza os displays e atuadores instantaneamente.

2. Transição e Calibração do Sensor
Substituímos o sensor DHT22 pelo DHT11, adequando a biblioteca para interpretar corretamente o protocolo de comunicação desse hardware específico.

3. Matriz de Prioridade de Alertas (Semáforo e Alarme)
Diferente da Fase 1 onde as variáveis se misturavam, na Fase 2 a lógica foi isolada. Se a umidade falhar, o alarme vermelho toca; se a temperatura falhar, o alarme amarelo toca; se a luz falhar em nível crítico, o alarme vermelho também toca. O código foi desenhado para que um alerta não "atropele" o outro no display, alternando as informações de forma limpa.

---

* Requisitos Técnicos de Negócio (A Faixa Ideal)
Para que o vinho envelheça perfeitamente a cerca de 13°C, o sistema foi calibrado com as seguintes regras estritas:

-- Luminosidade (Sensor LDR):

Condição Ideal (Escuro/Penumbra): Leituras abaixo de 300. Protege o vinho contra raios ultravioletas que causam reações químicas desagradáveis.
Condição de Alerta (Meia Luz): Leituras entre 301 e 700. Indica que o ambiente está saindo da penumbra.
Condição Crítica (Muito Claro): Leituras acima de 700. Ativa imediatamente o alarme visual e sonoro.

-- Temperatura (Sensor DHT11):

Condição Ideal: Entre 10°C e 15°C. Evita flutuações térmicas maiores que 3°C, que destroem os aromas originais do vinho.
Condição Crítica (Baixa): Leituras menores que 10°C.
Condição Crítica (Alta): Leituras maiores que 15°C. Ambas as condições críticas disparam o LED Amarelo e o Buzzer de forma contínua.

-- Umidade Relativa do Ar (Sensor DHT11):

Condição Ideal: Entre 50% e 70%. Evita o ressecamento da rolha (oxidação) e o excesso de umidade (proliferação de fungos nos rótulos).
Condição Crítica (Baixa): Leituras menores que 50%.
Condição Crítica (Alta): Leituras maiores que 70%. Ambas as condições críticas disparam o LED Vermelho e o Buzzer de forma contínua.

---

* Hardware Utilizado e Pinagem
-- 1x Arduino Uno (ou compatível)
-- 1x Sensor de Temperatura e Umidade DHT11 (Pino A1)
-- 1x Sensor de Luminosidade LDR (Pino A0)
-- 1x Display LCD 16x2 com Módulo I2C (Pinos SDA/SCL padrões do Arduino)
-- 1x LED Verde — Indicador de Ambiente Seguro (Pino 7)
-- 1x LED Amarelo — Indicador de Alerta de Temperatura / Meia Luz (Pino 6)
-- 1x LED Vermelho — Indicador de Perigo Crítico de Umidade / Luz Alta (Pino 5)
-- 1x Buzzer Ativo — Alarme Sonoro Contínuo (Pino 4)
-- Resistores para o LDR e LEDs, Protoboard e Jumpers.

---

Como Executar o Passo a Passo do Projeto

-- Passo 1: Preparação da IDE do Arduino

Abra a IDE do Arduino.
Vá em Ferramentas > Gerenciar Bibliotecas...
Busque por DHT sensor library (da Adafruit) e clique em instalar (instale também a dependência Adafruit Unified Sensor, caso solicitado).
Busque por LiquidCrystal I2C (do Frank de Brabander) e faça a instalação.

-- Passo 2: Montagem do Circuito

Conecte o sensor DHT11 ao pino digital A1 utilizando um resistor pull-up de 4.7k ou 10k Ohms entre o VCC e o pino de dados (caso seu sensor não seja em formato de módulo pronto).
Monte o circuito do LDR com um divisor de tensão conectado ao pino analógico A0.
Conecte os LEDs (Verde, Amarelo e Vermelho) nos pinos 7, 6 e 5, respectivamente, utilizando resistores de 220 Ohms em série.
Conecte o Buzzer ao pino digital 4.
Conecte as linhas SDA e SCL do display LCD I2C nos pinos correspondentes do seu modelo de Arduino (No Uno, SDA é A4 e SCL é A5).

-- Passo 3: Upload e Validação

Conecte o Arduino ao computador.
Selecione a placa correta e a porta COM em Ferramentas.
Carregue o código fornecido.
Abra o Monitor Serial (9600 bps) para acompanhar os relatórios de média matemática calculados a cada 5 segundos e compare com as transições visuais exibidas na tela do LCD.

---

O Código-Fonte Desenvolvido
Este código foi corrigido, otimizado e estruturado para evitar o uso de delay(), mantendo o processamento do carrossel do display fluido e as leituras precisas:




