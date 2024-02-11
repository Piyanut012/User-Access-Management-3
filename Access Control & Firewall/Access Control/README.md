# Access Control
 **Access Control** คือการควบคุมสิทธิ์และการเข้าถึงในการดำเนินการต่างๆที่เกี่ยวกับทรัพยากรโดยทั่วไปหรือไฟล์ 
 
## วิธีการทำ Acess Control
 * File permissions คือการใช้คำสั่ง ```chmod``` เพื่อกำหนดสิทธิ์ของ file และ directory โดยแบ่งออกเป็น 3 กลุ่มคือ เจ้าของ (owner), กลุ่ม (group) และสาธารณะ (others) โดยให้สิทธิ์ในการอ่าน, เขียน และดำเนินการกับ file และ directory นั้นๆ
 * User and Group Management คือ ผู้ดูแลระบบสามารถกำหนดว่าผู้ใช้แต่ละคนจะอยู่ในกลุ่มใดๆ และกำหนดสิทธิ์ของกลุ่มนั้นๆได้ เช่น ```useradd```, ```userdel```, ```usermod```, ```groupadd```เป็นต้น
 * Access Control Lists (ACLs)  คือการกำหนดสิทธิ์เพิ่มเติม โดยให้สิทธิ์ที่เฉพาะเจาะจงมากขึ้นกับ file หรือ directory โดยไม่ต้องเปลี่ยนความเป็นเจ้าและการอนุญาต ช่วยให้สามารถการเข้าถึงสำหรับผู้ใช้หรือกลุ่มอื่น เช่น การกำหนด ACL ให้กับ file หรือ directory ```setfacl -m u:username:rw filename``` เป็นต้น

## ชนิดของการควบคุม
Discretionary access control | Mandatory access control
------- | ------- |
เป็นการควบคุมการเข้าถึงตามดุลยพินิจของผู้ที่มีสิทธิ์บางอย่างซึ่งสามารถส่งต่อให้กับผู้ใช้อื่นๆได้ | เป็นการควบคุมการเข้าถึงที่มีลำดับชั้น โดยที่ผู้ใช้สามารถเข้าถึงทรัพยากรที่สอดคล้องกับลำดับชั้น ซึ่งผู้ใช้ไม่สามารถตั้งค่าสิทธิ์ด้วยตนเองได้ แม้ว่าจะเป็นเจ้าของทรัพยากร |
# AppArmor

![image](https://github.com/Piyanut012/User-Access-Management-3/assets/110012203/20e15192-1e34-4466-9274-e631cbae9d4a)

 **AppArmor** เป็นระบบ Mandatory access control (MAC) เป็นส่วนเสริมของ Kernel เพื่อจำกัดโปรแกรมให้อยู่ในชุดทรัพยากรที่จำกัด โดยรูปแบบรักษาความปลอดภัย คือการผูกคุณลักษณะการควบคุมการเข้าถึงกับโปรแกรม แทนที่จะผูกกับผู้ใช้ มีการใช้ผ่านทาง Profile มี 2 โหมดด้วยกัน
* Enforcement คือโหมดบังคับนโยบายไว้ใน Profile เลยไม่สามารถเข้าถึงไฟล์ได้ตามที่กำหนด
* Complain คือโหมดที่มีการรายงานเมื่อมีการละเมิดนโยบาย แต่ไม่มีการบังคับ

AppArmor แตกต่างจากระบบ MAC อื่นๆ บน Linux สามารถผสม 2 โหมดเข้าด้วยกันได้ มีการใช้ไฟล์รวมเพื่อความสะดวกในการพัฒนาและมีปัญหาน้อยในการเข้าใช้เมื่อเทียบกับ MAC อื่นๆ
## คำสั่งและวิธีการใช้งาน AppArmor
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

## ตัวอย่างการสร้างโปรไฟล์
* เริ่มจากสร้าง application ที่จะใช้ทดสอบ

ชื่อไฟล์คือ app_test.sh โดยมีฟังก์ชันสำหรับเขียนกับอ่านไฟล์
 
![image](https://github.com/Piyanut012/User-Access-Management-3/assets/110012203/7b0eab09-461d-4442-a325-3de4335bba32)


ผลลัพธ์การ execute ปกติ

![image](https://github.com/Piyanut012/User-Access-Management-3/assets/110012203/b726b8f1-9d18-4642-b02b-c880058eaca2)

* จากนั้นใช้คำสั้งสร้างโปรไฟล์ของ application

``` Bash
sudo aa-genprof app_test.sh
```

* ใช้คำสั่งเพื่อโหลดโปรไฟล์ลงใน kernel (-r เป็น option ถ้ามีการเปลื่ยนแปลงและต้องการอัพเดต)

``` Bash
sudo apparmor_parser -r /etc/apparmor.d/app_test.sh
```

หลังจากนั้นลอง execute จะได้ผลลัพธ์ดังรูปเนื่องจาก application ที่สร้างมีการปฎิเสธการอนุญาติการอ่านไฟล์และเขียนไฟล์

![Screenshot 2024-02-08 143642](https://github.com/Piyanut012/User-Access-Management-3/assets/110012203/7f3939c8-583f-4fb8-a76a-2f5a23cf71fd)

_โหมดปัจจุบันเป็น enforce ซึ่งเป็นการบังคับ โดยการอนุญาติจะเขียนไว้ใน profile ดังรูป_

![Screenshot 2024-02-08 144046](https://github.com/Piyanut012/User-Access-Management-3/assets/110012203/bc090bb4-e6b7-445f-ab7f-3f8239f6ae2c)

## การทดสอบการใช้ระหว่างโหมด enforce  และ complanin

### โหมด enforce

* ใช้คำสั่งอัพเดตโปรไฟล์เพื่อเพิ่มการอนุญาติให้กับ application และใช้คำสั่งโหลดลง kernel 

``` Bash
sudo aa-logprof
```

![image](https://github.com/Piyanut012/User-Access-Management-3/assets/110012203/90a3a3fd-c5c9-41df-95e7-6e551c0f239d)

ภายในโปรโฟล์มีการอนุญาติเพิ่มมา

![image](https://github.com/Piyanut012/User-Access-Management-3/assets/110012203/8b741d33-0061-4663-b91e-c1b61f10a020)

_owner /home/*/test/test_file.txt rw คือการอนุญาติให้อ่านกับเขียนไฟล์ได้_

ผลลัพธ์การ execute

![image](https://github.com/Piyanut012/User-Access-Management-3/assets/110012203/28692ebd-364b-41af-ab4f-909ae88cc026)

* ถ้าต้องการให้เขียนไฟล์ได้ แต่อ่านไฟล์ไม่ได้ โดยไปแก้ในโปรไฟล์ดังรูป จากนั้นให้อัพเดตและโหลดลง kernel 

![image](https://github.com/Piyanut012/User-Access-Management-3/assets/110012203/af62c4f8-c991-48b8-b99e-6a4b644cf5fc)

ผลลัพธ์การ execute

![image](https://github.com/Piyanut012/User-Access-Management-3/assets/110012203/b1d9802b-2ccc-46e5-81ce-6abb5ff3d5d8)

_สังเกตว่าอ่านไฟล์สำเร็จแต่อ่านไฟล์ถูกปฏิเสธ_

### โหมด complain

* ใช้คำสั่งเปลื่ยนเป็นโหมด complain 

``` Bash
sudo aa-complain /etc/apparmor.d/app_test.sh
```

_กรณีถ้าต้องการใช้โหมด enforce_

``` Bash
sudo aa-enforce /etc/apparmor.d/app_test.sh
```

ผลลัพธ์การ execute 

![image](https://github.com/Piyanut012/User-Access-Management-3/assets/110012203/4bd7e19a-fb57-4e9a-a87c-165519bfaae8)

_สังเกตว่าจะไม่ถูกปฏิเสธ แต่จะมีการบันทึกไว้ใน log file ซึ่งอยู่ที่ ```/var/log/syslog```_

![image](https://github.com/Piyanut012/User-Access-Management-3/assets/110012203/a9385578-2925-4cfa-aa69-d0b9c7ec3643)

## แหล่งอ้างอิง
* [4. Access Control - Learning Modern Linux [Book]](https://www.oreilly.com/library/view/learning-modern-linux/9781098108939/ch04.html)
* [Set Linux Permissions and Access Control Lists | Computer Networks | CompTIA](https://www.comptia.org/blog/set-linux-permissions-and-access-control-lists)
* [AppArmor - Ubuntu Wiki](https://wiki.ubuntu.com/AppArmor)
* [Security - AppArmor | Ubuntu](https://ubuntu.com/server/docs/security-apparmor)
* [Securing Applications with Apparmor](https://www.youtube.com/watch?v=0t-UZFBNyF0)

























































