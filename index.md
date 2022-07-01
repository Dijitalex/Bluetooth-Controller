# 231. Bluetooth Zoom/OBS Controller - All at the Click of a Button
"Oh sorry I was muted." "How do I share my screen?" Voice chatting online has been more prevalent than ever these years. Whether you're in an online class, or in a voice chat with a group of friends, it's near impossible to not have a technical issue on someone's side. Some of it may be contributed to bad network, but sometimes, it could also be due to the confusing interface. What if, hypothetically, there was a method to simplify how these controls work? What if, conceptually, it could be done with a box with buttons specifically made from voice chatting? What if, theoretically, it can be made wirelessly too? Look no further.

| **Engineer** | **School** | **Area of Interest** | **Grade** |
|:--:|:--:|:--:|:--:|
| Alex D. | Dougherty Valley High School | Software Engineering | Incoming Junior

![Final Project](https://user-images.githubusercontent.com/107717618/176519935-f29cb116-8753-47f1-bb6d-8ff35715d055.jpg)
(![parts_info](https://user-images.githubusercontent.com/107717618/176962325-a191fe89-86d6-436b-b4f7-d4595889ad60.jpg)


# Final Milestone
My final milestone consists of various smaller ones, such as wireless connections involving Bluetooth, more buttons and even a knob, and much more complex abilities, as opposed to before. Each button is connected to two wires, one to the ESP32 WROOM 32D, a bluetooth and Wifi module, which makes it superior to the Arduino Uno in this case, and the other wire is connected to a ground so that the circuit can be completed properly. The knob works similarly to the button, with additional wires due to a more complex input system from the knob. The system gets its power from a black box which is supplied with 4 AA batteries. Various issues and obstacles came up as expected, such as libraries being unidentified in the coding portion of it, as well as various hardware issues such as crating accidental short-circuits and the various issues that popped up whne the controller was connected to the computer, as it had no way of recieving input at the time.

[![Milestone 2](https://res.cloudinary.com/marcomontalbano/image/upload/v1656530226/video_to_markdown/images/youtube--W7x7V-Vu5jY-c05b58ac6eb4c4700831b2b3070cd403.jpg)](https://www.youtube.com/watch?v=W7x7V-Vu5jY "Bluetooth Zoom Controller Second Milestone"){:target="_blank" rel="noopener"}
 

# First Milestone  
My first milestone was setting up buttons to not only provide input to an Arduino, but also to perform a basic function, such as lighting up an LED bulb with the press of a button. To get this to work I had created two simple circuits that light up their respective bulbs, with two wires going into ground and two digital pins to complete the circuit. The buttons were also hooked up similarly, with one wire going into ground and the others going into different digital pins. Of course as stated in the video, alligator clips were used so that the button's plug could be compatible with that of the Arduino. From there we are now able to get an input into the Arduino when the buttons are pressed, however there was an issue of this input (which was a number) constantly changing on its own. After doing some tinkering with both the wiring and coding, it was discovered that the issue was due to the establishment of the buttons, in which they were changed from INPUT to INPUT_PULLUP.

[![MileStone 1](https://res.cloudinary.com/marcomontalbano/image/upload/v1655494836/video_to_markdown/images/youtube--eeJcswv33rA-c05b58ac6eb4c4700831b2b3070cd403.jpg)](https://www.youtube.com/watch?v=eeJcswv33rA "MileStone 1"){:target="_blank" rel="noopener"}
