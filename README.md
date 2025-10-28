# ponderada-farol

## Codigo

// Pinos dos LEDs
const uint8_t LED_VERDE    = 13;
const uint8_t LED_AMARELO  = 12;
const uint8_t LED_VERMELHO = 11;

// Tempos de cada fase (em milissegundos)
const unsigned long T_VERDE    = 5000UL;  // 5 s
const unsigned long T_AMARELO  = 2000UL;  // 2 s
const unsigned long T_VERMELHO = 5000UL;  // 5 s

// Fases do semáforo
enum Fase : uint8_t { VERDE = 0, AMARELO = 1, VERMELHO = 2 };

// Estado atual e controle de tempo
Fase faseAtual = VERDE;
unsigned long instanteMudanca = 0;

// Utilitário: aplica a fase aos LEDs
void aplicaFase(Fase f) {
  switch (f) {
    case VERDE:
      digitalWrite(LED_VERDE,    HIGH);
      digitalWrite(LED_AMARELO,  LOW);
      digitalWrite(LED_VERMELHO, LOW);
      break;
    case AMARELO:
      digitalWrite(LED_VERDE,    LOW);
      digitalWrite(LED_AMARELO,  HIGH);
      digitalWrite(LED_VERMELHO, LOW);
      break;
    case VERMELHO:
      digitalWrite(LED_VERDE,    LOW);
      digitalWrite(LED_AMARELO,  LOW);
      digitalWrite(LED_VERMELHO, HIGH);
      break;
  }
}

// Retorna a duração da fase atual
unsigned long duracaoDaFase(Fase f) {
  switch (f) {
    case VERDE:    return T_VERDE;
    case AMARELO:  return T_AMARELO;
    case VERMELHO: return T_VERMELHO;
  }
  return 0;
}

// Avança ciclicamente para a próxima fase
Fase proximaFase(Fase f) {
  if (f == VERDE)   return AMARELO;
  if (f == AMARELO) return VERMELHO;
  return VERDE;
}

void setup() {
  pinMode(LED_VERDE,    OUTPUT);
  pinMode(LED_AMARELO,  OUTPUT);
  pinMode(LED_VERMELHO, OUTPUT);

  // Inicializa na fase VERDE
  aplicaFase(faseAtual);
  instanteMudanca = millis();
}

void loop() {
  // Controle de tempo sem bloqueio (tolerante a overflow de millis)
  unsigned long agora = millis();
  unsigned long elapsed = agora - instanteMudanca; // cálculo seguro para overflow

  if (elapsed >= duracaoDaFase(faseAtual)) {
    // Próxima fase
    faseAtual = proximaFase(faseAtual);
    aplicaFase(faseAtual);
    instanteMudanca = agora; // reinicia cronômetro
  }

  // Espaço para futuras extensões sem travar o loop:
  // - Leitura de botões (pedestre)
  // - Sensores (LDR, ultrassom, etc.)
  // - Telemetria via Serial
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
