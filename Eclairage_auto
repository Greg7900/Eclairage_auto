//Par Grégory.D torch28@hotmail.fr

#include <EEPROM.h>
#include <NTPClient.h>
#include <ESP8266WiFi.h>
#include <LiquidCrystal_I2C.h>
#include <Wire.h>
#include <WiFiUdp.h>
#define EEPROM_SIZE 128

/////////////////////////////////
void effacerLigne(int ligne);
void heure_gestion (void);
const char* ssid = "Livebox-5D90";
const char* password = "ywGGHAf55mogoCasrh";

////////////heure_config/////////
WiFiClient espClient;  //heure
WiFiUDP ntpUDP;        //heure
NTPClient timeClient(ntpUDP, "europe.pool.ntp.org", -14400, 60000);
/////////////////////////////////
LiquidCrystal_I2C lcd(0x27, 16, 2);
////////////LED//////////////
int Timer=0;
int flag=1;
int LedARROSEUR = D7;
int ledWifi = D0;
int relai=D7;
int Heure_debut=12;
int Minute_debut=0;
int Heure_fin=12;
int Minute_fin=0;
int Marche=0;
int selection=0;
 //sauv////////
int address = 0;
int read_Heure_debut;
int read_Heure_fin;
int read_Minute_debut;
int read_Minute_fin;
int flag2=0;
/////////interrupt/////////
const byte TimeDown = D3;
const byte TimeUp = D4;
const byte select = D5;
const byte Start = D6;
volatile byte interruptCounter = 0;
byte customChar0[] = {B11111,B11000, B10100,B10010,B10001,B00000,B00000, B00000};
byte customChar1[] = {B11111,B00011,B00101,B01001,B10001,B00000,B00000,B00000}; 
byte customChar3[] = {B00000,B00000,B00000,B10001,B01001,B00101,B00011,B11111};
byte customChar2[] = {B00000,B00000,B00000,B10001,B10010,B10100,B11000,B11111};               
void setup() 
{

  pinMode(TimeDown, INPUT_PULLUP);
  pinMode(TimeUp, INPUT_PULLUP);
  pinMode(select, INPUT_PULLUP);
  pinMode(Start, INPUT_PULLUP);
  attachInterrupt(digitalPinToInterrupt(TimeDown), TimeDownINT, FALLING);
  attachInterrupt(digitalPinToInterrupt(TimeUp), TimeupINT, FALLING);
  attachInterrupt(digitalPinToInterrupt(select), selectINT, FALLING);
  attachInterrupt(digitalPinToInterrupt(Start), StartINT, FALLING);
  ////////INIT/com5//////////////////
  pinMode(ledWifi,OUTPUT);
  pinMode(relai,OUTPUT);
  digitalWrite(ledWifi,LOW);
  digitalWrite(relai,LOW);
  Serial.begin(115200);
  Serial.println("");
  Serial.print("Startup reason:");Serial.println(ESP.getResetReason());
  WiFi.begin(ssid,password);
  Serial.print("Wifi connecting to ");
  Serial.println( ssid );
  
  //////LCD////////////////////
  Wire.begin(D2, D1);//i2c ecran
  lcd.begin();
  lcd.home();
  lcd.print("Connecting to ");
  lcd.setCursor(0, 1);
  lcd.print(ssid);
  delay(2000);
  lcd.clear();
  /////////////////////////////


  while( WiFi.status() != WL_CONNECTED ){
      delay(500);
      Serial.print(".");
      lcd.print(".");      
  }
  lcd.clear();
  digitalWrite( ledWifi , HIGH); // LED D'indication connection

  
  
  Serial.println();
  Serial.println("Wifi Connected Success!");
  lcd.print(" Wifi Connected ");
  
  Serial.print("NodeMCU IP Address : ");
  Serial.println(WiFi.localIP() );

  
  Serial.println("\nWaiting for Internet time");
  
  // Démarrage du client NTP - Start NTP client
  timeClient.begin();
  lcd.clear();
  
}


void loop() // le corp de notre programme
{
   
   
   if( ( ( timeClient.getHours() ) > Heure_debut ) && ( ( timeClient.getHours() ) < Heure_fin ) (Minute_debut==(timeClient.getMinutes()) ))
  {
    digitalWrite(relai,HIGH);
    
    
  }
  
  if((Heure_fin==(timeClient.getHours()) )&& (Minute_fin==(timeClient.getMinutes()) ))
  {
    digitalWrite(relai,LOW);
   
    }
  
  */
  if((Heure_debut==(timeClient.getHours()) )&& (Minute_debut==(timeClient.getMinutes()) ))
  {
    digitalWrite(relai,HIGH);
    
    
  }
  
  if((Heure_fin==(timeClient.getHours()) )&& (Minute_fin==(timeClient.getMinutes()) ))
  {
    digitalWrite(relai,LOW);
   
    }
  if(interruptCounter>0){
      lcd.backlight();
      lcd.display();
      if(flag2==0)
      {
        EEPROM.begin(EEPROM_SIZE);
        address =0 ;
        EEPROM.get(address, read_Heure_debut);
        address += sizeof(read_Heure_debut); //update address value
        Heure_debut=read_Heure_debut;
        //Serial.println(read_Heure_debut);
        
        EEPROM.get(address, read_Heure_fin);
        address += sizeof(read_Heure_fin); //update address value
        Heure_fin=read_Heure_fin;
        //Serial.println(read_Heure_fin);
        
        EEPROM.get(address, read_Minute_debut);
        address += sizeof(read_Minute_debut); //update address value
        Minute_debut=read_Minute_debut;
       // Serial.println(read_Minute_debut);
        
        EEPROM.get(address, read_Minute_fin);
        address += sizeof(read_Minute_fin); //update address value
        Minute_fin=read_Minute_fin;
       // Serial.println(read_Minute_fin);
        
        
       
        
      }
      
      flag2=1;
      
      Timer=timeClient.getSeconds();
      interruptCounter=0;
      Serial.print("An interrupt has occurred. ");
      heure_gestion();
      if(Marche==1){
        digitalWrite(relai,HIGH);
      }else if(Marche==0)
      {
        digitalWrite(relai,LOW);
      }
       lcd.createChar(0,customChar0);
      lcd.createChar(1,customChar1);
      lcd.createChar(2,customChar2);
      lcd.createChar(3,customChar3);
      lcd.setCursor(6,1);
      lcd.write(selection);
      flag=1; 
   } 
 
  if(( (timeClient.getSeconds()) - Timer )>=10)
  {
     lcd.noBacklight();
     lcd.noDisplay();
    // Serial.println(flag2);
     if(flag2==1)
     {
     
      address =0 ;
      EEPROM.put(address, Heure_debut);
      address += sizeof(Heure_debut); //update address value
      
      EEPROM.put(address, Heure_fin);
      address += sizeof(Heure_fin); //update address value
      
      EEPROM.put(address, Minute_debut);
      address += sizeof(Minute_debut); //update address value
      
      EEPROM.put(address, Minute_fin);
      address += sizeof(Minute_fin); //update address value
      EEPROM.commit();
        
      EEPROM.end();
      flag2=0;
     }
  }
 
  timeClient.update();
  
  Serial.println(timeClient.getFormattedTime());
  lcd.setCursor(8, 1);
  lcd.print(timeClient.getFormattedTime());
  
  //////////////date///////////////////////////
  unsigned long epochTime = timeClient.getEpochTime();
  struct tm *ptm = gmtime ((time_t *)&epochTime);
  int monthDay = ptm->tm_mday;
  int currentMonth = ptm->tm_mon+1;
  int currentYear = ptm->tm_year+1900;
  String currentDate =  String(monthDay)+ "/" +String(currentMonth) + "/" + String(currentYear);
  Serial.print("Current date: ");
  Serial.println(currentDate); 
  lcd.setCursor(7, 0);
  lcd.print(currentDate);
  
  delay(500);
}
 
void heure_gestion (void)
{
////////////Aff-DEBUT///////////////////
      effacerLigne(0);
      lcd.setCursor(0,0);
      lcd.print("D");
      lcd.setCursor(1,0);
      lcd.print(Heure_debut);
      lcd.setCursor(3,0);
      lcd.print(":");
      lcd.setCursor(4,0);
      lcd.print(Minute_debut);
     
////////////Aff-FIN///////////////////
      effacerLigne(1);
      lcd.setCursor(0,1);
      lcd.print("F");
      lcd.setCursor(1,1);
      lcd.print(Heure_fin);
      lcd.setCursor(3,1);
      lcd.print(":");
      lcd.setCursor(4,1);
      lcd.print(Minute_fin);
      
}
ICACHE_RAM_ATTR void TimeupINT() {
  interruptCounter++;
  if(flag==1){
  if( (selection==0) ){if(Heure_debut<23){ Heure_debut++;}else{Heure_debut=0;}}
  else if(selection==1){if(Minute_debut<59){Minute_debut++;}else{Minute_debut=0;}}
  
  /////////HEURE DE FIN/////////////////
  else if(selection==2){if(Heure_fin<23){Heure_fin++;}else{Heure_fin==0;}}
  else if (selection==3){if(Minute_fin<59){Minute_fin++;}else{Minute_fin=0;}}
  flag=0;
 }
}
ICACHE_RAM_ATTR void TimeDownINT() 
{
  interruptCounter++;
  if(flag==1){
  if( (selection==0) ){if(Heure_debut>0){Heure_debut--;}else{Heure_debut=23;}}
  else if( (selection==1) ){if(Minute_debut>0){Minute_debut--;}else{Minute_debut=59;}}
  
  /////////HEURE DE FIN/////////////////
  else if(selection==2){if(Heure_fin>0){Heure_fin--;}else{Heure_fin=23;}}
  else if (selection==3){if(Minute_fin>0){Minute_fin--;}else{Minute_fin=59;}}
  flag=0;
 }
}

ICACHE_RAM_ATTR void StartINT() {
  interruptCounter++;
 if(flag==1){
     Marche=!Marche;
    
    flag=0;
  }
  
}

ICACHE_RAM_ATTR void selectINT() {
  interruptCounter++;
 if(flag==1){
  if(selection<3){selection++;}else{selection=0;}
  flag=0;
 }
 
}

void effacerLigne(int ligne)
{
    lcd.setCursor(0, ligne);
    lcd.print("                ");
}
