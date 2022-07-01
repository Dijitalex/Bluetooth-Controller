# 231. Bluetooth Zoom/OBS Controller - All at the Click of a Button
"Oh sorry I was muted." "How do I share my screen?" Voice chatting online has been more prevalent than ever these years. Whether you're in an online class, or in a voice chat with a group of friends, it's near impossible to not have a technical issue on someone's side. Some of it may be contributed to bad network, but sometimes, it could also be due to the confusing interface. What if, hypothetically, there was a method to simplify how these controls work? What if, conceptually, it could be done with a box with buttons specifically made from voice chatting? What if, theoretically, it can be made wirelessly too? Look no further.

| **Engineer** | **School** | **Area of Interest** | **Grade** |
|:--:|:--:|:--:|:--:|
| Alex D. | Dougherty Valley High School | Software Engineering | Incoming Junior

![Final Project](https://user-images.githubusercontent.com/107717618/176519935-f29cb116-8753-47f1-bb6d-8ff35715d055.jpg)
![parts_info](https://user-images.githubusercontent.com/107717618/176962325-a191fe89-86d6-436b-b4f7-d4595889ad60.jpg)

Building instructions: https://docs.google.com/document/d/1UvIXWb9zYw7yDkDU2u_Zi0vqahhcWTIuC-dZuAwVC5g/edit?usp=sharing 
(Coding Instructions are at the bottom of the page due to lenghth).


# Final Milestone
My final milestone consists of various smaller ones, such as wireless connections involving Bluetooth, more buttons and even a knob, and much more complex abilities, as opposed to before. Each button is connected to two wires, one to the ESP32 WROOM 32D, a bluetooth and Wifi module, which makes it superior to the Arduino Uno in this case, and the other wire is connected to a ground so that the circuit can be completed properly. The knob works similarly to the button, with additional wires due to a more complex input system from the knob. The system gets its power from a black box which is supplied with 4 AA batteries. Various issues and obstacles came up as expected, such as libraries being unidentified in the coding portion of it, as well as various hardware issues such as crating accidental short-circuits and the various issues that popped up whne the controller was connected to the computer, as it had no way of recieving input at the time.

[![Milestone 2](https://res.cloudinary.com/marcomontalbano/image/upload/v1656530226/video_to_markdown/images/youtube--W7x7V-Vu5jY-c05b58ac6eb4c4700831b2b3070cd403.jpg)](https://www.youtube.com/watch?v=W7x7V-Vu5jY "Bluetooth Zoom Controller Second Milestone"){:target="_blank" rel="noopener"}
 

# First Milestone  
My first milestone was setting up buttons to not only provide input to an Arduino, but also to perform a basic function, such as lighting up an LED bulb with the press of a button. To get this to work I had created two simple circuits that light up their respective bulbs, with two wires going into ground and two digital pins to complete the circuit. The buttons were also hooked up similarly, with one wire going into ground and the others going into different digital pins. Of course as stated in the video, alligator clips were used so that the button's plug could be compatible with that of the Arduino. From there we are now able to get an input into the Arduino when the buttons are pressed, however there was an issue of this input (which was a number) constantly changing on its own. After doing some tinkering with both the wiring and coding, it was discovered that the issue was due to the establishment of the buttons, in which they were changed from INPUT to INPUT_PULLUP.

[![MileStone 1](https://res.cloudinary.com/marcomontalbano/image/upload/v1655494836/video_to_markdown/images/youtube--eeJcswv33rA-c05b58ac6eb4c4700831b2b3070cd403.jpg)](https://www.youtube.com/watch?v=eeJcswv33rA "MileStone 1"){:target="_blank" rel="noopener"}

Code (alternatively can be found here: https://github.com/T-vK/ESP32-BLE-Keyboard):
```cpp
#include <BleKeyboard.h>

// Analogue battery sense input pin:
#define BATTSENS 15
#define BATTDIVIDER 0.776
int battery_percent();

BleKeyboard bleKeyboard("ZoomBox", "Philip", battery_percent());
boolean connected;

// Define functions supported
#define ZOOMBTNS
#define VOLUME
// NB Keyboard LEDs not supported in the Bluetooth version
//#define KYBDLEDS

// Optionally use touch pads
//#define TOUCH

// Define Arduino pin numbers for buttons and LEDs

//Built-in LEd is 2
#define LED 2

#ifdef VOLUME
#define ROTARY_A 21
#define ROTARY_B 18
#define ROTARY_C 19
#endif

#ifdef KYBDLEDS
#define CAPSLOCKLED 2
#define SCRLLOCKLED 3
#define NUMLOCKLED 4
#endif

#ifdef ZOOMBTNS

// Optionally (for testing) send a printable charater instead of Zoom hotkey:
//#define TEST

#ifdef TOUCH
#define BTN1 T4
#define BTN2 T5
#define BTN3 T6
#define BTN4 T7
#define BTN5 T8
#define BTN6 T9
#define BTN7 T0
#define NUMBUTTONS 7
#else
#define BTN1 13
#define BTN2 12
#define BTN3 14
#define BTN4 27
#define BTN5 33
#define BTN6 32
#define BTN7 35
#define NUMBUTTONS 7
#endif // TOUCH
#endif // ZOOMBTNS

#ifdef ZOOMBTNS
int buttons[NUMBUTTONS] = {BTN1, BTN2, BTN3, BTN4, BTN5, BTN6, BTN7};
unsigned long btntime[NUMBUTTONS];
boolean btnpress[NUMBUTTONS];
#endif

#ifdef VOLUME
boolean A, a, B, b, C, c;
#endif

char line[80];
unsigned long t;
int n;

void setup() {
  Serial.begin(9600);
  Serial.write("Starting...\n");
  
  pinMode(LED, OUTPUT);
  digitalWrite(LED, LOW);
#ifdef VOLUME
  pinMode(ROTARY_A, INPUT_PULLUP);
  pinMode(ROTARY_B, INPUT_PULLUP);
  pinMode(ROTARY_C, INPUT_PULLUP);
#endif
#ifdef KYBDLEDS
  pinMode(CAPSLOCKLED, OUTPUT);
  pinMode(SCRLLOCKLED, OUTPUT);
  pinMode(NUMLOCKLED, OUTPUT);

  // Flash the LEDs just to show we're in business
  digitalWrite(CAPSLOCKLED, HIGH); delay(200);
  digitalWrite(SCRLLOCKLED, HIGH); delay(200);
  digitalWrite(NUMLOCKLED, HIGH); delay(200);
  digitalWrite(CAPSLOCKLED, LOW); delay(200);
  digitalWrite(SCRLLOCKLED, LOW); delay(200);
  digitalWrite(NUMLOCKLED, LOW); delay(200);
#else
//  for (int i=0; i < 3; i++) {
    digitalWrite(LED, HIGH); delay(20);
    digitalWrite(LED, LOW); delay(200);
    digitalWrite(LED, HIGH); delay(20);
    digitalWrite(LED, LOW);
//  }
#endif

#ifdef ZOOMBTNS
  for (int i = 0; i < NUMBUTTONS; i++) {
    pinMode(buttons[i], INPUT_PULLUP);
    btntime[i] = 0;
    btnpress[i] = false;
  }
#endif
  
#ifdef VOLUME
  a = b = c = false;
#endif

#ifdef KYBDLEDS
  t = 0;
  n = 0;
#endif

//#ifdef VOLUME
//  Consumer.begin();
//#endif
//  System.begin(); // For System functions
//  Gamepad.begin(); // For gamepad functions
//  Mouse.begin(); // For mouse functions
//  AbsoluteMouse.begin(); // For the Absolute Mouse

  bleKeyboard.begin();
  connected = false;
}

void loop() {
  int leds, battpercent;
  boolean battled; // Used to make LED flash on battery low

  battpercent = battery_percent();
  battled = false;

  if (battpercent == 0) battled = (millis() % 500 < 50);
  else if (battpercent <= 10) battled = battled || (millis() % 1000 < 50);

  if (!bleKeyboard.isConnected()) {
    connected = false;
    digitalWrite(LED, (!battled)? LOW: HIGH);
    return;
  }
  if (!connected) {
//    bleKeyboard.println("ZoomBtns");
    connected = true;
  }
  digitalWrite(LED, (!battled)? HIGH: LOW);

#ifdef VOLUME
  A = (digitalRead(ROTARY_A) == LOW);
  B = (digitalRead(ROTARY_B) == LOW);
  C = (digitalRead(ROTARY_C) == LOW);
  if (A && !a) {
    if (B) {
      n++; if (n > 100) n = 100;
      bleKeyboard.write(KEY_MEDIA_VOLUME_UP);
    }
    else {
      n--; if (n < 0) n = 0;
      bleKeyboard.write(KEY_MEDIA_VOLUME_DOWN);
    }
//      sprintf(line, "%d\n", n);
//      Serial.write(line);
      bleKeyboard.setBatteryLevel(n);
      delay(20);
  }
  a = A;

  if (C != c) {
    if (C) {
      digitalWrite(LED, HIGH);
      bleKeyboard.write(KEY_MEDIA_MUTE);
      delay(50);
      digitalWrite(LED, LOW);
  //      sprintf(line, "!\n");
  //      Serial.write(line);
    }
    else delay(10);
  }
  c = C;
#endif // VOLUME

#ifdef KYBDLEDS
  if (millis() > t + 10) {
    t = millis();
    leds = BootKeyboard.getLeds();
    if (leds & LED_CAPS_LOCK)
      digitalWrite(CAPSLOCKLED, HIGH);
    else
      digitalWrite(CAPSLOCKLED, LOW);
    if (leds & LED_SCROLL_LOCK)
      digitalWrite(SCRLLOCKLED, HIGH);
    else
      digitalWrite(SCRLLOCKLED, LOW);
    if (leds & LED_NUM_LOCK)
      digitalWrite(NUMLOCKLED, HIGH);
    else
      digitalWrite(NUMLOCKLED, LOW); 
  }
#endif // KYBDLEDS

#ifdef ZOOMBTNS
  for (int i = 0; i < NUMBUTTONS; i++) {
#ifdef TOUCH
    if (touchRead(buttons[i]) < 60)
      // Value read is typically 10 - 20 touched, else 80+
#else
    if (!digitalRead(buttons[i]))
      // Button pressed (negative logic)
#endif
      {
      if (btntime[i] == 0) {
        // Button has just been pressed
        btntime[i] = millis();
      }
      else {
        // Button is still pressed
        if (millis() - btntime[i] > 20 && !btnpress[i]) {
          // This is not just a glitch          
          btnpress[i] = true;
          // Now do your stuff!
          digitalWrite(LED, LOW);
          Serial.println(i);
          switch (i) {
#ifdef TEST
          case 0:
            bleKeyboard.write('a');
            break;
          case 1:
            bleKeyboard.write('b');
            break;
          case 2:
            bleKeyboard.write('c');
            break;
          case 3:
            bleKeyboard.write('d');
            break;
          case 4:
            bleKeyboard.write('e');
            break;
          case 5:
            bleKeyboard.write('f');
            break;
          case 6:
            bleKeyboard.write('g');
            break;
#else
          case 0: // Alt-F2: Gallery view
            bleKeyboard.press(KEY_LEFT_ALT);
            bleKeyboard.press(KEY_F2);
            delay(100);
            bleKeyboard.releaseAll();
            break;
          case 1: // Alt-F1: Speaker view
            bleKeyboard.press(KEY_LEFT_ALT);
            bleKeyboard.press(KEY_F1);
            delay(100);
            bleKeyboard.releaseAll();
            break;
          case 2: // Alt-M: Mute all
            bleKeyboard.press(KEY_LEFT_ALT);
            bleKeyboard.press('m');
            delay(100);
            bleKeyboard.releaseAll();
            break;
          case 3: // Alt-A: Mute self
            bleKeyboard.press(KEY_LEFT_ALT);
            bleKeyboard.press('a');
            delay(100);
            bleKeyboard.releaseAll();
            break;
          case 4: // Alt-F: Full screen
            bleKeyboard.press(KEY_LEFT_ALT);
            bleKeyboard.press('f');
            delay(100);
            bleKeyboard.releaseAll();
            break;
          case 5: // Alt-S: Share screen
            bleKeyboard.press(KEY_LEFT_ALT);
            bleKeyboard.press('s');
            delay(100);
            bleKeyboard.releaseAll();
            break;
          case 6: // Alt-V: Toggle Video
            bleKeyboard.press(KEY_LEFT_ALT);
            bleKeyboard.press('v');
            delay(100);
            bleKeyboard.releaseAll();
            break;
#endif
          }
          digitalWrite(LED, HIGH);  
        }
      }
    }
    else {
      // Button not pressed
      if (btntime[i] != 0) {
        // Looks like it's just been released
        btntime[i] = 0;
        btnpress[i] = false;
      }
    }
  }
#endif ZOOMBTNS
}

//
//Return battery level as a (very approximate) percentage.
//The analogue input pin is conected to the battery via a potential divider
//consisting of a 100k and a 27k resistor. When the battery is 4.2V (full charge)
//the analogue input pin should then be 3.3V, which will read as 4095.
//
//The nominal battery voltage is 3.7v, so we assume 3.75V is 50%, 3.65V is 10% and
//3.5V is 0%, and interpelate linearly in between.

int battery_percent() {
  int val, percent;
  float voltage;

  val = analogRead(BATTSENS);
  voltage = (val / 4095) * (3.3 / BATTDIVIDER);
  
  if (voltage > 3.75) percent = 50 + 50 * (4.2 - voltage) / (4.2 - 3.75);
  else if (voltage > 3.65) percent = 10 + 40 * (voltage - 3.5) / (3.65 - 3.5);
  else percent = 10 * (voltage - 3.5) / (3.65 - 3.5);
  if (percent > 100) return 100;
  else if (percent < 0) return 0;
  else return percent;
}
```
