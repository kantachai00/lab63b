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
6. กำหนดชื่อและ password ของ wifi และสร้าง IPAddress,gateway,subnet พร้อมทั้งเตรียม web server
7. ใช้คำสั่ง pio run -t upload เพื่ออัพโหลดโปรแกรมลงในไมโครคอนโทรนเลอร์
8. กดปุ่มสีดำบนไมโครคอนโทรเลอร์เพื่อให้ดาวโหลด และกดปุ่มรีเซ็ตสีแดงเพื่อรีเซ็ตคำสั่ง
9. ดูผลลัพธ์โดยใช้คำสั่ง pio device monitor

<img width="743" alt="image" src="https://user-images.githubusercontent.com/80879585/112261869-331a9180-8c9f-11eb-8a42-03d81bf797df.png">

10. ลองค้นหาและเชื่อมต่อ wifi ที่สร้างมา

<img width="239" alt="image" src="https://user-images.githubusercontent.com/80879585/112262011-7f65d180-8c9f-11eb-897e-57fbbe98dc42.png">

## การบันทึกผลการทดลอง
จาการทดลองข้างต้น เมื่อทำการรันโปรแกรมลงบนไมโครคอนโทรลเลอร์แล้วจะปรากฎคำว่า ver started โดยสามารถตรวจสอบผลลัพธ์จากการสร้าง wifi ได้โดยการใช้โทรศัพท์มือถือหรือคอมพิวเตอร์ค้นหาชื่อ wifi ที่ได้ตั้งชื่อไว้ในตอนต้น (TUENG-ESP-WIFI) 

## อภิปรายผลการทดลอง
เราสามารถใช้ไมโครคอนโทรลเลอร์ในการสร้าง wifi ขึ้นเองได้

## คำถามหลังการทดลอง
ถาม จงอธิบายความหมายของคำว่า Access Point (AP)

ตอบ Access point (AP) คือ อุปกรณ์ที่รับสัญญาณอินเตอร์เน็ตมาจากอุปกรณ์ที่เรียกว่า Router หรือ Switch หรือแม้กระทั่งรับสัญญาณมาจากอุปกรณ์กระจายสัญญาณอีกตัวหนึ่งเพื่อทำการกระจายให้สัญญาณต่างๆสามารถเชื่อมต่อแบบไร้สายได้ไกลมากขึ้น







