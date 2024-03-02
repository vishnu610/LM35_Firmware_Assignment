/*
 * LM35_Firmware_Assignment
 *
 * Created: 02-03-2024 
 * Author : Vishnu V Nair
 */ 

#include <TimerOne.h>         //Header for timer1 function

#define sensor_pin A0         //LM35 pin to A0
#define LED_pin 13            //LED pin to 13(Built-in)

const int temp_limit = 30;          //Temp_limit variable

void setup() 
{
  Serial.begin(9600); 
  pinMode(LED_pin, OUTPUT);       //Led pin as o/p
}

void loop() 
{   
  LED_control();        //Calling function to control LED
}

//Function to read and display Temperature
float temperature() 
{
  float sensor_val = analogRead(sensor_pin);            //Read analog sensor value
  float volt = sensor_val * 0.004882814;                   //Convert analog value to voltage (sensor_value*(5/1024))
  float temp = (volt - 0.5) * 100;                        //Convert voltage to temperature ((Volt - 500mV)*100)

  Serial.println(temp);       //Print temperature in serial monitor

  return temp;        //Return temperature value
}

//Function to control LED as per requirment
void LED_control() 
{
  float temp = temperature();       //Store temperature value to temp variable

  //If temperature is below 30
  if (temp_limit > temp) 
  { 
   LED_toggle(250000);      //Toggling LED for 250ms
  } 
  else //If temperature is above the limit
  { 
   LED_toggle(500000);        //Toggling LED for 500ms
  }
}

//Function to toggle LED
void LED_toggle(int delay) 
{
  digitalWrite(LED_pin, !digitalRead(LED_pin));     //Toggle LED
  Timer1.setPeriod(delay);          //Timer function to set delay
}
