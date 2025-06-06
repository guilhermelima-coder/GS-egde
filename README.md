# GS-EDGE monitor de umidade do solo com Arduino
Olá meu nome é Guilherme venho por meio desse arquivo representar minha equipe nesse projeto apresentando esse circuito que foi baseado em alagamentos de ruas, e áreas de risco.
nosso projeto tem como **objetivo** a prevenção desses alagamentos utilizando um sensor de umidade do solo para que seja possível monitoriar o nível de agua da chuva, e assim poder alertar a população da determinada região.

## Funcionalidades

- Leitura analógica da umidade do solo.
- Exibição do nível de água no display LCD.
- Indicação visual com três LEDs:
  - **Verde**: Umidade segura.
  - **Amarelo**: Nível médio.
  - **Vermelho**: Risco de enchente.

## Componentes Utilizados

- 1x Arduino Uno
- 1x Sensor de umidade do solo
- 1x Display LCD 16x2 (com interface I2C ou paralela)
- 1x Potenciômetro (para contraste do LCD)
- 3x LEDs (vermelho, amarelo e verde)
- 4x Resistores (220Ω)
- Jumpers e Protoboard

## Circuito 

Veja o circuito na imagem abaixo (Tinkercad):
![image](https://github.com/user-attachments/assets/0a2e9684-4143-4295-b434-8e4fbd492825)
-**[Link do circuito](https://www.tinkercad.com/things/ggomxY7tfY4-gs-edge?sharecode=MOnl6etXtflNA4IHyGNQ6fPmP1IWC0XwbpJ2c36IWGQ)**

-**[Video de circuito em funcionamento](https://www.youtube.com/watch?v=6UoaTlFlRh4)**

## Código Fonte
```cpp
#include <LiquidCrystal.h>  // Biblioteca para controlar o display LCD

// Inicializa o LCD nos pinos: RS, E, D4, D5, D6, D7
LiquidCrystal lcd(12, 11, 5, 4, 3, 2);

// Define os pinos onde os LEDs estão conectados
const int ledVerde = 6;
const int ledAmarelo = 7;
const int ledVermelho = 8;

// Define o pino onde o sensor de umidade está conectado
const int sensorUmidade = A0;

void setup() {
  lcd.begin(16, 2);  // Inicializa o LCD com 16 colunas e 2 linhas

  // Define os pinos dos LEDs como saída
  pinMode(ledVerde, OUTPUT);
  pinMode(ledAmarelo, OUTPUT);
  pinMode(ledVermelho, OUTPUT);

  // Mostra uma mensagem inicial no LCD
  lcd.setCursor(0, 0);       // Define a posição do cursor (coluna 0, linha 0)
  lcd.print("Monitorando..."); // Mostra a mensagem "Monitorando..."
  delay(2000);               // Aguarda 2 segundos
  lcd.clear();               // Limpa o display
}

void loop() {
  // Lê o valor do sensor de umidade (0 a 1023)
  int valorSensor = analogRead(sensorUmidade);

  // Mostra o texto fixo na primeira linha do LCD
  lcd.setCursor(0, 0);
  lcd.print("Nivel da Agua:    "); // Escreve a frase

  // Verifica o nível da água de acordo com o valor do sensor
  if (valorSensor < 400) {
    // Nível seguro de umidade
    lcd.setCursor(0, 1);
    lcd.print("SEGURO         "); // Mostra "SEGURO"
    digitalWrite(ledVermelho, LOW);   // Desliga LED vermelho
    digitalWrite(ledAmarelo, LOW);   // Desliga LED amarelo
    digitalWrite(ledVerde, HIGH);    // Liga LED verde
  } else if (valorSensor < 700) {
    // Nível médio de umidade
    lcd.setCursor(0, 1);
    lcd.print("NIVEL MEDIO    "); // Mostra "NIVEL MEDIO"
    digitalWrite(ledVermelho, LOW);   // Desliga LED vermelho
    digitalWrite(ledAmarelo, HIGH);   // Liga LED amarelo
    digitalWrite(ledVerde, LOW);      // Desliga LED verde
  } else {
    // Alto nível de umidade: risco de enchente
    lcd.setCursor(0, 1);
    lcd.print("RISCO D ENCHENTE"); // Mostra "RISCO D ENCHENTE"
    digitalWrite(ledVermelho, HIGH);  // Liga LED vermelho
    digitalWrite(ledAmarelo, LOW);    // Desliga LED amarelo
    digitalWrite(ledVerde, LOW);      // Desliga LED verde
  }

  delay(1000); // Aguarda 1 segundo antes de repetir
}


