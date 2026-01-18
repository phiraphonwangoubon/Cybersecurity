-- LAB : Configuring ASA Basic Settings and Firewall Using CLI --

<img src="Screenshot 2026-01-18 112147.png" width="600">

1 ) ตั้งค่า Router R1
  
  Router>en
  
  Router#conf t
   
  Router(config)#hostname R1
  
  R1(config)#

1.1) ตั้งค่า password enable mode
  
  R1(config)#enable secret enpass

1.2) ตั้งค่า console และ ssh
  
  R1(config)#line con 0
  
  R1(config-line)#password P@ssw0rd
  
  R1(config-line)#login
  
  R1(config-line)#exit

1.3) ตั้งค่าการ Remote เข้ามาอุปกรณ์ (Telnet/SSH) จำนวน 5 ช่องพร้อมกัน
  
  R1(config)#line vty 0 4
  
  R1(config-line)#password telnetpass
  
  R1(config-line)#login
 
  R1(config-line)#exit
  
  R1(config)#

1.4 ) ตั้งค่า g 0/0
  
  R1(config)#int g0/0
  
  R1(config-if)#ip address 209.165.200.225 255.255.255.248
  
  R1(config-if)#no shut

1.5 ) ตั้งค่า R1 ให้เชื่อมต่อกับ R2 และ s0/0/0
  
  R1(config)#int s0/0/0
  
  R1(config-if)#ip address 10.1.1.1 255.255.255.252
  
  R1(config-if)#clock rate 64000
 
  R1(config-if)#no shut

*** ตั้งค่า default  route ***
  
  R1(config)#ip route 0.0.0.0 0.0.0.0 Serail0/0/0

 

 2. ตั้งค่า Router R2
  
  Router>en
  
  Router#conf t
  
  Router(config)#hostname R2
  
  R2(config)#

2.1 ) ตั้งค่า Password ให้ R2
  
  R2(config)#enable secret enpass
2.2 ) ตั้งค่า console และ ssh
  
  R2(config)#line con 0
  
  R2(config-line)#password P@ssw0rd
  
  R2(config-line)#login
  
  R2(config-line)#exit

2.3 ) ตั้งค่าการ Remote เข้ามาอุปกรณ์ (Telnet/SSH) จำนวน 5 ช่องพร้อมกัน
  
  R2(config)#line vty 0 4
  
  R2(config-line)#password sshpass
 
  R2(config-line)#login
  
  R2(config-line)#exit
  
  R2(config)#

2.4 ) ตั้งค่า R2 ให้เชื่อมต่อกับ R1 และ s0/0/0
  
  R2(config)#int s0/0/0
  
  R2(config-if)#ip address 10.1.1.2 255.255.255.252
  
  R2(config-if)#no shut

2.5 ) ตั้งค่า R2 ให้เชื่อมต่อกับ R3 และ s0/0/1
  
  R2(config)#int s0/0/1
  
  R2(config-if)#ip address 10.2.2.2 255.255.255.252
  
  R2(config-if)#clock rate 64000
 
  R2(config-if)#no shut

2.6 ) ตั้งค่า static route
  
  R2(config)#ip route 172.16.3.0 255.255.255.0 Serial0/0/1
  
  R2(config)#ip route 209.165.200.224 255.255.255.248 Serial0/0/0

 

3. ) ตั้งค่า Router R3

  Router>en
  
  Router#conf t
  
  Router(config)#hostname R3
  
  R3(config)#

3.1 ) ตั้งค่า Password ให้ R3
  
  R3(config)#enable secret enpass

3.2 ) ตั้งค่า console และ ssh
  
  R3(config)#line con 0
  
  R3(config-line)#password P@ssw0rd
  
  R3(config-line)#login
  
  R3(config-line)#exit

3.3 ) ตั้งค่าการ Remote เข้ามาอุปกรณ์ (Telnet/SSH) จำนวน 5 ช่องพร้อมกัน
  
  R3(config)#line vty 0 4
  
  R3(config-line)#password telnetpass
  
  R3(config-line)#login
  
  R3(config-line)#exit
  
  R3(config)#

3.4 )ตั้งค่า g0/1
  
  R3(config)#int g0/1
  
  R3(config-if)#ip address 172.16.3.1 255.255.255.0
  
  R3(config-if)#no shut

3.5) ตั้งค่า R3 ให้เชื่อมต่อกับ R2 และ s0/0/1
  
  R3(config)#int s0/0/1
  
  R3(config-if)#ip address 10.2.2.1 255.255.255.252
  
  R3(config-if)#no shut

*** ตั้งค่า default  route ***
  
  R3(config)#ip route 0.0.0.0 0.0.0.0 Serial0/0/1

  
4. ) การกำหนดค่า ASA และความปลอดภัยของ Interface ผ่าน CLI

4.1 ) กำหนดค่า hostname และ domain name โดยใช้คำสั่ง

  ciscoasa>en

  ciscoasa#conf t
  
  ciscoasa(config)#hostname CCNAS-ASA

  CCNAS-ASA(config)#domain-name cybersecurity.com

4.2 ) ตั้งค่ารหัสผ่านสำหรับ Enable Mode

  CCNAS-ASA(config)#enable password enpass

4.3 ) ตั้งค่า Interface ภายใน (inside) และภายนอก (outside) กำหนดค่า Interface VLAN 1 แบบ logical สำหรับเครือข่าย inside (192.168.1.0/24) ให้ตั้งค่า security level (ระดับความปลอดภัย) ให้เป็นค่าสูงสุดที่ 100 โดยใช้คำสั่ง

  CCNAS-ASA(config)# interface vlan 1

  CCNAS-ASA(config-if)# nameif inside

  CCNAS-ASA(config-if)# ip address 192.168.1.1 255.255.255.0

  CCNAS-ASA(config-if)# security-level 100

  CCNAS-ASA(config-if)# exit

  CCNAS-ASA(config)#

4.4 ) สร้าง Interface VLAN 2 แบบ logical สำหรับเครือข่าย outside (209.165.200.224/29) ให้ตั้งค่า security level (ระดับความปลอดภัย) ให้เป็นค่าต่ำสุดที่ 0 และ enable (เปิดใช้งาน) Interface VLAN 2 โดยใช้คำสั่ง

  CCNAS-ASA(config)#interface vlan 2

  CCNAS-ASA(config-if)#nameif outside

  CCNAS-ASA(config-if)#ip address 209.165.200.226 255.255.255.248

  CCNAS-ASA(config-if)#security-level 0
  
  CCNAS-ASA(config-if)#exit
  
  CCNAS-ASA(config)#

  CCNAS-ASA#sh switch vlan

<img src="images/lab_C12D013C-2F9F-48FA-9C01-FC304E1A5B4B6.png" width="400">







