/*Actual code available here 
# Siddi-Bhargava
Alive Human Detection Robot By Mobile Rescue Robot 
For more details visit our zar file available for free
Note: Port numbers can be changed according to our convinience*/
#include <VirtualWire.h>
int pirPin = 9;//set PIR sensor to pin 9
int statePir = 0;//initialize PIR sensor to 0(Low)
int airquality = 0;// initialize Gas sensor to 0
//dedinr pin numbers of Arduino to O/P variables
#define in1 4 
#define in2 5
#define in3 6
#define in4 7
void setup(){
    Serial.begin(9600);
    vw_setup(2000);
    vw_set_tx_pin(3);    //Tx Data pin to Digital  Pin 2
    vw_set_rx_pin(2);    //Rx Data pin to Digital  Pin 2
    vw_rx_start();
    //declare output variables as OUTPUT
    pinMode(in1, OUTPUT);
    pinMode(in2, OUTPUT);
    pinMode(in3, OUTPUT);
    pinMode(in4, OUTPUT);
}
//Loop to chech the performance of PIR and Gas Sensor when connected to arduino usino virtual wire
void loop(){
  //PIR operation
    pinMode(pirPin,INPUT);
    statePir = digitalRead(pirPin);
    int sensorValue = analogRead(A0);
      if (statePir == HIGH){
          void robo();
          const char *msg = "A";
          vw_send((uint8_t *)msg, strlen(msg));
          vw_wait_tx();
          robo();
        }
      if (statePir == LOW){
          const char *msg = "B";
          vw_send((uint8_t *)msg, strlen(msg));
          vw_wait_tx();
          robo();
        }
        //Gas Sensor Operation
      if(airquality < 500){
          const char *msg = "1";
          vw_send((uint8_t *)msg, strlen(msg));
          vw_wait_tx();
          robo();
        }
      if(airquality > 500){
          const char *msg = "2";
          vw_send((uint8_t *)msg, strlen(msg));
          vw_wait_tx();
          robo();
        }
}
void robo()
{
  //Virtual Wire Execution based on the message stored in the corresponding array
    uint8_t buf[VW_MAX_MESSAGE_LEN];
    uint8_t buflen = VW_MAX_MESSAGE_LEN;
    if (vw_get_message(buf, &buflen)){ // Non-blocking
	int i;
       // Message with a good checksum received, dump it.
	for (i = 0; i < buflen; i++){
            if(buf[i] == 'a'){//if button 1 is pressed.... i.e.forward buton
               // Set Motor A backward
               digitalWrite(in1, HIGH);
               digitalWrite(in2, LOW);
              // Set Motor B backward
               digitalWrite(in3, HIGH);
               digitalWrite(in4, LOW);
               loop();
            }
            else if(buf[i] == 'b')//if button 2 is pressed.... i.e.back buton
            {
              // Set Motor A forward
               digitalWrite(in1, LOW);
               digitalWrite(in2, HIGH);
             // Set Motor B forward
               digitalWrite(in3, LOW);
               digitalWrite(in4, HIGH);
               loop();
            }
            if(buf[i] == 'c')//if button 3 is pressed.... i.e.stop buton
            {
               digitalWrite(in1, LOW);
               digitalWrite(in2, LOW);
               digitalWrite(in3, LOW);
               digitalWrite(in4, LOW);
               loop();
            }
           if(buf[i] == 'd')//if button 4 is pressed.... i.e.right buton
           {
               digitalWrite(in1, LOW);
               digitalWrite(in2, HIGH);
               digitalWrite(in3, HIGH);
               digitalWrite(in4, LOW);
               loop();
            }
           if(buf[i] == 'e')//if button 5 is pressed.... i.e.left buton
            {
               digitalWrite(in1, HIGH);
               digitalWrite(in2, LOW);
               digitalWrite(in3, LOW);
               digitalWrite(in4, HIGH);
               loop();
            }
      }//close for
   }//close if
}//close robo


