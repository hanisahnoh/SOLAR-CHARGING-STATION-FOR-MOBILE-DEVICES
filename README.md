# SOLAR-CHARGING-STATION-FOR-MOBILE-DEVICES
Embedded Computer and Communication System - ECC3303

//PROJECT  : SOLAR CHARGING STATION FOR MOBILE DEVICES
//SEMESTER : SEM 2 - 2018/2019, Mei 2019
//SUBJECT : EMBEDDED COMPUTER AND COMMUNICATION SYSTEM
//TEAM MEMBERS  : HANISAH MOHD NOH, CHONG JOON WAI, MUHAMMAD HAKIM LOKMAN, UMI NABILA SHUKOR 
//programming author  : chong joon wai

// serial print for blynk software on serial monitor 
#define BLYNK_PRINT Serial
// call esp8266 wifi module library
// call blynk library for esp8266 wifi module
// call 16x2 LCD library to control 
#include <ESP8266_Lib.h>
#include <BlynkSimpleShieldEsp8266.h>
#include <LiquidCrystal.h>

// declare port number in arduino uno for LCD pin 
// initialize the library by associating any needed LCD interface pin
// with the arduino pin number it is connected to
const int rs = 12, en = 11, d4 = 5, d5 = 6, d6 = 10, d7 = 9;
LiquidCrystal lcd(rs, en, d4, d5, d6, d7);

// declare and assign value for the integer output
int greenLed = 7;
int redLed = 4;
int signal_battery = 13;
int signal_direct_load = 8;

// global declaration for variables
// declaration in interger for the varaibles 
int analogValue = 0;
int analogValuesolar = 0;
int ledDelay = 1000;
int num;

// declaration in floating point numbers for the varaibles 
float voltage_value = 0;
float voltage_value_solar = 0;
float vout =0;
float R1 = 30000;
float R2 = 7500;

// Auth Token in the Blynk App.
char auth[] = "9b01971828974d2a97a8db1525bd80a7";

// WiFi credentials.
// Set password to "" for open networks.
// set the wifi access point name to be access
// set the password of the access point
char ssid[] = "Honor 8X";
char pass[] = "31415926";

// call SoftwareSerial.h library 
#include <SoftwareSerial.h>

// set the pin in arduino uno to be transmit and receive 
SoftwareSerial EspSerial(2, 3); // RX, TX ( tx-rx, rx-tx)

// ESP8266 baud rate set to 9600
#define ESP8266_BAUD 9600

ESP8266 wifi(&EspSerial);

// allow to perfrom periodic in main loop() context
BlynkTimer timer;

// function to display content in 16x2 LCD display 
void LCD(int value, float volt_value, float volt_value_solar){

     // if the solar panel voltage more than or equals to 4 volts
   // display the solar panel voltage value
   // display the battery voltage value
   // display the battery percentage 
   // display the solar panel state
   // light up green LED
     if( volt_value_solar >= 4.0 ){
       lcd.clear();
       lcd.setCursor(0,0);
       lcd.print("BAT:");
       lcd.print(volt_value,3);
       lcd.print(" PT:");
       lcd.print(value);
       lcd.print("%");
       lcd.setCursor(0,1);
       lcd.print("SOL:");
       lcd.print(volt_value_solar,3);
       lcd.print(" STT:H");
       digitalWrite(greenLed, HIGH);
       digitalWrite(redLed, LOW);
     
     // if battery percentage equal or more than 90%
     // set output high for digital pin 8 in arduino uno
     // set output low for digital pin 13 in arduino uno
     
     // else-if battery percentage less than than 90%
     // set output high for digital pin 8 in arduino uno
     // set output high for digital pin 13 in arduino uno
     
       if( value >= 90 ){
        digitalWrite(signal_direct_load, HIGH);  
        digitalWrite(signal_battery, LOW);
       }else{
        digitalWrite(signal_battery, HIGH);
        digitalWrite(signal_direct_load, HIGH); 
       }
     
     // if the solar panel voltage less than 4 volts
   // display the solar panel voltage value
   // display the battery voltage value
   // display the battery percentage 
   // display the solar panel state
   // light up red LED
   // set output low for digital pin 8 in arduino uno
   // set output low for digital pin 13 in arduino uno
     }else if ( volt_value_solar < 4.0){
       lcd.clear();
       lcd.setCursor(0,0);
       lcd.print("BAT:");
       lcd.print(volt_value,3);
       lcd.print(" PT:");
       lcd.print(value);
       lcd.print("%");
       lcd.setCursor(0,1);
       lcd.print("SLR:");
       lcd.print(volt_value_solar,3);
       lcd.print(" STT:L");
       digitalWrite(redLed, HIGH);
       digitalWrite(greenLed, LOW);
       digitalWrite(signal_direct_load, LOW);
       digitalWrite(signal_battery, LOW);
     } 
  
  
}

// fucntion to determine the battery percentage based on the voltage value from battery
// return the battery percentage value to "num" variable
// call LCD(); function to display the battery percentage
void battery_percent(float voltage, float volt_value_solar_1 ){
  if( voltage > 4.0 ){
    num = 100;
    LCD( num, voltage, volt_value_solar_1 );
  }else if (voltage > 3.995 && voltage <= 4.0){
    num = 99;
    LCD( num, voltage, volt_value_solar_1 );
  }else if(voltage > 3.990 && voltage <= 3.995){ 
    num = 98;
    LCD( num, voltage, volt_value_solar_1 );
  }else if (voltage > 3.985 && voltage <= 3.990){
    num = 97;
    LCD( num, voltage, volt_value_solar_1 );
  }else if(voltage > 3.980 && voltage <= 3.985){
    num = 96;
    LCD( num, voltage, volt_value_solar_1 );
  }else if (voltage > 3.975 && voltage <= 3.980){
    num = 95;
    LCD( num, voltage, volt_value_solar_1 );
  }else if(voltage > 3.970 && voltage <= 3.975){
    num = 94;
    LCD( num, voltage, volt_value_solar_1 );
  }else if (voltage > 3.965 && voltage <= 3.970){
    num = 93;
    LCD( num, voltage, volt_value_solar_1 );
  }else if(voltage > 3.960 && voltage <= 3.965){
    num = 92;
    LCD( num, voltage, volt_value_solar_1 );
  }else if(voltage > 3.955 && voltage <= 3.960){
    num = 91;
    LCD( num, voltage, volt_value_solar_1 );
  }else if(voltage > 3.950 && voltage <= 3.955){
    num = 90;
    LCD( num, voltage, volt_value_solar_1 ); 
  }else if (voltage > 3.945 && voltage <= 3.950){
    num = 89;
    LCD( num, voltage, volt_value_solar_1 );
  }else if(voltage > 3.940 && voltage <= 3.945){
    num = 88;
    LCD( num, voltage, volt_value_solar_1 );
  }else if (voltage > 3.935 && voltage <= 3.940){
    num = 87;
    LCD( num, voltage, volt_value_solar_1 );
  }else if(voltage > 3.930 && voltage <= 3.935){  
    num = 86;
    LCD( num, voltage, volt_value_solar_1 );
  }else if (voltage > 3.925 && voltage <= 3.930){
    num = 85;
    LCD( num, voltage, volt_value_solar_1 );
  }else if(voltage > 3.920 && voltage <= 3.925){
    num = 84;
    LCD( num, voltage, volt_value_solar_1 );
  }else if(voltage > 3.915 && voltage <= 3.920){ 
    num = 83;
    LCD( num, voltage, volt_value_solar_1 );
  }else if(voltage > 3.910 && voltage <= 3.915){
    num = 82;
    LCD( num, voltage, volt_value_solar_1 ); 
  }else if (voltage > 3.905 && voltage <= 3.910){
    num = 81;
    LCD( num, voltage, volt_value_solar_1 );
  }else if(voltage > 3.900 && voltage <= 3.905){ 
    num = 80;
    LCD( num, voltage, volt_value_solar_1 );
  }else if (voltage > 3.895 && voltage <= 3.900){
    num = 79;
    LCD( num, voltage, volt_value_solar_1 );
  }else if(voltage > 3.890 && voltage <= 3.895){ 
    num = 78;
    LCD( num, voltage, volt_value_solar_1 );
  }else if(voltage > 3.885 && voltage <= 3.890){
    num = 77;
    LCD( num, voltage, volt_value_solar_1 );
  }else if (voltage > 3.880 && voltage <= 3.885){
    num = 76;
    LCD( num, voltage, volt_value_solar_1 );
  }else if(voltage > 3.875 && voltage <= 3.880){
    num = 75;
    LCD( num, voltage, volt_value_solar_1 );
  }else if(voltage > 3.870 && voltage <= 3.875){
    num = 74;
    LCD( num, voltage, volt_value_solar_1 );
  }else if(voltage > 3.865 && voltage <= 3.870){
    num = 73;
    LCD( num, voltage, volt_value_solar_1 );
  }else if (voltage > 3.860 && voltage <= 3.865){
    num = 72;
    LCD( num, voltage, volt_value_solar_1 );
  }else if(voltage > 3.855 && voltage <= 3.860){ 
    num = 71;
    LCD( num, voltage, volt_value_solar_1 );
  }else if(voltage > 3.850 && voltage <= 3.855){
    num = 70;
    LCD( num, voltage, volt_value_solar_1 );
  }else if(voltage > 3.845 && voltage <= 3.850){
    num = 69;
    LCD( num, voltage, volt_value_solar_1 );
  }else if(voltage > 3.840 && voltage <= 3.845){
    num = 68;
    LCD( num, voltage, volt_value_solar_1 );
  }else if (voltage > 3.835 && voltage <= 3.840){
    num = 67;
    LCD( num, voltage, volt_value_solar_1 );
  }else if(voltage > 3.830 && voltage <= 3.835){
    num = 66;
    LCD( num, voltage, volt_value_solar_1 );
  }else if(voltage > 3.825 && voltage <= 3.830){
    num = 65;
    LCD( num, voltage, volt_value_solar_1 );
  }else if(voltage > 3.820 && voltage <= 3.825){
    num = 64;
    LCD( num, voltage, volt_value_solar_1 );
  }else if(voltage > 3.815 && voltage <= 3.820){
    num = 63;
    LCD( num, voltage, volt_value_solar_1 );
  }else if (voltage > 3.810 && voltage <= 3.815){
    num = 62;
    LCD( num, voltage, volt_value_solar_1 );
  }else if(voltage > 3.805 && voltage <= 3.810){
    num = 61;
    LCD( num, voltage, volt_value_solar_1 );
  }else if(voltage > 3.800 && voltage <= 3.805){
    num = 60;
    LCD( num, voltage, volt_value_solar_1 );
  }else if(voltage > 3.795 && voltage <= 3.800){
    num = 59;
    LCD( num, voltage, volt_value_solar_1 );
  }else if(voltage > 3.790 && voltage <= 3.795){ 
    num = 58;
    LCD( num, voltage, volt_value_solar_1 );
  }else if (voltage > 3.785 && voltage <= 3.790){
    num = 57;
    LCD( num, voltage, volt_value_solar_1 );
  }else if(voltage > 3.780 && voltage <= 3.785){
    num = 56;
    LCD( num, voltage, volt_value_solar_1 );
  }else if (voltage > 3.775 && voltage <= 3.780){
    num = 55;
    LCD( num, voltage, volt_value_solar_1 );
  }else if(voltage > 3.770 && voltage <= 3.775){
    num = 54;
    LCD( num, voltage, volt_value_solar_1 );
  }else if (voltage > 3.765 && voltage <= 3.770){
    num = 53;
    LCD( num, voltage, volt_value_solar_1 );
  }else if(voltage > 3.760 && voltage <= 3.765){
    num = 52;
    LCD( num, voltage, volt_value_solar_1 );
  }else if(voltage > 3.755 && voltage <= 3.760){
    num = 51;
    LCD( num, voltage, volt_value_solar_1 );
  }else if(voltage > 3.750 && voltage <= 3.755){
    num = 50;
    LCD( num, voltage, volt_value_solar_1 );
  }else if (voltage > 3.745 && voltage <= 3.750){
    num = 49;
    LCD( num, voltage, volt_value_solar_1 );
  }else if(voltage > 3.740 && voltage <= 3.745){
    num = 48;
    LCD( num, voltage, volt_value_solar_1 ); 
  }else if (voltage > 3.735 && voltage <= 3.740){
    num = 47;
    LCD( num, voltage, volt_value_solar_1 );
  }else if(voltage > 3.730 && voltage <= 3.735){  
    num = 46;
    LCD( num, voltage, volt_value_solar_1 );
  }else if (voltage > 3.725 && voltage <= 3.730){
    num = 45;
    LCD( num, voltage, volt_value_solar_1 );
  }else if(voltage > 3.720 && voltage <= 3.725){
    num = 44;
    LCD( num, voltage, volt_value_solar_1 );
  }else if(voltage > 3.715 && voltage <= 3.720){ 
    num = 43;
    LCD( num, voltage, volt_value_solar_1 );
  }else if(voltage > 3.710 && voltage <= 3.715){
    num = 42;
    LCD( num, voltage, volt_value_solar_1 );
  }else if (voltage > 3.705 && voltage <= 3.710){
    num = 41;
    LCD( num, voltage, volt_value_solar_1 );
  }else if(voltage > 3.700 && voltage <= 3.705){ 
    num = 40;
    LCD( num, voltage, volt_value_solar_1 );
  }else if (voltage > 3.695 && voltage <= 3.700){
    num = 39;
    LCD( num, voltage, volt_value_solar_1 );
  }else if(voltage > 3.690 && voltage <= 3.695){ 
    num = 38;
    LCD( num, voltage, volt_value_solar_1 );
  }else if(voltage > 3.685 && voltage <= 3.690){
    num = 37;
    LCD( num, voltage, volt_value_solar_1 );
  }else if (voltage > 3.680 && voltage <= 3.685){
    num = 36;
    LCD( num, voltage, volt_value_solar_1 );
  }else if(voltage > 3.675 && voltage <= 3.680){
    num = 35;
    LCD( num, voltage, volt_value_solar_1 );
  }else if(voltage > 3.670 && voltage <= 3.675){
    num = 34;
    LCD( num, voltage, volt_value_solar_1 );
  }else if(voltage > 3.665 && voltage <= 3.670){ 
    num = 33;
    LCD( num, voltage, volt_value_solar_1 );
  }else if (voltage > 3.660 && voltage <= 3.665){
    num = 32;
    LCD( num, voltage, volt_value_solar_1 );
  }else if(voltage > 3.655 && voltage <= 3.660){ 
    num = 31;
    LCD( num, voltage, volt_value_solar_1 );
  }else if(voltage > 3.650 && voltage <= 3.655){
    num = 30;
    LCD( num, voltage, volt_value_solar_1 );
  }else if(voltage > 3.645 && voltage <= 3.650){
    num = 29;
    LCD( num, voltage, volt_value_solar_1 );
  }else if(voltage > 3.640 && voltage <= 3.645){
    num = 28;
    LCD( num, voltage, volt_value_solar_1 );
  }else if (voltage > 3.635 && voltage <= 3.640){
    num = 27;
    LCD( num, voltage, volt_value_solar_1 );
  }else if(voltage > 3.630 && voltage <= 3.635){
    num = 26;
    LCD( num, voltage, volt_value_solar_1 );
  }else if(voltage > 3.625 && voltage <= 3.630){
    num = 25;
    LCD( num, voltage, volt_value_solar_1 );
  }else if(voltage > 3.620 && voltage <= 3.625){
    num = 24;
    LCD( num, voltage, volt_value_solar_1 );
  }else if(voltage > 3.615 && voltage <= 3.620){
    num = 23;
    LCD( num, voltage, volt_value_solar_1 );
  }else if (voltage > 3.610 && voltage <= 3.615){
    num = 22;
    LCD( num, voltage, volt_value_solar_1 );
  }else if(voltage > 3.605 && voltage <= 3.610){
    num = 21;
    LCD( num, voltage, volt_value_solar_1 );
  }else if(voltage > 3.600 && voltage <= 3.605){
    num = 20;
    LCD( num, voltage, volt_value_solar_1 );
  }else if(voltage > 3.595 && voltage <= 3.600){
    num = 19;
    LCD( num, voltage, volt_value_solar_1 );
  }else if(voltage > 3.590 && voltage <= 3.595){ 
    num = 18;
    LCD( num, voltage, volt_value_solar_1 );
  }else if(voltage > 3.585 && voltage <= 3.590){
    num = 17;
    LCD( num, voltage, volt_value_solar_1 );
  }else if (voltage > 3.580 && voltage <= 3.585){
    num = 16;
    LCD( num, voltage, volt_value_solar_1 );
  }else if(voltage > 3.575 && voltage <= 3.580){
    num = 15;
    LCD( num, voltage, volt_value_solar_1 );
  }else if(voltage > 3.570 && voltage <= 3.575){
    num = 14;
    LCD( num, voltage, volt_value_solar_1 );
  }else if(voltage > 3.565 && voltage <= 3.570){ 
    num = 13;
    LCD( num, voltage, volt_value_solar_1 );
  }else if (voltage > 3.560 && voltage <= 3.565){
    num = 12;
    LCD( num, voltage, volt_value_solar_1 );
  }else if(voltage > 3.555 && voltage <= 3.560){ 
    num = 11;
    LCD( num, voltage, volt_value_solar_1 );
  }else if(voltage > 3.550 && voltage <= 3.555){
    num = 10;
    LCD( num, voltage, volt_value_solar_1 );
  }else if(voltage > 3.545 && voltage <= 3.550){
    num = 9;
    LCD( num, voltage, volt_value_solar_1 );
  }else if(voltage > 3.540 && voltage <= 3.545){
    num = 8;
    LCD( num, voltage, volt_value_solar_1 );
  }else if (voltage > 3.535 && voltage <= 3.540){
    num = 7;
    LCD( num, voltage, volt_value_solar_1 );
  }else if(voltage > 3.530 && voltage <= 3.535){
    num = 6;
    LCD( num, voltage, volt_value_solar_1 );
  }else if(voltage > 3.525 && voltage <= 3.530){
    num = 5;
    LCD( num, voltage, volt_value_solar_1 );
  }else if(voltage > 3.520 && voltage <= 3.525){
    num = 4;
    LCD( num, voltage, volt_value_solar_1 );
  }else if(voltage > 3.515 && voltage <= 3.520){
    num = 3;
    LCD( num, voltage, volt_value_solar_1 );
  }else if (voltage > 3.510 && voltage <= 3.515){
    num = 2;
    LCD( num, voltage, volt_value_solar_1 );
  }else if(voltage > 3.505 && voltage <= 3.510){
    num = 1;
    LCD( num, voltage, volt_value_solar_1 );
  }else if(voltage <= 3.505){
    num = 0;
    LCD( num, voltage, volt_value_solar_1 );
  }
  }

// function for initialize variables, pin modes, start using libraries 
void setup()
{
  // set the variables as output in designated port
  pinMode(greenLed, OUTPUT);
  pinMode(redLed,OUTPUT);
  pinMode(signal_battery, OUTPUT);
  pinMode(signal_direct_load,OUTPUT);

  // Debug console
  Serial.begin(9600);
  
  // initialize the interface to LCD screen
  lcd.begin(16, 2);
  
  // Set ESP8266 baud rate
  EspSerial.begin(ESP8266_BAUD);
  // delay 0.01 second
  delay(10);

  Blynk.begin(auth, wifi, ssid, pass);
  // You can also specify server:
  //Blynk.begin(auth, wifi, ssid, pass, "blynk-cloud.com", 80);
  //Blynk.begin(auth, wifi, ssid, pass, IPAddress(192,168,1,100), 8080);


}

// This function sends Arduino's up time every second to Virtual Pin (5).
// In the app, Widget's reading frequency should be set to PUSH. This means
// that you define how often to send data to Blynk App.
    // You can send any value at any time.
  // Please don't send more that 10 values per second.

void loop()
{
  // read voltage value of battery in analog pin A0
  analogValue = analogRead(A0);
  // change the analog of the input to voltage reading 
  // use volatage division rule to obtain volatge value 
  // assign value to the variable "voltage_value"
  vout = (analogValue * 5.0) / 1024.0;
  voltage_value = vout / (R2/(R1+R2));

  // assign 0 value to the temporary variable "vout"
  vout = 0;
  
  // read voltage value of battery in analog pin A1
  analogValuesolar = analogRead(A1);
  // change the analog of the input to voltage reading 
  // use volatage division rule to obtain volatge value 
  // assign value to the variable "voltage_value_solar"
  vout = (analogValuesolar * 5.0) / 1024.0;
  voltage_value_solar = vout / (R2/(R1+R2));

  // call battery_percent(); function to determine battery percentage based on the callibrated battery voltage value 
  battery_percent(voltage_value,voltage_value_solar);
  // delay 0.1 second
  delay(100);
  
  // set virtual pin to be display in blynk software
  // set virtual pin V5 for "voltage_value" variable
  // set virtual pin V6 for "voltage_value_solar" variable
  // set virtual pin V7 for "num" variable
  Blynk.virtualWrite(V5, voltage_value);
  Blynk.virtualWrite(V6, voltage_value_solar);
  Blynk.virtualWrite(V7, num);
  
  // keep connection alive, sending data and receiving data from blynk software
  Blynk.run();
  // Initiates BlynkTimer
  timer.run(); 
}
