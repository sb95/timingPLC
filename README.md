//incomplete, just an example

//time management
long currentTime;
long onTime;
long offTime;
const long oneMinute = 60000;
long minCompare;
int indexMin;

//analog readings
float onTimeRead;
float offTimeRead;
int onSelect;
int offSelect;

//latching management
boolean sysOn = false;
boolean sysOff;
const int butOn = 4;
const int butOff = 5;
int butOnState;
int butOffState;
boolean stateDesided; 

//physical control
const int motRelay = 6;

void setup() {
  Serial.begin(9600);
  pinMode(butOn, INPUT);
  pinMode(butOff, INPUT);
  pinMode(motRelay, OUTPUT);
}

void loop() {
  currentTime = millis();
  
  onTimeRead = analogRead(A0); //pot for on time
  offTimeRead = analogRead(A1); //pot for off time
  stateDesided = onOffState(); //2 buttons will decide on off state

  if(stateDesided){ //if button on decided
    onSelect = minuteInterval(onTimeRead); //save selected minutes to run
    offSelect = minuteInterval(offTimeRead); //save selected minutes to stay off
    minCompare = currentTime; //update time for proper comparison
      while(sysOn){ //while off button is NOT pressed
        onOffState(); //monitor what button is pressed
        int totalRunTime = onSelect + offSelect; //total cycle time in minutes
          if(timeCompare){ //every minute increment index
            indexMin++;
           }
          if(indexMin <= onSelect){
          digitalWrite(motRelay, HIGH); 
          }
          else if(indexMin > offSelect && indexMin <= totalRunTime){
          digitalWrite(motRelay, LOW);
          }
          else if(indexMin > totalRunTime){
          indexMin = 0;
          }
      }
    }

  
}

int minuteInterval(float dataIn){
  if(dataIn >= 0 && dataIn <= 102.3){
    return 1;
    }
  else if(dataIn > 102.3 && dataIn <= 204.8){
    return 2;
    }
  else if(dataIn > 204.8 && dataIn <= 306.9){
    return 3;
    }
  else if(dataIn > 306.9 && dataIn <= 409.2){
    return 4;
    }
  else if(dataIn > 409.2 && dataIn <= 511.5){
    return 5;
    }
  else if(dataIn > 511.5 && dataIn <= 614.4){
    return 6;
    }
  else if(dataIn > 614.4 && dataIn <= 716.1){
    return 7;
    }
  else if(dataIn > 716.1 && dataIn <= 818.4){
    return 8;
    }
  else if(dataIn > 818.4 && dataIn <= 920.7){
    return 9;
    }
  else if(dataIn > 920.7 && dataIn <= 1023){
    return 10;
    }
  }

boolean timeCompare(long minCompare){
  if(currentTime - minCompare >= oneMinute){
    return true;
    }
  else{
    return false;
    }
  }

boolean onOffState(){
  if(digitalRead(butOn) == HIGH && sysOn == false){
    sysOn = true;
    sysOff = false;
    return true;
    }
  else if(digitalRead(butOff) == HIGH && sysOff == false){
    sysOn = false;
    sysOff = true;
    return false;
    }
  }
