#include <SoftwareSerial.h>
#include <ArduinoBlue.h>
#include <LiquidCrystal.h>
#define MOTOR 9 // PWM control for motor
#define Heat_LED 13 // LED Replacement for Heating systems in the Massaging Belt
#define sensorPin A5 // Thermal Senor
#define buzzerPin 8 // Piezoelectric Buzzer
14
#define BLUETOOTH_TX 3 //Bluetooth module's TX pin connected to 3
#define BLUETOOTH_RX 2 //Bluetooth module's RX pin connected to 2
const unsigned long BAUD_RATE = 9600;
const unsigned int MIN_SPEED = 50; //Minimum Speed for CAM motor
const unsigned int MAX_SPEED = 200; //Maximum Speed for CAM motor
const unsigned int START_SPEED = 100; //Strting/Default Speed for CAM motor
const unsigned int MAX_TEMP_LIMIT = 105; // Temperature limit to turn on the buzzer, 100 Farenheit
const unsigned int TEMP_RESET_TIME = 10000; // Time to cool down if temperature limit is reached: 10 seconds
float HotMassageTemp = 85; // Default Temperature for Hot Massage
const int rs = 12, en = 11, d4 = 7, d5 = 6, d6 = 5, d7 = 4; //LCD communication Pins
LiquidCrystal lcd(rs, en, d4, d5, d6, d7); //Setting Up LCD
SoftwareSerial bluetooth(BLUETOOTH_TX, BLUETOOTH_RX); // Connecting to the Bluetooth Module
ArduinoBlue phone(bluetooth); // pass reference of bluetooth object to ArduinoBlue constructor
//Variables for Code
int speed=START_SPEED;
char cold_message[]= "Cold Massage ON!";
char heat_message[]= "Heat Massage ON!";
String default_message = "Hey! Im Ready";
String my_message = "";
int current_mode = 2;
int button, sliderId, sliderVal;
int high_temp_count = 0;
int req_temp_reached = 0;
void setup() {
// Start serial communications.
Serial.begin(BAUD_RATE);
bluetooth.begin(BAUD_RATE);
delay(100);
pinMode(Heat_LED, OUTPUT); // LED to replace thermal equipments
pinMode(MOTOR, OUTPUT); // CAM Motor pin
pinMode(sensorPin, INPUT); //Thermal Sensor Pin
pinMode(buzzerPin, OUTPUT); // Piezo Electric Buzzer Pin
lcd.begin(16, 2);
reset (1);
Serial.println("setup complete");
}
// Put your main code here, to run repeatedly:
void loop() {
// ID of the button pressed pressed.
button = phone.getButton();
// ID of the slider moved.
sliderId = phone.getSliderId();
// Slider value goes from 0 to 200.
sliderVal = phone.getSliderVal();
// Get the voltage reading from the TMP36
15
int reading = analogRead(sensorPin);
float voltage = reading * (5.0 / 1024.0);
float temperatureC = (voltage - 0.5) * 100;
float temperatureF = (temperatureC * 9.0 / 5.0) + 32.0;
Serial.print(temperatureF);
Serial.print("\xC2\xB0"); // shows degree symbol
Serial.println("F");
if (current_mode != 2){
temperatureF = temperatureF - 5; // Temperature sensor give high values when the motor is running. Not sure why
}
if (temperatureF > MAX_TEMP_LIMIT){
// The temperature sensor sometime gives a high temperature as noise when reading was taken at a high frequency.
// So ensuring that the there is high temperature for atleast 20 times (using high_temp_count variable) before starting the cooling down
procedure
high_temp_count +=1;
if (high_temp_count>20){
Serial.println("Buzzering");
tone(buzzerPin, 300);
delay (1000);
noTone(buzzerPin);
digitalWrite(Heat_LED, LOW); // switch OFF Heat_LED
digitalWrite (MOTOR, LOW); // Turn off motor
send_message ("HIGH TEMP ALERT", 0);
delay (2000);
send_message ("Wait 10s to Cool", 1);
delay (TEMP_RESET_TIME); // Waits for 10 s till the system coold down before executing the next command
reset (1);
}
}
else{
high_temp_count = 0;
}
// Display button data whenever its pressed.
if (button != -1) {
Serial.print("Button: ");
Serial.println(button);
}
if (button == 1) { //Heat Massage Button Pressed
Serial.println("Starting heat Massage");
Serial.println("Heat_LED ON");
digitalWrite(Heat_LED, HIGH); // switch ON Heat_LED
analogWrite (MOTOR, speed); //Starting Motor with current speed (If the speed was not modified by the user, start speed will be used)
my_message = heat_message;
send_message (my_message, 1);
current_mode = 1;
}
if (button == 0) { // Cold Massage Button pressed
Serial.println("Starting Cold Massage");
Serial.println("Heat_LED OFF");
digitalWrite(Heat_LED, LOW); // switch ON Heat_LED
analogWrite (MOTOR, speed); //Starting Motor with current speed (If the speed was not modified by the user, start speed will be used)
my_message = cold_message;
16
send_message (my_message, 1);
current_mode = 0;
}
if (button == 2){ // Stop button Pressed
reset (0);
my_message = default_message;
}
if (sliderId == 0) { //Speed slider Modified
// Slider sends value from 0 to 200- Default value (the mid value) is 100
if (sliderVal <MIN_SPEED){ // If slider inputs value less than Min speed, set min speed
sliderVal = MIN_SPEED;
send_message ("Setting MinSpeed", 0);
delay (1000);
send_message (my_message, 0);
}
if (sliderVal > MAX_SPEED){ // If slider inputs value greater than Max speed, set Max speed
sliderVal = MAX_SPEED;
send_message ("Setting MaxSpeed", 0);
delay (1000);
send_message (my_message, 0);
}
speed = sliderVal;
Serial.print(speed);
Serial.println(" is my new Speed");
// If motor was running already, then write this speed.
// This checkpoint is to handle situations in which user modifes the slider even before starting the massage or after stopping it
if (current_mode!=2){
analogWrite (MOTOR, speed);
}
}
if (current_mode == 1){
Serial.println(temperatureF > HotMassageTemp);
if (temperatureF-5 > HotMassageTemp){
// The temperature sensor sometime gives a high temperature when motor is running- Not sure why. So adjusting it.
// So ensuring that the there is high temperature for atleast 20 times (using high_temp_count variable) before starting the cooling down
procedure
req_temp_reached +=1;
if (req_temp_reached>10){
digitalWrite(Heat_LED, LOW); // switch OFF Heat_LED
Serial.println ("Temp Reached");
delay (500);
}
}
else{
digitalWrite(Heat_LED, HIGH); // switch OFF Heat_LED
req_temp_reached = 0;
}
}
17
String str = phone.getText();
// If a text from the phone was sent print it to the serial monitor
if (str != "") {
HotMassageTemp = str.toFloat();
send_message ("Updated", 1);
delay (1000);
send_message (my_message, 0);
}
}
void reset(int send_phone){ //Function to turn off motor and LED, Tell user that the system is ready to receive commands
digitalWrite(Heat_LED, LOW); // switch OFF Heat_LED
digitalWrite (MOTOR, LOW);
send_message (default_message, send_phone);
current_mode = 2;
}
void send_message(String msg, int send_phone){// Function to write a message to Serial Monitor, LCD and send to phone at the same time
lcd.clear ();
lcd.print(msg);
Serial.println (msg);
if (send_phone==1){
phone.sendMessage(msg);
}
}
