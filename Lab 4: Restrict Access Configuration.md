# Lab 2 : Restrict Access

### แลปนี้เป็นการจำลอง การรักษาความปลอดภัยบนอุปกรณ์เครือข่ายเบื้องต้น โดยเน้นไปที่:

- สร้าง local user เพื่อแทนที่ password เดียวของ VTY/Console

- จำกัดการเข้าถึง Console, VTY, AUX ด้วย timeout, login local และ transport control

- บังคับใช้ SSH แทน Telnet เพื่อความปลอดภัยในการ remote

- เสริมด้วย key encryption และ policy timeout/retries


ทั้งหมดนี้เป็นแนวปฏิบัติด้าน Secure Router Access Hardening ที่ควรใช้ในเครือข่ายจริงเพื่อป้องกันการเจาะระบบและการเข้าถึงที่ไม่ได้รับอนุญาต

2.1 ) การสร้าง User และ Privilege Level

Router# config t

Router(config)#username My_User secret P@ssw0rd

Router(config)#username My_User privilege 15

*** 
- username : กำหนดชื่อผู้ใช้ (My_User)

- secret : ตั้งรหัสผ่าน (เข้ารหัสแบบ MD5 ไม่ใช่ plain text)

- privilege 15 : ให้สิทธิ์สูงสุด (full admin)


ประโยชน์: ใช้ระบบ login แบบ local authentication แทนการพึ่งพา password เดียวใน VTY/Console



2.2 ) Restrict Console Line (con 0)

Router(config)#line con 0

Router(config-line)#exec-timeout 5 0

Router(config-line)#login local

***
- exec-timeout 5 0 : ถ้าไม่มีการใช้งานเป็นเวลา 5 นาที จะถูก logout อัตโนมัติ

- login local : ใช้ username/password ที่สร้างไว้ (local user)


ประโยชน์: ป้องกันการลืม logout ที่ console



2.3 ) Restrict VTY Line (Telnet/SSH)

Router(config)#line vty 0 4

Router(config-line)#password P@ssw0rd

Router(config-line)#exec-timeout 5 0

Router(config-line)#login local

Router(config-line)#transport output none

***

- line vty 0 4 : ครอบคลุม session ระยะไกลสูงสุด 5 ช่อง (0–4)

- exec-timeout : ตั้งเวลาหมดอายุ session

- login local : ใช้ username/password แทนการใช้ password เดียว

- transport output none : ไม่อนุญาตให้ router initiate ออกไปเอง (ลดการใช้ vty ออก)


ประโยชน์: ควบคุม remote access เฉพาะผู้ใช้ที่ได้รับอนุญาต




2.4 ) Restrict AUX Port

Router(config)#line aux 0

Router(config-line)#password P@ssw0rd

Router(config-line)#exec-timeout 5 0

Router(config-line)#transport output none

Router(config-line)#no login

***

- line aux 0 : กำหนดการเข้าถึงผ่าน AUX port (ใช้กับ modem ในการ dial-in)

- no login : ปิดการใช้งาน login ผ่าน AUX

- transport output none : ไม่ให้ initiate ออก

ประโยชน์: ป้องกันการเข้าถึง router ผ่าน AUX port ที่มักไม่ถูกใช้งานในเครือข่ายปัจจุบัน



2.5 ) How to Set SSH Authentication

Router(config)#hostname name

Router(config)#ip domain-name name-server

Router(config)#crypto key generate rsa general-keys modulus 1024

Router(config)#ip ssh time-out 90

Router(config)#ip ssh authentication-retries 2

***

- hostname + ip domain-name : จำเป็นต้องตั้งก่อนสร้าง RSA key
- crypto key generate rsa : สร้าง RSA key 1024-bit สำหรับ SSH
- ip ssh time-out 90 : ถ้าไม่มีการใส่รหัสภายใน 90 วินาที → ตัดการเชื่อมต่อ
- ip ssh authentication-retries 2 : ใส่รหัสผ่านผิดได้สูงสุด 2 ครั้ง

2.6 ) กำหนดให้ VTY ใช้ SSH เท่านั้น

Router(config)#line vty 0 4

Router(config-line)#exec-timeout 5 0

Router(config-line)#login local

Router(config-line)#transport input ssh

Router(config)#line vty 5 15

Router(config-line)#transport input none

Router(config-line)#no login

Router(config-line)#exit

***

- line vty 0 4 → อนุญาตเฉพาะ SSH

- line vty 5 15 → ปิดการใช้งาน (ไม่มี remote access เกิน 5 session)

ประโยชน์:

- ป้องกันการใช้ Telnet (ที่ส่งรหัสผ่านเป็น plain text)

- จำกัดจำนวน remote sessions

ใช้ SSH ซึ่งมีการเข้ารหัส ปลอดภัยกว่า
