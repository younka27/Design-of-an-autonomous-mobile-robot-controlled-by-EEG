# Design-of-a-Real-Time-electronic-system-for-the-navigation-of-an-autonomous-mobile-robot-controlled-by-EEG
# Design-of-an-autonomous-mobile-robot-controlled-by-EEG
The C code to use to control the robot with EEG:
#define BAUDRATE 57600 
#define DEBUGOUTPUT 0 
#define powercontrol 10 
#define in1 9
#define enA 10
#define in4 6
#define enB 5
#define in2 8
#define in3 7
const int trigPin = 53;
const int echoPin = 23;
int redPin = 2;
int greenPin = 3;
int bluePin = 4;
byte generatedChecksum = 0; 
byte checksum = 0; 
int payloadLength = 0; 
byte payloadData[64] = {0}; 
byte poorQuality = 0; 
byte attention = 0;
byte meditation = 0; 
long lastReceivedPacket = 0; 
boolean bigPacket = false; 
void setup() 
{ 
  pinMode(trigPin, OUTPUT);
  pinMode(echoPin, INPUT);
  pinMode(in1, OUTPUT);
  pinMode(enA, OUTPUT);
  pinMode(in4, OUTPUT);
  pinMode(enB, OUTPUT);
  pinMode(in2, OUTPUT);
  pinMode(in3, OUTPUT);
  pinMode(redPin, OUTPUT);
  pinMode(greenPin, OUTPUT);
  pinMode(bluePin, OUTPUT);
Serial.begin(BAUDRATE); // USB 
}
long duration, distance;
byte ReadOneByte(){ 
int ByteRead; 
while(!Serial.available()); 
ByteRead = Serial.read(); 
#if DEBUGOUTPUT 
Serial.print((char)ByteRead); 
#endif 
return ByteRead;
}
void loop()  {
if(ReadOneByte() == 170) 
{ 
if(ReadOneByte() == 170) 
{
payloadLength = ReadOneByte(); 
if(payloadLength > 169) 
return; 
generatedChecksum = 0; 

for(int i = 0; i < payloadLength; i++) 
{ 
payloadData[i] = ReadOneByte(); 
generatedChecksum += payloadData[i]; 
}
checksum = ReadOneByte(); 
generatedChecksum = 255 - generatedChecksum; 
if(checksum == generatedChecksum) 
{ 
poorQuality = 0; 
attention = 0; 
meditation = 200; 
for(int i = 0; i < payloadLength; i++) 
{ 
switch (payloadData[i]) 
{ 
case 2: 
i++; 
poorQuality = payloadData[i]; 
bigPacket = true; 
break; 
case 4: 
i++; 
attention = payloadData[i]; 
break; 
case 5: 
i++; 
meditation = payloadData[i]; 
break; 
case 0x80: 
i = i + 3; 
break; 
case 0x83:
i = i + 25; 
break; 
default: 
break; 
} // switch 
} // for loop 
#if !DEBUGOUTPUT
if(bigPacket) 
{ 
Serial.print("PoorQuality: "); 
Serial.print(PoorQuality, DEC); 
Serial.print(" Attention: "); 
Serial.print(attention, DEC); 
Serial.print(" Time since last packet: "); 
Serial.print(millis() - lastReceivedPacket, DEC); 
lastReceivedPacket = millis(); 
Serial.print("\n");
switch(attention / 10) {
          case 0:
            setColor(0, 5,0);
            digitalWrite(in1, LOW);
            digitalWrite(in2, HIGH);
            analogWrite(enA,5);
            digitalWrite(in4, HIGH);
            digitalWrite(in3, LOW);
            analogWrite(enB,5);      
            break;
          case 1:
             setColor(0, 25,0);
            digitalWrite(in1, LOW);
            digitalWrite(in2, HIGH);
            analogWrite(enA,12.5);
            digitalWrite(in4, HIGH);
             digitalWrite(in3, LOW);
            analogWrite(enB,12.5);      
            break;
          case 2:
            setColor(0, 50,0);
            digitalWrite(in1, LOW);
            digitalWrite(in2, HIGH);
            analogWrite(enA,25);
            digitalWrite(in4, HIGH);
             digitalWrite(in3, LOW);
            analogWrite(enB,55);      
            break;
          case 3: 
            setColor(0, 100,0);
            digitalWrite(in1, LOW);
            digitalWrite(in2, HIGH);
            analogWrite(enA,37.5);
            digitalWrite(in4, HIGH);
             digitalWrite(in3, LOW);
            analogWrite(enB,37.5);      
            break;
          case 4: 
            setColor(0, 150,0);
            digitalWrite(in1, LOW);
            digitalWrite(in2, HIGH);
            analogWrite(enA,50);
digitalWrite(in4, HIGH);
             digitalWrite(in3, LOW);
            analogWrite(enB,50);      
            break;
          case 5:
            setColor(0, 200,0);
            digitalWrite(in1, LOW);
            digitalWrite(in2, HIGH);
            analogWrite(enA,75);
            digitalWrite(in4, HIGH);
             digitalWrite(in3, LOW);
            analogWrite(enB,75);      
            break;
          case 6:
            setColor(0, 255,0);
            digitalWrite(in1, LOW);
            digitalWrite(in2, HIGH);
            analogWrite(enA,87.5);
            digitalWrite(in4, HIGH);
             digitalWrite(in3, LOW);
            analogWrite(enB,87.5);      
            break;
          case 7:
            setColor(50, 0,0);
            digitalWrite(in1, LOW);
            digitalWrite(in2, HIGH);
            analogWrite(enA,100);
            digitalWrite(in4, HIGH);
             digitalWrite(in3, LOW);
            analogWrite(enB,100);      
            break;
          case 8:
            setColor(150, 0,0);
            digitalWrite(in1, LOW);
            digitalWrite(in2, HIGH);
            analogWrite(enA,112.5);
            digitalWrite(in4, HIGH);
             digitalWrite(in3, LOW);
            analogWrite(enB,112.5);      
            break;
          case 9:
            setColor(200, 0,0);
            digitalWrite(in1, LOW);
            digitalWrite(in2, HIGH);
            analogWrite(enA,125);
            digitalWrite(in4, HIGH);
             digitalWrite(in3, LOW);
            analogWrite(enB,125);      
            break;
          case 10:
            setColor(255, 0,0);
            digitalWrite(in1, LOW);
            digitalWrite(in2, HIGH);
            analogWrite(enA,150);
            digitalWrite(in4, HIGH);
             digitalWrite(in3, LOW);
            analogWrite(enB,150);      
            break;
          }
   digitalWrite(trigPin, LOW);
  delayMicroseconds(2);
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);  
  duration = pulseIn(echoPin, HIGH);
  distance = duration/58.2;
if(poorQuality>0){
    setColor(0, 255,255);
   digitalWrite(in4,HIGH);
   digitalWrite(in3,LOW);
   analogWrite(enB,100);
   digitalWrite(in1,HIGH);
   digitalWrite(in2,LOW);
   analogWrite(enA,62.5);
}
 if(distance<30){
   setColor(0, 0,255);
   digitalWrite(in4,LOW);
   digitalWrite(in3,LOW);
  digitalWrite(in1,LOW);
   digitalWrite(in2,LOW);
   if(poorQuality>0){
   setColor(0, 255,255);
  digitalWrite(in4,HIGH);
   digitalWrite(in3,LOW);
    analogWrite(enB,100);
  digitalWrite(in1,HIGH);
   digitalWrite(in2,LOW);
   analogWrite(enA,62.5);
}
 } 
}
#endif 
bigPacket = false; 
}
else { 
// Checksum Error 
} // end if else for checksum 
} // end if read 0xAA byte 
} // end if read 0xAA byte 
}
void setColor(int red, int green, int blue)
{
analogWrite(redPin, red);
analogWrite(greenPin, green);
analogWrite(bluePin, blue);
} 
