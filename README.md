 

Code:-
// Include Libraries

#include "Arduino.h"

#include "Buzzer.h"

#include "Wire.h"

#include "SPI.h"

#include "Adafruit_SSD1306.h"

#include "Adafruit_GFX.h"

#include "PiezoSpeaker.h"

#include "Relay.h"

#include "Servo.h"

#include "Button.h"





// Pin Definitions

#define BUZZER_PIN_SIG	2

#define IROBJAVOID_PIN_OUT	4

#define OLED128X32_PIN_RST	5

#define PIEZOSPEAKER_5V_PIN_SIG	3

#define RELAYMODULE_PIN_SIGNAL	6

#define SERVO360MICRO_PIN_SIG	7

#define SLIDESWITCH_PIN_2	8







// Global variables and defines

unsigned int piezoSpeaker_5vHoorayLength          = 6;                                                      // amount of notes in melody

unsigned int piezoSpeaker_5vHoorayMelody[]        = {NOTE_C4, NOTE_E4, NOTE_G4, NOTE_C5, NOTE_G4, NOTE_C5}; // list of notes. List length must match HoorayLength!

unsigned int piezoSpeaker_5vHoorayNoteDurations[] = {8      , 8      , 8      , 4      , 8      , 4      }; // note durations; 4 = quarter note, 8 = eighth note, etc. List length must match HoorayLength!

// object initialization

Buzzer buzzer(BUZZER_PIN_SIG);

#define SSD1306_LCDHEIGHT 32

Adafruit_SSD1306 oLed128x32(OLED128X32_PIN_RST);

PiezoSpeaker piezoSpeaker_5v(PIEZOSPEAKER_5V_PIN_SIG);

Relay relayModule(RELAYMODULE_PIN_SIGNAL);

Servo servo360Micro;

Button slideSwitch(SLIDESWITCH_PIN_2);





// define vars for testing menu

const int timeout = 10000;       //define timeout of 10 sec

char menuOption = 0;

long time0;



// Setup the essentials for your circuit to work. It runs first every time your circuit is powered with electricity.

void setup() 

{

    // Setup Serial which is useful for debugging

    // Use the Serial Monitor to view printed messages

    Serial.begin(9600);

    while (!Serial) ; // wait for serial port to connect. Needed for native USB

    Serial.println("start");

    

    pinMode(IROBJAVOID_PIN_OUT, INPUT);

    oLed128x32.begin(SSD1306_SWITCHCAPVCC, 0x3C);  // initialize with the I2C addr 0x3C (for the 128x32)

    oLed128x32.clearDisplay(); // Clear the buffer.

    oLed128x32.display();

    slideSwitch.init();

    menuOption = menu();

    

}



// Main logic of your circuit. It defines the interaction between the components you selected. After setup, it runs over and over again, in an eternal loop.

void loop() 

{

    

    

    if(menuOption == '1') {

    // Buzzer - Test Code

    // The buzzer will turn on and off for 500ms (0.5 sec)

    buzzer.on();       // 1. turns on

    delay(500);             // 2. waits 500 milliseconds (0.5 sec). Change the value in the brackets (500) for a longer or shorter delay in milliseconds.

    buzzer.off();      // 3. turns off.

    delay(500);             // 4. waits 500 milliseconds (0.5 sec). Change the value in the brackets (500) for a longer or shorter delay in milliseconds.

    }

    else if(menuOption == '2') {

    // IR Obstacle Avoidance Sensor - Test Code

    //Read IR obstacle Sensor. irObjAvoidVar will be '1' if an Obstacle was detected

    //Use the onboard trimmer to set the distance of alert

    bool irObjAvoidVar = !digitalRead(IROBJAVOID_PIN_OUT);

    Serial.print(F("ObjAvoid: ")); Serial.println(irObjAvoidVar);



    }

    else if(menuOption == '3') {

    // Monochrome 128x32 I2C OLED graphic display - Test Code

    oLed128x32.setTextSize(1);

    oLed128x32.setTextColor(WHITE);

    oLed128x32.setCursor(0, 10);

    oLed128x32.clearDisplay();

    oLed128x32.println("Circuito.io Rocks!");

    oLed128x32.display();

    delay(1);

    oLed128x32.startscrollright(0x00, 0x0F);

    delay(2000);

    oLed128x32.stopscroll();

    delay(1000);

    oLed128x32.startscrollleft(0x00, 0x0F);

    delay(2000);

    oLed128x32.stopscroll();

    }

    else if(menuOption == '4') {

    // Piezo Speaker - Test Code

    // The Speaker will play the Hooray tune

    piezoSpeaker_5v.playMelody(piezoSpeaker_5vHoorayLength, piezoSpeaker_5vHoorayMelody, piezoSpeaker_5vHoorayNoteDurations); 

    delay(500);   

    }

    else if(menuOption == '5') {

    // Relay Module - Test Code

    // The relay will turn on and off for 500ms (0.5 sec)

    relayModule.on();       // 1. turns on

    delay(500);             // 2. waits 500 milliseconds (0.5 sec). Change the value in the brackets (500) for a longer or shorter delay in milliseconds.

    relayModule.off();      // 3. turns off.

    delay(500);             // 4. waits 500 milliseconds (0.5 sec). Change the value in the brackets (500) for a longer or shorter delay in milliseconds.

    }

    else if(menuOption == '6') {

    // Continuous Rotation Micro Servo - FS90R - Test Code

    // The servo will rotate CW in full speed, CCW in full speed, and will stop  with an interval of 2000 milliseconds (2 seconds) 

    servo360Micro.attach(SERVO360MICRO_PIN_SIG);         // 1. attach the servo to correct pin to control it.

    servo360Micro.write(180);  // 2. turns servo CW in full speed. change the value in the brackets (180) to change the speed. As these numbers move closer to 90, the servo will move slower in that direction.

    delay(2000);                              // 3. waits 2000 milliseconds (2 sec). change the value in the brackets (2000) for a longer or shorter delay in milliseconds.

    servo360Micro.write(0);    // 4. turns servo CCW in full speed. change the value in the brackets (0) to change the speed. As these numbers move closer to 90, the servo will move slower in that direction.

    delay(2000);                              // 5. waits 2000 milliseconds (2 sec). change the value in the brackets (2000) for a longer or shorter delay in milliseconds.

    servo360Micro.write(90);    // 6. sending 90 stops the servo 

    delay(2000);                              // 7. waits 2000 milliseconds (2 sec). change the value in the brackets (2000) for a longer or shorter delay in milliseconds.

    servo360Micro.detach();                    // 8. release the servo to conserve power. When detached the servo will NOT hold it's position under stress.

    }

    else if(menuOption == '7') {

    // SPDT Slide Switch (Breadboard-friendly) - Test Code

    //read Slide Switch state. 

    //if Switch is open function will return LOW (0). 

    //if it is closed function will return HIGH (1).

    bool slideSwitchVal = slideSwitch.read();

    Serial.print(F("Val: ")); Serial.println(slideSwitchVal);

    }

    

    if (millis() - time0 > timeout)

    {

        menuOption = menu();

    }

    

}







// Menu function for selecting the components to be tested

// Follow serial monitor for instrcutions

char menu()

{



    Serial.println(F("\nWhich component would you like to test?"));

    Serial.println(F("(1) Buzzer"));

    Serial.println(F("(2) IR Obstacle Avoidance Sensor"));

    Serial.println(F("(3) Monochrome 128x32 I2C OLED graphic display"));

    Serial.println(F("(4) Piezo Speaker"));

    Serial.println(F("(5) Relay Module"));

    Serial.println(F("(6) Continuous Rotation Micro Servo - FS90R"));

    Serial.println(F("(7) SPDT Slide Switch (Breadboard-friendly)"));

    Serial.println(F("(menu) send anything else or press on board reset button\n"));

    while (!Serial.available());



    // Read data from serial monitor if received

    while (Serial.available()) 

    {

        char c = Serial.read();

        if (isAlphaNumeric(c)) 

        {   

            

            if(c == '1') 

    			Serial.println(F("Now Testing Buzzer"));

    		else if(c == '2') 

    			Serial.println(F("Now Testing IR Obstacle Avoidance Sensor"));

    		else if(c == '3') 

    			Serial.println(F("Now Testing Monochrome 128x32 I2C OLED graphic display"));

    		else if(c == '4') 

    			Serial.println(F("Now Testing Piezo Speaker"));

    		else if(c == '5') 

    			Serial.println(F("Now Testing Relay Module"));

    		else if(c == '6') 

    			Serial.println(F("Now Testing Continuous Rotation Micro Servo - FS90R"));

    		else if(c == '7') 

    			Serial.println(F("Now Testing SPDT Slide Switch (Breadboard-friendly)"));

            else

            {

                Serial.println(F("illegal input!"));

                return 0;

            }

            time0 = millis();

            return c;

        }

    }

}



/*******************************************************



*    Circuito.io is an automatic generator of schematics and code for off

*    the shelf hardware combinations.



*    Copyright (C) 2016 Roboplan Technologies Ltd.



*    This program is free software: you can redistribute it and/or modify

*    it under the terms of the GNU General Public License as published by

*    the Free Software Foundation, either version 3 of the License, or

*    (at your option) any later version.



*    This program is distributed in the hope that it will be useful,

*    but WITHOUT ANY WARRANTY; without even the implied warranty of

*    MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the

*    GNU General Public License for more details.



*    You should have received a copy of the GNU General Public License

*    along with this program.  If not, see <http://www.gnu.org/licenses/>.



*    In addition, and without limitation, to the disclaimers of warranties 

*    stated above and in the GNU General Public License version 3 (or any 

*    later version), Roboplan Technologies Ltd. ("Roboplan") offers this 

*    program subject to the following warranty disclaimers and by using 

*    this program you acknowledge and agree to the following:

*    THIS PROGRAM IS PROVIDED ON AN "AS IS" AND "AS AVAILABLE" BASIS, AND 

*    WITHOUT WARRANTIES OF ANY KIND EITHER EXPRESS OR IMPLIED.  ROBOPLAN 

*    HEREBY DISCLAIMS ALL WARRANTIES, EXPRESS OR IMPLIED, INCLUDING BUT 

*    NOT LIMITED TO IMPLIED WARRANTIES OF MERCHANTABILITY, TITLE, FITNESS 

*    FOR A PARTICULAR PURPOSE, NON-INFRINGEMENT, AND THOSE ARISING BY 

*    STATUTE OR FROM A COURSE OF DEALING OR USAGE OF TRADE. 

*    YOUR RELIANCE ON, OR USE OF THIS PROGRAM IS AT YOUR SOLE RISK.

*    ROBOPLAN DOES NOT GUARANTEE THAT THE PROGRAM WILL BE FREE OF, OR NOT 

*    SUSCEPTIBLE TO, BUGS, SECURITY BREACHES, OR VIRUSES. ROBOPLAN DOES 

*    NOT WARRANT THAT YOUR USE OF THE PROGRAM, INCLUDING PURSUANT TO 

*    SCHEMATICS, INSTRUCTIONS OR RECOMMENDATIONS OF ROBOPLAN, WILL BE SAFE 

*    FOR PERSONAL USE OR FOR PRODUCTION OR COMMERCIAL USE, WILL NOT 

*    VIOLATE ANY THIRD PARTY RIGHTS, WILL PROVIDE THE INTENDED OR DESIRED

*    RESULTS, OR OPERATE AS YOU INTENDED OR AS MAY BE INDICATED BY ROBOPLAN. 

*    YOU HEREBY WAIVE, AGREE NOT TO ASSERT AGAINST, AND RELEASE ROBOPLAN, 

*    ITS LICENSORS AND AFFILIATES FROM, ANY CLAIMS IN CONNECTION WITH ANY OF 

*    THE ABOVE. 

********************************************************/
