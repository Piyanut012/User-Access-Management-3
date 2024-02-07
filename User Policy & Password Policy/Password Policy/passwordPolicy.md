# บทบาทและหน้าที่บน Linux
Password policy เป็นมาตรการสำคัญในด้าน Security ของระบบ Linux การเข้าถึงข้อมูลโดยไม่ได้รับอนุญาตส่วนใหญ่เกิดจากการ Brute force ธรรมดาหรือใช้ Dictionary attack กับรหัสผ่านที่อ่อนแอ เพื่อป้องกันไม่ให้ผู้ที่ไม่มีสิทธิ์สามารถเข้าถึงระบบได้โดยง่ายและเสี่ยงต่อการถูกโจรกรรมข้อมูล ข้อมูลรั่วไหล และอื่นๆ การกำหนด Password policy ที่ดีจึงมีความสำคัญอย่างมากในด้าน User/access management

# พื้นฐานและหลักการทำงาน
ในระบบปฏิบัติการ Ubuntu มีการกำหนดค่าเริ่มต้นของ Password policy เพียงแค่ความยาวของรหัสผ่านต้องไม่ต่ำกว่า 6 ตัวอักษร และมีการตรวจสอบ Password entropy แบบง่ายเท่านั้น ดังนั้น User จึงจำเป็นต้องกำหนด Password policy ขึ้นมาใหม่ให้เหมาะสม ในที่นี้ จะใช้ Library ของ Pluggable Authentication Module (PAM) ที่ชื่อว่า pam_pwquality สำหรับการตรวจสอบคุณภาพของรหัสผ่าน และกำหนด Requirement ในการตั้งรหัสผ่าน โดยหลังจากติดตั้งสำเร็จแล้ว จะสามารถตั้งค่าต่างๆ สำหรับการกำหนด Password policy ที่จำเป็นได้ เช่น ความยาวที่น้อยที่สุดของรหัสผ่าน จำนวนตัวอักษรพิมพ์ใหญ่และพิมพ์เล็กในรหัสผ่าน การบังคับใช้อักษรพิเศษและตัวเลข เป็นต้น

# การเรียกใช้งานและผลลัพธ์ที่ได้
1. เริ่มจากการติดตั้ง pam_pwquality ด้วยคำสั่ง [sudo] apt-get [-y] install libpam-pwquality ซึ่งควรจะได้ผลลัพธ์ประมาณดังรูป