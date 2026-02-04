Remote Code Execution ( exploit of qdPM 9.1 )

***

<img width="795" height="570" alt="539550080-c4c065df-a4ab-490b-a44a-5b50ec8041d5" src="https://github.com/user-attachments/assets/9adb4226-9316-4fe8-acd8-4f6f24708f4b" />


จาก url ของเป้าหมายเราได้พบช่องทางที่คิดว่าน่าจะเป็นช่องโหว่ที่หน้าเว็บคือ qdPM 9.1

* qdPM 9.1 คือ ซอฟต์แวร์บริหารจัดการโครงการ (Project Management Software) แบบ โอเพนซอร์ส ที่พัฒนาด้วย PHP และใช้ฐานข้อมูล MySQL/MariaDB โดยเวอร์ชัน 9.1 เป็นหนึ่งในรุ่นของ qdPM *

*** 

## ใช้ exploit-DB ค้นหาช่องโหว่ qdPM 9.1 ว่ามี exploit อะไรบ้าง 

หลังจากที่ได้ค้นหาก็ได้พบว่ามีช่องโหว่ Remote Code Execution และดาวน์โหลดไฟล์ exploit เพื่อเข้าถึงเครื่องของเป้าหมาย

<img width="1498" height="529" alt="image" src="https://github.com/user-attachments/assets/13489b64-7e0b-4218-b94c-587e7a09f2ba" />

และ Download ไฟล์ Exploit

<img width="1141" height="611" alt="image" src="https://github.com/user-attachments/assets/8acfa587-080c-4149-b2cf-73b4c8eba7fc" />

***

## จากนั้นผมใช้ python เพื่อทำงานไฟล์ exploit เพื่อเข้าถึงเครื่องเป้าหมาย

python3 50944.py -url http://[target_ip]/ -u user@localhost.com -p password

<img width="1564" height="135" alt="2" src="https://github.com/user-attachments/assets/137ed4bf-702e-48b7-86f6-d01cf04dfc7c" />


หลังจากที่ได้ url 

<img width="1568" height="322" alt="image" src="https://github.com/user-attachments/assets/61eb2694-d4e5-46b4-9721-288c02628dfe" />

- ให้เราไปตาม URL และเราจะพบว่าที่ url มีการเรียกใช้คำสั่ง whoami ที่จะแสดงถึงชื่อผู้ใช้งานที่กำลังเข้าสู่ระบบปัจจุบัน

จากพื้นฐานของ Linux File System

<img width="1280" height="720" alt="image" src="https://github.com/user-attachments/assets/8ed8aec6-908c-41ab-9139-7e0fc9ff503a" />

ผมจึงใช้งานคำสั่งที่ด้านหลังของ url ใช้คำสั่ง cat+-/etc/passwd เพื่อดูเนื้อหาในไฟล์ passwd ได้

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/5dc2eff0-8c81-4a12-991d-05ac5c1e8777" />

และยังสามารถใช้คำสั่งอื่นๆเพื่อค้นหาข้อมูลต่างๆที่ต้องการได้อีกมากมาย


