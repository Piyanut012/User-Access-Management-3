# Access Control
  **Access Control** คือการควบคุมการเข้าถึงทรัพยากรโดยทั่วไปหรือไฟล์
### Types of Access Control
Discretionary access control | Mandatory access control
------- | ------- |
เป็นการควบคุมการเข้าถึงตามดุลยพินิจของผู้ที่มีสิทธิ์บางอย่างซึ่งสามารถส่งต่อให้กับผู้ใช้อื่นๆได้ | เป็นการควบคุมการเข้าถึงที่มีลำดับชั้น โดยที่ผู้ใช้สามารถเข้าถึงทรัพยกรที่สอดคล้องกับลำดับชั้น ซึ่งผู้ใช้ไม่สามารถตั้งค่าสิทธิ์ด้วยตนเองได้ แม้ว่าจะเป็นเจ้าของทรัพยากร |
### AppArmor
 **AppArmor** เป็นระบบ Mandatory access control (MAC) เป็นส่วนเสริมของ Kernel เพื่อจำกัดโปรแกรมให้อยู่ในชุดทรัพยากรที่จำกัด โดยรูปแบบรักษาความปลอดภัย คือการผูกคุณลักษณะการควบคุมการเข้าถึงกับโปรแกรม แทนที่จะผูกกับผู้ใช้ มีการใช้ผ่านทาง Profile มี 2 โหมดด้วยกัน
* Enforcement คือโหมดบังคับนโยบายไว้ใน Profile เลยไม่สามารถเข้าถึงไฟล์ได้ตามที่กำหนด
* Complain คือโหมดที่มีการรายงานเมื่อมีการละเมิดนโยบาย แต่ไม่มีการบังคับ

AppArmor แตกต่างจากระบบ MAC อื่นๆ บน Linux สามารถผสม 2 โหมดเข้าด้วยกันได้ มีการใช้ไฟล์รวมเพื่อความสะดวกในการพัฒนาและมีปัญหาน้อยในการเข้าใช้เมื่อเทียบกับ MAC อื่นๆ
### วิธิ้การใช้ AppArmor
* ติดตั้ง AppArmor

``` Bash
sudo apt install apparmor-profiles
```
* การตรวจสอบสถานะของ AppArmor

``` Bash
sudo aa-status
```
ตัวอย่างผลลัพธ์

![image](https://github.com/Piyanut012/User-Access-Management-3/assets/110012203/d6b9205e-1490-4233-842c-1d7311a78cd8)
* /etc/apparmor.d คือที่ตั้งของโปรไฟล์ AppArmor สามารถใช้เพื่อจัดการโหมดของโปรไฟล์ทั้งหมดได้

ตัวอย่างโปรไฟล์บางส่วน

![image](https://github.com/Piyanut012/User-Access-Management-3/assets/110012203/5e3384ad-fba2-47be-ac42-cd33ccbdff6d)

* **โปรไฟล์ AppArmor** เป็นไฟล์ข้อความธรรมดาที่อยู่ในรูปแบบ/etc/apparmor.d/. ไฟล์ต่างๆ มีการตั้งชื่อตามเส้นทาง 
 เช่น /etc/apparmor.d/bin.ping โดยมีองค์ประกอบ 2 อย่าง
    * เส้นทางที่ appication สามารถเข้าถึงได้ในระบบไฟล์ เช่น /path/to/test_app.sh
    * กำหนดสิทธิ์ที่กระบวนการที่จำกัดได้รับอนุญาตให้ใช้ เช่นการอ่านไฟล์ เขียนไฟล์ เป็นต้น

### ตัวอย่างการสร้างโปรไฟล์
* เริ่มจากสร้าง application ที่จะใช้ทดสอบ

ชื่อไฟล์คือ test_app.sh โดยมีฟังก์ชันสำหรับเขียนกับอ่านไฟล์
 
![image](https://github.com/Piyanut012/User-Access-Management-3/assets/110012203/b1831dd6-849a-4c5b-ad1b-50682b8eaac5)

ผลลัพธ์

![image](https://github.com/Piyanut012/User-Access-Management-3/assets/110012203/b726b8f1-9d18-4642-b02b-c880058eaca2)

* จากนั้นใช้คำสั้งสร้างโปรไฟล์

_แบบแรก_

แบบนี้จะสร้างให้อัตโนมัติ

``` Bash
sudo aa-genprof test_app.sh
```

_เราจะใช้แบบที่สองกัน_

สร้างพร้อมกับเขียนเส้นทางและการอนุญาติ

``` Bash
sudo nano /etc/apparmor.d/test_app_profile
```

![Screenshot 2024-02-08 003515](https://github.com/Piyanut012/User-Access-Management-3/assets/110012203/2f011331-630b-46f9-a737-e759e2aa6b34)

จากนั้นเพิ่มโหมด complain ให้กับโปรไฟล์นี้

``` Bash
sudo aa-complain /etc/apparmor.d/test_app_profile
```

ผลลัพธ์

![image](https://github.com/Piyanut012/User-Access-Management-3/assets/110012203/3ef30b7b-cdf1-4dfe-a45a-b2d465989be8)

อธิบายการอนุญาติที่เขียนไว้
* dac_override คือให้สิทธิ์ในการเขียนทับ directory นั้นๆ
* dac_read_search คือให้สิทธิ์ในการค้นหาและอ่าน directory นั้นๆ
* /test/test_file.txt rw คือให้สิทธิ์ในแอปพลิเคชันสามารถทำการเขียน (rw) ไฟล์ที่อยู่ใน /test/test_file.txt ได้

### การทดสอบการใช้ระหว่างโหมด complain และ enforce
* complain

``` Bash
sudo aa-complain /etc/apparmor.d/test_app_profile
```











































