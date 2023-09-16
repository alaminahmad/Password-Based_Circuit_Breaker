# Password-Based_Circuit_Breaker

----------

### Simulation of Password-Based Circuit Breaker using Proteus Software

This project uses a password-based system to enhance security and user-friendly access control for electrical circuits. It integrates hardware components like an Arduino Microcontroller, keypad, relay module, LED indicators, and a display module for efficient control.  
The primary aim of this project is to enhance safety, security, and operational control in the management of electrical circuits by developing a robust and user-friendly password-based circuit breaker system.

This project is driven by the following two main objectives  
• To interface Arduino microcontroller with Keypad, Liquid Crystal Display, Relay, and LED.

• To build an Arduino program to on an LED if the correct password is inputted to a keypad and to turn off a lamp is an incorrect password is inputted

----------

### **BLOCK DIAGRAM OF THE PROJECT**

The project’s block diagram shown below illustrates its architecture and key components. The Arduino microcontroller is the central controller, connecting to inputs like the power supply, switch, and keypad. The power supply provides energy, the switch controls, and the keypad allows secure password input. The LCD displays the system status, and the circuit breaker controls circuits. This diagram shows how user input interacts with the Arduino to manage electrical circuits effectively.

![](https://cdn-images-1.medium.com/max/720/1*zyN1_TfTDg64nSv0qnvSqw.png)

### **CIRCUIT DIAGRAM OF THE PROJECT WITH RESULT ACHIEVED**

The figure below shows the interfacing of the intefacing of the circuit diagram and the result when correct password is used with the keypad.

![](https://cdn-images-1.medium.com/max/720/1*K90NtrVthcmPTL8-WYCuGQ.png)

In the figure above we can see the relay is turned on and the lamp is ON.

However, when an incorrect password is used the relay will be deactivated and the lamp will be turned off as shown in the figure below.

![](https://cdn-images-1.medium.com/max/720/1*orTm46XCFcrbmZeqjZGcAg.png)

----------

#### **PROGRAMMING THE ARDUINO MICROCONTROLLER**

The flowchart below outlines the system’s operation smoothly. It starts with system preparation and moves to reading the user’s password. Then, it checks if the password is correct; if yes, it proceeds to turn the system on/off. If not, it prompts the user to try again. This loop ensures a user-friendly and secure process, encapsulating the system’s logical flow from start to finish.

![](https://cdn-images-1.medium.com/max/720/1*KAfBnTFpFw1vNX976orANQ.png)

The Arduino code in c programming language.

 // include the library code for LCD  
#include <LiquidCrystal.h> //library for LCD   
  
// initialize the library with the numbers of the interface pins  
LiquidCrystal lcd(13, 12, 11, 10, 9, 8);  
   
 // include the library code for Keypad  
#include <Keypad.h> //library for Keypad   
   
int Lock = A0;    // relay  
  
const byte numRows= 4;  //number of rows on the keypad  
const byte numCols= 4;    //number of columns on the keypad  
   
char keymap[numRows][numCols]=   
{  
{'7', '8', '9', '/'},   
{'4', '5', '6', 'X'},   
{'1', '2', '3', '-'},  
{'C', '0', '=', '+'}  
};  
   
char keypressed;                 //Where the keys are stored it changes very often  
char code[]= {'1','2','3','4'};  //The default code, you can change it or make it a 'n' digits one  
   
char check1[sizeof(code)];  //Where the new key is stored  
char check2[sizeof(code)];  //Where the new key is stored again so it's compared to the previous one  
   
short a=0,i=0,s=0,j=0;   //Variables  
   
byte rowPins[numRows] = {0,1,2,3}; //connect to the row pinouts of the keypad  
byte colPins[numCols]= {4,5,6,7}; //connect to the column pinouts of the keypad  
   
Keypad myKeypad= Keypad(makeKeymap(keymap), rowPins, colPins, numRows, numCols);  
   
void setup()  
{  
  lcd.begin(20, 4); // set up the LCD's number of columns and rows:  
  lcd.setCursor(0, 0);  
  lcd.print("  THE BRIGHT LIGHT");  
  lcd.setCursor(0, 1);  
  lcd.print("Password Bsd Breaker");  
  lcd.setCursor(0,2);  
  lcd.print("Press: ");  
  lcd.setCursor(0,3);  
  lcd.print("C To ON & X To OFF");  
  pinMode(Lock, OUTPUT);  
}  
   
void loop()  
{  
  keypressed = myKeypad.getKey();  //Constantly waiting for a key to be pressed  
  if(keypressed == 'C')  // C to open the lock  
  {  
    lcd.clear();  
    lcd.setCursor(0,0);  
    lcd.print("Enter Code");   //Message to show  
    ReadCode();               //Call the code function  
    if(a==sizeof(code))   //The ReadCode function assign a value to a (it's correct when it has the size of the code array)  
    {  
      OpenDoor();   //Open lock function  
    }  
    else  
    {  
      lcd.clear();  
      lcd.print("Wrong Password.!!"); //Message to print when the code is wrong  
      lcd.setCursor(0,1);  
      lcd.print("Press C and Try");  
      lcd.setCursor(0,2);  
      lcd.print("Again");  
    }  
  }  
  
  if(keypressed == 'X')  
  {  
    lcd.clear();  
    digitalWrite(Lock, LOW);  
    lcd.print("Breaker is OFF:");  
    lcd.setCursor(0,1);  
    lcd.print("Press C To ON The");  
    lcd.setCursor(0,2);  
    lcd.print("Circuit Breaker!!");   
  }  
}  
  
//Function to Get code  
void ReadCode()  
{  
  lcd.setCursor(0,2);  
  lcd.print("and Press '='");  
    
  //All variables set to 0  
  i=0;  
  a=0;  
  j=0;  
    
  while(keypressed != '=')  //The user press = to confirm the code otherwise he can keep typing  
  {  
    keypressed = myKeypad.getKey();                           
    if(keypressed != NO_KEY && keypressed != '=' ) //If the char typed isn't = and neither "nothing"  
    {  
      lcd.setCursor(j,1);  //This to write "*" on the lcd whenever a key is pressed it's position is controlled by j  
      lcd.print("*");  
      j++;  
        
      if(keypressed == code[i]&& i<sizeof(code)) //if the char typed is correct a and i increments to verify the next caracter  
      {  
        a++;  
        i++;  
      }  
      else  
      a--;  //if the character typed is wrong a decrements and cannot equal the size of code []  
    }  
  }  
    
  keypressed = NO_KEY;  
}  
  
 //Door Opening function  
void OpenDoor()  
{  
  lcd.clear();  
  lcd.print("Sucessfull...");  
  digitalWrite(Lock,HIGH);  
  delay(2000);  
  lcd.clear();  
  lcd.print("Breaker is ON.");  
  lcd.setCursor(0,2);  
  lcd.print("Press X To OFF ");  
  lcd.setCursor(0,3);  
  lcd.print("The Breaker");   
 } 

### SUMMARY AND CONCLUSION

In conclusion, this project successfully implements a secure and user-friendly system for controlling electrical circuits. The system elegantly guides users through the process, ensuring password validation and enabling efficient control. By prioritizing both security and ease of use, this project provides a practical solution for managing electrical circuits, enhancing overall functionality and user experience.

To get access to the complete project visit the github repository [Password-Based Circuit Breaker](http://www.github.com/alaminahmad)
