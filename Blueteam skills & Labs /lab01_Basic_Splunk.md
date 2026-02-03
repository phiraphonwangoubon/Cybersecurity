# Basic Splunk

***

1 ) ติดตั้ง Splunk App / Splunk Add-on (TA) เข้าไปใน Splunk Enterprise (Splunk Search)

<img width="932" height="391" alt="image" src="https://github.com/user-attachments/assets/e6fb7de7-7621-4174-a945-68e6c373b3c4" />

1.1 ) เลือกไฟล์และทำการ upload

<img width="1058" height="521" alt="image" src="https://github.com/user-attachments/assets/4e5a5cba-9048-4a24-a4e7-72bfd453271c" />

    👉 สิ่งที่กำลังติดตั้งคืออะไร

    เป็นการติดตั้ง Splunk Add-on สำหรับอุปกรณ์เครือข่าย เช่น

    - Cisco

    - pfSense

    โดยเฉพาะในภาพล่างจะเห็นไฟล์
    👉 ta-pfsense_210.tgz ซึ่งคือ Splunk Add-on for pfSense (TA-pfSense)

    Add-on (TA) พวกนี้มีหน้าที่:

    - แปลง log ให้อยู่ใน format ที่ Splunk เข้าใจ

    - ทำ field extraction (src_ip, dest_ip, action ฯลฯ)

    - เตรียมข้อมูลให้เอาไปค้นหา / ทำ Dashboard ต่อได้

***

2 ) หลังจากนั้นทำการ Add data / log ที่เรารวบรวมมาได้

<img width="1919" height="965" alt="image" src="https://github.com/user-attachments/assets/bf16f3ad-5d4c-42d1-a4d7-7c9acbb64f58" />

<img width="900" height="484" alt="image" src="https://github.com/user-attachments/assets/406db8da-f489-441a-8da7-6d0026086577" />

<img width="879" height="520" alt="image" src="https://github.com/user-attachments/assets/dcc179be-aa33-4ed0-9ac3-9da1b3131431" />

<img width="1096" height="389" alt="image" src="https://github.com/user-attachments/assets/52cdffdd-08c9-45de-8d90-c372a1144108" />

***

3 ) การค้นหาใน Splunk

<img width="1384" height="662" alt="image" src="https://github.com/user-attachments/assets/1b58d65b-3c48-437d-89b9-dff0c6c2d837" />

<img width="486" height="762" alt="image" src="https://github.com/user-attachments/assets/c5a6f6c2-b574-4e41-bfd5-962c18275b26" />

4 ) ทำการ Test Search & Specifying time range
 
 <img width="1919" height="969" alt="image" src="https://github.com/user-attachments/assets/613e1da1-232c-49f0-b9ce-da622aaaa90f" />

 <img width="1919" height="967" alt="image" src="https://github.com/user-attachments/assets/ee46718b-1a20-4092-917f-b6b2a6f4fcbd" />

5 ) ทำความเข้าใจเกี่ยวกับในแต่ละ fields 

<img width="1257" height="676" alt="image" src="https://github.com/user-attachments/assets/02464622-0a14-4fd5-9c1a-0aa899ded74c" />

<img width="1253" height="435" alt="image" src="https://github.com/user-attachments/assets/c731780b-dd18-4712-b236-cde4f6c63d82" />

<img width="1043" height="482" alt="image" src="https://github.com/user-attachments/assets/119bf878-b1fe-49ed-8dae-fae044f03bda" />

5.1 ) การใช้ภาษาการค้นหาของ Splunk ในเมนู 

ค้นหาคำสั่ง: index="lab0" sourcetype="cisco:asa" tcp | top severity_level

<img width="1959" height="327" alt="image" src="https://github.com/user-attachments/assets/70650c7b-44cc-46a8-8a7c-5a42010b341b" />

5.2 ) 

<img width="1917" height="963" alt="image" src="https://github.com/user-attachments/assets/803fca96-6737-496a-b1db-f6210833a58b" />


1️⃣ ส่วนค้นหา (Search part)

    index="lab0" sourcetype="cisco:asa" transport=tcp severity_level=informational


ความหมาย:

index="lab0" → ค้น log จาก index ชื่อ lab0

sourcetype="cisco:asa" → log จาก Cisco ASA firewall

transport=tcp → เฉพาะ traffic ที่เป็น TCP

severity_level=informational → log ระดับ informational

2️⃣ ส่วนสรุปข้อมูล (Transforming command)

    | stats count by dest_ip


นี่คือ หัวใจของคำสั่งนี้เลย

แปลเป็นภาษาคน:

“นับจำนวน log แล้วแยกตาม IP ปลายทาง (dest_ip)”

Splunk จะทำสิ่งนี้:

เอา event ทั้งหมดที่ค้นเจอ

จัดกลุ่ม (group) ตามค่า dest_ip

นับจำนวน event ของแต่ละ IP

แปลว่า:

มี log TCP informational

ที่วิ่งไปหา 10.122.68.227 จำนวน 25 ครั้ง

ดูได้เลยว่า IP ไหนถูกติดต่อบ่อยที่สุด

5.3 ) HEAD 

        command : index="lab0" sourcetype=cisco:asa | head 5

<img width="1925" height="722" alt="image" src="https://github.com/user-attachments/assets/94ac5b52-0ea9-443f-85ab-3a1961a072eb" />

--- คำอธิบาย ---

1️⃣ | head 5

👉 เอา 5 event แรกมาแสดง

2️⃣ | tail 5

👉 เอา 5 event จากล่างสุดมาแสดง

5.4 ) TOP

        command : index="lab0" sourcetype=cisco:asa | top src_ip

<img width="1899" height="638" alt="image" src="https://github.com/user-attachments/assets/1a15610d-b368-4ff9-8ecf-28297cea79d8" />

<img width="1919" height="920" alt="image" src="https://github.com/user-attachments/assets/348984b0-7e4a-45a3-b8ce-da690caa4300" />

Splunk จะทำให้โดยอัตโนมัติ:

- จัดกลุ่มตาม src_ip

- นับจำนวน event ของแต่ละ IP

- เรียงจากมาก → น้อย

- แสดง Top 10 เป็นค่าเริ่มต้น

👉 ใช้ในงานจริงยังไง

- คำสั่งนี้ใช้บ่อยมากในงาน security 🔐

- หา IP ที่ยิงเข้ามาบ่อยที่สุด

- หา เครื่องภายในที่ออก traffic เยอะผิดปกติ

- ตรวจ Brute force / Scan

- เอาไปทำ Dashboard Top Talkers

5.5 ) rare

        command : index="lab0" sourcetype=cisco:asa | rare src_ip

<img width="1901" height="600" alt="image" src="https://github.com/user-attachments/assets/328e8962-7796-4eda-bcd9-0dbf91d5318c" />

👉top src_ip	IP ที่พบบ่อย

👉rare src_ip	IP ที่พบน้อย

5.6 ) table

        command: index="lab0" sourcetype=cisco:asa transport=* | table src_ip transport

<img width="1886" height="439" alt="image" src="https://github.com/user-attachments/assets/6c8abbbd-bac2-471f-8ef7-463532e48bc0" />

1️⃣ ส่วนค้นหา (Search)
index="lab0" sourcetype=cisco:asa transport=*


ความหมาย:

index="lab0" → ค้นจาก index lab0

sourcetype=cisco:asa → log จาก Cisco ASA

transport=* →

👉 เอาเฉพาะ event ที่มี field transport

* = มีค่าอะไรก็ได้ แต่ต้อง “มีอยู่”

📌 ใช้กรอง event ที่เป็น TCP / UDP / ICMP ฯลฯ

2️⃣ ส่วนแสดงผล (Formatting)

| table src_ip transport

ความหมาย:

แสดงผลลัพธ์เป็น ตาราง

เอาเฉพาะ field:

src_ip → IP ต้นทาง

transport → โปรโตคอล (tcp / udp ฯลฯ)

Field อื่น ไม่แสดง

5.7 )

        command : index="lab0" sourcetype="cisco:asa" | stats count by severity_level

<img width="959" height="730" alt="image" src="https://github.com/user-attachments/assets/2fcea09b-fb82-4c48-9883-a98edcc3ef1b" />

เช็คว่า “Firewall ส่ง log ระดับไหนออกมามากที่สุด”

severity_level คืออะไร (Cisco ASA)

    📌 ระดับความรุนแรงของ syslog (ยิ่งเลขน้อย = ยิ่งร้ายแรง):

        emergency (0) -	ระบบล่ม

        alert (1)	  - วิกฤตมาก

        critical (2)  - ร้ายแรง

        error (3)	  - ผิดพลาด

        warning (4)	  - เตือน

        notice (5)	  - แจ้งเตือนทั่วไป

        informational (6)	- ข้อมูลทั่วไป

        debug (7)     - ดีบัก 

Firewall action trend

- ใช้แผนภูมิเวลาเพื่อลงจุดข้อมูลอนุกรมเวลา span parameter ที่ควบคุมรายละเอียด เช่น จำนวนข้อมูลที่ถูกนับต่อช่วงเวลา

- Set

     - Time Picker: Last 7 days

     - Visualization to Area

     - Scale of Y-Axis to Log

ค้นหาคำสั่ง: index="lab0" sourcetype="cisco:asa" | timechart span=1h count by action usenull=f

Result:

<img width="950" height="516" alt="image" src="https://github.com/user-attachments/assets/eb027842-452b-4602-ac8b-495c7c8589e0" />

<img width="1236" height="294" alt="image" src="https://github.com/user-attachments/assets/235ce76a-7333-4eef-b3b8-f32981ed000d" />

<img width="528" height="343" alt="image" src="https://github.com/user-attachments/assets/d1da5aa5-3e15-43c0-b649-6008ddadff4d" />


--------------------------------------------------------------------------------------------------------------------------------------------------------------



Top N Network Metric

- This report requires the field src_ip (Source IP)

- Set

     - Time Picker = Last 7 days

     - Visualization = Pie



ค้นหาคำสั่ง: index="lab0" sourcetype="cisco:asa" | top src_ip

Result:

<img width="1241" height="510" alt="image" src="https://github.com/user-attachments/assets/57320241-5c6b-4af6-991a-ac3fc3125553" />

- คลิก Dashboards view และเลือก Create New Dashboard.

- <img width="1262" height="386" alt="image" src="https://github.com/user-attachments/assets/ba4b2d2c-e3d1-4460-8010-fd9ac42636fd" />


- ซึ่งจะเปิดกล่องโต้ตอบสร้างแดชบอร์ดใหม่ ซึ่งช่วยให้คุณสามารถกำหนดแดชบอร์ดใหม่ได้

- กรอกรายละเอียดสำหรับ Title และเลือก Shared in App.

- และสุดท้าย คลิก Create

- <img width="528" height="500" alt="image" src="https://github.com/user-attachments/assets/6e8c63e3-6889-404d-ac0c-9e6fcdd63388" />

<img width="1256" height="458" alt="image" src="https://github.com/user-attachments/assets/a84c50d3-a580-4098-a774-debbc3d83948" />

<img width="1252" height="880" alt="image" src="https://github.com/user-attachments/assets/d96cfb52-1d54-4625-b7a0-afafc9b137a7" />
















