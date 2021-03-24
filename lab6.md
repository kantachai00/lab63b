# การทดลองที่ 6 เรื่อง การเขียนโปรแกรมสร้าง wifi AP

## วัตถุประสงค์
เพื่อศึกษาการเขียนโปรแกรมการสร้าง wifi AP

## อุปกรณ์ที่ใช้
1. ไมโครคอนโทรเลอร์ (ESP-01)
2. สาย USB
3. USB to serial
4. หลอดไฟ LED
5. คอมพิวเตอร์

## ศึกษาข้อมูลเบื้องต้น
[06 run wifi AP](https://youtu.be/T26DVHePlTs) 

## วิธีการทดลอง
1. เชื่อมต่อไมโครคอนโทรลเลอร์และ USB to serial เข้าด้วยกัน
2. เปิด command prompt
3. เข้าโปรแกรมตัวอย่างโดยใช้คำสั่ง **cd pattani*
4. เลือกโปรแกรมคำสั่ง **cd 06_Wifi-AP-Web-Server** 
5. พิมพ์คำสั่ง vi src/main.cpp เพื่อดูโค้ดโปรแกรม
```c
#include <ESP8266WiFi.h>
//#include <WiFiClient.h>
#include <ESP8266WebServer.h>

const char* ssid = "MY-ESP8266";
const char* password = "choompol";

IPAddress local_ip(192, 168, 1, 1);
IPAddress gateway(192, 168, 1, 1);
IPAddress subnet(255, 255, 255, 0);

ESP8266WebServer server(80);

int cnt = 0;

void setup(void){
	Serial.begin(115200);

	WiFi.softAP(ssid, password);
	WiFi.softAPConfig(local_ip, gateway, subnet);
	delay(100);

	server.onNotFound([]() {
		server.send(404, "text/plain", "Path Not Found");
	});

	server.on("/", []() {
		cnt++;
		String msg = "Hello cnt: ";
		msg += cnt;
		server.send(200, "text/plain", msg);
	});

	server.begin();
	Serial.println("HTTP server started");
}

void loop(void){
  server.handleClient();
}
```
