# My First Workspace

In this exercise you will create a work environment with platform.io for esp-32.

1) Download driver for esp32 [here](https://docs.espressif.com/projects/esp-idf/en/latest/esp32/get-started/establish-serial-connection.html)

2) Download [vscode](https://code.visualstudio.com/)

3) (Optional) create your profile for embedded system

4) Download two main extension

   - Espressif IDF
   - PlatformIO IDE

5) Enter in the platformIO home menu by typing ctr + Maj + P > PlatformIO: PlatformIO home

6) In the Left you will see :heavy_plus_sign: New Project click on it

7) Create a project with name "Test My Env", board: ESP32 Dev Module,  Framworks: Arduino

8) when the project is loaded,copy the folowwing code inside src > main.cpp and  upload it by clicking the :arrow_right: in the top right corner. 

   ```c++
   #include <Arduino.h>
   
   
   void setup() {
     Serial.begin(9600);
   }
   
   void loop() {
     Serial.write("test");
     delay(1000);
   }
   
   ```

9) If You receive an error "A fatal error occurred: Failed to connect to ESP32: Wrong boot mode detected (0x13)! The chip needs to be in download mode." please see [this](A fatal error occurred: Failed to connect to ESP32: Wrong boot mode detected (0x13)! The chip needs to be in download mode.) **Solution is press boot button on the esp32 board when run the code, its simple**

10) press ctr + Maj + P > PlatformIO: PlatformIO serial monitor and show that test is displayed every 1S



DONE !