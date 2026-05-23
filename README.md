# 🍷 Vinheria Agnello — Sistema de Monitoramento Ambiental

> Solução de computação física desenvolvida em Arduino para monitoramento de temperatura, umidade e luminosidade no armazenamento de vinhos finos.

---

## 👥 Integrantes do Grupo

| Nome completo | RM |
|---|---|
| Vitor Assis |  572192  |
| Lucas Rodrigues |  571778  |
| Raul Moreira | 571042 |
| Vinicius Scalone | 573783 |


---

## 📸 Foto / Imagem do Projeto

<!-- Substitua o caminho abaixo pela imagem do seu projeto montado -->
<!-- Exemplo: ![Projeto montado](./imagens/projeto.jpg) -->

<img width="663" height="393" alt="Captura de tela 2026-05-22 224848" src="https://github.com/user-attachments/assets/ae268991-4dd8-4fa6-9f1c-5e9ec15976f5" />


---

## 🔗 Link do Projeto (Simulação) e Do Video explicativo

<!-- Cole abaixo o link do seu projeto no Tinkercad, Wokwi ou plataforma similar -->

[🔗(https://wokwi.com/projects/464730814628509697)

---

## 📋 Descrição do Projeto

Este projeto consiste em uma solução robusta de computação física para a **Vinheria Agnello**, com o objetivo de monitorar as condições ambientais de armazenamento de vinhos finos — especialmente brancos e espumantes, altamente sensíveis a variações climáticas e luminosas.

O sistema realiza leituras em tempo real de **Temperatura**, **Umidade Relativa do Ar** e **Luminosidade**, processa a **média móvel** dessas variáveis para evitar falsos positivos e aciona um ecossistema de alertas visuais (LEDs), sonoros (Buzzer) e textuais (Display LCD 16x2 I2C).

---

## ⚙️ Requisitos Técnicos — Faixas Ideais de Operação

### 💡 Luminosidade (Sensor LDR)

| Condição | Leitura | Ação do Sistema |
|---|---|---|
| ✅ Ideal (Escuro/Penumbra) | Abaixo de 300 | LED Verde |
| ⚠️ Alerta (Meia Luz) | Entre 301 e 700 | LED Amarelo |
| 🚨 Crítico (Muito Claro) | Acima de 700 | LED Vermelho + Buzzer |

### 🌡️ Temperatura (Sensor DHT11)

| Condição | Leitura | Ação do Sistema |
|---|---|---|
| ✅ Ideal | Entre 10°C e 15°C | LED Verde |
| 🚨 Crítico (Baixa) | Menor que 10°C | LED Amarelo + Buzzer |
| 🚨 Crítico (Alta) | Maior que 15°C | LED Amarelo + Buzzer |

### 💧 Umidade Relativa do Ar (Sensor DHT11)

| Condição | Leitura | Ação do Sistema |
|---|---|---|
| ✅ Ideal | Entre 50% e 70% | LED Verde |
| 🚨 Crítico (Baixa) | Menor que 50% | LED Vermelho + Buzzer |
| 🚨 Crítico (Alta) | Maior que 70% | LED Vermelho + Buzzer |

---

## 🔩 Hardware Utilizado e Pinagem

| Componente | Quantidade | Pino |
|---|---|---|
| Arduino Uno | 1x | — |
| Sensor DHT11 (Temp. e Umidade) | 1x | A1 |
| Sensor LDR (Luminosidade) | 1x | A0 |
| Display LCD 16x2 com módulo I2C | 1x | SDA (A4) / SCL (A5) |
| LED Verde — Ambiente Seguro | 1x | Pino 7 |
| LED Amarelo — Alerta de Temperatura | 1x | Pino 6 |
| LED Vermelho — Perigo Crítico | 1x | Pino 5 |
| Buzzer Ativo — Alarme Sonoro | 1x | Pino 4 |
| Resistores (220Ω para LEDs, 10kΩ para LDR) | Vários | — |
| Protoboard e Jumpers | — | — |

---

## 🧠 Arquitetura da Lógica

### 1. Média Estável com Dupla Temporização
O Arduino colhe **1 amostra por segundo** e acumula os dados. A cada **5 segundos**, calcula a média das últimas 5 leituras e atualiza os alertas — sem uso de `delay()`, mantendo o sistema responsivo.

### 2. Matriz de Prioridade de Alertas (Semáforo)
- **Umidade crítica** → LED Vermelho + Buzzer intermitente
- **Temperatura crítica** → LED Amarelo + Buzzer intermitente
- **Luz crítica (alta)** → LED Vermelho + Buzzer intermitente
- **Luz em alerta (meia luz)** → LED Amarelo
- **Tudo OK** → LED Verde

### 3. Display LCD com Carrossel
A linha 1 exibe mensagens em carrossel (rolagem suave). A linha 2 alterna a cada 3 segundos entre: leituras de umidade/temperatura, status da umidade e status da temperatura.

---

## 📦 Bibliotecas Necessárias

- `DHT sensor library` — Adafruit
- `Adafruit Unified Sensor` — Adafruit (dependência)
- `LiquidCrystal I2C` — Frank de Brabander

> Instale via **Ferramentas > Gerenciar Bibliotecas** na IDE do Arduino.

---

## 🚀 Como Executar

**1. Prepare a IDE do Arduino**
- Instale as bibliotecas listadas acima.

**2. Monte o Circuito**
- Conecte o DHT11 ao pino A1 (com resistor pull-up de 10kΩ se necessário).
- Monte o LDR com divisor de tensão no pino A0.
- Conecte LEDs nos pinos 7, 6 e 5 com resistores de 220Ω.
- Conecte o Buzzer ao pino 4.
- Conecte o LCD I2C nos pinos SDA (A4) e SCL (A5).

**3. Faça o Upload**
- Selecione a placa e porta correta em **Ferramentas**.
- Carregue o código no Arduino.
- Abra o Monitor Serial em **9600 bps** para acompanhar os logs de média a cada 5 segundos.

---

## 📁 Estrutura do Repositório
