# The nsswitch.conf File
ไฟล์ nsswitch.conf Default ที่ติดตั้งเมื่อคุณติดตั้งสภาพแวดล้อมการทำงาน Solaris เป็นครั้งแรกจะถูกกำหนดโดยเซอร์วิสการตั้งชื่อที่คุณเลือกระหว่างกระบวนการติดตั้งซอฟต์แวร์ Solaris แต่ละบรรทัดของไฟล์จะระบุข้อมูลเครือข่ายประเภทใดประเภทหนึ่ง เช่น โฮสต์ รหัสผ่าน และกลุ่ม ตามด้วยแหล่งข้อมูลตั้งแต่หนึ่งแหล่งขึ้นไป เช่น ตาราง NIS+, แผนที่ NIS, ตารางโฮสต์ DNS หรือ /etc ในเครื่อง โดยที่ไคลเอ็นต์ คือการหาข้อมูลนั้น เมื่อคุณเลือกบริการตั้งชื่อ ไฟล์เทมเพลตสวิตช์สำหรับบริการนั้นจะถูกคัดลอกเพื่อสร้างไฟล์ nsswitch.conf ใหม่ ตัวอย่างเช่น หากคุณเลือก NIS+ ไฟล์ nsswitch.nisplus จะถูกคัดลอกเพื่อสร้างไฟล์ nsswitch.conf ใหม่

ตัวอย่าง /etc/nsswitch.conf

![image](https://github.com/Piyanut012/User-Access-Management-3/assets/109953139/95702ce5-d33d-40a2-80dc-da0798e72d49)


ไฟล์ /etc/nsswitch.conf จะถูกโหลดโดยอัตโนมัติไปยังไดเร็กทอรี /etc ของเครื่องทุกเครื่องโดยซอฟต์แวร์ Solaris 9release พร้อมด้วยเวอร์ชันสำรอง (เทมเพลต) ต่อไปนี้
* **/etc/nsswitch.nisplus**
* **/etc/nsswitch.nis**
* **/etc/nsswitch.files**
* **/etc/nsswitch.ldap**

ไฟล์เทมเพลตสำรองเหล่านี้มีการกำหนดค่าสวิตช์เริ่มต้นที่ใช้โดยบริการ NIS+ และ NIS ไฟล์ในเครื่อง และ LDAP ไม่มีไฟล์เริ่มต้นสำหรับ DNS แต่คุณสามารถแก้ไขไฟล์เหล่านี้เพื่อใช้ DNS ได้ ดูบทที่ 5 การดูแลระบบ DNS (ข้อมูลอ้างอิง) เมื่อมีการติดตั้งสภาพแวดล้อมการทำงาน Solaris บนเครื่องเป็นครั้งแรก โปรแกรมติดตั้งจะเลือกบริการตั้งชื่อเริ่มต้นของเครื่อง: NIS+, NIS, ไฟล์ในเครื่อง หรือ LDAP ระหว่างการติดตั้ง ไฟล์เทมเพลตที่เกี่ยวข้องจะถูกคัดลอกไปยัง /etc/nsswitch.conf ตัวอย่างเช่น สำหรับเครื่องไคลเอ็นต์ที่ใช้ NIS+ กระบวนการติดตั้งจะคัดลอก nsswitch.nisplus ไปยัง nsswitch.conf

# THE NAME SERVICE SWITCH 
**THE NAME SERVICE SWITCH** เป็นไฟล์ชื่อ nsswitch.conf ควบคุมวิธีที่เครื่องไคลเอนต์หรือแอพพลิเคชั่นรับข้อมูลเครือข่าย มันถูกใช้โดยแอปพลิเคชันไคลเอนต์ที่เรียกใช้อินเทอร์เฟซ getXbyY() ดังต่อไปนี้
* **gethostbyname()**
* **เก็ทพวุด()**
* **เก็ทพวันนัม()**
* **getipnodebyname()**
  
แต่ละเครื่องมีไฟล์สวิตช์อยู่ในไดเร็กทอรี /etc แต่ละบรรทัดของไฟล์จะระบุข้อมูลเครือข่ายประเภทใดประเภทหนึ่ง เช่น โฮสต์ รหัสผ่าน และกลุ่ม ตามด้วยแหล่งข้อมูลตั้งแต่หนึ่งแหล่งขึ้นไปที่ไคลเอ็นต์จะค้นหาข้อมูลนั้น

ไคลเอนต์สามารถรับข้อมูลการตั้งชื่อจากแหล่งที่มาของสวิตช์ตั้งแต่หนึ่งแหล่งขึ้นไป ตัวอย่างเช่น ไคลเอ็นต์ NIS+ สามารถรับข้อมูลโฮสต์จากตาราง NIS+ และข้อมูลรหัสผ่านจากไฟล์ /etc ในเครื่อง นอกจากนี้ยังสามารถระบุเงื่อนไขที่สวิตช์ต้องใช้แต่ละแหล่งได้ ดูตารางที่ 2–1

สภาพแวดล้อมการทำงานของ Solaris จะโหลดไฟล์ nsswitch.conf ลงในไดเร็กทอรี /etc ของเครื่องทุกเครื่องโดยอัตโนมัติโดยเป็นส่วนหนึ่งของกระบวนการติดตั้ง ไฟล์สวิตช์เวอร์ชันสำรอง (เทมเพลต) สี่เวอร์ชันจะถูกโหลดลงใน /etc สำหรับ LDAP, NIS, NIS+ หรือไฟล์ด้วย ดูไฟล์เทมเพลต nsswitch.conf

ไฟล์ทั้งสี่นี้เป็นไฟล์สวิตช์เริ่มต้นสำรอง แต่ละบริการได้รับการออกแบบมาสำหรับบริการตั้งชื่อหลักที่แตกต่างกัน: ไฟล์ /etc, NIS, NIS+ หรือ LDAP เมื่อติดตั้งซอฟต์แวร์ Solaris บนเครื่องเป็นครั้งแรก โปรแกรมติดตั้งจะเลือกบริการตั้งชื่อเริ่มต้นของเครื่อง: NIS+, NIS, ไฟล์ในเครื่อง หรือ LDAP ระหว่างการติดตั้ง ไฟล์เทมเพลตที่เกี่ยวข้องจะถูกคัดลอกไปยัง nsswitch.conf ตัวอย่างเช่น สำหรับเครื่องไคลเอ็นต์ที่ใช้ LDAP กระบวนการติดตั้งจะคัดลอก nsswitch.ldap ไปยัง nsswitch.conf เว้นแต่ว่าคุณมีเนมสเปซที่ผิดปกติ ไฟล์เทมเพลตเริ่มต้นที่คัดลอกไปยัง nsswitch.conf ควรจะเพียงพอสำหรับการทำงานปกติ

ไม่มีไฟล์ Default สำหรับ DNS หรือ IPv6 แต่คุณสามารถแก้ไขไฟล์เหล่านี้เพื่อใช้ DNS หรือ IPv6 ได้ ดู DNS และการเข้าถึงอินเทอร์เน็ต หรือ IPv6 และ Solaris Naming Services

หากคุณเปลี่ยนบริการตั้งชื่อหลักของเครื่องในภายหลัง คุณจะคัดลอกไฟล์สวิตช์สำรองที่เหมาะสมไปที่ nsswitch.conf ดูไฟล์เทมเพลต nsswitch.conf คุณยังสามารถเปลี่ยนแหล่งที่มาของข้อมูลเครือข่ายบางประเภทที่ไคลเอนต์ใช้โดยแก้ไขบรรทัดที่เหมาะสมของไฟล์ /etc/nsswitch.conf ไวยากรณ์สำหรับการทำเช่นนี้อธิบายไว้ด้านล่าง และมีคำแนะนำเพิ่มเติมในการแก้ไขสวิตช์บริการชื่อ

# Format of the nsswitch.conf File

ไฟล์ nsswitch.conf โดยพื้นฐานแล้วคือรายการข้อมูล 16 ประเภท และแหล่งที่มาที่รูทีน getXXbyYY() ค้นหาข้อมูลนั้น ข้อมูลทั้ง 16 ประเภทที่ไม่จำเป็นต้องเรียงลำดับมีดังต่อไปนี้
* **aliases** : 
* **bootparams**
* **ethers**
* **group**
* **hosts**
* **ipnodes**
* **netgroup**
* **netmasks**
* **networks**
* **passwd (includes shadow information)**
* **protocols**
* **publickey**
* **rpc**
* **services**
* **automount**
* **sendmailvars**

ตารางต่อไปนี้แสดงคำอธิบายประเภทของแหล่งข้อมูลที่สามารถแสดงรายการในไฟล์สวิตช์สำหรับประเภทข้อมูลข้างต้น
Information Sources  | Description
------- | ------- |
files|ไฟล์ที่จัดเก็บไว้ในไดเร็กทอรี /etc ของไคลเอ็นต์ ตัวอย่างเช่น /etc/passwd|
nisplus|ตาราง NIS+ เช่น ตาราง hosts|
nis|Map NIS+ เช่น Map hosts|
compat|ใช้สำหรับข้อมูลรหัสผ่านและกลุ่มเพื่อรองรับไวยากรณ์ + หรือ - แบบเก่าในไฟล์ /etc/passwd, /etc/shadow และ /etc/group|
dns|สามารถใช้เพื่อระบุข้อมูลโฮสต์ที่ได้รับจาก DNS|
ldap|สามารถใช้เพื่อระบุรายการที่ได้รับจากไดเร็กทอรี LDAP|

# Search Criteria

**Single Source** : หากประเภทข้อมูลมีแหล่งเดียว เช่น nisplus รูทีนโดยใช้สวิตช์ค้นหาข้อมูลในแหล่งนั้นเท่านั้น หากพบข้อมูลก็จะส่งคืนข้อความสถานะความสำเร็จ หากไม่พบข้อมูล ระบบจะหยุดการค้นหาและส่งคืนข้อความสถานะอื่น สิ่งที่กิจวัตรทำกับข้อความสถานะจะแตกต่างกันไปในแต่ละกิจวัตร

**Multiple Sources** : หากตารางมีแหล่งข้อมูลมากกว่าหนึ่งแหล่งสำหรับประเภทข้อมูลที่กำหนด สวิตช์จะสั่งให้รูทีนเริ่มค้นหาข้อมูลในแหล่งข้อมูลแรกที่แสดง หากพบข้อมูลก็จะส่งคืนข้อความสถานะความสำเร็จ หากไม่พบข้อมูลในแหล่งแรก ระบบจะพยายามค้นหาแหล่งถัดไป รูทีนจะค้นหาแหล่งที่มาทั้งหมดจนกว่าจะพบข้อมูลที่ต้องการ หรือหยุดโดยพบข้อกำหนดการส่งคืน หากแหล่งที่มาทั้งหมดในรายการถูกค้นหาโดยไม่พบข้อมูล รูทีนจะหยุดการค้นหาและส่งกลับข้อความสถานะที่ไม่สำเร็จ
# Switch Status Messages
หากรูทีนพบข้อมูล ก็จะส่งคืนข้อความสถานะความสำเร็จ หากไม่พบข้อมูลที่ต้องการ ระบบจะแสดงข้อความสถานะที่ไม่สำเร็จหนึ่งในสามข้อความ ขึ้นอยู่กับสาเหตุที่ไม่พบข้อมูล ข้อความสถานะที่เป็นไปได้แสดงอยู่ในตารางต่อไปนี้
Status Message   | Meaning of Message 
------- | ------- |
SUCCESS|พบรายการที่ร้องขอในแหล่งที่ระบุ|
UNAVAIL|แหล่งที่มาไม่ตอบสนองหรือไม่พร้อมใช้งาน นั่นคือ ไม่พบหรือเข้าถึงตาราง NIS+ หรือแม็พ NIS หรือไฟล์ /etc|
NOTFOUNDแหล่งข่าวตอบกลับว่า "ไม่มีรายการดังกล่าว" กล่าวอีกนัยหนึ่ง มีการเข้าถึงตาราง แผนที่ หรือไฟล์ แต่ไม่มีข้อมูลที่จำเป็น|
TRYAGAIN|แหล่งที่มาไม่ว่าง มันอาจจะตอบสนองในครั้งต่อไป กล่าวอีกนัยหนึ่ง พบตาราง แผนที่ หรือไฟล์ แต่ไม่สามารถตอบสนองต่อแบบสอบถามได้|

# The nsswitch.conf Template Files

ไฟล์เทมเพลต nsswitch.conf(4) สี่ไฟล์มาพร้อมกับสภาพแวดล้อมการทำงาน Solaris เพื่อรองรับบริการการตั้งชื่อที่แตกต่างกัน แต่ละแหล่งข้อมูลจะมีชุดเริ่มต้นที่แตกต่างกันของแหล่งข้อมูลหลักและแหล่งข้อมูลที่ตามมา

ไฟล์เทมเพลตทั้งสี่ไฟล์มีดังต่อไปนี้
* **LDAP template file.** : ไฟล์คอนฟิกูเรชัน nsswitch.ldap ระบุไดเร็กทอรี LDAP เป็นแหล่งข้อมูลหลักสำหรับเครื่อง
* **NIS+ template file.** : ไฟล์คอนฟิกูเรชัน nsswitch.nisplus ระบุ NIS+ เป็นแหล่งหลักสำหรับข้อมูลทั้งหมด ยกเว้น passwd, กลุ่ม, automount และนามแฝง สำหรับสี่ไฟล์นั้น แหล่งที่มาหลักคือไฟล์ /etc ในเครื่อง และแหล่งที่มารองคือตาราง NIS+ เกณฑ์การค้นหา [NOTFOUND=return] จะสั่งให้สวิตช์หยุดการค้นหาตาราง NIS+ หากได้รับข้อความ "No such entry" จากตารางเหล่านั้น โดยจะค้นหาผ่านไฟล์ในเครื่องเฉพาะในกรณีที่เซิร์ฟเวอร์ NIS+ ไม่พร้อมใช้งาน
* **NIS template file.** : ไฟล์คอนฟิกูเรชัน nsswitch.nis เกือบจะเหมือนกันกับไฟล์คอนฟิกูเรชัน NIS+ ยกเว้นว่าระบุแมป NIS แทนที่ตาราง NIS+ เนื่องจากลำดับการค้นหาสำหรับ passwd และกลุ่มคือไฟล์ nis คุณจึงไม่จำเป็นต้องใส่รายการ + ในไฟล์ /etc/passwd และ /etc/group
* **Files template file.** : ไฟล์คอนฟิกูเรชัน nsswitch.files ระบุไฟล์ /etc ในเครื่องเป็นแหล่งข้อมูลเดียวสำหรับเครื่อง ไม่มีแหล่ง "ไฟล์" สำหรับ netgroup ดังนั้นไคลเอนต์จะไม่ใช้รายการนั้นในไฟล์สวิตช์

# The Default Switch Template Files

ตัวอย่าง NIS+ Switch File Template (nsswitch.nisplus)

``` Bash
#
#
# /etc/nsswitch.nisplus:
#
#
# An example file that could be copied over to /etc/nsswitch.conf;
# it uses NIS+ (NIS Version 3) in conjunction with files.
#
# "hosts:" and "services:" in this file are used only if the
# /etc/netconfig file has a "-" for nametoaddr_libs of "inet"
# transports.
 
# the following two lines obviate the "+" entry in /etc/passwd 
# and /etc/group.
passwd: files nisplus
group: files nisplus
# consult /etc "files" only if nisplus is down. 
hosts: nisplus [NOTFOUND=return] files
# Uncomment the following line, and comment out the above, to use 
# both DNS and NIS+. You must also set up the /etc/resolv.conf 
# file for DNS name server lookup. See resolv.conf(4).
# hosts: nisplus dns [NOTFOUND=return] files
services: nisplus [NOTFOUND=return] files
networks: nisplus [NOTFOUND=return] files
protocols: nisplus [NOTFOUND=return] files
rpc: nisplus [NOTFOUND=return] files
ethers: nisplus [NOTFOUND=return] files
netmasks: nisplus [NOTFOUND=return] files	
bootparams: nisplus [NOTFOUND=return] files
publickey: nisplus
netgroup: nisplus
automount: files nisplus
aliases: files nisplus
sendmailvars: files nisplus
```
ตัวอย่าง NIS Switch File Template
``` Bash
#
# /etc/nsswitch.nis:
#
# An example file that could be copied over to /etc/nsswitch.conf;
# it uses NIS (YP) in conjunction with files.
#
# "hosts:" and "services:" in this file are used only if the
# /etc/netconfig file has a "-" for nametoaddr_libs of "inet"
# transports.
#
# the following two lines obviate the "+" entry in /etc/passwd
# and /etc/group.
passwd: files nis
group: files nis
# consult /etc "files" only if nis is down. 
hosts: nis [NOTFOUND=return] files
networks: nis [NOTFOUND=return] files
protocols: nis [NOTFOUND=return] files
rpc: nis [NOTFOUND=return] files
ethers: nis [NOTFOUND=return] files
netmasks: nis [NOTFOUND=return] files	
bootparams: nis [NOTFOUND=return] files
publickey: nis [NOTFOUND=return] files
netgroup: nis
automount: files nis
aliases: files nis
# for efficient getservbyname() avoid nis
services: files nis
sendmailvars: files
```
ตัวอย่าง Files Switch File Template
``` Bash
#
# /etc/nsswitch.files:
#
# An example file that could be copied over to /etc/nsswitch.conf;
# it does not use any naming service.
#
# "hosts:" and "services:" in this file are used only if the
# /etc/netconfig file has a "-" for nametoaddr_libs of "inet"
# transports.
passwd: files
group: files
hosts: files
networks: files
protocols: files
rpc: files
ethers: files
netmasks: files	
bootparams: files
publickey: files
# At present there isn't a 'files' backend for netgroup;
# the system will figure it out pretty quickly, and will notuse
# netgroups at all.
netgroup: files
automount: files
aliases: files
services: files
sendmailvars: files
```
ตัวอย่าง LDAP Switch File Template
``` Bash
#
# /etc/nsswitch.ldap:
#
# An example file that could be copied over to /etc/nsswitch.conf; it
# uses LDAP in conjunction with files.
#
# "hosts:" and "services:" in this file are used only if the
# /etc/netconfig file has a "-" for nametoaddr_libs of "inet" transports.

# the following two lines obviate the "+" entry in /etc/passwd 
and /etc/group.
passwd:     files ldap
group:      files ldap

hosts:      ldap [NOTFOUND=return] files

networks:   ldap [NOTFOUND=return] files
protocols:  ldap [NOTFOUND=return] files
rpc:        ldap [NOTFOUND=return] files
ethers:     ldap [NOTFOUND=return] files
netmasks:   ldap [NOTFOUND=return] files
bootparams: ldap [NOTFOUND=return] files
publickey:  ldap [NOTFOUND=return] files

netgroup:   ldap

automount:  files ldap
aliases:    files ldap

# for efficient getservbyname() avoid ldap
services:   files ldap
sendmailvars:   files
```

Thank you for refference : https://docs.oracle.com/cd/E19683-01/806-4077/6jd6blbbb/index.html
