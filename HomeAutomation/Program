/******************************************************************************** ESP32 Home Automation by Salman
   Control appliances through IR-Remote, Touch-Switch, Alexa, Google Home.
   Use preferences library to store appliances state in case of power cut.
                                  
    http://arduino.esp8266.com/stable/package_esp8266com_index.json,https://raw.githubusercontent.com/espressif/arduino-esp32/gh-pages/package_esp32_index.json
      ESP32: https://github.com/espressif/arduino-esp32

     AceButton Library: https://github.com/bxparks/AceButton
     IRremote Library: https://github.com/Arduino-IRremote/Arduino-IRremote
     DHT Library: https://github.com/adafruit/DHT-sensor-library
     *  SimpleTimer Library: https://github.com/kiryanenko/SimpleTimer
 **********************************************************************************/
//this is for fan indicator


unsigned long previousMillis = 0;  // Store the last time the LED was toggled

const unsigned long blinkInterval = 500;  // Interval between blinks (500 ms)
int currentBlink = 0;              // Track the current blink
bool ledState = LOW;               // State of the LED
bool blinking = false;             // Flag to control blinking
int targetBlinks = 0; 
#define ledPin 17
//this is for fan indicator


#include "RMaker.h"
#include "WiFi.h"
#include "WiFiProv.h"
#include <IRremote.h>
#include <DHT.h> 
#include <Preferences.h>
#include <SimpleTimer.h>
#include <AceButton.h>
using namespace ace_button;

Preferences pref;

const char *service_name = "PROV_Salman2024";
const char *pop = "1234";

// define the Chip Id
uint32_t espChipId = 0;

// define the Node Name
char nodeName[] = "HomeAutomation";

// define the Device Names
char deviceName_1[] = "RoomLight";
char deviceName_2[] = "FootLight";
char deviceName_3[] = "ReadingLight";
char deviceName_4[] = "NightLamp";
char deviceName_5[] = "Fan";

//Hex code of ir remote

#define FAN_STATE        0xCF8976  //MODE button
#define IR_RoomLight     0xCFD12E  //1
#define IR_FootLight     0xCF51AE //2
#define IR_ReadingLight  0xCF09F6  //3
#define NightLamp        0xCFC936  //4
#define IR_Fan_Up        0xCF718E  //+
#define IR_Fan_Down      0xCF6996  //-

              //Thid is for 2nd remote(SITI Digital)
#define IR_All_Off       0xD911A05F  //Red
#define IR_FAN1          0xD911708F   //FanSpeed1
#define IR_FAN2          0xD91148B7  //FanSpeed2
#define IR_FAN3          0xD91150AF  //FanSpeed3
#define IR_FAN4          0xD91158A7   //FanSpeEd4
#define IR_FAN           0xD911D02F
#define IR_1             0xD911D827
#define IR_2             0xD911F00F
#define IR_3             0xD911C03F
#define IR_4             0xD9119867





// define the GPIO connected with Relays and switches
static uint8_t Relay1Pin = 23;  //D23
static uint8_t Relay2Pin = 22;  //D22
static uint8_t Relay3Pin = 21;  //D21
static uint8_t Relay4Pin = 19;  //D19
static uint8_t Relay5Pin = 15;  //Fan1
static uint8_t Relay6Pin = 5;  //fan2
static uint8_t Relay7Pin = 18;   //fan3

static uint8_t Switch1Pin = 13;  //D13
static uint8_t Switch2Pin = 12;  //D12
static uint8_t Switch3Pin = 14;  //D14
static uint8_t Switch4Pin = 27;  //D27
static uint8_t Switch5Pin = 26;  //D33
static uint8_t Switch6Pin = 25;  //D32

static uint8_t wifiLed      = 2;  // D2
static uint8_t gpio_reset   = 0;  // Press BOOT for reset WiFi
static uint8_t IR_RECV_PIN  = 35; // D35 (IR receiver pin)
static uint8_t DHTPIN       = 4; // RX2  pin connected with DHT
static uint8_t LDR_PIN      = 33; // D34  pin connected with LDR


/* Variable for reading pin status*/
bool toggleState_1 = LOW; //Define integer to remember the toggle state for relay 1
bool toggleState_2 = LOW; //Define integer to remember the toggle state for relay 2
bool toggleState_3 = LOW; //Define integer to remember the toggle state for relay 3
bool toggleState_4 = LOW; //Define integer to remember the toggle state for relay 4
bool toggleState_5 = LOW; //Define integer to remember the toggle state for relay 5
/*bool toggleState_6 = LOW; //Define integer to remember the toggle state for relay 6
bool toggleState_7 = LOW; //Define integer to remember the toggle state for relay 7
bool toggleState_8 = LOW;
bool toggleState_9 = LOW; //Define integer to remember the toggle state for relay 8
bool autoMode;*/

int currSpeed = 4;
bool fanSpeed_0 = LOW;
bool fanSpeed_1 = LOW; 
bool fanSpeed_2 = LOW; 
bool fanSpeed_3 = LOW; 
bool fanSpeed_4 = LOW; 

float temperature1 = 0;
float humidity1   = 0;
float ldrVal  = 0;

DHT dht(DHTPIN, DHT11);

IRrecv irrecv(IR_RECV_PIN);
decode_results results;

SimpleTimer Timer;

ButtonConfig config1;
AceButton button1(&config1);
ButtonConfig config2;
AceButton button2(&config2);
ButtonConfig config3;
AceButton button3(&config3);
ButtonConfig config4;
AceButton button4(&config4);
ButtonConfig config5;
AceButton button5(&config5);
ButtonConfig config6;
AceButton button6(&config6);

void handleEvent1(AceButton*, uint8_t, uint8_t);
void handleEvent2(AceButton*, uint8_t, uint8_t);
void handleEvent3(AceButton*, uint8_t, uint8_t);
void handleEvent4(AceButton*, uint8_t, uint8_t);
void handleEvent5(AceButton*, uint8_t, uint8_t);
void handleEvent6(AceButton*, uint8_t, uint8_t);

//The framework provides some standard device types like switch, lightbulb, fan, temperature sensor.
static LightBulb my_switch1(deviceName_1, &Relay1PinPin);
static LightBulb my_switch2(deviceName_2, &Relay2Pin);
static LightBulb my_switch3(deviceName_3, &Relay3Pin);
static LightBulb my_switch4(deviceName_4, &Relay4Pin);
static Fan my_fan(deviceName_5);
static TemperatureSensor temperature("Temperature");
static TemperatureSensor humidity("Humidity");
static TemperatureSensor ldr("LDR");

void sysProvEvent(arduino_event_t *sys_event)
{
    switch (sys_event->event_id) {      
        case ARDUINO_EVENT_PROV_START:
#if CONFIG_IDF_TARGET_ESP32
        Serial.printf("\nProvisioning Started with name \"%s\" and PoP \"%s\" on BLE\n", service_name, pop);
        printQR(service_name, pop, "ble");
#else
        Serial.printf("\nProvisioning Started with name \"%s\" and PoP \"%s\" on SoftAP\n", service_name, pop);
        printQR(service_name, pop, "softap");
#endif        
        break;
        case ARDUINO_EVENT_WIFI_STA_CONNECTED:
        Serial.printf("\nConnected to Wi-Fi!\n");
        digitalWrite(wifiLed, true);
        break;
    }
}

void write_callback(Device *device, Param *param, const param_val_t val, void *priv_data, write_ctx_t *ctx)
{
    const char *device_name = device->getDeviceName();
    const char *param_name = param->getParamName();

    if(strcmp(device_name, deviceName_1) == 0) {
      
      Serial.printf("Lightbulb = %s\n", val.val.b? "true" : "false");
      
      if(strcmp(param_name, "Power") == 0) {
          Serial.printf("Received value = %s for %s - %s\n", val.val.b? "true" : "false", device_name, param_name);
        toggleState_1 = val.val.b;
        (toggleState_1 == false) ? digitalWrite(Relay1Pin, LOW) : digitalWrite(Relay1Pin, HIGH);
        param->updateAndReport(val);
        pref.putBool("Relay1", toggleState_1);
        delay(50);
      }
      
    } else if(strcmp(device_name, deviceName_2) == 0) {
      
      Serial.printf("Switch value = %s\n", val.val.b? "true" : "false");

      if(strcmp(param_name, "Power") == 0) {
        Serial.printf("Received value = %s for %s - %s\n", val.val.b? "true" : "false", device_name, param_name);
        toggleState_2 = val.val.b;
        (toggleState_2 == false) ? digitalWrite(Relay2Pin, LOW) : digitalWrite(Relay2Pin, HIGH);
        param->updateAndReport(val);
        pref.putBool("Relay2", toggleState_2);
        delay(50);
      }
  
    } else if(strcmp(device_name, deviceName_3) == 0) {
      
      Serial.printf("Switch value = %s\n", val.val.b? "true" : "false");

      if(strcmp(param_name, "Power") == 0) {
        Serial.printf("Received value = %s for %s - %s\n", val.val.b? "true" : "false", device_name, param_name);
        toggleState_3 = val.val.b;
        (toggleState_3 == false) ? digitalWrite(Relay3Pin, LOW) : digitalWrite(Relay3Pin, HIGH);
        param->updateAndReport(val);
        pref.putBool("Relay3", toggleState_3);
        delay(50);
      }
  
    } else if(strcmp(device_name, deviceName_4) == 0) {
      
      Serial.printf("Switch value = %s\n", val.val.b? "true" : "false");

      if(strcmp(param_name, "Power") == 0) {
        Serial.printf("Received value = %s for %s - %s\n", val.val.b? "true" : "false", device_name, param_name);
        toggleState_4 = val.val.b;
        (toggleState_4 == false) ? digitalWrite(Relay4Pin, LOW) : digitalWrite(Relay4Pin, HIGH);
        param->updateAndReport(val);
        pref.putBool("Relay4", toggleState_4);
        delay(50);
      } 
       
    }
    else if(strcmp(device_name, deviceName_5) == 0) {
    if (strcmp(param_name, "Power") == 0)
    {
      Serial.printf("Received Fan power = %s for %s - %s\n", val.val.b ? "true" : "false", device_name, param_name);
      toggleState_5 = val.val.b;
      (toggleState_5 == false) ? fanSpeedControl(0) : fanSpeedControl(currSpeed);
      param->updateAndReport(val);
      pref.putBool("Fan_Power", toggleState_5);
    }  
    if (strcmp(param_name, "My_Speed") == 0)
    {
      Serial.printf("\nReceived value = %d for %s - %s\n", val.val.i, device_name, param_name);
      currSpeed = val.val.i;
      if(toggleState_5 == 1){
        fanSpeedControl(currSpeed);
      }
      param->updateAndReport(val);
      pref.putInt("Fan_Speed", currSpeed);
    }
 }
}
void readSensor(){

  ldrVal = map(analogRead(LDR_PIN), 0, 4095, 0, 100);
  //Serial.print("LDR - "); Serial.println(ldrVal);
  float h = dht.readHumidity();
  float t = dht.readTemperature(); // or dht.readTemperature(true) for Fahrenheit
  
  if (isnan(h) || isnan(t)) {
    Serial.println("Failed to read from DHT sensor!");
    return;
  }
  else {
    humidity1 = h;
    temperature1 = t;
    Serial.print("Temperature - "); Serial.println(t);
    Serial.print("Humidity - "); Serial.println(h);
  }  
}

void sendSensor()
{
  readSensor();
  temperature.updateAndReportParam("Temperature", temperature1);
  humidity.updateAndReportParam("Temperature", humidity1);
  ldr.updateAndReportParam("Temperature", ldrVal);
}

void ir_remote(){
  if (irrecv.decode(&results)) {
      switch(results.value){
          case IR_RoomLight:  
          case 0x88A03C9:
          case 0x88A0789:
          case 0x88A0B49:
          case 0x88A0F09:
          case 0x88A12C9:
          case 0x88A1689:
          case 0x88A1A49:
          case 0x88A000A:

            digitalWrite(Relay1Pin, !toggleState_1);
            toggleState_1 = !toggleState_1;
            my_switch1.updateAndReportParam(ESP_RMAKER_DEF_POWER_NAME, toggleState_1);
            pref.putBool("Relay1", toggleState_1);
            delay(100);            
            break;

          case IR_FootLight:  
          case 0x880E749:
          case 0x8809700:
          case 0x8808743:

            digitalWrite(Relay2Pin, !toggleState_2);
            toggleState_2 = !toggleState_2;
            my_switch2.updateAndReportParam(ESP_RMAKER_DEF_POWER_NAME, toggleState_2);
            pref.putBool("Relay2", toggleState_2);
            delay(100);            
            break;

          case IR_ReadingLight:
          case 0x88903C8: 
          case 0x8890788: 
          case 0x8890B48:
          case 0x8890F08: 
          case 0x88912C8: 
          case 0x8891688: 
          case 0x8891A48: 
          case 0x8891E08: 
          case 0x88921C8: 
          case 0x8892588:
          case 0x8892948:
          case 0x8892D08:
          case 0x8890009:

            digitalWrite(Relay3Pin, !toggleState_3);
            toggleState_3 = !toggleState_3;
            my_switch3.updateAndReportParam(ESP_RMAKER_DEF_POWER_NAME, toggleState_3);
            pref.putBool("Relay3", toggleState_3);
            delay(100);            
            break;

          case NightLamp: 
          case 0x8813004:

            digitalWrite(Relay4Pin, !toggleState_4);
            toggleState_4 = !toggleState_4;
            my_switch4.updateAndReportParam(ESP_RMAKER_DEF_POWER_NAME, toggleState_4);
            pref.putBool("Relay4", toggleState_4);
            delay(100);            
            break;

          case  FAN_STATE:  
          case 0x880A723: 
          case 0x880A745: 
          case 0x880A701:

            if(toggleState_5 == 0){
              fanSpeedControl(currSpeed); //Turn ON Fan
            }
            else {
              
              fanSpeedControl(0); //Turn OFF Fan
            }
            toggleState_5 = !toggleState_5;
            pref.putBool("Fan_Power", toggleState_5);
            my_fan.updateAndReportParam(ESP_RMAKER_DEF_POWER_NAME, toggleState_5);
            
            delay(100);            
            break;
            
          case IR_Fan_Up: 
            if(currSpeed < 4 && toggleState_5 == 1){
              currSpeed = currSpeed + 1;
              fanSpeedControl(currSpeed);
              pref.putInt("Fan_Speed", currSpeed);
              my_fan.updateAndReportParam("My_Speed", currSpeed);
              delay(100); 
            }      
            break;
          case IR_Fan_Down: 
            if(currSpeed > 1 && toggleState_5 == 1){
              currSpeed = currSpeed - 1;
              fanSpeedControl(currSpeed);
              pref.putInt("Fan_Speed", currSpeed);
              my_fan.updateAndReportParam("My_Speed", currSpeed);
              delay(100); 
            }
            break;
          case IR_All_Off:
            all_SwitchOff();  
            break;

                //2nd remote

        case IR_1:  
            digitalWrite(Relay1Pin, !toggleState_1);
            toggleState_1 = !toggleState_1;
            my_switch1.updateAndReportParam(ESP_RMAKER_DEF_POWER_NAME, toggleState_1);
            pref.putBool("Relay1", toggleState_1);
            delay(100);            
            break;
          case IR_2:  
            digitalWrite(Relay2Pin, !toggleState_2);
            toggleState_2 = !toggleState_2;
            my_switch2.updateAndReportParam(ESP_RMAKER_DEF_POWER_NAME, toggleState_2);
            pref.putBool("Relay2", toggleState_2);
            delay(100);            
            break;
          case IR_3:  
            digitalWrite(Relay3Pin, !toggleState_3);
            toggleState_3 = !toggleState_3;
            my_switch3.updateAndReportParam(ESP_RMAKER_DEF_POWER_NAME, toggleState_3);
            pref.putBool("Relay3", toggleState_3);
            delay(100);            
            break;
          case IR_4:  
            digitalWrite(Relay4Pin, !toggleState_4);
            toggleState_4 = !toggleState_4;
            my_switch4.updateAndReportParam(ESP_RMAKER_DEF_POWER_NAME, toggleState_4);
            pref.putBool("Relay4", toggleState_4);
            delay(100);            
            break;
          case  IR_FAN:   
            if(toggleState_5 == 0){
              fanSpeedControl(currSpeed); //Turn ON Fan
            }
            else {
              
              fanSpeedControl(0); //Turn OFF Fan
            }
            toggleState_5 = !toggleState_5;
            pref.putBool("Fan_Power", toggleState_5);
            my_fan.updateAndReportParam(ESP_RMAKER_DEF_POWER_NAME, toggleState_5);
            
            delay(100);            
            break;

            case IR_FAN1: 
            if(toggleState_5 == 1){
              currSpeed = 1;
              fanSpeedControl(currSpeed);
              pref.putInt("Fan_Speed", currSpeed);
              my_fan.updateAndReportParam("My_Speed", currSpeed);
              delay(100); 
            } break;

            case IR_FAN2: 
            if(toggleState_5 == 1){
              currSpeed = 2;
              fanSpeedControl(currSpeed);
              pref.putInt("Fan_Speed", currSpeed);
              my_fan.updateAndReportParam("My_Speed", currSpeed);
              delay(100); 
            } break;

            case IR_FAN3: 
            if(toggleState_5 == 1){
              currSpeed = 3;
              fanSpeedControl(currSpeed);
              pref.putInt("Fan_Speed", currSpeed);
              my_fan.updateAndReportParam("My_Speed", currSpeed);
              delay(100); 
            } break;

            case IR_FAN4: 
            if(toggleState_5 == 1){
              currSpeed = 4;
              fanSpeedControl(currSpeed);
              pref.putInt("Fan_Speed", currSpeed);
              my_fan.updateAndReportParam("My_Speed", currSpeed);
              delay(100); 
            } break;

            




            
          default : break;         
        }   
        Serial.println(results.value, HEX);    
        irrecv.resume();   
  } 
}

void all_SwitchOff(){
  if(toggleState_1){
  toggleState_1 = 0; digitalWrite(Relay1Pin, toggleState_1); 
  my_switch1.updateAndReportParam(ESP_RMAKER_DEF_POWER_NAME, toggleState_1); 
  pref.putBool("Relay1", toggleState_1);
  delay(200);}
  if(toggleState_2){
  toggleState_2 = 0; digitalWrite(Relay2Pin, toggleState_2); my_switch2.updateAndReportParam(ESP_RMAKER_DEF_POWER_NAME, toggleState_2);
  pref.putBool("Relay2", toggleState_2);
  delay(200);}
  if(toggleState_3){
  toggleState_3 = 0; digitalWrite(Relay3Pin, toggleState_3); my_switch3.updateAndReportParam(ESP_RMAKER_DEF_POWER_NAME, toggleState_3);
  pref.putBool("Relay3", toggleState_3);
   delay(200);}
  if(toggleState_4){
  toggleState_4 = 0; digitalWrite(Relay4Pin, toggleState_4); my_switch4.updateAndReportParam(ESP_RMAKER_DEF_POWER_NAME, toggleState_4);
  pref.putBool("Relay4", toggleState_4);
   delay(200);}
  if(toggleState_5){
  toggleState_5 = 0;
  fanSpeedControl(0);
  pref.putBool("Fan_Power", toggleState_5);
  delay(200);}
  
}

void getRelayState(){
  toggleState_1 = pref.getBool("Relay1", 0);
  digitalWrite(Relay1Pin, toggleState_1); 
  my_switch1.updateAndReportParam(ESP_RMAKER_DEF_POWER_NAME, toggleState_1);
  delay(250);

  toggleState_2 = pref.getBool("Relay2", 0);
  digitalWrite(Relay2Pin, toggleState_2); 
  my_switch2.updateAndReportParam(ESP_RMAKER_DEF_POWER_NAME, toggleState_2);
  delay(250);

  toggleState_3 = pref.getBool("Relay3", 0);
  digitalWrite(Relay3Pin, toggleState_3); 
  my_switch3.updateAndReportParam(ESP_RMAKER_DEF_POWER_NAME, toggleState_3);
  delay(250);

  toggleState_4 = pref.getBool("Relay4", 0);
  digitalWrite(Relay4Pin, toggleState_4); 
  my_switch4.updateAndReportParam(ESP_RMAKER_DEF_POWER_NAME, toggleState_4);
  delay(250);

  currSpeed = pref.getInt("Fan_Speed", 4);
  my_fan.updateAndReportParam("My_Speed", currSpeed);
  delay(250);
  
  toggleState_5 = pref.getBool("Fan_Power", 0);
  if(toggleState_5 == 1 && currSpeed > 0){
    fanSpeedControl(currSpeed); 
  }
  my_fan.updateAndReportParam(ESP_RMAKER_DEF_POWER_NAME, toggleState_5);
  delay(200);  
}

void setup()
{
    //uint32_t espChipId = espespChipId;
    
    Serial.begin(115200);
    pref.begin("Relay_State", false);
    
    // Set the Relays GPIOs as output mode
    pinMode(Relay1Pin, OUTPUT);
    pinMode(Relay2Pin, OUTPUT);
    pinMode(Relay3Pin, OUTPUT);
    pinMode(Relay4Pin, OUTPUT);
    pinMode(Relay5Pin, OUTPUT);
    pinMode(Relay6Pin, OUTPUT);
    pinMode(Relay7Pin, OUTPUT);
    pinMode(ledPin, OUTPUT);
    
    pinMode(wifiLed, OUTPUT);
     
    // Configure the input GPIOs
    pinMode(Switch1Pin, INPUT);
    pinMode(Switch2Pin, INPUT);
    pinMode(Switch3Pin, INPUT);
    pinMode(Switch4Pin, INPUT);
    pinMode(Switch5Pin, INPUT);
    pinMode(Switch6Pin, INPUT);
    pinMode(gpio_reset, INPUT);
    
    // Write to the GPIOs the default state on booting
    digitalWrite(Relay1Pin, toggleState_1);
    digitalWrite(Relay2Pin, toggleState_2);
    digitalWrite(Relay3Pin, toggleState_3);
    digitalWrite(Relay4Pin, toggleState_4);
    digitalWrite(Relay5Pin, LOW);
    digitalWrite(Relay6Pin, LOW);
    digitalWrite(Relay7Pin, LOW);

    digitalWrite(wifiLed, LOW);

    irrecv.enableIRIn(); // Enabling IR sensor
    dht.begin();    // Enabling DHT sensor

    config1.setEventHandler(button1Handler);
    config2.setEventHandler(button2Handler);
    config3.setEventHandler(button3Handler);
    config4.setEventHandler(button4Handler);
    config5.setEventHandler(button5Handler);
    config6.setEventHandler(button6Handler);
       
    button1.init(Switch1Pin);
    button2.init(Switch2Pin);
    button3.init(Switch3Pin);
    button4.init(Switch4Pin);
    button5.init(Switch5Pin);
    button6.init(Switch6Pin);

    Node my_node;    
    my_node = RMaker.initNode(nodeName);

    //Standard switch device
    my_switch1.addCb(write_callback);
    my_switch2.addCb(write_callback);
    my_switch3.addCb(write_callback);
    my_switch4.addCb(write_callback);
    my_fan.addCb(write_callback);

    //Define Param Fan Speed
  Param speed("My_Speed",ESP_RMAKER_PARAM_RANGE , value(0), PROP_FLAG_READ | PROP_FLAG_WRITE);
  speed.addBounds(value(1), value(4), value(1));
  speed.addUIType(ESP_RMAKER_UI_SLIDER);
  my_fan.addParam(speed);

    //Add switch device to the node   
    my_node.addDevice(my_switch1);
    my_node.addDevice(my_switch2);
    my_node.addDevice(my_switch3);
    my_node.addDevice(my_switch4);
    my_node.addDevice(my_fan);
    my_node.addDevice(temperature);
    my_node.addDevice(humidity);
    my_node.addDevice(ldr);

    Timer.setInterval(60000); 

    delay(500);

    //This is optional 
    RMaker.enableOTA(OTA_USING_PARAMS);
    //If you want to enable scheduling, set time zone for your region using setTimeZone(). 
    //The list of available values are provided here https://rainmaker.espressif.com/docs/time-service.html
    // RMaker.setTimeZone("Asia/Shanghai");
    // Alternatively, enable the Timezone service and let the phone apps set the appropriate timezone
    RMaker.enableTZService();
    RMaker.enableSchedule();

    //Service Name
    for(int i=0; i<17; i=i+8) {
      espChipId |= ((ESP.getEfuseMac() >> (40 - i)) & 0xff) << i;
    }

    Serial.printf("\nChip ID:  %d Service Name: %s\n", espChipId, service_name);

    Serial.printf("\nStarting ESP-RainMaker\n");
    RMaker.start();

    WiFi.onEvent(sysProvEvent);

   
#if CONFIG_IDF_TARGET_ESP32
    WiFiProv.beginProvision(WIFI_PROV_SCHEME_BLE, WIFI_PROV_SCHEME_HANDLER_FREE_BTDM, WIFI_PROV_SECURITY_1, pop, service_name);
#else
    WiFiProv.beginProvision(WIFI_PROV_SCHEME_SOFTAP, WIFI_PROV_SCHEME_HANDLER_NONE, WIFI_PROV_SECURITY_1, pop, service_name);
#endif
   delay(200);

   getRelayState();
}

void loop()
{
    // Read GPIO0 (external button to reset device
    if(digitalRead(gpio_reset) == LOW) { //Push button pressed
        Serial.printf("Reset Button Pressed!\n");
        // Key debounce handling
        delay(100);
        int startTime = millis();
        while(digitalRead(gpio_reset) == LOW) delay(50);
        int endTime = millis();

        if ((endTime - startTime) > 10000) {
          // If key pressed for more than 10secs, reset all
          Serial.printf("Reset to factory.\n");
          RMakerFactoryReset(2);
        } else if ((endTime - startTime) > 3000) {
          Serial.printf("Reset Wi-Fi.\n");
          // If key pressed for more than 3secs, but less than 10, reset Wi-Fi
          RMakerWiFiReset(2);
        }
    }
    delay(100);

    if (WiFi.status() != WL_CONNECTED)
    {
      //Serial.println("WiFi Not Connected");
      digitalWrite(wifiLed, false);
    }
    else
    {
      //Serial.println("WiFi Connected");
      digitalWrite(wifiLed, true);
      if (Timer.isReady()) { 
        //Serial.println("Sending Sensor Data");
        sendSensor();
        Timer.reset();      // Reset a second timer
      }
    }

    ir_remote(); //IR remote Control
    
    button1.check();
    button2.check();
    button3.check();
    button4.check();
    button5.check();
    button6.check();
    delay(200);


    

   //This is for led blinking for fan indicator
   if (blinking) {
    unsigned long currentMillis = millis();

    if (currentMillis - previousMillis >= blinkInterval / 2) {  // Half of the blink interval
      previousMillis = currentMillis;

      // Toggle LED state
      ledState = !ledState;
      digitalWrite(ledPin, ledState);

      // Increment blink count when LED turns off
      if (ledState == LOW) {
        currentBlink++;
      }

      // Stop blinking when the desired number of blinks is reached
      if (currentBlink >= targetBlinks) {
        blinking = false;
        currentBlink = 0;            // Reset for the next round of blinking
        digitalWrite(ledPin, LOW);    // Ensure the LED is off at the end
      }
    }
  }
}


void fanSpeedControl(int fanSpeed){
   switch(fanSpeed){
    case 0:
      startBlinking(0);
      digitalWrite(Relay5Pin, HIGH);
      delay(100);
      digitalWrite(Relay7Pin, LOW);
      delay(200);
      digitalWrite(Relay6Pin, LOW);
      delay(200);
      digitalWrite(Relay5Pin, LOW);
 

      
    break;
    case 1:
    startBlinking(1);
      digitalWrite(Relay5Pin, HIGH);
      delay(200);
      digitalWrite(Relay6Pin, LOW);
      delay(200);
      digitalWrite(Relay7Pin, LOW); 
      break;

    case 2:
      startBlinking(2);
      digitalWrite(Relay6Pin, HIGH);
      delay(200);
      digitalWrite(Relay5Pin, LOW);
      digitalWrite(Relay7Pin, LOW);

    break;
    case 3:
    startBlinking(3);
      digitalWrite(Relay6Pin, HIGH);
      delay(200);
      digitalWrite(Relay5Pin, HIGH);
      digitalWrite(Relay7Pin, LOW);
      
     break;
    case 4:
    startBlinking(4);
      digitalWrite(Relay7Pin, HIGH);
      delay(200);
      digitalWrite(Relay5Pin, LOW);
      digitalWrite(Relay6Pin, LOW);
      
    break;          
    default : break;         
   } 


}

void button1Handler(AceButton* button, uint8_t eventType, uint8_t buttonState) {
  Serial.println("EVENT1");
  switch (eventType) {
    case AceButton::kEventPressed:
      digitalWrite(Relay1Pin, !toggleState_1);
      toggleState_1 = !toggleState_1;
      my_switch1.updateAndReportParam(ESP_RMAKER_DEF_POWER_NAME, toggleState_1);
      pref.putBool("Relay1", toggleState_1);
      break;
  }
}
void button2Handler(AceButton* button, uint8_t eventType, uint8_t buttonState) {
  Serial.println("EVENT2");
  switch (eventType) {
    case AceButton::kEventPressed:
      digitalWrite(Relay2Pin, !toggleState_2);
      toggleState_2 = !toggleState_2;
      my_switch2.updateAndReportParam(ESP_RMAKER_DEF_POWER_NAME, toggleState_2);
      pref.putBool("Relay2", toggleState_2);
      break;
    
  }
}
void button3Handler(AceButton* button, uint8_t eventType, uint8_t buttonState) {
  Serial.println("EVENT3");
  switch (eventType) {
    case AceButton::kEventPressed:
      digitalWrite(Relay3Pin, !toggleState_3);
      toggleState_3 = !toggleState_3;
      my_switch3.updateAndReportParam(ESP_RMAKER_DEF_POWER_NAME, toggleState_3);
      pref.putBool("Relay3", toggleState_3);
      break;
  }
}
void button4Handler(AceButton* button, uint8_t eventType, uint8_t buttonState) {
  Serial.println("EVENT4");
  switch (eventType) {
    case AceButton::kEventPressed:
      Serial.println("kEventPressed");
      toggleState_5 = !toggleState_5;
      (toggleState_5 == false) ? fanSpeedControl(0) : fanSpeedControl(currSpeed);
      pref.putBool("Fan_Power", toggleState_5);
      my_fan.updateAndReportParam("My_Speed", currSpeed);
      my_fan.updateAndReportParam(ESP_RMAKER_DEF_POWER_NAME, toggleState_5);
      break;
  }
}
void button5Handler(AceButton* button, uint8_t eventType, uint8_t buttonState) {
  Serial.println("EVENT5");
  switch (eventType) {
    case AceButton::kEventPressed:
      if(currSpeed > 1 && toggleState_5 == 1){
              currSpeed = currSpeed - 1;
              fanSpeedControl(currSpeed);
              pref.putInt("Fan_Speed", currSpeed);
              my_fan.updateAndReportParam("My_Speed", currSpeed);
              delay(100); 
            }
  }
}
void button6Handler(AceButton* button, uint8_t eventType, uint8_t buttonState) {
  Serial.println("EVENT6");
  switch (eventType) {
    case AceButton::kEventPressed:                                              
      if(currSpeed < 4 && toggleState_5 == 1){
              currSpeed = currSpeed + 1;
              fanSpeedControl(currSpeed);
              pref.putInt("Fan_Speed", currSpeed);
              my_fan.updateAndReportParam("My_Speed", currSpeed);
              delay(100); 
            }
  }
}


void startBlinking(int blinkCount) {
  blinking = true;
  targetBlinks = blinkCount;  // Set the target number of blinks
  currentBlink = 0;           // Reset the current blink count
  previousMillis = millis();  // Reset the millis for timing
  ledState = LOW;             // Ensure LED starts from off state
  digitalWrite(ledPin, ledState);
}


