# DE96356_FINAL_CLASS_PROJECT
Final Class Project (Reverse Parking Sensor Using Arduino)

//Library code for LCD
#include <LiquidCrystal.h>

//Initialize the variables
LiquidCrystal lcd = LiquidCrystal(8,9,10,11,12,13);

int distanceThreshold = 0;	//declare variable for containing distance sensed
int i	  			  = 0;	//for Ultrasonic Sensor
int inches            = 0;	//for Ultrasonic Sensor

float pulse1, cm ; 


void setup()
{  
 Serial.begin(9600);	 	//Send the data rate in bits per second for serial data
  
  pinMode(2, OUTPUT); 		//set LED 1(green) as output
  pinMode(3, OUTPUT); 		//set LED 2(yellow) as output
  pinMode(4, OUTPUT); 		//set LED 3(red) as output
  pinMode(5, OUTPUT); 		//set Piezo buzzer as output
  pinMode(7, OUTPUT); 		//set the trigger pin as output
  pinMode(6,  INPUT); 		//set the echo pin as input  
  
  lcd.begin(16,2);			//setup the LCDs number of columns and rows
  //For LCD Display  
  lcd.setCursor(0,0);
  lcd.print("Distance in CM"); //print current distance setting on lcd		
  lcd.setCursor(0,1);
  lcd.print(cm);
}


void loop()
{
  int pulse1;
  distanceThreshold = 330;	    // Maximum distance for sensor
  digitalWrite(7, HIGH);	   //Set the trigger pin on
  delay(5);
  digitalWrite(7,LOW);		   //Set the trigger pin off
  pulse1=pulseIn(6,HIGH);	   //Echo pin turn on to give pulse before
  cm = 0.01723 * pulse1;	   //program start
  inches = (cm / 2.54);
  Serial.print(cm);
  Serial.print("cm, ");
  Serial.print(inches);
  Serial.print("in,");
  Serial.print(pulse1);
  Serial.println(" ");
  
//For LCD Display  
  lcd.setCursor(0,0);
  lcd.print("Distance in CM"); //print current distance setting on lcd		
  lcd.setCursor(0,1);
  lcd.print(cm);
  
    
  if (cm > distanceThreshold) {
    digitalWrite(2, LOW);
    digitalWrite(3, LOW);
    digitalWrite(4, LOW);
    
  }
  
  if (cm <= distanceThreshold && cm > distanceThreshold - 65)
  {
    digitalWrite(2, HIGH);	//Green LED turn ON
    digitalWrite(3,  LOW);	//Yellow LED turn OFF
    digitalWrite(4,  LOW);	//Red LED turn OFF
    i=1;
    while (i==1)
    {
    tone(5,350);
      delay(500);
      noTone(5);
      delay(500);
      i=0;
    }
  }
  if (cm <= distanceThreshold - 260 && cm > distanceThreshold - 300) {
    digitalWrite(2, HIGH);	//Green LED turn ON
    digitalWrite(3, HIGH);	//Yellow LED turn ON
    digitalWrite(4,  LOW);	//Red LED turn OFF
    i=1;
    while (i==1)
    {
    tone(5,350);
      delay(100);
      noTone(5);
      delay(100);
      i=0;
    }
  }
  if (cm <= distanceThreshold - 300 && cm > distanceThreshold - 330) {
    digitalWrite(2, HIGH);	//Green LED turn ON
    digitalWrite(3, HIGH);	//Yellow LED turn ON
    digitalWrite(4, HIGH);	//Red LED turn ON
    i=1;
    while (i==1)
    {
    tone(5,350);
      delay(50);
      noTone(5);
      delay(50);
      i=0;
    }
  }
  else if (cm<= distanceThreshold-330)
  {
    tone(5,350);
    noTone(5);
  }
  delay(100); 
}
