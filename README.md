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
- 

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


## Código Fonte

```cpp
#include <LiquidCrystal.h>

// Pinos do LCD: RS, E, D4, D5, D6, D7
LiquidCrystal lcd(12, 11, 5, 4, 3, 2);

// Pinos dos LEDs
const int ledVerde = 6;
const int ledAmarelo = 7;
const int ledVermelho = 8;

// Sensor de umidade
const int sensorUmidade = A0;

void setup() {
  lcd.begin(16, 2);

  pinMode(ledVerde, OUTPUT);
  pinMode(ledAmarelo, OUTPUT);
  pinMode(ledVermelho, OUTPUT);

  lcd.setCursor(0, 0);
  lcd.print("Monitorando...");
  delay(2000);
  lcd.clear();
}

void loop() {
  int valorSensor = analogRead(sensorUmidade);

  lcd.setCursor(0, 0);
  lcd.print("Nivel da Agua:    ");

  if (valorSensor < 400) {
    lcd.setCursor(0, 1);
    lcd.print("SEGURO");
    digitalWrite(ledVermelho, LOW);
    digitalWrite(ledAmarelo, LOW);
    digitalWrite(ledVerde, HIGH);
  } else if (valorSensor < 700) {
    lcd.setCursor(0, 1);
    lcd.print("NIVEL MEDIO");
    digitalWrite(ledVermelho, LOW);
    digitalWrite(ledAmarelo, HIGH);
    digitalWrite(ledVerde, LOW);
  } else {
    lcd.setCursor(0, 1);
    lcd.print("RISCO D ENCHENTE");
    digitalWrite(ledVermelho, HIGH);
    digitalWrite(ledAmarelo, LOW);
    digitalWrite(ledVerde, LOW);
  }

  delay(1000);
}
