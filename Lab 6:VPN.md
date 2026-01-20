# Enable Site-to-Site VPN
--- Lab นี้เป็นการจำลองการตั้งค่า Site-to-Site VPN (IPsec VPN) ระหว่าง R1 ↔ R3 โดยผ่าน R2 บน Packet Tracer เพื่อให้ LAN ทั้งสองฝั่ง (192.168.1.0/24 ↔ 192.168.3.0/24) เชื่อมต่อกันอย่างปลอดภัย ---

<img width="612" height="557" alt="image" src="https://github.com/user-attachments/assets/9b86ec28-cd64-4a4b-99e0-2ddd1b57fcf8" />


***

## Configure a Site-to-Site VPN with  Cisco IOS
#### Configure ISAKMP policy parameters on R1 and R3
R1(config)#crypto isakmp policy 10
R1(config-isakmp)#authentication pre-share
R1(config-isakmp)#encryption aes 256
R1(config-isakmp)#hash sha
R1(config-isakmp)#group 5
R1(config-isakmp)#lifetime 3600
R1(config-isakmp)#end

R3(config)#crypto isakmp policy 10
R3(config-isakmp)#authentication pre-share
R3(config-isakmp)#encryption aes 256
R3(config-isakmp)#hash sha
R3(config-isakmp)#group 5
R3(config-isakmp)#lifetime 3600

***

authentication pre-share → ใช้ Pre-Shared Key (PSK)

encryption aes 256 → ใช้ AES 256-bit สำหรับการเข้ารหัส Phase 1

hash sha → ใช้ SHA สำหรับ message integrity

group 5 → ใช้ Diffie-Hellman Group 5

lifetime 3600 → อายุ SA (Security Association) 1 ชั่วโมง

***

## ตรวจสอบนโยบาย IKE ด้วยคำสั่งแสดงนโยบาย crypto isakmp

#### ตั้งค่า Pre-Shared Key
R1(config)#crypto isakmp key cisco123 address 10.2.2.2

R3(config)#crypto isakmp key cisco123 address 10.1.1.2

**ใช้ key เดียวกันทั้งสองฝั่ง (cisco123)**

**IP ที่ระบุคือ Public IP ของ peer (ใน topology คือ Serial IP ของ R2 ที่ต่อกับอีกฝั่ง)**

***

## ตั้งค่า IPsec Transform Set (Phase 2)
R1(config)#crypto ipsec transform-set 50 esp-aes 256 esp-sha-hmac

R1(cfg-crypto-trans)#exit

R3(config)#crypto ipsec transform-set 50 esp-aes 256 esp-sha-hmac

R3(cfg-crypto-trans)#exit

R1(config)#crypto ipsec security-association lifetime seconds 1800

R3(config)#crypto ipsec security-association lifetime seconds 1800

***

esp-aes 256 → ใช้ AES 256 สำหรับเข้ารหัส Data

esp-sha-hmac → ใช้ HMAC-SHA สำหรับตรวจสอบความถูกต้อง

Lifetime 1800 วินาที (30 นาที)

***

## กำหนด Interesting Traffic (ACL)

R1(config)#access-list 100 permit ip 192.168.1.0 0.0.0.255 192.168.3.0 0.0.0.255

R3(config)#access-list 100 permit ip 192.168.3.0 0.0.0.255 192.168.1.0 0.0.0.255


**ACL 100 ระบุว่า traffic ระหว่าง LAN ของ R1 ↔ R3 จะถูกเข้ารหัสผ่าน VPN**

***

## สร้างและผูก Crypto Map

R1(config)#crypto map CMAP 10 ipsec-isakmp

R1(config-crypto-map)#match address 100

R1(config-crypto-map)#set peer 10.2.2.2

R1(config-crypto-map)#set pfs group5

R1(config-crypto-map)#set transform-set 50

R1(config-crypto-map)#set security-association lifetime seconds 900

R1(config-crypto-map)#exit

***

R3(config)#crypto map CMAP 10 ipsec-isakmp

R3(config-crypto-map)#match address 100

R3(config-crypto-map)#set peer 10.1.1.2

R3(config-crypto-map)#set pfs group5

R3(config-crypto-map)#set transform-set 50

R3(config-crypto-map)#set security-association lifetime seconds 900

R3(config-crypto-map)#exit

***

match address 100 → ผูกกับ ACL 100

set peer 10.2.2.2 → ระบุ peer (IP Router R3 ฝั่งตรงข้าม)

set pfs group5 → ใช้ Perfect Forward Secrecy (DH Group 5)

set transform-set 50 → ใช้ transform set ที่สร้างไว้

Lifetime SA ของ Phase 2 = 900 วินาที

***

## ผูก Crypto Map เข้ากับ Interface WAN

R1(config)#interface S0/2/0

R1(config-if)#crypto map CMAP

R1(config)#end

R3(config)#interface S0/2/1

R3(config-if)#crypto map CMAP

R3(config)#end

นำ Crypto Map ไปผูกกับ Serial interface ที่เชื่อมต่อ Internet/WAN

VPN จะเริ่มทำงานเมื่อมี traffic ที่ตรงกับ ACL

***

ตรวจสอบ VPN

R1#show crypto ipsec transform-set

R1#show crypto map

R1#show crypto isakmp sa

show crypto ipsec transform-set → ตรวจสอบ transform set

<img width="428" height="69" alt="image" src="https://github.com/user-attachments/assets/2105ac83-150a-4ee2-834c-909b88d61ee1" />


show crypto map → ตรวจสอบ crypto map ที่ applied

<img width="714" height="198" alt="image" src="https://github.com/user-attachments/assets/6eba5209-af3e-4c96-bb24-5dda8a1b5877" />


show crypto isakmp sa → ตรวจสอบ IKE Security Associations (ควรเห็น state QM_IDLE เมื่อ VPN ทำงาน)

<img width="634" height="82" alt="image" src="https://github.com/user-attachments/assets/93005436-91ea-4fc6-9bdf-8221ae9e83bf" />




