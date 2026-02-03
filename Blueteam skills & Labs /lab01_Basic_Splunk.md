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
