// =============NPK pH Temp Humidity EC===========//
// --------------TTL to RS485 module -------------//
// --------------auto 485 module Blue board ===//
#include <SoftwareSerial.h>
#include "crc16.h"
#include <Wire.h>
#include <Arduino.h>
#include <U8x8lib.h>

#ifdef U8X8_HAVE_HW_SPI
#include <SPI.h>
#endif
#ifdef U8X8_HAVE_HW_I2C
#include <Wire.h>
#endif

U8X8_SSD1327_EA_W128128_HW_I2C u8x8(/* reset=*/ U8X8_PIN_NONE);

SoftwareSerial SoilSerial(0,2);   // (RX,TX)  of ESP8266

byte Adrr=0x01, Fcode=0x03;
//unsigned int Soil_N,Soil_P,Soil_K,Soil_pH=0;
byte values[11];
int regval[2];

void setup() {

  Serial.begin(4800);
  while (!Serial);
  SoilSerial.begin(4800);
  u8x8.begin();
}

void loop() {
  
  unsigned int val1=0;
  val1 = modRead(0x00);//moise(%);
  Serial.print ("moisture=");
  Serial.println(val1*0.1);delay(250);
 
  val1 = modRead(0x01);//Temperature(C);
  Serial.print ("temp=");
  Serial.println(val1*0.1);  delay(250);
 
  val1 = modRead(0x02);//Conductivity(EC-us/cm);
  Serial.print ("ec=");
  Serial.println(val1*0.1);  delay(250);
 
  val1 = modRead(0x03);//pH;
  Serial.print ("pH=");
  Serial.println(val1*0.1);delay(250);
 
  val1 = modRead(0x04);//Nitrogen(N-ppm);
  Serial.print ("N=");
  Serial.println(val1);delay(250);
 
  val1 = modRead(0x05); //Phosphorus(P-ppm);
  Serial.print ("P=");
  Serial.println(val1);  delay(250);
 
  val1 = modRead(0x06); //Potassium(K-ppm);
  Serial.print ("K=");
  Serial.println(val1);  delay(250);
 
  delay(5000);

  Show();
}

unsigned int modRead(byte reg){

  byte RegisterH=0x00,RegisterL=reg, len =0x01;
  byte x[6] = { Adrr, Fcode,RegisterH,RegisterL,0x00,len};
  uint16_t u16CRC=0xffff;
  for (int i = 0; i < 6; i++)
    {
        u16CRC = crc16_update(u16CRC, x[i]);

    }   //Serial.print(Adrr,HEX);Serial.print(",");Serial.print(Fcode,HEX);Serial.print(",");Serial.print(len,HEX);Serial.print(",");Serial.print(lowByte(u16CRC),HEX); Serial.print(",");
        //Serial.println(highByte(u16CRC),HEX);
  byte request[8] = { Adrr, Fcode,RegisterH,RegisterL,0x00,len, lowByte(u16CRC),highByte(u16CRC)};
 
  //digitalWrite(EnTxPin, HIGH); digitalWrite(EnRxPin, HIGH);
  //delay(50);
  if(SoilSerial.write(request,sizeof(request))==8)
  {
     //digitalWrite(EnTxPin, LOW); digitalWrite(EnRxPin, LOW);
     delay(50);
     for(byte i=0;i<7;i++){
       
        values[i] = SoilSerial.read();
        //Serial.print(" 0x");
        //Serial.print(values[i],HEX);
       
     }
     //Serial.println("");
     //Serial.flush();
  }
   unsigned int val = (values[3]<<8)| values[4];
    
   //Serial.print(val);
  return val;
}
void Show(void){
    unsigned int Soil_N,Soil_P,Soil_K,Soil_pH,Soil_EC=0;
  Soil_N = modRead(0x04);//N;
  Soil_P = modRead(0x05);//P
  Soil_K = modRead(0x06); //K;
  u8x8.setFont(u8x8_font_amstrad_cpc_extended_f);    
  u8x8.clear();
  u8x8.setCursor(0,1);
  u8x8.print("N=");
  u8x8.setCursor(2,1);
  u8x8.print(Soil_K);
  u8x8.setCursor(5,1);
  u8x8.print("ppm");
}
