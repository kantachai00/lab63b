# การทดลองที่ 3 การเขียนโปรแกรมเอาท์พุตสัญญาณดิจิทัล

## วัตถุประสงค์
1. เพื่อศึกษาการเขียนโปรแกรมลงบนไมโครคอนโทรลเลอร์
2. เพื่อนำไมโครคอนโทรลเลอร์ไปประยุกต์ใช้กับอุปกรณ์รับสัญญาณ

## อุปกรณ์ที่ใช้
1. ไมโครคอนโทรเลอร์ (ESP-01)
2. สาย USB
3. USB to serial
4. หลอดไฟ LED
5. Adapter
6. Relay
7. ขั้วชาร์ต
8. คอมพิวเตอร์

## ศึกษาข้อมูลเบื้องต้น
[03 run example 3](https://www.youtube.com/watch?v=CCnN1WJsXQY)

## วิธีการทดลอง
1. นำ Adapter มาต่อเข้ากับ USB to serial (พอร์ต 0 สีขาว พอร์ต 1 สีเหลือง)
2. ต่อไมโครลคอนโทรลเลอร์และพอร์ทเข้าด้วยกัน
3. เปิด command prompt
4. เข้าโปรแกรมตัวอย่างโดยใช้คำสั่ง **cd pattani**
5. เลือกโปรแกรมคำสั่ง **cd 03_Output-Port** 
6. พิมพ์คำสั่ง vi src/main.cpp เพื่อดูโค้ดโปรแกรม
```c
#include <Arduino.h>
#include <ESP8266WiFi.h>

int cnt = 0;

void setup()
{
	Serial.begin(115200);
	pinMode(0, OUTPUT);
	Serial.println("\n\n\n");
}

void loop()
{
	cnt++;
	if(cnt % 2) {
		Serial.println("========== ON ===========");
		digitalWrite(0, HIGH);
	} else {
		Serial.println("========== OFF ===========");
		digitalWrite(0, LOW);
	}
	delay(500);
}
```
7. ใช้คำสั่ง pio run -t upload เพื่ออัพโหลดโปรแกรมลงในไมโครคอนโทรนเลอร์
8. กดปุ่มสีดำบนไมโครคอนโทรเลอร์เพื่อให้ดาวโหลด และกดปุ่มรีเซ็ตสีแดงเพื่อรีเซ็ตคำสั่ง

![image](https://user-images.githubusercontent.com/80879585/111983415-b1125780-8b3c-11eb-8a58-5834a2a39016.png)

9. ใช้คำสั่ง**pio device monitor** เพื่อดูผลลัพธ์

![image](https://user-images.githubusercontent.com/80879585/111983562-d901bb00-8b3c-11eb-9aff-ef4dd67dfad1.png)

10. ต่อไมโครคอนโทรลเลอร์และ Relay เข้าด้วยกัน
11. ต่อ Relay เข้ากับขั้วชาร์ต

<img width="535" alt="image" src="https://user-images.githubusercontent.com/80879585/112272232-821bf300-8cae-11eb-9503-9e2cc0c713cf.png">

## การบันทึกผลการทดลอง
จากการสังเกตผลลัพธ์จากการทดลองจะเห็นว่า ทุกๆ 500 ms หลอด LED จะติดและดับสลับกันไป

![image](https://user-images.githubusercontent.com/80879585/111984399-e8353880-8b3d-11eb-8153-150a4688e8c8.png)

## อภิปรายผลการทดลอง
จากการทดลอง เมื่อทำการอัพโหลดโปรแกรม 03_Output-Port ลงในตัวไมโครคอนโทรลเลอร์ และทำการรันคำสั่ง พบว่าผลลัพ์ที่แสดงบนหน้าจอคือ on และ off สลับกันไปทุก 500 ms และเมื่อนำไมโครคอนโทรลเลอร์ไปต่อกับหลอดไฟ LED ก็จะพบว่าฟลอดไฟมีการสว่างและดับตามผลลัพธ์ on และ off ที่แสดงบนหน้าจอ นอกจากนี้หากนำไมโครคอนโทรลเลอร์ไปต่อกับ Relay ก็จะสามารถควบคุมการเปิดปิดสวิซ์ตามผลลัพธ์ on และ off ที่แสดงบนหน้าจอได้อีกด้วย

## คำถามหลังการทดลอง
ถาม หากต้องการให้หลอด LED สีฟ้าสว่างต้องทำอย่างไร

ตอบ ต่อ LED สีฟ้าเข้ากับพอร์ตศูนย์ เนื่องจาก output ต่ออยู่ที่พอร์ตศูนย์




