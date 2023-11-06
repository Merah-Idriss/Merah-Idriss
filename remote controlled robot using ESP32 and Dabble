#include <analogWrite.h>
#define CUSTOM_SETTINGS
#define INCLUDE_GAMEPAD_MODULE
#include <DabbleESP32.h>

//Déclaration des contantes mises en jeu et des pins des capteurs infrarouges et du buzzer en tant qu'entrées/sorties
int capteurD = 34;
int capteurG = 35;
int buzzer = 12;

// Declaration des variables (distances mesurées par les capteurs)
int delta=0;

// Les variables state qui nous permettent de switcher entre les modes
int state;
int statetel=0;
int sensorState=1;
int sensorlateral;


// Variables duree et distance
long duration;
float DistD;
float DistG;
float DistA;
float distanceCm;
const float SOUND_SPEED=0.034;

// Declaration des pins des capteurs et des LEDs
const int TRIGGER_PIN1 = 15;
const int ECHO_PIN1 = 2;
const int TRIGGER_PIN2 = 5;
const int ECHO_PIN2 = 18;
const int TRIGGER_PIN3 = 22;
const int ECHO_PIN3 = 21;
const byte LED_autonome = 4;
const byte LED_teleguidee = 13;


// Vitesse des moteurs
int Power=100; // vitesse du moteur 

// On définit les capteurs

// On définit les pins du motor A
int motor1Pin1 = 27;
int motor1Pin2 = 26;
int enable1Pin = 14;

// On définit les pins du motor B
int motor2Pin1 = 32;
int motor2Pin2 = 33;
int enable2Pin = 25;


//On définit les pin (output ou input) et on les initialise



void setup(){

Serial.begin(115200);
 // Mettant les pins des capteurs et des LEDs comme des OUTPUTs
 pinMode(TRIGGER_PIN1, OUTPUT);
 digitalWrite(TRIGGER_PIN1, LOW);
 pinMode(TRIGGER_PIN2, OUTPUT);
 digitalWrite(TRIGGER_PIN2, LOW);
 pinMode(TRIGGER_PIN3, OUTPUT);
 digitalWrite(TRIGGER_PIN3, LOW);
 pinMode(ECHO_PIN1, INPUT);
 pinMode(ECHO_PIN2, INPUT);
 pinMode(ECHO_PIN3, INPUT);
 pinMode(LED_autonome, OUTPUT);
 digitalWrite(LED_autonome, LOW);
 pinMode(LED_teleguidee, OUTPUT);
 digitalWrite(LED_teleguidee, LOW);
 pinMode(buzzer,OUTPUT);
 digitalWrite(buzzer, LOW);

// Appellant la bib de L'application Dabble et choisissons un nom pour notre robot
Dabble.begin("KOUKIS.420");

//Mettant les pins du moteur A comme des OUTPUTs
pinMode(motor1Pin1, OUTPUT);
pinMode(motor1Pin2, OUTPUT);
pinMode(enable1Pin, OUTPUT);

// Mettant les pins du moteur B as outputs:
pinMode(motor2Pin1, OUTPUT);
pinMode(motor2Pin2, OUTPUT);
pinMode(enable2Pin, OUTPUT);

// On initialise les moteurs en etat haut
digitalWrite(enable1Pin, HIGH);
digitalWrite(enable2Pin, HIGH);

// Mettant les pins du buzzer, ainsi que des capteurs infrarouges as inputs/outputs
pinMode(capteurD, INPUT);
pinMode(capteurG, INPUT);
}


// La fonction Enable du mouvement
void movementEnable(){
digitalWrite(enable1Pin,HIGH); 
digitalWrite(enable2Pin,HIGH);
}

// La fonction Disable du mouvement
void movementDisable(){
digitalWrite(enable1Pin,LOW); 
digitalWrite(enable2Pin,LOW);
}

// La fonction marcher vers l'avant
void GoForward(int Power ){
movementEnable();
analogWrite(motor1Pin1,Power);
analogWrite(motor1Pin2,0);
analogWrite(motor2Pin1,Power);
analogWrite(motor2Pin2,0);

}

// La fonction marcher en arrière
void GoBackward(int Power){
movementEnable();
analogWrite(motor1Pin1,0);
analogWrite(motor1Pin2,Power);
analogWrite(motor2Pin1,0);
analogWrite(motor2Pin2,Power);

}

// La fonction tourner à gauche
void TurnLeft(int Power){
movementEnable();
analogWrite(motor1Pin1,0);
analogWrite(motor1Pin2,Power);
analogWrite(motor2Pin1,Power);
analogWrite(motor2Pin2,0);

}

// La fonction tourner à droite
void TurnRight(int Power){
movementEnable();
analogWrite(motor1Pin1,Power);
analogWrite(motor1Pin2,0);
analogWrite(motor2Pin1,0);
analogWrite(motor2Pin2,Power);

}

// La fonction arreter les moteurs
void MotorStop(){
movementDisable();
analogWrite(motor1Pin1,0);
analogWrite(motor1Pin2,0);
analogWrite(motor2Pin1,0);
analogWrite(motor2Pin2,0);

}


// La fonction qui nous calcule les distances mesurées par le capteurs
float distance(int trigPin, int echoPin) {
  // Clears the trigPin
  digitalWrite(trigPin, LOW);
  delayMicroseconds(2);
  // Sets the trigPin on HIGH state for 10 micro seconds
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);
  
  // Reads the echoPin, returns the sound wave travel time in microseconds
  duration = pulseIn(echoPin, HIGH);
  
  // Calculate the distance
  distanceCm = duration * SOUND_SPEED/2;    
 
  
  // Prints the distance in the Serial Monitor
  Serial.print("Distance (cm): ");
  Serial.println(distanceCm);
  
  
  delay(100);
  return(distanceCm);
}





//  Les buttons
void buttons(){
  Dabble.processInput();
  boolean a = GamePad.isUpPressed();
  boolean b = GamePad.isDownPressed();
  boolean c = GamePad.isLeftPressed();
  boolean d = GamePad.isRightPressed();
  boolean f = GamePad.isCirclePressed();
  boolean h = GamePad.isSquarePressed();
  boolean g = GamePad.isCrossPressed();
  if (f) {
    statetel=1;
  }
  else if (h) {
    statetel=2;
  }
  else if (a) {
    statetel=3;
  }
  else if (b) {
    statetel=4;
  }
  else if (c) {
    statetel=5;
  }
  else if (d) {
    statetel=6;
  }
  else{
    statetel=7;
  }
}


// La fonction du mode téleguidée
void modeteleguide() {
   
 buttons();
 switch(statetel){
  case 1: 
  Power=Power+10;
  delay(500);
  break;
  case 2:
  Power=Power-10;
  delay(500);
  break;
  case 3:
  GoForward(Power);
  break;
  case 4:
  GoBackward(Power);
  break;
  case 5:
  TurnLeft(Power);
  break;
  case 6:
  TurnRight(Power);
  break;
  case 7:
  MotorStop();
  break;

}

}

// La fonction qui nous indique si on a un obstacle ou pas (capteur avant)
void readSensor(){
   DistA=distance(TRIGGER_PIN1, ECHO_PIN1);
    if(DistA<20){
   if (sensorState==1) {sensorState=2;}
  }else{
  sensorState=1; 
  }
}

// En cas de la présence d'un obstacle, cette fonction compare la distance de gauche et de droite 
 // pour pouvoir choisir la direction dans laquelle le robot va tourner
void comparaison() {
 
  DistD=distance(TRIGGER_PIN3, ECHO_PIN3);
  DistG=distance(TRIGGER_PIN2, ECHO_PIN2);
  delta=DistG-DistD;
  if(delta>=0){
    sensorlateral=1;
      
  }
  else {
    sensorlateral=2;
  }

 
}

// La fonction mode autonome

void autonomousMode(){

  
  readSensor();
  switch(sensorState){
    case 1:
    GoForward(Power);
    break;
    case 2:
    MotorStop();
    delay(500);
    GoBackward(Power);
    delay(500);
    MotorStop();
   comparaison();
    switch(sensorlateral){
      case 1: 
      TurnLeft(Power);
      break;
      case 2:
      TurnRight(Power);
      break;
    }
   break;
  }
}

// La fonction siveur de ligne
void suiveurDeLigne(){
  boolean etat_capteurD=digitalRead(capteurD);
  boolean etat_capteurG=digitalRead(capteurG);
  if((etat_capteurD)&&(!etat_capteurG)){
    
   
    TurnRight(Power);
  }
  else if ((!etat_capteurD)&&(etat_capteurG)){
   
    TurnLeft(Power);
  }
  else{
    GoForward(Power);
  }
}


// La fonction qui nous permet de changer les modes
void changerlemode() {
   Dabble.processInput();
     boolean i = GamePad.isStartPressed();
     boolean j = GamePad.isSelectPressed();
     boolean e = GamePad.isTrianglePressed();
     if(i){
      state=1;
      }
    if(j){
    state=2;
    }
    if(e){
      state=3;
    }
}

// Notre main
void loop(){
  changerlemode();
switch(state){
  case 1:
  digitalWrite(LED_autonome, LOW);
  digitalWrite(LED_teleguidee,HIGH);
  modeteleguide();
  break;
  case 2:
  digitalWrite(LED_autonome, HIGH);
  digitalWrite(LED_teleguidee, LOW);
  autonomousMode();
  break;
  case 3:
  digitalWrite(LED_autonome, LOW);
  digitalWrite(LED_teleguidee, LOW);
  suiveurDeLigne();
  break;
  }
}
