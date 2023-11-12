https://youtube.com/shorts/R35_6O43qO0
```
// *** MAIN SETTINGS ***
// Replace this block with correct template settings.
// You can find it for every template here:
//
//   https://blynk.cloud/dashboard/templates

#define BLYNK_TEMPLATE_ID "TMPL6CeS0AbBG"
#define BLYNK_TEMPLATE_NAME "LED ESP32"

#define BLYNK_FIRMWARE_VERSION        "0.1.0"

#define BLYNK_PRINT Serial
//#define BLYNK_DEBUG

#define APP_DEBUG

#include "BlynkEdgent.h"
#include "DHT.h"

BlynkTimer timer; 

#define DHTTYPE DHT11 
#define DHTPIN 23
#define ldrPin 35

#define LED_PIN 2  // Use pin 2 for LED (change it, if your board uses another pin)
DHT dht(DHTPIN, DHTTYPE);
const int potPin = 34;
int potValue = 0;
// V0 is a datastream used to transfer and store LED switch state.
// Evey time you use the LED switch in the app, this function
// will listen and update the state on device
BLYNK_WRITE(V0)
{
  // Local variable value stores the incoming LED switch state (1 or 0)
  // Based on this value, the physical LED on the board will be on or off:
  int value = param.asInt();

  if (value == 1) {
    digitalWrite(LED_PIN, HIGH);
    Serial.print("value =");
    Serial.println(value);
  } else {
    digitalWrite(LED_PIN, LOW);
    Serial.print("value = ");
    Serial.println(value);
  }
}

void reandAndSendSensorsData() 
{
// Reading temperature or humidity takes about 250 milliseconds!
  // Sensor readings may also be up to 2 seconds 'old' (its a very slow sensor)
  float h = dht.readHumidity();
  // Read temperature as Celsius (the default)
  float t = dht.readTemperature();
  // Read temperature as Fahrenheit (isFahrenheit = true)
  float f = dht.readTemperature(true);

  // Check if any reads failed and exit early (to try again).
  if (isnan(h) || isnan(t) || isnan(f)) {
    Serial.println(F("Failed to read from DHT sensor!"));
    return;
  }

  potValue = analogRead(potPin);
  Serial.print("Sending data from sensors"); 
  Serial.println(potValue);
  Blynk.virtualWrite(V1, potValue); // Send potValue to virtual pin V2 in Blynk
    Blynk.virtualWrite(V3, t); 
    Blynk.virtualWrite(V4, h); 
      int ldrValue = analogRead(ldrPin);
Blynk.virtualWrite(V2,ldrValue ); 
}


void setup()
{
  pinMode(LED_PIN, OUTPUT);
  Serial.begin(115200);
  delay(100);
  BlynkEdgent.begin();
  timer.setInterval(2000L, reandAndSendSensorsData);
  Serial.println("Init Timer ");
   dht.begin();
}

void loop() {
  BlynkEdgent.run();
  timer.run();
  
}
```
![image](https://github.com/TikPoramat2545/Introduction-to-blynk/assets/134470274/84f19e11-27c4-47a0-9012-afe4e86710b3)

