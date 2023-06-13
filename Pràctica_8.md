# Pràctica 8 - BUSES DE COMUNICACIÓN IV

Aquest codi té com a objectiu demostrar el funcionament de la comunicació sèrie asíncrona USART (Universal Synchronous Asynchronous Receiver Transmitter) mitjançant l'ús de dues interfícies sèrie en una placa Arduino compatible amb ESP32.

En el setup, s'inicialitzen les dues comunicacions sèrie. La primera és la comunicació per UART0, que s'inicia amb una velocitat de transmissió de 115200 bauds amb "Serial.begin(115200)". La segona és la comunicació per UART2, que també s'inicia amb una velocitat de 115200 bauds i es configura amb SERIAL_8N1 (8 bits de dades, cap bit de paritat i 1 bit d'arribada) i els pins 16 i 17 com a pins RX i TX respectivament amb "Serial2.begin(115200, SERIAL_8N1, 16, 17)":
```cpp 
#include <Arduino.h>

void setup() {
  Serial.begin(115200);
  Serial2.begin(115200, SERIAL_8N1, 16, 17);
}
```

A la funció loop(), es comprova si hi ha dades disponibles a la UART0 amb "Serial.available()". Si hi ha dades disponibles, es llegeix un byte de la UART0 amb "Serial.read()" i s'envia a la UART2 amb "Serial2.write(data)":
```cpp 
void loop() {
  if (Serial.available()) {char data = Serial.read(); 
    Serial2.write(data);}
```
A continuació, es comprova si hi ha dades disponibles a la UART2 amb "Serial2.available()". Si hi ha dades disponibles, es llegeix un byte de la UART2 amb "Serial2.read()" i s'envia a la UART0 amb "Serial.write(data)":
```cpp 
  if (Serial2.available()) {
    char data = Serial2.read(); 
    Serial.write(data); 
  }
}
```
## Conclusions Pràctica 8

Aquest codi permet establir una comunicació sèrie bidireccional entre UART0 i UART2. Qualsevol dada rebuda per UART0 es reenvia a UART2, i qualsevol dada rebuda per UART2 es reenvia a UART0. Això permet la comunicació entre dispositius connectats a les dues interfícies sèrie. L'objectiu de la pràctica és comprendre el funcionament de la comunicació sèrie asíncrona USART i utilitzar-la en diverses aplicacions, com ara l'enviament de dades a una impressora sèrie mitjançant la comanda "Serial.println()"

