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
    * เส้นทางที่แอปพลิเคชันสามารถเข้าถึงได้ในระบบไฟล์
    * กำหนดสิทธิ์ที่กระบวนการที่จำกัดได้รับอนุญาตให้ใช้

ตัวอย่าง /etc/apparmor.d/bin.ping

![image](https://github.com/Piyanut012/User-Access-Management-3/assets/110012203/4621a560-ae1a-4cf6-94d6-b9620be16ddd)

**คำอธิบายบางส่วน** 
* **#include <tunables/global>** รวมคำสั่งจากไฟล์อื่น ซึ่งช่วยให้สามารถวางคำสั่งที่เกี่ยวข้องกับหลายแอปพลิเคชันไว้ในไฟล์ทั่วไปได้
* **/bin/ping flags=(complain)** เส้นทางไปยังโปรแกรมและการตั้งค่าโหมด complain ไว้
* **capability net_raw** การอนุญาตให้แอปพลิเคชันเข้าถึงความสามารถ CAP_NET_RAW Posix.1e
* **/bin/ping mixr** อนุญาตให้แอปพลิเคชันอ่านและดำเนินการเข้าถึงไฟล์


#### การเปลื่ยนโหมด
จากตัวอย่างข้างต้นลองเปลื่ยนโหมดเป็น enforce โดยใช้คำสั่ง

``` Bash
sudo aa-enforce /etc/apparmor.d/bin.ping
```

ผลลัพธ์คือ flags=(complain) หายไป

![image](https://github.com/Piyanut012/User-Access-Management-3/assets/110012203/6ece5fd2-595d-460b-b95b-f7c620cde043)

![image](https://github.com/Piyanut012/User-Access-Management-3/assets/110012203/d9884310-6e56-44f8-ae3e-b6820a8fcc7d)

เปลื่ยนกลับมาเป็นโหมด complain

``` Bash
sudo aa-complain /etc/apparmor.d/bin.ping
```

![image](https://github.com/Piyanut012/User-Access-Management-3/assets/110012203/aad26a4c-b8c8-41f1-a6a1-2cb0a2e20f86)

ส่วนใหญ่เมื่อมีการละเมิดจะเก็บข้อมูลไว้ใน syslog หรือ audit log 








































