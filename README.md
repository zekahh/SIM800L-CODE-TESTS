# SIM800L-CODE-TESTS
//SIM800L Codes for to implement in your arduino/esp32/devboard#


//The very basic code for trying the SIM800L in Aruino IDE 

//1)###############################################################################
[CODE

void setup() {
  // put your setup code here, to run once:

}

void loop() {
  // put your main code here, to run repeatedly:

}

//##################################################################################
//Exactly you don't need any written code for testing the work of the SIM800L module.

//STEPS

//Connect the Arduino's TX pin to the SIM800L's RX pin, and the Arduino's RX PIN to the SIM800L's TX pin
//Connect the Arduino's GND pin to the SIM800L's GND pin
//Feed SIM800L's vcc with the Arduino's 3.3V pin 

//Set up your dev board configuration in arduino's IDE tools, then open the Serial monitor 
//try the next commands

AT  




//2) Now you can try to change the TX and RX pins from arduino, 
//in this code those pins are 8 = RX and 9 = TX

// The special requirement for this is that you have to implement a resistor division from the arduino's TX pin to the RX
// SIM800L pin, this way you can make sure that you won't put your SIM800L module in risk.

// Personally
//(some reckless people use the Arduino's TX pin straight to the SIM800L RX pin) I wouldn't because too risky (maybe not)
// and to get new modules from Alliexpress or from the city takes too much time.
//[CODE

#include <SoftwareSerial.h>

SoftwareSerial mySerial(8, 9);  // RX  TX to SIM800 TX RX
void setup()
{
mySerial.begin(9600);
 Serial.begin(9600);
 delay(1000);
}

void loop()
{
 //Envíamos y recibimos datos
 if (Serial.available() > 0)
mySerial.write(Serial.read());
 if (mySerial.available() > 0)
 Serial.write(mySerial.read());
}

//STEPS

//For this case the schematic will change as written above.



//3) Now you can try a very sofisticated code (in spanish which I got from this web page
// https://www.minitronica.com/enviando-sms-arduino-sim800l/ they also show you the schematic of the circuit
// kind of similar to the last one.
//they also show you how to properly push the simcard into the SIM800L module.

//[CODE

SoftwareSerial mySerial(8, 9); // Declaramos los pines RX(8) y TX(9) que vamos a usar
 
void setup(){
Serial.begin(9600);       // Iniciamos la comunicacion serie
mySerial.begin(9600);     // Iniciamos una segunda comunicacion serie
delay(1000);              // Pausa de 1 segundo
EnviaSMS();               // Llamada a la funcion que envia el SMS
}
 
void loop(){
if (mySerial.available()){          // Si la comunicacion SoftwareSerial tiene datos
  Serial.write(mySerial.read());    // Los sacamos por la comunicacion serie normal
} 
  
if (Serial.available()){            // Si la comunicacion serie normal tiene datos
  while(Serial.available()) {       // y mientras tenga datos que mostrar 
    mySerial.write(Serial.read());  // Los sacamos por la comunicacion SoftwareSerial
  } 
  mySerial.println();               // Enviamos un fin de linea
}
}
 
// Funcion para el envio de un SMS
void EnviaSMS(){              
 mySerial.println("AT+CMGF=1");                 // Activamos la funcion de envio de SMS
 delay(100);                                    // Pequeña pausa
 mySerial.println("AT+CMGS=\"+34XXXXXXXXX\"");  // Definimos el numero del destinatario en formato internacional
 delay(100);                                    // Pequeña pausa
 mySerial.print("Hola Mundo!");                 // Definimos el cuerpo del mensaje
 delay(500);                                    // Pequeña pausa
 mySerial.print(char(26));                      // Enviamos el equivalente a Control+Z 
 delay(100);                                    // Pequeña pausa
 mySerial.println("");                          // Enviamos un fin de linea
 delay(100);                                    // Pequeña pausa
}





That's it
