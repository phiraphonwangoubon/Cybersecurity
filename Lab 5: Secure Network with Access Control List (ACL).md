# Secure Network with Access Control List (ACL)
แลปนี้เป็นการจำลอง การรักษาความปลอดภัยบนอุปกรณ์เครือข่ายเบื้องต้น เราจะใช้ ACL (Access Control List) เพื่อเพิ่มความปลอดภัยให้กับอุปกรณ์เครือข่าย โดยครอบคลุมการควบคุมการเข้าถึง VTY (Telnet/SSH), การจำกัดการเข้าถึง HTTP Server และการป้องกันการโจมตี Smurf Attack

- Secure VTY Access → อนุญาตเฉพาะ IP ที่กำหนดให้เข้า Telnet/SSH

- Secure HTTP Management → จำกัด subnet ที่เข้าถึง Web Management ของ Router

- Anti Smurf Attack → บล็อก ICMP traffic ที่พยายามโจมตีผ่าน broadcast

<img src="images/aclpng.png" width="600">

1 ) จำกัด IP ที่สามารถเข้า VTY (Telnet/SSH)

Router(config)#access-list 105 permit tcp host 192.168.x.x any eq 23 log

Router(config)#access-list 105 permit tcp host 192.168.x.x any eq 22 log

Router(config)#access-list 105 deny ip any any log

***
## TIPS

Router(config)#line vty 0 4

Router(config)#access-class 105 in

permit tcp host 192.168.x.x any eq 23 → อนุญาตเฉพาะ IP ที่กำหนดให้เข้า Telnet (port 23)

permit tcp host 192.168.x.x any eq 22 → อนุญาตเฉพาะ IP ที่กำหนดให้เข้า SSH (port 22)

deny ip any any log → ปฏิเสธการเข้าถึงจาก IP อื่นทั้งหมด

access-class 105 in → ผูก ACL เข้ากับ line vty

ประโยชน์: ป้องกันไม่ให้ใครจาก IP อื่นเข้ามา telnet/ssh ยกเว้น IP ที่อนุญาต

***

2 ) จำกัดการเข้าถึง HTTP Server

Router(config)#ip http auth local

Router(config)#access-list 50 permit 192.168.x.0 0.0.0.255 log

Router(config)#access-list 50 deny any log

Router(config)#ip http access-class 50

***
## TIPS

ip http auth local → ใช้ local username/password สำหรับการเข้าถึง web management

access-list 50 permit 192.168.x.0 0.0.0.255 → อนุญาตเฉพาะ subnet 192.168.x.0/24

access-list 50 deny any → ปฏิเสธ subnet อื่น

ip http access-class 50 → ผูก ACL เข้ากับ HTTP server

ประโยชน์: จำกัดสิทธิ์การเข้าถึง web management ของ Router ให้อยู่แค่ใน subnet ที่อนุญาต

***

3 ) Prevent Smurf Attack

Router(config)#access-list 150 deny ip any host 192.168.x.255 log

Router(config)#access-list 150 deny ip any host 192.168.x.0 log

Router(config)#access-list 150 permit ip 192.168.x.0 0.0.0.255 log

Router(config)#interface f0/1

Router(config-if)#ip access-group 150 in

Router(config-if)#exit

***
## TIPS

deny ip any host 192.168.x.255 → ปฏิเสธการส่ง ICMP ไปยัง Broadcast Address (.255)

deny ip any host 192.168.x.0 → ปฏิเสธการส่ง ICMP ไปยัง Network Address (.0)

permit ip 192.168.x.0 0.0.0.255 → อนุญาตให้ subnet ใช้งานได้ตามปกติ

ip access-group 150 in → ใช้ ACL กับ inbound traffic ของ interface f0/1

ประโยชน์: ป้องกันการโจมตีแบบ Smurf Attack ที่ใช้การส่ง ICMP broadcast เพื่อทำ DDoS

