# บทบาทและหน้าที่บน Linux
setfacl ย่อมาจาก Set File Access Control List ใช้สำหรับการควบคุมรายการการเข้าถึง Files และ Directories ซึ่งมีประโยชน์อย่างมากในการจัดการสิทธิ์การเข้าถึงของ Files ที่ Users และ Groups โดยตรงแทนการกำหนดสิทธิ์ในการเข้าถึงที่ตัว Files เอง

## Access Control List
ACL คือชุดกฎเกณฑ์ของ Files, Directories, อุปกรณ์ด้าน Network และอื่นๆ โดยกฏและสิทธิ์ต่างๆ ที่ Users และ Group ได้รับจะขึ้นอยู่กับ Roles ของ Users และ Group นั้นๆ ซึ่งทั้งหมดนี้จะถูกควบคุมและกำหนดโดย Admin

### ข้อดีของการใช้ setfacl
- Admin สามารถกำหนดการเข้าถึง Files และ Directories ของแต่ละ Users และ Groups ได้แบบเฉาะเจาะจง
- มีความยิดหยุ่นมากกว่าการกำหนดสิทธิ์ Files ทั่วไป เพราะสามารถกำหนดสิทธิ์ได้หลายสิทธิ์พร้อมๆ กัน
- ช่วยในการจัดการสิทธิ์ใดๆ แบบเจาะจงโดยไม่กระทบกับสิทธิ์อื่นๆ
- ยกระดับความปลอดภัยเพราะมีเพียงผู้ที่ได้รับอนุญาตเท่านั้นจึงสามารถเข้าถึง Files และ Directories นั้นๆ ได้
- สามารถเปลี่ยนสิทธิ์ได้ทันทีโดยไม่กระทบกับการทำงานส่วนอื่น

# พื้นฐานและหลักการทำงาน
setfacl จะกำหนด (แทนที่), เปลี่ยนแปลง, หรือลบ ACL จาก Files และ Directories นอกจากนี้ยังสามารถ Update และลบบันทึก ACL สำหรับแต่ละ Files และ Directories ตาม Path ที่ระบุได้ หากไม่ได้ระบุ Path จะรับมาจาก stdin แทน

setfacl จะใช้ได้ก็ต่อเมื่อเป็นเจ้าของ File หรือ Superuser เท่านั้น

# การเรียกใช้งานและผลลัพธ์ที่ได้
| Option       | Description                                                    |
|--------------|----------------------------------------------------------------|
| -m, --modify | For modifying the ACL.                                         |
| -x, --remove | For removing the permission from the ACL.                      |
| -b, --remove-all | For removing all permissions from the ACL.                |
| -d, --default | Apply default permissions to newly created files and folders along the route. |
| -R, --recursive | Recursively apply modifications to all files and directories in the given path. |
| -k, --remove-default | Remove a file or directory’s default entry from the ACL. |
| -n, --no-mask | Recalculating the effective rights mask using ACL entries is not permitted. |
| -m, --mask | For specifying the effective right mask for modifying ACL. |
| -M, --restore=file | For restoring ACL from a specific file. |
| -set file | For applying the permission to specific files or directories. |

file_owner: file-owner มีอยู่ 3 ประเภท:
| Type | Description                                             |
|------|---------------------------------------------------------|
| 'u'  | Specify the name of the User/Owner for configuring the ACL |
| 'g'  | Specify the name of the Group for configuring the ACL  |
| 'o'  | Specify the name of Other for configuring the ACL      |

file_permission: file-permission มีอยู่ 3 ประเภท:
| Type | Description                                       |
|------|---------------------------------------------------|
| 'r'  | For read, it will allow the user to access the file. |
| 'w'  | For writing, it will allow the user to make modifications or changes in the file. |
| 'x'  | For execution, it will allow the user to execute or run the file. |

# ตัวอย่าง
```bash
getfacl myfile.txt
```
## ขั้นตอนที่ 1: กำหนดสิทธิ์ในการเข้าถึงไฟล์สำหรับผู้ใช้บนไฟล์ที่ระบุ

ใช้เพื่อกำหนดสิทธิ์การเข้าถึงบนไฟล์หนึ่งหรือมากกว่าหนึ่งไฟล์โดยใช้ประเภทผู้ใช้ (ผู้ใช้, กลุ่ม, ผู้ใช้อื่นๆ) สามารถกำหนดผู้ใช้หลายคนสำหรับไฟล์เดียวกันได้:
```bash
setfacl -m u:kali:rw gfg.txt
setfacl -m u:kali:rw gfg.txt
```
![image](https://github.com/Piyanut012/User-Access-Management-3/assets/118057837/f052ed9c-8146-41b1-bd56-16fbcd6c56a3)

## ขั้นตอนที่ 2: กำหนดสิทธิ์ให้แก่ผู้ใช้สำหรับไฟล์และไดเรกทอรีหลายๆ รายการ

เนื่องจากเรามีข้อดีของคำสั่ง setfacl เราสามารถกำหนดสิทธิ์บนไฟล์และไดเรกทอรีหลายๆ รายการพร้อมกัน

```bash
setfacl -m u:kali:rx f1.txt f2.txt d1
setfacl2
```
![image](https://github.com/Piyanut012/User-Access-Management-3/assets/118057837/b4c72084-02a7-4493-bd1a-5214ac58cccd)


## ขั้นตอนที่ 3: ปฏิเสธสิทธิ์ทั้งหมดบนไดเรกทอรีที่ระบุ

เราสามารถลบสิทธิ์ ACL โดยใช้ตัวเลือก (-x) พร้อมกับการระบุประเภทผู้ใช้และชื่อไฟล์หรือไดเรกทอรี
```bash
setfacl -x u:kali d1
setfacl3
```
![image](https://github.com/Piyanut012/User-Access-Management-3/assets/118057837/696477a7-2a1e-48aa-819b-5f91fc83b8ea)
## ขั้นตอนที่ 4: แสดงรายการควบคุมการเข้าถึงไฟล์

ใช้เพื่อแสดงรายละเอียดของ ACL บนไฟล์หรือไดเรกทอรีที่ระบุ ประกอบด้วยข้อมูลเช่น ชื่อไฟล์, เจ้าของและชื่อกลุ่ม, สิทธิ์ไฟล์, และ umask
```bash
getfacl -a f2.txt
aagetfacl1
```
![image](https://github.com/Piyanut012/User-Access-Management-3/assets/118057837/d032fedd-f455-4b55-b7ba-f731f9e8e4cb)


## ขั้นตอนที่ 5: แสดงรายการควบคุมการเข้าถึงเริ่มต้น

ใช้เพื่อแสดงข้อมูลพื้นฐานเช่น ชื่อไฟล์ และ ชื่อเจ้าของ/กลุ่ม
```bash
getfacl -d f2.txt
```
![image](https://github.com/Piyanut012/User-Access-Management-3/assets/118057837/53c573cd-2c36-49de-99bb-49a7be96af05)

---

### อ้างอิง
https://www.geeksforgeeks.org/linux-setfacl-command-with-example/

https://www.ibm.com/docs/en/zos/3.1.0?topic=scd-setfacl-set-remove-change-access-control-lists-acls
