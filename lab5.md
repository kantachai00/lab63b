# การทดลองที่ 5 การสร้างโปรแกรม web server ผ่าน wifi

## วัตถุประสงค์
เพื่อศึกษาการเขียนโปรแกรมเชื่อมต่อ wifi และ web server

## อุปกรณ์ที่ใช้
1. ไมโครคอนโทรเลอร์
2. สาย USB
3. USB to serial
4. คอมพิวเตอร์

## ศึกษาข้อมูลเบื้องต้น
[05 run wifi](https://youtu.be/VX-QNQcO-b4 "05 run wifi")

## วิธีการทดลอง
1. เปิด command prompt
2. เข้าโปรแกรมตัวอย่างโดยใช้คำสั่ง **cd pattani**
3. เลือกโปรแกรมคำสั่ง **cd 05_Wifi-Web-Server**
4. พิมพ์คำสั่ง vi src/main.cpp เพื่อดูโค้ดโปรแกรม

```c
#include <ESP8266WiFi.h>
//#include <WiFiClient.h>
#include <ESP8266WebServer.h>
const char* ssid = "HI_BMFWIFI_2.4G";   #พิมพ์ชื่อไวไฟและรหัสผ่าน
const char* password = "0819110933";
ESP8266WebServer server(80);
int cnt = 0;
void setup(void){
	Serial.begin(115200);
	WiFi.mode(WIFI_STA);
	WiFi.begin(ssid, password);
	while (WiFi.status() != WL_CONNECTED) {
		delay(500);
		Serial.print(".");
	}
	Serial.print("\n\nIP address: ");
	Serial.println(WiFi.localIP());
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
5. ใช้คำสั่ง pio run -t upload เพื่ออัพโหลดโปรแกรมลงในไมโครคอนโทรนเลอร์
6. เชื่อมต่อไมโครคอนโทรลเลอร์กับ USB to serial
7. กดปุ่มสีดำบนไมโครคอนโทรเลอร์เพื่อให้ดาวโหลด และกดปุ่มรีเซ็ตสีแดงเพื่อรีเซ็ตคำสั่ง
8. ดูผลลัพธ์โดยใช้คำสั่ง pio device monitor จะแสดง ip address ขึ้นมา
9. ทำการ copy ip address ไปที่เว็บบราวเซอร์ในการทดสอบ

## การบันทึกผลการทดลอง 
เมื่อทำการ copy ip address ไปที่บราวเซอร์ในการทดสอบ จะมีปรากฎคำว่า Hello 1 โดย cnt จะเปลี่ยนตัวแปรไปเรื่อยๆ

## อภิปรายผลการทดลอง
จากการทดลองข้างต้นพบว่าสามารถใช้ไมโครคอนโทรลเลอร์ที่เชื่อมต่อกับ wifi และ web server ให้สามารถทำงานเสมือนเว็บทั่วไปได้

## คำถามหลังการทดลอง
ถาม เว็บบราวเซอร์ที่แตกต่างกันมีผลต่อการทดสอบโปรแกรมหรือไม่

ตอบ ไม่มีผล ใช้เว็บบราวเซอร์อะไรก็ได้ที่สามารถเข้าไปดูเว็บไซต์ทั่วไปได้
