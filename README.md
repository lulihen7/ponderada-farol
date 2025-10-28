# Projeto: Semáforo com Arduino — Ponderada da Semana

## Contexto

Neste projeto foi desenvolvido um **sistema de semáforo funcional** para controlar o fluxo de veículos e pedestres em uma via movimentada no bairro Butantã.

O objetivo foi aplicar conceitos de **eletrônica básica**, **programação embarcada** e **temporização não bloqueante** para criar um semáforo que funcione como os sistemas reais encontrados nas ruas, garantindo segurança e fluidez no trânsito.


## Montagem Física

A montagem foi realizada em uma protoboard utilizando três LEDs (vermelho, amarelo e verde), resistores de 220 Ω e jumpers.

Cada LED foi conectado a um pino digital do Arduino e ao GND com resistores para limitar a corrente elétrica e proteger os componentes.

### Esquema de Conexão

* LED Verde → Pino digital 2
* LED Amarelo → Pino digital 3
* LED Vermelho → Pino digital 4
* Todos os LEDs conectados ao GND através de resistores de 220 Ω.

---

## Tabela de Componentes

| Quantidade | Componente                       | Função                                     |
| ---------- | -------------------------------- | ------------------------------------------ |
| 1          | Arduino Uno                      | Microcontrolador principal                 |
| 1          | Protoboard                       | Montagem do circuito                       |
| 3          | LEDs (vermelho, amarelo e verde) | Representação visual das fases do semáforo |
| 3          | Resistores 220 Ω                 | Proteção dos LEDs                          |
| 6          | Jumpers                          | Conexões elétricas                         |

---

## Código da Programação (com `millis()`)

```cpp
const uint8_t LED_VERDE    = 13;
const uint8_t LED_AMARELO  = 12;
const uint8_t LED_VERMELHO = 11;

const unsigned long T_VERDE    = 2000UL;
const unsigned long T_AMARELO  = 4000UL;
const unsigned long T_VERMELHO = 6000UL;

enum Fase : uint8_t { VERDE = 0, AMARELO = 1, VERMELHO = 2, AMARELO_POS = 3 };

Fase faseAtual = VERDE;
unsigned long instanteMudanca = 0;

void aplicaFase(Fase f) {
  switch (f) {
    case VERDE:
      digitalWrite(LED_VERDE, HIGH);
      digitalWrite(LED_AMARELO, LOW);
      digitalWrite(LED_VERMELHO, LOW);
      break;
    case AMARELO:
      digitalWrite(LED_VERDE, LOW);
      digitalWrite(LED_AMARELO, HIGH);
      digitalWrite(LED_VERMELHO, LOW);
      break;
    case VERMELHO:
      digitalWrite(LED_VERDE, LOW);
      digitalWrite(LED_AMARELO, LOW);
      digitalWrite(LED_VERMELHO, HIGH);
      break;
    case AMARELO_POS:
      digitalWrite(LED_VERDE, LOW);
      digitalWrite(LED_AMARELO, HIGH);
      digitalWrite(LED_VERMELHO, LOW);
      break;
  }
}

unsigned long duracaoDaFase(Fase f) {
  switch (f) {
    case VERDE:       return T_VERDE;
    case AMARELO:     return T_AMARELO;
    case VERMELHO:    return T_VERMELHO;
    case AMARELO_POS: return T_AMARELO;
  }
  return 0;
}

Fase proximaFase(Fase f) {
  switch (f) {
    case VERDE:       return AMARELO;
    case AMARELO:     return VERMELHO;
    case VERMELHO:    return AMARELO_POS;
    case AMARELO_POS: return VERDE;
  }
  return VERDE;
}

void setup() {
  pinMode(LED_VERDE, OUTPUT);
  pinMode(LED_AMARELO, OUTPUT);
  pinMode(LED_VERMELHO, OUTPUT);

  aplicaFase(faseAtual);
  instanteMudanca = millis();
}

void loop() {
  unsigned long agora = millis();
  unsigned long elapsed = agora - instanteMudanca;

  if (elapsed >= duracaoDaFase(faseAtual)) {
    faseAtual = proximaFase(faseAtual);
    aplicaFase(faseAtual);
    instanteMudanca = agora;
  }

  // Espaço livre para futuras extensões:
  // - Botão de pedestre
  // - Sensores de presença
  // - Comunicação serial
}

```


## Link do video

https://youtube.com/shorts/q65U_h5NXRo?si=TV_BtZ77Wz-473to

## Tabela de avaliacao

| Critério                                                                                                            | Pontos Máximos | Avaliador 1 (Reimar Filho) | Avaliador 2 (Diego Figueiredo Silva) | Observações                                  |
| ------------------------------------------------------------------------------------------------------------------- | -------------: | -------------------: | -----------------------------------: | -------------------------------------------- |
| Montagem física com cores corretas, boa disposição dos fios e uso adequado de resistores                            |              3 |                    3 |                                    3 | Montagem bem organizada e com cores corretas |
| Temporização adequada conforme tempos medidos com auxílio de algum instrumento externo                              |              3 |                    3 |                                    3 | Temporização correta                         |
| Código implementa corretamente as fases do semáforo e estrutura do código (variáveis representativas e comentários) |              3 |                    3 |                                    3 | Código implementado corretamente             |
| Ir além: Implementou um componente extra, fez com `millis()` ao invés de `delay()` e/ou usou ponteiros no código    |              1 |                  0,5 |                                    1 | Utilizou `millis()` e implementou melhorias  |
| **Pontuação Total**                                                                                                 |         **10** |              **9,5** |                               **10** |                                              |

Media final : 9.75

## Lógica de Funcionamento

1. **Fase Verde**: LED verde acende por 5 segundos, liberando o trânsito.
2. **Fase Amarela**: LED amarelo acende por 2 segundos, alertando sobre a transição.
3. **Fase Vermelha**: LED vermelho acende por 5 segundos, indicando parada.
4. O ciclo reinicia continuamente.

> A utilização de `millis()` permite que o sistema seja **não bloqueante**, possibilitando futuras expansões como sensores e botões de pedestres sem interferir no tempo do semáforo.

