//Included Libraries
#include <Arduino.h>
#include <EEPROM.h>

//Variables and definitions
#define TOUCH1 4
#define TOUCH2 13
#define TOUCH3 2
int Detections = 0;
int STRONGTOUCH = 30;
byte size = 4;

void setup() {
  // put your setup code here, to run once:
  Serial.begin(115200);
  //Set 3 pins as input
  pinMode(TOUCH1, INPUT);
  pinMode(TOUCH2, INPUT);
  pinMode(TOUCH3, INPUT);
  EEPROM.begin(size); // EEPROM saves values at 4 byte (32 bits) size
}

void loop() {
  Detections = EEPROM.read(0); //Read detection count from EEPROM address 0.
  Serial.println("Detection Count: " + String(Detections)); // Print detection count and convert the data in EEPROM address 0 to String.
  delay(1000); // 1 second delay
  int TouchDetected = 0;//Set {t1,t2,t3} in one int to check the first 3 bits of input of each variable at once.
  int t1,t2,t3;//Sets t1,t2,t3 values to 0 and resets them after every loop
  while(TouchDetected==0){ // Wait for touch to be detected
  //Scan all touch pins
  t1 = touchRead(TOUCH1);
  t2 = touchRead(TOUCH2);
  t3 = touchRead(TOUCH3);
  //check if pins meet the threshold set (30). Anything below means there was a touch detection.
  if(t1 < STRONGTOUCH){
    TouchDetected |= (1<<0); // Bit 0: Pin 4
    }
  if(t2 < STRONGTOUCH){
    TouchDetected |= (1<<1); // Bit 1: Pin 13
    }
  if(t3  < STRONGTOUCH){
    TouchDetected |= (1<<2); // Bit 3: Pin 2
    }
  }
  if(TouchDetected==1){ // TouchDetected == 0b001 in binary?
    Serial.print("Touch detection! (Pin 4)\n");
    delay(1000); // 1 second delay
    EEPROM.write(0, ++Detections); //Pre-increment Detection count and write to EEPROM address 0
  }
  else if(TouchDetected==2){ // TouchDetected == 0b010 in binary?
    Serial.print("Touch detection! (Pin 13)\n");
    delay(1000); // 1 second delay
    EEPROM.write(0, ++Detections); //Pre-increment Detection count and write to EEPROM address 0
  }
  else if(TouchDetected==4){ // TouchDetected == 0b100 in binary?
    Serial.print("Touch detection! (Pin 2)\n");
    delay(1000); // 1 second delay
    EEPROM.write(0, ++Detections); //Pre-increment Detection count and write to EEPROM address 0
  }
   else{ // handles multiple detections at once.
    switch(TouchDetected){ //0b000: 2, 13, 4
      case 0b011: //4, 13 pins detected touch
        Serial.print("Touch detection! (Pins 13 and 4)\n");
        delay(1000); // 1 second delay
        Detections = Detections+2;
        EEPROM.write(0, Detections); //Write new Detection count EEPROM address 0
        break;
      case 0b101://2, 4 pins detected touch
        Serial.print("Touch detection! (Pins 4 and 2)\n");
        delay(1000); // 1 second delay
        Detections = Detections+2;
        EEPROM.write(0, Detections); //Write new Detection count EEPROM address 0
        break;
      case 0b110://2, 13 pins detected touch
        Serial.print("Touch detection! (Pins 2 and 13)\n");
        delay(1000); // 1 second delay
        Detections = Detections+2;
        EEPROM.write(0, Detections); //Write new Detection count EEPROM address 0
        break;
      case 0b111://2, 4, 13 pins detected touch
        Serial.print("Touch detection! (Pins 13,2 and 4)\n");
        delay(1000); // 1 second delay
        Detections = Detections+3;
        EEPROM.write(0, Detections); //Write new Detection count EEPROM address 0
        break;
    }
  }
  EEPROM.commit(); //Saves changed values between detections.
}
