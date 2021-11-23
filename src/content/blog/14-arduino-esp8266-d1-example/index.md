---
id: 14
urlTitle: "arduino-esp8266-d1-example"
title: "Arduino ESP8266 D1 WiFi HTTPClient 範例程式"
cid: 2
publishDate: "2019-01-13 00:00:00"
updateDate: "2019-01-13 00:00:00"
cover: "./images/1.jpg"
tags: ["Arduino", "ESP8266", "D1", "WiFi", "HTTPClien", "範例程式"]
published: true
description: "自己筆記記錄一下… 連上 WiFi 後，每隔 10 秒會進行 HTTP POST 動作"
---

自己筆記記錄一下…

連上 WiFi 後，每隔 10 秒會進行 HTTP POST 動作

```java{numberLines: true}
#include <ArduinoJson.h>
#include <ESP8266HTTPClient.h>
#include <ESP8266WiFi.h>
const char* ssid = "wifiname";
const char* password = "password";
String jsonStr = "";
HTTPClient http;
void httpSend(String url) {
jsonStr = "{\"type\": \"fall\" ,\"latitude\": 25.118210,\"longitude\": 121.521243 }";
Serial.println(jsonStr);

http.begin(url);
http.addHeader("Content-Type", "application/json");
http.POST(jsonStr);
String json = http.getString();
StaticJsonBuffer<80> jsonBuffer;
JsonObject& root = jsonBuffer.parse(json);

if (!root.success()) {
Serial.println("parseObject() failed");
return;
}
 const char\* status = root["result"];
Serial.println(status);
http.end();
}

void setup() {
Serial.begin(9600);
WiFi.begin(ssid, password);

while (WiFi.status() != WL_CONNECTED) {
delay(500);
Serial.print(".");
}
Serial.println("WiFi connected");
}

void loop()
{
 if (WiFi.status() == WL_CONNECTED) {
httpSend("http://ip/post.php");
}
 delay(10000);
}

```
