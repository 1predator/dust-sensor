#include <PMsensor.h>
#include <Wire.h> 
#include <LiquidCrystal_I2C.h>

LiquidCrystal_I2C lcd(0x3F,16,2);
PMsensor PM;
int redPin=3;
int greenPin=5;
int bluePin=6;
void setup() {
  pinMode(redPin, OUTPUT);
    pinMode(greenPin, OUTPUT);
    pinMode(bluePin, OUTPUT);

  Serial.begin(9600);

  /////(infrared LED pin, sensor pin)  /////
  PM.init(2, A0);
}

void loop() {
  
  Serial.println("=================================");
  Serial.println("Read PM2.5");

  float filter_Data = PM.read(0.1);//센서값에서 전의 Filter값을 90%만 반영하고 방금 읽은 값을 10%만 반영해서 정확도를 높이겠다.
  float noFilter_Data = PM.read();//방금 읽은 값이 nofilter_data

  Serial.print("Filter : ");
  Serial.println(filter_Data);
  Serial.print("noFilter : ");
  Serial.println(noFilter_Data);
  
  if (filter_Data<=30)
  {digitalWrite(redPin, LOW);
    digitalWrite(greenPin, HIGH);
    digitalWrite(bluePin,LOW);
    delay(100);   
  }
  if(filter_Data<=80 && filter_Data >30)
  {digitalWrite(redPin, LOW);
    digitalWrite(greenPin, LOW);
    digitalWrite(bluePin,HIGH);
    delay(100);   
  }
  if (filter_Data>80) 
  {digitalWrite(redPin, HIGH);
    digitalWrite(greenPin, LOW);
    digitalWrite(bluePin,LOW);
    delay(100); 
  }
  lcd.init();
  lcd.backlight();
  lcd.setCursor(1,0);
  lcd.print("Filter:");

  lcd.setCursor(0,1);
  lcd.print("filter_Data");
  lcd.setCursor(12,1);
  lcd.print(filter_Data);

  
  delay(100);
}
