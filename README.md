#include <LiquidCrystal.h>
#include <DFRobot_DHT11.h>
DFRobot_DHT11 DHT;
#define DHT11_PIN A1
int rs = 8, en = 9, d4 = 10, d5 = 11, d6 = 12, d7 = 13;
LiquidCrystal lcd(rs, en, d4, d5, d6, d7);
int buz=A4;
int ldr=A3;
int ws=A2;
int gs=A0;
int cnt=0;
void setup() {
// put your setup code here, to run once:
Serial.begin(9600);
delay(200);
pinMode(buz,OUTPUT);
lcd.begin(16, 2);
lcd.print(" WELCOME");
delay(200);
}
void loop() {
// put your main code here, to run repeatedly//
int lval=analogRead(ldr)/10.23;
int gval=analogRead(gs)/10.23;
int wval=100-analogRead(ws)/10.23;
DHT.read(DHT11_PIN);
int tval=DHT.temperature;
int hval=DHT.humidity;
lcd.clear();
lcd.print("T:"+String(tval) + " H:"+String(hval) + " L:"+String(lval));
lcd.setCursor(0,1);
lcd.print("G:"+String(gval)+ " W:"+ String(wval));
cnt=cnt+1;
if(cnt>15)
{
cnt=0;
Serial.print("248930,64B5G8TKFAYZMNRZ,0,0,Desktop123,1234567890,"+ String(tval) +
","+String(hval)+ ","+String(lval)+ ","+String(gval)+ ","+String(wval) +",0\n");
}
if(tval>40 || hval>80 || gval>30 || wval>30)
{
digitalWrite(buz,1);
delay(300);
digitalWrite(buz,0);
}
delay(1000);
}
