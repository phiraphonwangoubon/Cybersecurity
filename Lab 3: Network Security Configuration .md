*** LAB : Network Security Configuration (Cisco IOS) V.1 ***

------- Lab 1 : Create Banner -------

<img src="images/lab1_2.png" width="600">

# แลปนี้เป็นการจำลอง การรักษาความปลอดภัยบนอุปกรณ์เครือข่ายเบื้องต้น โดยเน้นไปที่:

- การแสดงข้อความเตือน (Banner MOTD)

- การเข้ารหัสรหัสผ่านใน config

- การบังคับใช้รหัสผ่านที่แข็งแรง

- การป้องกันการ brute force ด้วย lockout policy

- การเก็บบันทึกการ login ทั้งสำเร็จและล้มเหลว


เมื่อนำไปประยุกต์ใช้ในเครือข่ายจริง จะช่วยป้องกันการเข้าถึงที่ไม่ได้รับอนุญาตและเพิ่มการตรวจสอบย้อนหลังได้

1 ) การสร้าง Banner MOTD

﻿Router>en

Router#configure terminal

Router(config)#banner motd #Unauthorized access is prohibited#

### MOTD (Message of the Day) คือข้อความที่จะแสดงเมื่อมีผู้ใช้พยายามเข้าถึงอุปกรณ์ (ผ่าน Console, Telnet หรือ SSH)

ใช้สำหรับแจ้งเตือน เช่น “Unauthorized access is prohibited”

เป็นการเตือนด้านกฎหมาย/นโยบายความปลอดภัย
### 

ประโยชน์: ทำให้ผู้ใช้งานรู้ว่าการเข้าถึงต้องได้รับอนุญาตเท่านั้น และมีผลด้านกฎหมายหากมีการเจาะระบบ

1.1 ) การเข้ารหัสรหัสผ่าน (Encrypt Clear-text Password)

Router(config)#service password-encryption

- คำสั่งนี้ทำให้รหัสผ่านที่อยู่ใน running-config หรือ startup-config ไม่แสดงเป็น plain text 
Cisco จะเข้ารหัสด้วย Type 7 encryption (ไม่ได้แข็งแกร่งมาก แต่ป้องกันสายตาได้)


ประโยชน์: ป้องกันไม่ให้ผู้ดูไฟล์ config เห็นรหัสผ่านชัด

1.2 ) การกำหนดความยาวขั้นต่ำของรหัสผ่าน

Router(config)#security passwords min-length 8

- บังคับให้ผู้ใช้ต้องตั้งรหัสผ่านยาวอย่างน้อย 8 ตัวอักษร

- ลดความเสี่ยงจากการตั้งรหัสผ่านที่อ่อนแอ (เช่น 1234, cisco)

ประโยชน์: ช่วยเพิ่มความแข็งแกร่งของรหัสผ่านตามหลักการ Secure Password Policy

1.3 ) การตั้งค่า Lockout สำหรับการเข้าสู่ระบบ

Router(config)#login block-for 60 attempts 2 within 30

ความหมาย:

- หากพยายาม login ผิด 2 ครั้ง ภายใน 30 วินาที

- จะถูกล็อคการเข้าถึงเป็นเวลา 60 วินาที

- เป็นการป้องกัน Brute Force Attack


ประโยชน์: ลดโอกาสที่แฮกเกอร์จะเดารหัสผ่านสำเร็จ


1.4 ) การบันทึกกิจกรรมการ Login

Router(config)#login on-success log

Router(config)#login on-failure log every 2

- on-success log : บันทึกทุกครั้งที่ login สำเร็จ

- on-failure log every 2 : บันทึกทุกครั้งที่ login ผิด (ทุก ๆ 2 ครั้งเพื่อป้องกัน log flooding)


ประโยชน์: ทำให้ผู้ดูแลระบบสามารถติดตามกิจกรรมและตรวจจับความผิดปกติได้ง่ายขึ้น (Audit Trail)





