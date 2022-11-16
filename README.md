#include <Wire.h>
#include <ESP8266WiFi.h>
#include <WiFiClientSecure.h>
#include <UniversalTelegramBot.h>


const char* ssid = "nama hospot/wifi";
const char* password = "password";


#define BOTtoken "token bot"
#define CHAT_ID "id bot"

#define Sensor D0


X509List cert(TELEGRAM_CERTIFICATE_ROOT);
WiFiClientSecure client;
UniversalTelegramBot bot(BOTtoken, client);


void setup() {
  Serial.begin(115200);
 
  Wire.begin(D2, D1);
  configTime(0, 0, "pool.ntp.org");      // get UTC time via NTP
  client.setTrustAnchors(&cert); // Add root certificate for api.telegram.org

  pinMode(Sensor, INPUT);
  

  WiFi.mode(WIFI_STA);
  WiFi.begin(ssid, password);

  int a = 0;
  while (WiFi.status() != WL_CONNECTED) {
    Serial.print(".");
    delay(50);
    a++;
  }

  Serial.println("");
  Serial.println("WiFi connected");
  Serial.print("IP address: ");
  Serial.println(WiFi.localIP());
  delay(300);

  bot.sendMessage(CHAT_ID, "System memulai", "");
 
  
}
void loop() {
  bool value = digitalRead(Sensor);
  Serial.println(value);
  if (value == 1) {
    Serial.println("asap terdeteksi");
   
    bot.sendMessage(CHAT_ID, "ada api!!", "");
  } else if (value == 0) {
  
    bot.sendMessage(CHAT_ID, "aMan!!", "");
  }
}
