# บทบาทและหน้าที่บน Linux
Password policy เป็นมาตรการสำคัญในด้าน Security ของระบบ Linux การเข้าถึงข้อมูลโดยไม่ได้รับอนุญาตส่วนใหญ่เกิดจากการ Brute force ธรรมดาหรือใช้ Dictionary attack กับรหัสผ่านที่อ่อนแอ เพื่อป้องกันไม่ให้ผู้ที่ไม่มีสิทธิ์สามารถเข้าถึงระบบได้โดยง่ายและเสี่ยงต่อการถูกโจรกรรมข้อมูล ข้อมูลรั่วไหล และอื่นๆ การกำหนด Password policy ที่ดีจึงมีความสำคัญอย่างมากในด้าน User/access management

# พื้นฐานและหลักการทำงาน
ในระบบปฏิบัติการ Ubuntu มีการกำหนดค่าเริ่มต้นของ Password policy เพียงแค่ความยาวของรหัสผ่านต้องไม่ต่ำกว่า 6 ตัวอักษร และมีการตรวจสอบ Password entropy แบบง่ายเท่านั้น ดังนั้น User จึงจำเป็นต้องกำหนด Password policy ขึ้นมาใหม่ให้เหมาะสม ในที่นี้ จะใช้ Library ของ Pluggable Authentication Module (PAM) ที่ชื่อว่า pam_pwquality สำหรับการตรวจสอบคุณภาพของรหัสผ่าน และกำหนด Requirement ในการตั้งรหัสผ่าน โดยหลังจากติดตั้งสำเร็จแล้ว จะสามารถตั้งค่าต่างๆ สำหรับการกำหนด Password policy ที่จำเป็นได้ เช่น ความยาวที่น้อยที่สุดของรหัสผ่าน จำนวนตัวอักษรพิมพ์ใหญ่และพิมพ์เล็กในรหัสผ่าน การบังคับใช้อักษรพิเศษและตัวเลข เป็นต้น

# การเรียกใช้งานและผลลัพธ์ที่ได้
1. เริ่มจากการติดตั้ง pam_pwquality ด้วยคำสั่ง [sudo] apt-get [-y] install libpam-pwquality ซึ่งควรจะได้ผลลัพธ์ประมาณดังรูป
   ```
   sudo apt-get install libpam-pwquality
   ```
   
   ![](https://github.com/Piyanut012/User-Access-Management-3/assets/112673913/1ebb71e9-7c30-4e32-a911-985a98e8d127)

2. หลังจากนั้นจึงสามารถเรียกใช้คำสั่งต่างๆ เพื่อสร้าง Password policy ได้ เริ่มจากการกำหนดให้ User ต้องเปลี่ยนรหัสผ่านทุกๆ 2 เดือน ด้วยคำสั่ง [sudo] nano /etc/login.defs
   ```
   sudo nano /etc/login.defs
   ```

   ![](https://github.com/Piyanut012/User-Access-Management-3/assets/112673913/6a279d54-ce1a-4aa7-88b6-6e73951210a0)

   ให้เลื่อนลงมาหรือค้นหาบรรทัดที่เขียนว่า PASS_MAX_DAYS แล้วแก้เป็น 60
   
   ![](https://github.com/Piyanut012/User-Access-Management-3/assets/112673913/7e315271-7df6-45c1-800b-791b17fc2b4e)

   ในส่วนของ PASS_MIN_DAYS และ PASS_WARN_AGE จะหมายถึงจำนวนวันที่ต้องเว้นหากต้องการเปลี่ยนรหัสผ่านอีกครั้ง และจำนวนวันในการแจ้งเตือนว่ารหัสผ่านกำลังจะหมดอายุตามลำดับ หากต้องการแก้ไขก็สามารถทำได้เช่นเดียวกัน ในที่นี้จะใช้ค่าเดิม

3. ในขั้นตอนต่อไป จะทำการกำหนด Requirements ขั้นต่ำในการตั้งรหัสผ่าน ซึ่งทำได้ผ่านคำสั่ง [sudo] nano /etc/pam.d/common-password
   ```
   sudo nano /etc/pam.d/common-password
   ```

   ![](https://github.com/Piyanut012/User-Access-Management-3/assets/112673913/6c1e4142-e5be-4861-ac46-f9318a48d97d)

   สามารถแก้ไขและเพิ่มเงื่อนไขในการตั้งรหัสผ่านได้ โดยแต่ละเงื่อนไขมีความหมายต่าง ๆ ดังต่อไปนี้ กำหนดให้ n เป็นจำนวนเต็ม
    -	retry=n หมายถึงขึ้นเตือนอย่างน้อย n ครั้งก่อนขึ้น Error
    -	difok=n หมายถึงจำนวนตัวอักษร n ตัวในรหัสผ่านใหม่ต้องไม่อยู่ในรหัสผ่านเก่า
    -	minlen=n หมายถึงขนาดของรหัสผ่านต้องมีความยาวไม่ต่ำกว่า n เครดิต อักษรแต่ละตัวมีค่าตั้งต้นเท่ากับ 1 เครดิต
    -	dcredit=n หมายถึงจำนวนตัวเลขแต่ละตัวในรหัสผ่านมีค่าเท่ากับ n เครดิต หาก n ติดลบจะหมายถึงจำนวนตัวเลขในรหัสผ่านต้องมีอย่างน้อย n ตัว
    -	ucredit=n หมายถึงจำนวนตัวอักษรพิมพ์ใหญ่แต่ละตัวในรหัสผ่านมีค่าเท่ากับ n เครดิต หาก n ติดลบจะหมายถึงจำนวนตัวอักษรพิมพ์ใหญ่ในรหัสผ่านต้องมีอย่างน้อย n ตัว
    -	lcredit=n หมายถึงจำนวนตัวอักษรพิมพ์เล็กแต่ละตัวในรหัสผ่านมีค่าเท่ากับ n เครดิต หาก n ติดลบจะหมายถึงจำนวนตัวอักษรพิมพ์เล็กในรหัสผ่านต้องมีอย่างน้อย n ตัว
    -	ocredit=n หมายถึงจำนวนอักษรพิเศษแต่ละตัวในรหัสผ่านมีค่าเท่ากับ n เครดิต หาก n ติดลบจะหมายถึงจำนวนอักษรพิเศษในรหัสผ่านต้องมีอย่างน้อย n ตัว
    -	minclass=n หมายถึงจำนวนประเภทของอักขระในรหัสผ่านต้องมีไม่ต่ำกว่า n ประเภท โดยมีทั้งหมด 4 ประเภท ได้แก่ ตัวเลข ตัวอักษรพิมพ์ใหญ่ ตัวอักษรพิมพ์เล็ก และอักษรพิเศษ
    -	maxrepeat=n หมายถึงจำนวนอักขระวางเรียงกันต่อเนื่องได้มากที่สุด n ตัว หาก n เป็น 0 จะไม่มีผล
    -	maxclassrepeat=n หมายถึงจำนวนประเภทของอักขระวางเรียงกันต่อเนื่องได้มากที่สุด n ตัว หาก n เป็น 0 จะไม่มีผล
    -	gecoscheck=n หมายถึงการตรวจสอบว่าคำในรหัสผ่านอยู่ใน [GECOS](https://en.wikipedia.org/wiki/Gecos_field) หรือไม่ หาก n ไม่ใช่ 0 จะหมายถึงให้ตรวจสอบ
    -	dictcheck=n หมายถึงการตรวจสอบว่าคำในรหัสผ่านอยู่ใน [Cracklib Dictionary](https://github.com/cracklib/cracklib) หรือไม่ หาก n ไม่ใช่ 0 จะหมายถึงให้ตรวจสอบ
    -	usercheck=n หมายถึงการตรวจสอบว่ารหัสผ่านมี username ในรูปแบบใดๆ หรือไม่ หาก n ไม่ใช่ 0 จะหมายถึงให้ตรวจสอบ
    -	usersubstr=n หมายถึงการตรวจสอบความยาวของ substring จาก username ในรหัสผ่านที่ตั้ง ตรวจสอบก็ต่อเมื่อ n ไม่ใช่ 0 และ usercheck มีการตรวจสอบ
    -	badwords=word1 word2 … หมายถึงคำต้องห้ามในการตั้งรหัสผ่าน

    เพื่อความเรียบง่าย จะกำหนด Requirements เบื้องต้นเพียงดังต่อไปนี้
    -	minlen=10 (ต้องยาวไม่ต่ำกว่า 10 ตัวอักษร)
    -	ucredit=-1 (ต้องมีตัวอักษรพิมพ์ใหญ่อย่างน้อย 1 ตัว)
    -	lcredit=-1 (ต้องมีตัวอักษรพิมพ์เล็กอย่างน้อย 1 ตัว)
    -	dcredit=-1 (ต้องมีตัวเลขอย่างน้อย 1 ตัว)
    -	ocredit=-1 (ต้องมีตัวอักษรพิเศษอย่างน้อย 1 ตัว)

    ที่บรรทัดที่ 25 จะเขียนว่า `password      requisite      pam_pwquality.so retry=3` ให้เพิ่มเงื่อนไขข้างต้นเข้าไป

    ![](https://github.com/Piyanut012/User-Access-Management-3/assets/112673913/b2b2790a-3f82-4d77-acb2-c074b2395288)

4. ทดสอบ Password policy ที่สร้างขึ้นด้วยการ reboot และสร้าง User ทดลองขึ้นมา
   ```
   reboot
   ```
   ```
   sudo useradd puppet1
   ```
   ตามด้วย
   ```
   sudo passwd puppet1
   ```
   หากกรอกรหัสผ่านไม่ครบตามเงื่อนไขจะขึ้นผลลัพธ์ประมาณดังรูปเป็นอันสำเร็จ

   ![](https://github.com/Piyanut012/User-Access-Management-3/assets/112673913/7ea52833-6930-438c-973e-607a0f65ccd8)

## Troubleshooting
หาก reboot แล้ว ไม่สามารถ login ได้ ให้ทำการ [reset รหัสผ่าน](https://kb.parallels.com/123324) ผ่าน Recovery mode และแก้ไขไฟล์ pwquality.conf ผ่านคำสั่ง
```
   nano /etc/security/pwquality.conf
```
uncomment (ลบ # ด้านหน้าออก) เงื่อนไขที่กำหนดก่อนหน้าให้ตรงกัน

![](https://github.com/Piyanut012/User-Access-Management-3/assets/112673913/512f9f00-8204-42a0-a1ea-858ade3220e0)

จากนั้นทำการ exit ออกมาหน้า menu
```
exit
```
เลือกตัวเลือก `resume         Resume normal boot` และลอง login  ด้วยรหัสผ่านใหม่

---

### อ้างอิง
https://askubuntu.com/questions/244115/how-do-i-enforce-a-password-complexity-policy

https://www.server-world.info/en/note?os=Ubuntu_20.04&p=password

https://hostadvice.com/how-to/web-hosting/ubuntu/how-to-enable-and-enforce-secure-password-policies-on-ubuntu/

https://discourse.ubuntu.com/t/user-management/11881
   
