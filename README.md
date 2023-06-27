/*************************************************************
  Blynk is a platform with iOS and Android apps to control
  ESP32, Arduino, Raspberry Pi and the likes over the Internet.
  You can easily build mobile and web interfaces for any
  project by simply dragging and dropping widgets.

    Downloads, docs, tutorials: https://www.blynk.io
    Sketch generator:           https://examples.blynk.cc
    Blynk community:            https://community.blynk.cc
    Follow us:                  https://www.fb.com/blynkapp
                                https://twitter.com/blynk_app

  Blynk Library is licensed under the MIT license
 *************************************************************
  Blynk.Edgent implements:
  - Blynk.Inject - Dynamic WiFi credentials provisioning
  - Blynk.Air    - Over The Air firmware updates
  - Device state indication using a physical LED
  - Credentials reset using a physical Button
 *************************************************************/

#define BLYNK_TEMPLATE_ID " ID "
#define BLYNK_TEMPLATE_NAME "NAME"

#define BLYNK_FIRMWARE_VERSION        "0.1.0"

#define BLYNK_PRINT Serial
//#define BLYNK_DEBUG

#define APP_DEBUG

// Uncomment your board, or configure a custom board in Settings.h
//#define USE_SPARKFUN_BLYNK_BOARD
//#define USE_NODE_MCU_BOARD
//#define USE_WITTY_CLOUD_BOARD
//#define USE_WEMOS_D1_MINI

#include "BlynkEdgent.h"
#include "Servo.h"

Servo servo1;
Servo servo2;
int val0=0;
int val1=0;
int val2=0;
int val3=0;
int val4=0;
int val5=0;
int val6=0;
 BLYNK_WRITE(V0) {
     val0 = param.asInt();
    //digitalWrite(16, val0);
    Serial.println("FWD");
        Serial.println(val0);
}
BLYNK_WRITE(V1) {
     val1 = param.asInt();
    //digitalWrite(5, val1);
    Serial.println("BWD");
        Serial.println(val1);
}
BLYNK_WRITE(V2) {
     val2 = param.asInt();
    //digitalWrite(12, val2);
    Serial.println("Right");
        Serial.println(val2);
}
BLYNK_WRITE(V3) {
     val3 = param.asInt();
    //digitalWrite(13, val3);
    Serial.println("Left");
    Serial.println(val3);
}


BLYNK_WRITE(V4) {
    val4 = param.asInt();
    digitalWrite(14,val4);
    Serial.println("shooter on");
    Serial.println(val4);
}


BLYNK_WRITE(V5) {
    val5 = param.asInt();
    servo1.write(val5);
    Serial.println("UP and Down");
    Serial.println(val5);
}

BLYNK_WRITE(V6) {
    val6 = param.asInt();
    servo2.write(val6);
    Serial.println("left and right");
    Serial.println(val6);
}
void smartcar() {
  if (val0 == 1) {
    carforward();
    Serial.println("carforward");
  } else if (val1 == 1) {
    carbackward();
    Serial.println("carbackward");
  } else if (val2 == 1) {
    carturnleft();
    Serial.println("carfleft");
  } else if (val3 == 1) {
    carturnright();
    Serial.println("carright");
  } else if (val0 == 0 && val1 == 0 && val2 == 0 && val3 == 0) {
    carStop();
    Serial.println("carstop");
  }
}
void setup()
{
 pinMode(16,OUTPUT);
  pinMode(12,OUTPUT);
  pinMode(5,OUTPUT);
  pinMode(13,OUTPUT);

  pinMode(14,OUTPUT);

  servo1.attach(4);
  servo2.attach(15);
  Serial.begin(115200);
  delay(100);
BlynkEdgent.begin();
  
}

void loop() {
 if (val0==1){          // FORWard V0
  digitalWrite(16,HIGH);                  //right connection
  digitalWrite(5,LOW);
  
  digitalWrite(13,HIGH);                  //Left connection
  digitalWrite(12,LOW);
  //Serial.print("Forward");
  }
  if (val1==1){          // BAckWard V1
  
  digitalWrite(16,LOW);
  digitalWrite(5,HIGH);
  
  digitalWrite(13,LOW);
  digitalWrite(12,HIGH);
   //Serial.print("Backward");
  }
  if (val2==1){           // Right V2
  digitalWrite(16,HIGH);
  digitalWrite(5,LOW);
  
  digitalWrite(13,LOW);
  digitalWrite(12,HIGH);      //low
  //Serial.print("Right");
  }
  //delay(1200);
  if(val3==1){             // left V3
  digitalWrite(16,LOW); 
  digitalWrite(5,HIGH);   // low
  
  digitalWrite(13,HIGH);
  digitalWrite(12,LOW);     
  //Serial.print("Left");
  }
  //delay(1200);
  else {
  digitalWrite(16,LOW);//stop
  digitalWrite(5,LOW);
  digitalWrite(12,LOW);
  digitalWrite(13,LOW);
  //Serial.print("stop");
  }
  
  BlynkEdgent.run();
  
}
