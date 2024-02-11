# User Authorization with chown

## Understanding User Ownership and Permission in Linux

แต่ละ user ในระบบปฏิบัติการจะมีสิทธิ์และความเป็นเจ้าของอยู่ เพื่อให้มั่นใจว่าไฟล์นั้นปลอดภัย และสามารถจำกัดการเข้าถึงให้ใครบ้างที่จะสามารถดัดแปลงแก้ไขไฟล์นั้น ๆ ของผู้ใช้คนนั้นได้ ใน Linux จะมี user อยู่ 2 ประเภท

* ***Root user*** คือ superuser ที่มีสิทธิ์เข้าถึงได้ทุกไฟล์ทุก directory และสามารถกระทำการใด ๆ กับไฟล์หรือ directory นั้นได้เลย และที่สำคัญคือ root user สามารถเปลี่ยนสิทธิ์หรือความเป็นเจ้าของของไฟล์ที่ไม่ใช่ของตัวเองได้ด้วย
* ***Regular user*** คือผู้ใช้ทั่วไป ซึ่งผู้ใช้เหล่านี้จะถูกจำกัดในการเข้าถึงไฟล์หรือ directory ผู้ใช้กลุ่มนี้สามารถดัดแปลงไฟล์ที่พวกเขาเป็นเจ้าของเท่านั้น

<br><br>

## Ownership and Permissions

เพื่อป้องกันไฟล์และ directory จากการเข้าถึงโดยไม่ได้รับอนุญาต เราใช้ permissions (สิทธิ์) ในการควบคุมว่า user สามารถทำอะไรได้บ้างกับไฟล์หรือ directory นั้น ๆ โดย Linux จะมีสิทธิ์อยู่ 3 อย่างคือ
* ***Read*** สิทธิ์นี้คือการอนุญาตให้ผ user สามารถอ่านไฟล์ใน directory ได้ และสามารถทำให้ user อ่าน directory ย่อยที่เก็บอยู่ใน directory ลงไปได้อีก
* ***Write*** สิทธิ์นี้จะอนุญาตให้ user สร้าง แก้ไขดัดแปลง หรือ ลบไฟล์นั้นทิ้งได้
* ***Execute*** สิทธิ์นี้จะอนุญาตให้ไฟล์นั้นถูก run ได้ เช่น ไฟล์ php.sh ถ้าเราไม่ให้สิทธิ์ execute กับมันมันก็จะไม่สามารถ run ได้

<br><br>

## Type of file Permissions in chown Command in Linux

ประเภทของสิทธิ์ในการเข้าถึงไฟล์จะมีอยู่ 3 กลุ่ม
* ***User*** สิทธิ์ของไฟล์นี้จะส่งผลกระทบกับ user ที่เป็นเจ้าของไฟล์ (Owner)
* ***Group*** สิทธิ์ของไฟล์นี้จะส่งผลกับ group ที่เป็นเจ้าของไฟล์ ถ้า user ที่เป็นเจ้าของไฟล์อยู๋ใน group นี้ด้วยจะยึดจากสิทธิ์ของ user ที่เป็นเจ้าของไฟล์ (User permissions) เป็นหลัก
* ***Other*** สิทธิ์ประเภทนี้ของไฟล์จะส่งผลกับ user คนอื่น ๆ ทั้งหมดในระบบ

<br><br>วิธีการดูสิทธิ์ของไฟล์หรือ directory เราใช้คำสั่ง:

``` Bash
ls -l
```

คำสั่ง ***chown*** นั้นใช้ในการเปลี่ยนเจ้าของไฟล์ หรือ กลุ่ม เมื่อใดที่คุณต้องการเปลี่ยนเจ้าของก็ใช้คำสั่ง ***chown***

<br><br>

## Syntax of chown Command in Linux

Syntax ของคำสั่ง chown มีดังนี้

``` Bash
chown [options] new_owner[:new_group] file(s)
```

* ***chown*** เป็นคำสั่งพื้นฐาน
* ***options*** เป็นส่วนของตัวเลือกซึ่งจะส่งผลกับพฤติกรรมการทำงานของคำสั่ง chown
* ***new_owner[:new_group]*** กำหนดเจ้าของใหม่ และกำหนดกลุ่มใหม่ ซึ่งการกำหนดกลุ่มใหม่เป็น optional หากไม่ระบุไฟล์จะยังอยู่ group เดิมเปลี่ยนแค่เจ้าของ
* ***file(s)*** ระบุไฟล์ที่ต้องการจะเปลี่ยนเจ้าของ

<br><br>

## Options Available in chown Command in Linux

1. -c

ในการใช้ option -c ในคำสั่ง chown นั้นเป็นการให้ chown แจ้งกลับมาด้วยว่าไฟล์นั้นเปลี่ยนเจ้าของเรียบร้อยแล้ว

<br>

***ตัวอย่าง***

``` Bash
chown -c master file1.txt
```

ผลลัพธ์ที่ได้คือเมื่อไฟล์ถูกการเปลี่ยนเจ้าของสำเร็จแล้ว chown จะแสดงข้อความแจ้งเตือนเราด้วย

![Image](../.assets/chown%20-c.png)

<br><br>

2. -v

ในการใช้ option -v จะทำให้ chown command แสดง output ว่า chown กำลังทำงานกับไฟล์ใดอยู่ หรือแม้ไม่ได้กระทำการใดๆกับไฟล์ก็จะแสดง ซึ่งนั่นทำให้ -v ต่างจาก -c คือเราสามารถเห็น feedback การทำงานของคำสั่งของทุกไฟล์ที่เราสั่งให้ chown ไปกระทำ
<br>

***ตัวอย่าง***

``` Bash
chown -v master file1.txt
```

![Image](../.assets/chown%20-v.png)

<br><br>

3. -f

option -f จะส่งผลให้หากมีข้อผิดพลาดใด ๆ ก็ไม่แสดง error และให้ดำเนินการต่อไป ถึงแม้ว่าจะไม่มีไฟล์ที่สั่งให้ไปเปลี่ยนๆ จริงๆ เป็นการบังคับ (force) ให้ทำงานจนจบ

<br>

***ตัวอย่าง***

``` Bash
chown -f master file1.txt
```

<br><br>

## Example to Change File Ownership in Linux

1. Change the Owner

เปลี่ยน user เจ้าของไฟล์ง่ายๆ syntax ดังนี้

``` Bash
chown owner_name file_name
```

<br>

***ตัวอย่าง***

``` Bash
chown master file1.txt
```

<br><br>

2. Change the Group

เปลี่ยน group เจ้าของของไฟล์ได้ดังนี้

<br>

***ตัวอย่าง***

``` Bash
chown :group1 file1.txt
```

เราจะใช้ : แล้วตามด้วยชื่อ group owner ใหม่

<br><br>

3. Change Owner and Group of the File

เปลี่ยน user เจ้าของและ group เจ้าของของไฟล์ได้ดังนี้

<br>

***ตัวอย่าง***

``` Bash
chown master:group1 file1.txt
```

เราจะใช้ : ในการแบ่งระหว่าง user owner กับ group owner

<br><br>

4. Change Owner of Multiple Files

ในการเปลี่ยน user เจ้าของและ group เจ้าของหลาย ๆ ไฟล์พร้อมกัน สามารถทำได้ดังนี้

<br>

***ตัวอย่าง***

``` Bash
chown master:group1 file1.txt file2.txt
```

![Image](../.assets/-v%20multi.png)

เท่านี้เราก็สามารถเปลี่ยน user และ group เจ้าของไฟล์เป็นกันแล้ว!

<br><br>

## References
* [GeeksForGeeks](https://www.geeksforgeeks.org/chown-command-in-linux-with-examples/)