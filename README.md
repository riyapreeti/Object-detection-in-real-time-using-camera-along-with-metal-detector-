# Object-detection-in-real-time-using-camera-along-with-metal-detector-
Built a detection algorithm to identify objects using camera and metal detector to detect hidden metallic objects.
Object detection in real life using cameras and metal detectors is a powerful technology that is widely used for security and safety purposes. It involves using a camera to capture real-time images of an area, which are then analyzed by an object detection algorithm to identify any objects of interest. At the same time, a metal detector can be used to detect metallic objects that may be hidden or concealed from view. When combined, these two technologies provide a powerful tool for identifying potential threats, such as weapons or other dangerous objects. In addition to security applications, object detection using a camera and metal detector can also be used in industrial settings, such as mining and construction, to identify hazards and potential safety risks. Overall, object detection technology is a crucial component of modern safety and security systems and continues to play an important role in ensuring public safety.. Metal detectors aid in detecting and preventing accidents caused by metal items, such as tools or debris, which can cause injuries or damage to equipment. Metal detectors may also detect valuable minerals such as gold or copper and protect equipment by removing large metal fragments that may have fallen into machinery. Furthermore, metal detectors are utilized to find unexploded munitions, assuring worker and community safety. Coal mines also use metal detectors to identify metal debris and avoid accidents caused by hidden metal items. Mining businesses can safeguard the security of their personnel and equipment, enhance productivity, and locate lucrative minerals by utilizing metal detectors.
Arduino code:
 #define capPin A5
#define buz 9
#define pulsePin A4
#define led 10

long sumExpect=0; //running sum of 64 sums 
long ignor=0; //number of ignored sums
long diff=0; //difference between sum and avgsum
long pTime=0;
long buzPeriod=0; 

void setup() 
{
Serial.begin(9600);
  pinMode(pulsePin, OUTPUT); 
  digitalWrite(pulsePin, LOW);
  pinMode(capPin, INPUT);  
  pinMode(buz, OUTPUT);
digitalWrite(buz, LOW);
  pinMode(led, OUTPUT);
}

void loop() 
{
  int minval=1023;
  int maxval=0;
  long unsigned int sum=0;
  for (int i=0; i<256; i++)
  {
    //reset the capacitor
pinMode(capPin,OUTPUT);
    digitalWrite(capPin,LOW);
    delayMicroseconds(20);
    pinMode(capPin,INPUT);
    applyPulses();
//read the charge of capacitor
    int val = analogRead(capPin); //takes 13x8=104 microseconds
    minval = min(val,minval);
    maxval = max(val,maxval);
    sum+=val;

long unsigned int cTime=millis();
    char buzState=0;
    if (cTime<pTime+10) { if (diff>0)
        buzState=1;
      else if(diff<0) buzState=2; } if (cTime>pTime+buzPeriod)
    {
      if (diff>0)
buzState=1;  
else if (diff<0) buzState=2; pTime=cTime; } if (buzPeriod>300)
    buzState=0;
if (buzState==0)
    {
      digitalWrite(led, LOW);
      noTone(buz);

    }  
    else if (buzState==1)
    {
      tone(buz,100);
      digitalWrite(led, HIGH);
    }   
 else if (buzState==2)
    {
      tone(buz,500);
    }
  })
//subtract minimum and maximum value to remove spikes
  sum-=minval; 
  sum-=maxval;
  
  if (sumExpect==0) 
  sumExpect=sum<<6; //set sumExpect to expected value 
  long int avgsum=(sumExpect+32)>>6; 
  diff=sum-avgsum;
  if (abs(diff)>10)
{
    sumExpect=sumExpect+sum-avgsum;
    ignor=0;
  } 
  else 
    Ignor++;
if (ignor>64)
  { 
sumExpect=sum<<6;
ignor=0;
  }
if (diff==0) 
    buzPeriod=1000000;
  else 
  buzPeriod=avgsum/(2*abs(diff));    
}
  void applyPulses()
{
    for (int i=0;i<3;i++) 
    {
      digitalWrite(pulsePin,HIGH); //take 3.5 uS
      delayMicroseconds(3);
      digitalWrite(pulsePin,LOW); //take 3.5 uS
      delayMicroseconds(3);
    }
}
Object Detection model : https://colab.research.google.com/drive/1vKbZrJo08DCUqmgKIPas-9e_snyxD9hw?usp=sharing





