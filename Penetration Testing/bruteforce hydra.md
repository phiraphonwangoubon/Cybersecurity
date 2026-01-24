# Bruteforce hydra.

ขั้นตอนที่ 1 : nmap เพื่อค้นหาบริการและพอร์ตบนเครื่องเป้าหมาย

<img width="1561" height="613" alt="image" src="https://github.com/user-attachments/assets/b25dc0f4-f601-481a-9d38-ea6f4da1434a" />

***

ขั้นตอนที่ 2 : ใช้ gobuster เพื่อค้นหาเส้นทางหรือไดเร็กทอรีที่ซ่อนอยู่

Command : gobuster dir -u http://172.31.73.26// -w /usr/share/wordlists/dirb/common.txt 

คำอธิบายเพิ่มเติม

 -w = ใช้เพื่อระบุเส้นทางไปยังไฟล์ wordlist ที่ Gobuster จะใช้สำหรับไดเร็กทอรีแบบ brute-force

 -u = ใช้เพื่อระบุ URL เป้าหมายหรือ URL พื้นฐานที่ Gobuster จะใช้ในการค้นหาไดเรคทอรี่ 

 -/usr/share/wordlists/dirb/common.txt = wordlists
 
<img width="1886" height="1112" alt="image" src="https://github.com/user-attachments/assets/3896c564-b351-4654-ba79-2de5442e8a8c" />

<img width="787" height="202" alt="image" src="https://github.com/user-attachments/assets/2a4d16f3-2e03-4327-b71c-30719516b496" />

<img width="593" height="278" alt="image" src="https://github.com/user-attachments/assets/f6839b99-e359-4159-84cc-47f77fe6dd03" />

***

ขั้นตอนที่ 3 : ใช้ cewl เพื่อสร้างรายการคำศัพท์ของรหัสผ่าน จากนั้นจึงคัดลอกไปสร้างเป็นไฟล์ .txt

Command : cewl http://172.31.73.26/

<img width="1206" height="684" alt="image" src="https://github.com/user-attachments/assets/a29f35d6-97a7-4b8c-998f-9529626aaec1" />


คำอธิบายเพิ่มเติม

 -cewl = รวบรวมคำจากเว็บ target มาสร้างเป็น wordlist เพื่อใช้เดารหัสผ่าน

3.1) ใส่ wordlist ทีได้มาในไฟล์ memory.txt

<img width="523" height="85" alt="image" src="https://github.com/user-attachments/assets/c6daef85-e874-41bb-b0b1-06dd01cb90c2" />

***

ขั้นตอนที่ 4 : ใช้ไฮดราเพื่อค้นหารหัสผ่านสำหรับผู้ใช้คำสั่ง black ต่อไปนี้

Command : hydra –l black –P /usr/bin/memory.txt 172.31.73.26 ssh 

คำอธิบายเพิ่มเติม

 -l: ใช้เพื่อระบุชื่อผู้ใช้หรือชื่อเข้าสู่ระบบที่ Hydra จะใช้ในระหว่างการโจมตีแบบ brute-forcing 

 -P ใช้เพื่อระบุเส้นทางไปยังไฟล์รหัสผ่านที่ Hydra จะใช้สำหรับการโจมตีแบบ brute-forcing 
 
 <img width="1488" height="619" alt="image" src="https://github.com/user-attachments/assets/0d75e00b-293c-4469-9624-e1bdb344d039" />

***

ขั้นตอนที่ 5 : หลังจากที่ได้ password ของเครื่องเป้าหมาย เราจะ ssh เพื่อเข้าถึงเครื่องเป้าหมาย 

Command : ssh black@172.31.73.26

<img width="516" height="64" alt="image" src="https://github.com/user-attachments/assets/c870ab64-b8b6-427b-a4ee-a753f7a4b666" />

<img width="564" height="238" alt="image" src="https://github.com/user-attachments/assets/c4d94ae6-306a-4e60-abf0-c9a4bf420792" />





