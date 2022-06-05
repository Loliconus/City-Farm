# City-Farm
#define water_down 2
#define water_up 3
#define CO2_IN 11
#define DHTPIN 10
#define DHTTYPE DHT22
#include "EasyNextionLibrary.h"
#include "DHT.h"
#include <MHZ.h>
DHT dht(DHTPIN, DHTTYPE);
MHZ co2(CO2_IN, MHZ19B);
EasyNex myNex(Serial);
void setup() {
  // put your setup code here, to run once:
  myNex.begin(9600);
  pinMode(water_down, INPUT_PULLUP);
  pinMode(water_up, INPUT_PULLUP);
  pinMode(CO2_IN, INPUT);
  dht.begin();
}

void loop() {
  // put your main code here, to run repeatedly:
  myNex.NextionListen();
}

void trigger0() {
  // ВОДА
  if (check_water_down == LOW && check_water_up == LOW)myNex.writeStr("t6.txt", "FULL");
  else if (check_water_down == LOW && check_water_up == HIGH)myNex.writeStr("t6.txt", "AVG");
  else if (check_water_down == HIGH && check_water_up == HIGH)myNex.writeStr("t6.txt", "LOW");
  else myNex.writeStr("OSHIBKA");
  // ТЕМПЕРАТУРА И ВЛАЖНОСТЬ
  float h = dht.readHumidity();
  float t = dht.readTemperature();
  myNex.writeNum("x0.val", t * 10);
  myNex.writeNum("x1.val", h * 10);
  // CO2
  int ppm_pwm = co2.readCO2PWM();
  myNex.writeNum("n0.val", ppm_pwm);
}
