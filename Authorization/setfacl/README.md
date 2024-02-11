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

---

### อ้างอิง
https://www.geeksforgeeks.org/linux-setfacl-command-with-example/

https://www.ibm.com/docs/en/zos/3.1.0?topic=scd-setfacl-set-remove-change-access-control-lists-acls
