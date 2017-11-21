# Arduino-Projects
const int VCCPin = A0;
const int xPin   = A1;
const int yPin   = A2;
const int zPin   = A3;

const int GNDPin = A4;        //accelerometer
const int motorPin = 3;      //vibrator

int output_Pin = A5;
unsigned int rate;           //pulse_rate

int x = 0;
int y = 0;
int z = 0;
int i=0;
int count=0;
int angle[500];

void setup() {
// pin A0 (pin14) is VCC and pin A4 (pin18) in GND to activate the GY-61-module
pinMode(4, OUTPUT);
pinMode(5, OUTPUT);

pinMode(A0, OUTPUT);
pinMode(A4, OUTPUT);
digitalWrite(14, HIGH);
digitalWrite(18, LOW);

// activating debugging for arduino UNO
Serial.begin(9600);
pinMode(motorPin, OUTPUT);
} // end setup

void loop() { 

  //accelerometer
x = analogRead(xPin);
y = analogRead(yPin);
z = analogRead(zPin);
// show x, y and z-values
Serial.print("x = ");
Serial.print(x);
Serial.print("  y = ");
Serial.print(y);
Serial.print(" z = ");
Serial.print(z);
// show angle
Serial.print("  angle = ");
angle[i]=constrain(map(x,349,281,0,90),0,90);
Serial.println(angle[i]);
i++; 
if(angle[i]==angle[i-1])
{count++;
Serial.println(count);
}
else
{count=0;
}
if(count>5)
{Serial.print("Sleeping");
digitalWrite(4, HIGH);
digitalWrite(5, HIGH);
digitalWrite(motorPin, HIGH);
delay(2000);
}
digitalWrite(4, LOW);
digitalWrite(motorPin, LOW);


static double ol=0;                                 //heart beat sensor
  static double nv=0;
  int rv=analogRead(output_Pin);
  rate=(0.75*ol)+(0.25*rv);
  rate=rv/3-120;
  Serial.print(rv);
  Serial.print(",");
  Serial.print("Heart Rate:  ");
  Serial.println(rate);
  ol=rate;
  delay(1000);

  
} // end loop
