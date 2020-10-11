# octolcd
LCD python 2.7 script serial comms to Arduino (DONT USE PYTHON 3 - asks for no unicode)

Tells the pi to send serial commands to the Arduino (uses port ACM0 but you can change this to whatever yours is on)
Arduino uses the preinstalled LCD library
Outputs the strings to the display
Arduino code:

#include <LiquidCrystal.h>    // include the library
LiquidCrystal lcd(12, 11, 5, 4, 3, 2);     // datapin, latchpin, clockpin
int contrastPin = 6;
int brightnessPin = 10;
int contrast = 135;
int brightness = 255;
String input = "";
String stringRec = "";
String pos = "";
int posX = 0;
int posY = 0;

void setup() {
    pinMode(contrastPin,OUTPUT);
    pinMode(brightnessPin,OUTPUT);
    lcd.begin(16,2);             // 16 characters, 2 rows
    lcd.clear();
    analogWrite(contrastPin, contrast);
    analogWrite(brightnessPin, brightness);
    Serial.begin(9600);
    Serial.setTimeout(500);
}

void loop() {
  if(Serial.available() > 0){
    input = Serial.readString();
    pos = input.substring(0,2);
    posX = pos.substring(0,1).toInt();
    posY = pos.substring(1,2).toInt();
    stringRec = input.substring(2); 
    /*Serial.println("String: ");
    Serial.println(stringRec);
    Serial.println();
    Serial.println("Pos: ");
    Serial.print(posX);
    Serial.print(" ");
    Serial.print(posY);
    Serial.println();*/
  }
  if(stringRec.equals("clr")){
    lcd.clear();
    stringRec = "";    
  }
  lcd.setCursor(posX,posY);
  lcd.print(stringRec); 
}
