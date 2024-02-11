# pam.d
**pam.conf** เป็นไฟล์การกำหนดค่าแบบดั้งเดิมสำหรับสถาปัตยกรรม Pluggable Authentication Module หรือ PAM ไฟล์นโยบายต่อบริการใน /etc/pam.d/ จัดเตรียมกลไกการกำหนดค่าทางเลือกและที่ต้องการสำหรับ PAM
``` Bash
/etc/pam.conf
/etc/pam.d/service
```
การกำหนดค่า PAM ในไฟล์ต่อไปนี้ :
* **/etc/pam.conf สำหรับชื่อบริการ PAM ปัจจุบัน**
* **/etc/pam.d/service สำหรับชื่อบริการ PAM ปัจจุบัน**
* **/etc/pam.conf สำหรับชื่อบริการ PAM ของ “other”**
* **/etc/pam.d/other**
# PAM
PAM ประกอบด้วยชุดไลบรารีที่ใช้ร่วมกันซึ่งช่วยให้ผู้ดูแลระบบภายในสามารถเลือกวิธีที่แอปพลิเคชันต่างๆ สามารถตรวจสอบสิทธิ์ผู้ใช้ได้ ตัวอย่างเช่น โปรแกรมเฉพาะจะรับสายเมื่อผู้ใช้เชื่อมต่อกับพอร์ตอนุกรมหรือเครือข่ายระหว่างเข้าสู่ระบบโปรแกรม SSH จะรับสายที่เกี่ยวข้องกับการเชื่อมต่อเครือข่ายและ Getty สำหรับสายอนุกรม ในขณะที่ telnet ยังสามารถรับสายที่เกี่ยวข้องกับการเชื่อมต่อเครือข่ายได้อีกด้วย

เมื่อโปรแกรมใด ๆ ข้างต้นรับสาย มันจะเริ่มต้นโปรแกรมเข้าสู่ระบบ มันจะเข้าสู่ระบบและขอชื่อผู้ใช้และรหัสผ่านในที่สุดเพื่อตรวจสอบข้อมูลรับรองในไฟล์ /etc/passwd PAM มักจะสร้างชั้นการป้องกันระหว่างแอปพลิเคชันและโปรโตคอลการตรวจสอบความถูกต้องจริง

อย่างไรก็ตาม PAM ยังสามารถรองรับโปรแกรมตรวจสอบความถูกต้องอื่น ๆ ได้ แต่แตกต่างจากโปรแกรมตรวจสอบสิทธิ์ทั่วไป PAM สามารถจัดการเซสชันและข้อมูลบัญชีได้แม่นยำยิ่งขึ้น เช่น PAM สามารถปฏิเสธการเข้าใช้งานของผู้ใช้ทั่วไปได้ระหว่างเวลา 17.30 น. ถึง 06.00 น. ไฟล์ PAM มีอยู่ที่ /etc/pam.d

ตัวอย่างของรายการการกำหนดค่าไฟล์ PAM ในการตั้งค่าคือ
``` Bash
#
# default configuration: /etc/pam.d/other
#
auth required pam_deny.so
account required pam_deny.so
password required pam_deny.so
session required pam_deny.so
```
ระบบพิสูจน์ตัวตน Linux ที่สำคัญที่สุด ได้แก่

**1. LDAP (โปรโตคอลการเข้าถึงไดเรกทอรีแบบไลท์เวท)**

ตามชื่อที่กล่าวถึง LDAP เป็นโปรโตคอลการตรวจสอบสิทธิ์แบบไลท์เวทที่มักใช้ในการเข้าถึงบริการไดเร็กทอรี X.500 และอื่นๆ มันทำงานผ่านบริการถ่ายโอน เช่น IP/TCP หรือโมดูลการถ่ายโอนเชิงการเชื่อมต่อ

โปรโตคอล LDAP จัดเก็บรายการซึ่งเป็นคอลเลกชันของคุณลักษณะที่มี Distinguished Names (DN) ที่ไม่ซ้ำกันทั่วโลก วัตถุประสงค์เพียงอย่างเดียวของ DN คือการอ้างอิงถึงแต่ละรายการอย่างชัดเจน นอกจากนี้ แต่ละแอตทริบิวต์ของรายการจะมีประเภทและค่าเดียวเป็นอย่างน้อย
``` Bash
mail=KenBrian@LINHINT.COM
```
ในแอตทริบิวต์ข้างต้น ประเภทคือเมล ในขณะที่ LINHINT.COM คือค่า

ระบบ LDAP ทำงานบนโมเดลไคลเอ็นต์-เซิร์ฟเวอร์ มันโดดเด่นในฐานะกลไกที่ไคลเอนต์สามารถตรวจสอบหรือพิสูจน์ตัวตนของตนไปยังไดเร็กทอรีเซิร์ฟเวอร์และตั้งคำถามกับเซิร์ฟเวอร์ LDAP ยังมีบทบาทสำคัญในการสนับสนุนบริการด้านความเป็นส่วนตัวและการรักษาความปลอดภัยอีกด้วย

**2. SASL (การตรวจสอบสิทธิ์อย่างง่ายและชั้นความปลอดภัย)**

SASL คือระบบประเภทนั้น ระบบการตรวจสอบสิทธิ์นี้มักกำหนดไว้ใน RFC-2222 รวมถึงคำสั่งที่ระบุและตรวจสอบสิทธิ์ผู้ใช้ไปยังเซิร์ฟเวอร์ได้อย่างแม่นยำ และเจรจาต่อรองชั้นความปลอดภัยระหว่างโปรโตคอลนี้และการเชื่อมต่อ

SASL ช่วยให้ผู้ใช้ตรวจสอบสิทธิ์เซิร์ฟเวอร์โดยไม่ต้องแสดงรหัสผ่านในที่โล่ง แม้ว่า SASL จะสามารถใช้เป็นโปรโตคอลสำหรับการตรวจสอบความถูกต้องเท่านั้น แต่ยังมีประโยชน์ในการตรวจสอบความสมบูรณ์ของระบบหรือเซิร์ฟเวอร์และการเข้ารหัสอีกด้วย สามารถรวมระบบนี้เข้ากับ PAM และโปรโตคอล Kerberos ได้

ไลบรารี SASL เริ่มต้นจะมีลักษณะดังนี้
``` Bash
/usr/lib/sasl/App.conf
```
ในทางกลับกัน แอปเซิร์ฟเวอร์ตัวอย่างที่รวมอยู่ในไลบรารีจะใช้โครงสร้างนี้
``` Bash
/usr/lib/sasl/sample.conf
```
**3. NIS และ NIS+ (บริการข้อมูลเครือข่ายและบริการข้อมูลเครือข่าย Plus)**

ตามชื่อที่แนะนำ บริการเหล่านี้เป็นบริการค้นหาเครือข่ายอย่างง่ายที่ประกอบด้วยฐานข้อมูลและกระบวนการง่ายๆ บริการนี้เปลี่ยนชื่อจาก Sun Yellow Pages เป็น Network Information Service เนื่องจากปัญหาลิขสิทธิ์


แม้ว่าทั้งสองฟังก์ชันจะคล้ายกัน แต่ก็มีชื่อที่แตกต่างกันเนื่องจากการปรับปรุงทางเทคโนโลยี ดังนั้น คุณสามารถเลือกใช้ NIS และใช้ NIS+ ได้เฉพาะในกรณีที่มีความต้องการด้านความปลอดภัยที่ร้ายแรงเท่านั้น

NIS และ NIS+ มักจะเผยแพร่ข้อมูล รวมถึง;

* **ชื่อเข้าสู่ระบบหรือโฮมไดเร็กทอรีและรหัสผ่าน (/etc/passwd)**
* **ชื่อโฮสต์และที่อยู่ IP (/etc/hosts)**
* **ข้อมูลกลุ่ม (ฯลฯ/กลุ่ม)**

โดยปกติ หากทำการค้นหาไฟล์ /etc/host.conf ผลลัพธ์จะให้รายละเอียดที่กำหนดเองของบรรทัดนี้
``` Bash
hosts: files nisplus dns
```
**4. เคอร์เบรอส**

Kerberos เป็นโปรโตคอลการตรวจสอบความถูกต้องเครือข่ายที่เชื่อถือได้ซึ่งนิยมใช้ในการตรวจสอบผู้ใช้หรือแอปพลิเคชันเซิร์ฟเวอร์โดยใช้การเข้ารหัสคีย์ลับ Kerberos เป็นเครื่องมือตรวจสอบสิทธิ์ที่มีช่องโหว่น้อยกว่าระบบตรวจสอบสิทธิ์ Linux OS อื่นๆ

จะไม่ส่งรหัสผ่านที่ไม่ได้เข้ารหัสผ่านเครือข่ายเช่นเดียวกับระบบการตรวจสอบความถูกต้องส่วนใหญ่ นอกจากนี้ ไม่เหมือนกับระบบอื่นๆ ที่ต้องอาศัยโปรแกรมไคลเอนต์อื่นๆ ในเรื่องความน่าเชื่อถือ Kerberos ค่อนข้างเป็นอิสระ

Kerberos จะให้ข้อมูลประจำตัวเมื่อคุณมีรายการในฐานข้อมูล Kerberos แน่นอนว่ารายการจะรวมชื่อหลักและรหัสผ่าน Kerberos ของคุณไว้ด้วย และนอกเหนือจากการให้ข้อมูลประจำตัวแล้ว Kerberos ยังให้รายละเอียดของไฟล์แท็บคีย์อีกด้วย

รายการสำหรับโฮสต์จะมีลักษณะเช่นนี้
``` Bash
hosts/ken@KENHINT.COM
```
ในทางกลับกัน ข้อมูลรับรอง Kerberos ที่ดูโดยใช้คำสั่ง klist จะอยู่ในรูปแบบนี้
``` Bash
klist
Credentials cache : FILE:tmp/kbr5cc_005
        Principal : manager/KenBrain@KENHINT.COM

Issued              Expires            Principal      
June 15 10:53:34   June 16 10: 53:34  kbrt/KENHINT.COM@KENHINT.COM
```
**5. SSH (เชลล์ปลอดภัย)**

หนึ่งในวิธีที่ได้รับความนิยมมากที่สุดในการเข้าถึงระบบ Linux คือผ่านโปรแกรมเทลเน็ต แต่การเข้าถึงบัญชี Shell โดยใช้ telnet อาจทำให้เกิดอันตรายได้ เนื่องจากการกระทำทั้งหมดของคุณผ่านโปรแกรม telnet ตลอดเซสชันจะยังคงมองเห็นได้เป็นข้อความธรรมดาภายในเครือข่ายของคุณ

SSH มาเป็นโซลูชันที่เข้ารหัสข้อมูลผ่านเครือข่ายท้องถิ่น นอกจากนี้ยังมีตัวเลือกในการส่งเครือข่ายพร้อมทางเลือกในการป้องกันไม่ให้รหัสผ่านรั่วไหลผ่านเครือข่าย ผลลัพธ์ที่ได้คือการกำจัดการดักจับข้อมูลเหนือระบบ และการกำจัดการแอบอ้างเป็นโฮสต์

แม้ว่าจะสามารถมีบทบาทได้หลากหลายบนเครือข่าย แต่ฟังก์ชันหลักของมันคือการเชื่อมต่อกับโฮสต์ระยะไกลสำหรับเซสชันเทอร์มินัล คำสั่งนี้มักใช้

``` Bash
ssh KenBrain@SSHserver.KenHint.com
```

# PAM Configuration File Syntax
ไฟล์ pam.conf มีรายการบริการ แต่ละบริการจะจับคู่กับโมดูลบริการที่เกี่ยวข้อง เมื่อมีการร้องขอบริการ โมดูลที่เกี่ยวข้องจะถูกเรียกใช้ แต่ละรายการมีความยาวได้สูงสุด 256 อักขระรวมจุดสิ้นสุดบรรทัด และต้องเป็นหนึ่งในสองรูปแบบต่อไปนี้:
``` Bash
service_name module_type control_flag module_path options
service_name module_type include path-to-included-PAM-configuration
```
ไฟล์นโยบายต่อบริการใน /etc/pam.d/ มีไวยากรณ์เกือบจะเหมือนกับ pam.conf; อย่างไรก็ตาม มีเพียงสี่ฟิลด์แทนที่จะเป็นห้าฟิลด์ใน pam.conf: ไม่มีฟิลด์ service_name service_name นำมาจากชื่อของไฟล์นโยบายใน /etc/pam.d/ แทน สองรูปแบบที่อนุญาตสำหรับรายการในไฟล์นโยบายต่อบริการคือ:
``` Bash
module_type control_flag module_path options
module_type include path-to-included-PAM-configuration
```
ตัวอย่างของไฟล์การกำหนดค่า pam.conf ที่รองรับการตรวจสอบสิทธิ์ การจัดการบัญชี การจัดการเซสชัน และโมดูลการจัดการรหัสผ่าน:
``` Bash
login   auth requisite          pam_authtok_get.so.1
login   auth required           pam_unix_auth.so.1

other   account requisite       pam_roles.so.1
other   account required        pam_unix_account.so.1

other   session required        pam_unix_session.so.1

other   password requisite      pam_authtok_get.so.1
other   password requisite      pam_authtok_check.so.1
other   password required       pam_authtok_store.so.1
```
การกำหนดค่า PAM ที่เทียบเท่าใน /etc/pam.d/ จะเป็นรายการต่อไปนี้ใน /etc/pam.d/login:
``` Bash
auth requisite	       pam_authtok_get.so.1
auth required	       pam_unix_auth.so.1
```
และรายการต่อไปนี้ใน /etc/pam.d/other:
``` Bash
account requisite       pam_roles.so.1
account required	    pam_unix_account.so.1

session required	    pam_unix_session.so.1

password	requisite      pam_authtok_get.so.1
password	requisite      pam_authtok_check.so.1
password	required       pam_authtok_store.so.1
```
# Integrating Multiple Authentication Services With Stacking
เมื่อมีการกำหนด service_name ของ module_type เดียวกันมากกว่าหนึ่งครั้ง บริการดังกล่าวจะถูกซ้อนกัน แต่ละโมดูลที่อ้างอิงใน module_path สำหรับบริการนั้นจะถูกประมวลผลตามลำดับที่ปรากฏในไฟล์คอนฟิกูเรชัน ฟิลด์ control_flag ระบุซีแมนทิกส์ความต่อเนื่องและความล้มเหลวของโมดูล และสามารถมีค่าใดค่าหนึ่ง

ตัวอย่างไฟล์การกำหนดค่าที่ซ้อนบริการ su, เข้าสู่ระบบและ rlogin.
``` Bash
su     auth required       pam_inhouse.so.1
su     auth requisite      pam_authtok_get.so.1
su     auth required       pam_unix_auth.so.1

login   auth requisite     pam_authtok_get.so.1
login   auth required      pam_unix_auth.so.1
login   auth optional      pam_inhouse.so.1

rlogin  auth sufficient    pam_rhosts_auth.so.1
rlogin  auth requisite     pam_authtok_get.so.1
rlogin  auth required      pam_unix_auth.so.1
```
การกำหนดค่า PAM ที่เทียบเท่าใน /etc/pam.d/ จะเป็นรายการต่อไปนี้ใน /etc/pam.d/su:
``` Bash
auth required	   pam_inhouse.so.1
auth requisite	   pam_authtok_get.so.1
auth required     pam_unix_auth.so.1
```
รายการต่อไปนี้ใน /etc/pam.d/login:
``` Bash
auth requisite    pam_authtok_get.so.1
auth required     pam_unix_auth.so.1
auth optional     pam_inhouse.so.1
```
และรายการต่อไปนี้ใน /etc/pam.d/rlogin:
``` Bash
auth sufficient   pam_rhosts_auth.so.1
auth requisite    pam_authtok_get.so.1
auth required     pam_unix_auth.so.1
```
# Utilities and Files
ชื่อบริการและประเภทโมดูลเฉพาะสำหรับแต่ละบริการควรได้รับการบันทึกไว้ใน man page ของบริการนั้น ตัวอย่างเช่น man page sshd(8) แสดงรายการชื่อบริการ PAM และประเภทโมดูลทั้งหมดสำหรับคำสั่ง sshd

ไฟล์การกำหนดค่า PAM ไม่ได้กำหนดชื่อหรือตำแหน่งของโมดูลเฉพาะบริการ อย่างไรก็ตาม อนุสัญญามีดังต่อไปนี้:
* pam_module_name.so.x : 
ไฟล์ที่ใช้ฟังก์ชันต่างๆ ของบริการตรวจสอบสิทธิ์เฉพาะ ตามที่ระบุชื่อพาธสัมพัทธ์ /usr/lib/security/$ISA จะถูกเติมไว้ข้างหน้า

* /etc/pam.conf : 
ไฟล์การกำหนดค่า PAM แบบดั้งเดิม

* /etc/pam.d/service : 
ไฟล์การกำหนดค่า PAM สำรอง

* /usr/lib/$ISA/libpam.so.1 : 
ไฟล์ที่ใช้ไลบรารีกรอบงาน PAM

#Example
การใช้แฟล็กควบคุมรวม
ตัวอย่างต่อไปนี้รวบรวมโมดูล UNIX ทั่วไปเป็นไฟล์เดียวเพื่อรวมไว้ตามความจำเป็นในตัวอย่างของไฟล์ pam.conf ไฟล์โมดูล UNIX ทั่วไปมีชื่อว่า unix_common และประกอบด้วย:
``` Bash
OTHER   auth requisite          pam_authtok_get.so.1
OTHER   auth required           pam_dhkeys.so.1
OTHER   auth required           pam_unix_auth.so.1
OTHER   auth required           pam_unix_cred.so.1
OTHER   account requisite       pam_roles.so.1
OTHER   account required        pam_unix_account.so.1
OTHER   session required        pam_unix_session.so.1
OTHER   password required       pam_dhkeys.so.1
OTHER   password requisite      pam_authtok_get.so.1
OTHER   password requisite      pam_authtok_check.so.1
OTHER   password required       pam_authtok_store.so.1
```
ไฟล์ pam.conf ประกอบด้วย:
``` Bash
# Authentication management
#
# login service
#
login   auth include            unix_common
#
# rlogin service (explicit because of pam_rhost_auth)
#
rlogin  auth sufficient         pam_rhosts_auth.so.1
rlogin  auth include            unix_common
#
# Default definitions for Authentication management
# Used when service name is not explicitly mentioned
#
OTHER   auth include            unix_common
#
# Default definition for Account management
# Used when service name is not explicitly mentioned
#
OTHER   account include	     unix_common
#
# Default definition for Session management
# Used when service name is not explicitly mentioned
#
OTHER   session include         unix_common
#
# Default definition for  Password management
# Used when service name is not explicitly mentioned
#
OTHER   password include        unix_common
```
การกำหนดค่า PAM ที่เทียบเท่าใน /etc/pam.d/ จะเป็นรายการต่อไปนี้ใน /etc/pam.d/login:
``` Bash
# Authentication        management
#
# login service
#
auth include           unix_common
``` 
รายการต่อไปนี้ใน /etc/pam.d/rlogin:
``` Bash
#
# rlogin        service (explicit because of pam_rhost_auth)
#
auth sufficient        pam_rhosts_auth.so.1
auth include           unix_common
``` 
และรายการต่อไปนี้ใน /etc/pam.d/OTHER:
``` Bash
#
# Default definitions for Authentication management
# Used when service name is not explicitly mentioned
#
auth include           unix_common
#
# Default definition for Account management
# Used when service name is not explicitly mentioned
#
account include      unix_common
#
# Default definition for Session management
# Used when service name is not explicitly mentioned
#
session include        unix_common
#
# Default definition for Password management
# Used when service name is not explicitly mentioned
#
password        include        unix_common
```

/etc/pam.conf จัดส่งในแพ็คเกจ system/core-os

ไฟล์ /etc/pam.d/ จะถูกส่งมาในแพ็คเกจที่มีโมดูลหรือซอฟต์แวร์ที่เกี่ยวข้องกัน


Thank you for refference : https://linuxhint.com/linux-authentication-systems/ , https://docs.oracle.com/cd/E88353_01/html/E37852/pam.d-5.html
