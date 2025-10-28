# ponderada-farol

## Codigo

// Definição dos pinos dos LEDs
const int ledVerde = 13;
const int ledAmarelo = 12;
const int ledVermelho = 11;

// Definição dos tempos (em milissegundos)
const int tempoVerde = 5000;   // 5 segundos
const int tempoAmarelo = 2000; // 2 segundos
const int tempoVermelho = 5000; // 5 segundos

void setup() {
  // Configura os pinos como saída
  pinMode(ledVerde, OUTPUT);
  pinMode(ledAmarelo, OUTPUT);
  pinMode(ledVermelho, OUTPUT);
}

void loop() {
  // Verde aceso
  digitalWrite(ledVerde, HIGH);
  digitalWrite(ledAmarelo, LOW);
  digitalWrite(ledVermelho, LOW);
  delay(tempoVerde);

  // Amarelo aceso
  digitalWrite(ledVerde, LOW);
  digitalWrite(ledAmarelo, HIGH);
  digitalWrite(ledVermelho, LOW);
  delay(tempoAmarelo);

  // Vermelho aceso
  digitalWrite(ledVerde, LOW);
  digitalWrite(ledAmarelo, LOW);
  digitalWrite(ledVermelho, HIGH);
  delay(tempoVermelho);
}


## Documentacao

# Projeto: Semáforo com Arduino

## Visão Geral

Este projeto implementa um **sistema de semáforo eletrônico** utilizando a plataforma **Arduino Uno**, controlando três LEDs (vermelho, amarelo e verde) para simular o comportamento de um cruzamento real.

O objetivo é **introduzir conceitos fundamentais de eletrônica e programação embarcada**, sendo ideal para iniciantes que desejam aprender sobre controle de hardware com Arduino.


## Componentes Utilizados

| Quantidade | Componente                       | Função                                 |
| ---------- | -------------------------------- | -------------------------------------- |
| 1          | Arduino Uno                      | Microcontrolador principal             |
| 1          | Protoboard                       | Montagem do circuito                   |
| 3          | LEDs (vermelho, amarelo e verde) | Sinalização do semáforo                |
| 3          | Resistores 220 Ω                 | Proteção contra sobrecorrente nos LEDs |
| 6          | Jumpers                          | Conexões elétricas                     |

---

## Esquema de Conexão

* LED Verde → Pino digital 2
* LED Amarelo → Pino digital 3
* LED Vermelho → Pino digital 4
* Cada LED conectado a GND através de um resistor de 220 Ω.

---

## Código Fonte

```cpp
// ==========================
// Projeto: Semáforo Arduino
// Autor: Henrique Rodrigues Diniz
// ==========================

// Definição dos pinos dos LEDs
const int ledVerde = 2;
const int ledAmarelo = 3;
const int ledVermelho = 4;

// Definição dos tempos (em milissegundos)
const int tempoVerde = 5000;
const int tempoAmarelo = 2000;
const int tempoVermelho = 5000;

void setup() {
  pinMode(ledVerde, OUTPUT);
  pinMode(ledAmarelo, OUTPUT);
  pinMode(ledVermelho, OUTPUT);
}

void loop() {
  // Verde ligado — trânsito liberado
  digitalWrite(ledVerde, HIGH);
  digitalWrite(ledAmarelo, LOW);
  digitalWrite(ledVermelho, LOW);
  delay(tempoVerde);

  // Amarelo ligado — atenção
  digitalWrite(ledVerde, LOW);
  digitalWrite(ledAmarelo, HIGH);
  digitalWrite(ledVermelho, LOW);
  delay(tempoAmarelo);

  // Vermelho ligado — parada
  digitalWrite(ledVerde, LOW);
  digitalWrite(ledAmarelo, LOW);
  digitalWrite(ledVermelho, HIGH);
  delay(tempoVermelho);
}
```

---
##Link do video

https://youtube.com/shorts/q65U_h5NXRo?si=TV_BtZ77Wz-473to

## Lógica de Funcionamento

1. Fase Verde: LED verde acende, permitindo passagem.
2. Fase Amarela: LED amarelo acende para alertar sobre a transição.
3. Fase Vermelha: LED vermelho acende, indicando parada.
4. O ciclo se repete continuamente, simulando o comportamento de um semáforo real.
