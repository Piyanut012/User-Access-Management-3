# Users
 useradd เป็นคำสั่งในระบบปฏิบัติการ Linux ที่ใช้สำหรับการสร้างผู้ใช้ใหม่ในระบบ โดยมักใช้โดยผู้ดูแลระบบ (administrators) เพื่อเพิ่มผู้ใช้ใหม่ในระบบ เมื่อใช้คำสั่ง useradd จะสร้างบัญชีผู้ใช้ใหม่พร้อมกับการกำหนดค่าเริ่มต้นและสร้างโฮมไดเรกทอรีส่วนบุคคล (home directory) สำหรับผู้ใช้นั้นด้วย

ข้อมูลผู้ใช้จะถูกเก็บในไฟล์ต่อไปนี้  
- /etc/passwd
- /etc/group


## การสร้างบัญชีผู้ใช้
  การสร้างและจัดการบัญชีผู้ใช้นั้นมีแค่ root ที่สามารถทำได้ดังนั้นจำเป็นต้องเข้าสู่ระบบในฐานะ root 
โดยใช้ คำสัง sudo -i ใน terminal จากนั้นใส่รหัสผ่าน

ใช้คำสั่ง useradd username เช่น
``` bash
useradd ironman
```
ใช้คำสั่ง 
``` bash
cat /etc/passwd
```
เพื่อตรวจสอบผู้ใช้ที่ถูกสร้าง
![image](https://github.com/Piyanut012/User-Access-Management-3/assets/118871708/1f1c47f4-c584-4f1d-ab2d-426e5bf16ddb)

## การตั้งรหัสผ่านผู้ใช้
ใช้คำสั่ง passwd username เช่น
``` bash
passwd ironman
```
![image](https://github.com/Piyanut012/User-Access-Management-3/assets/118871708/36202829-b376-4fd8-ae42-3a49a0ca2344)

## การลบผู้ใช้
ใช้คำสั่ง userdel username เช่น
``` bash
userdel ironman
```
![image](https://github.com/Piyanut012/User-Access-Management-3/assets/118871708/9b945817-38ec-4a44-940e-531d71ecc947)

## OPTION
``` bash
Options:
      --badnames                do not check for bad names
  -b, --base-dir BASE_DIR       base directory for the home directory of the
                                new account
      --btrfs-subvolume-home    use BTRFS subvolume for home directory
  -c, --comment COMMENT         GECOS field of the new account
  -d, --home-dir HOME_DIR       home directory of the new account
  -D, --defaults                print or change default useradd configuration
  -e, --expiredate EXPIRE_DATE  expiration date of the new account
  -f, --inactive INACTIVE       password inactivity period of the new account
  -g, --gid GROUP               name or ID of the primary group of the new
                                account
  -G, --groups GROUPS           list of supplementary groups of the new
                                account
  -h, --help                    display this help message and exit
  -k, --skel SKEL_DIR           use this alternative skeleton directory
  -K, --key KEY=VALUE           override /etc/login.defs defaults
  -l, --no-log-init             do not add the user to the lastlog and
                                faillog databases
  -m, --create-home             create the user's home directory
  -M, --no-create-home          do not create the user's home directory
  -N, --no-user-group           do not create a group with the same name as
                                the user
  -o, --non-unique              allow to create users with duplicate
                                (non-unique) UID
  -p, --password PASSWORD       encrypted password of the new account
  -r, --system                  create a system account
  -R, --root CHROOT_DIR         directory to chroot into
  -P, --prefix PREFIX_DIR       prefix directory where are located the /etc/* files
  -s, --shell SHELL             login shell of the new account
  -u, --uid UID                 user ID of the new account
  -U, --user-group              create a group with the same name as the user
  -Z, --selinux-user SEUSER     use a specific SEUSER for the SELinux user mapping
      --extrausers              Use the extra users database

```
ตัวอย่างการใช้ OPTION เพิ่มเติม

``` bash
useradd -m -d /home/tony -s /bin/bash -u 3000 ironman
```
- -m: สร้างโฮมไดเร็กทอรีสำหรับผู้ใช้ใหม่
- -d /home/tony: กำหนดโฮมไดเร็กทอรีใหม่เป็น "/home/tony"
- -s /bin/bash: กำหนด Shell ให้เป็น Bash (/bin/bash)
- -u 3000 กำหนด user id เป็น 3000
![image](https://github.com/Piyanut012/User-Access-Management-3/assets/118871708/d85613d3-d5f6-4821-938a-3c44cb4f31c9)


# Group
ใน Linux มีกลุ่มอยู่ 2 ประเภท 
* **Primary group** -> คือ กลุ่มที่ถูกตั้งเป็น default group ของ user เมื่อเข้าสู่ระบบ
ซึ่ง group id จะอยู่ใน fieldที่ 4 ในไฟล์ etc/passwd  และ Directory, file ที่user สร้างจะมี group id นี้อยู่
แต่ละผู้ใช้ในระบบ Linux จะมี Primary Group ที่เป็นกลุ่มหลักของตนเอง มักจะมีชื่อเดียวกับผู้ใช้เองและมีกลุ่ม ID (GID) เฉพาะของตัวเอง
* **Secondary/Supplementary group** -> คือกลุ่มอื่นๆ ที่ userอยู่นอกเหนือจาก Primary group ผู้ใช้ในระบบ Linux สามารถเป็นสมาชิกของ Supplementary group ได้หลายกลุ่ม

ใช้คำสั่ง groups username เพื่อดูกลุ่มของผู้ใช้ เช่น
``` bash
groups ironman
```
![image](https://github.com/Piyanut012/User-Access-Management-3/assets/118871708/323ce652-453e-46be-94ac-54bb67e663e7)

จะเห็นได้ว่าเมื่อผู้ใข้ถูกสร้าง Primary group จะถูกสร้างขึ้นมาพร้อมกันโดยมีชื่อเหมือนกันกับผู้ใช้ 


## การสร้างกลุ่ม
ใช้คำสั่ง groupadd groupname เช่น
``` bash
groupadd Avengers
```
ใช้คำสั่ง cat /etc/group เพื่อตรวจดูรายชื่อกลุ่ม
``` bash
cat /etc/group
```
![image](https://github.com/Piyanut012/User-Access-Management-3/assets/118871708/cc15ac83-6af9-41b7-bf9c-767be2cf245f)

## การเพิ่มผู้ใช้ลงในกลุ่ม
สามารถใช้คำสั่ง usermod 
### เปลี่ยน Primary group ของผู้ใช้
ใช้คำสั่ง usermod -g groupname username
``` bash
usermod -g Avengers ironman
```
![image](https://github.com/Piyanut012/User-Access-Management-3/assets/118871708/3ce7263a-5cbe-416c-913d-cc691c0be465)

![image](https://github.com/Piyanut012/User-Access-Management-3/assets/118871708/c79e974b-0677-4e5e-aeb1-b5878ea2717a) 
จะสังเกตว่า GID ใน field ถูกเปลี่ยนเป็น 3001 ซึ่งตือ GID ของ Avengers

### เปลี่ยน Secondary group ของผู้ใช้
หากผู้ใช้ไม่มี Secondary group จะเป็นการเพิ่มผู้ใช้ลง Secondary group
แต่หากผู้ใช้มี Secondary group อยู่แล้ว จะเป็นการเปลี่ยน Secondary group ของผู้ใช้
ใช้คำสั่ง useradd -G groupname username เช่น
``` bash
 usermod -G guardians ironman
```
![image](https://github.com/Piyanut012/User-Access-Management-3/assets/118871708/a2f5eb9f-3295-4ff6-8394-08d004ba1100)

### เพิ่ม Secondary group ของผู้ใช้
จะเป็นการเพิ่มผู้ใช้ลงกลุ่มต่าง ๆ
ใช้คำสั่ง useradd -aG groupname username เช่น
``` bash
usermod -aG scientist ironman
```
![image](https://github.com/Piyanut012/User-Access-Management-3/assets/118871708/38272506-5224-454c-b3e8-7e443c9a5294)

หรือใช้คำสั่ง gpasswd -a username groupname

### ลบผู้ใช้จาก Secondary group
ใช้คำสั่ง gpasswd -d username groupname เช่น

``` bash
gpasswd -d ironman guardians
```
![image](https://github.com/Piyanut012/User-Access-Management-3/assets/118871708/3aeb5a91-fc73-42f2-8495-52a202e3c243)

### การลบกลุ่ม
ใช้คำสั่ง groupdel groupname เช่น
``` bash
 groupdel guardians
```
![image](https://github.com/Piyanut012/User-Access-Management-3/assets/118871708/f4ba0ab5-80ac-4a73-9a61-ca305bbd4cdd)


# User/Grop Modify
## Usermod
usermod เป็นคำสั่งในระบบปฏิบัติการ Linux ที่ใช้สำหรับการแก้ไขหรือปรับเปลี่ยนค่าและตัวแปรต่างๆ ของผู้ใช้ในระบบ โดยสามารถใช้คำสั่ง usermod เพื่อเปลี่ยนแปลงค่าต่างๆ ของผู้ใช้ เช่น ชื่อผู้ใช้, กลุ่มผู้ใช้, โฮมไดเรกทอรี, หรือตั้งค่ารหัสผ่าน เป็นต้น
### OPTIONS
``` bash
Options:
      --badnames                do not check for bad names
  -b, --base-dir BASE_DIR       base directory for the home directory of the
                                new account
      --btrfs-subvolume-home    use BTRFS subvolume for home directory
  -c, --comment COMMENT         GECOS field of the new account
  -d, --home-dir HOME_DIR       home directory of the new account
  -D, --defaults                print or change default useradd configuration
  -e, --expiredate EXPIRE_DATE  expiration date of the new account
  -f, --inactive INACTIVE       password inactivity period of the new account
  -g, --gid GROUP               name or ID of the primary group of the new
                                account
  -G, --groups GROUPS           list of supplementary groups of the new
                                account
  -h, --help                    display this help message and exit
  -k, --skel SKEL_DIR           use this alternative skeleton directory
  -K, --key KEY=VALUE           override /etc/login.defs defaults
  -l, --no-log-init             do not add the user to the lastlog and
                                faillog databases
  -m, --create-home             create the user's home directory
  -M, --no-create-home          do not create the user's home directory
  -N, --no-user-group           do not create a group with the same name as
                                the user
  -o, --non-unique              allow to create users with duplicate
                                (non-unique) UID
  -p, --password PASSWORD       encrypted password of the new account
  -r, --system                  create a system account
  -R, --root CHROOT_DIR         directory to chroot into
  -P, --prefix PREFIX_DIR       prefix directory where are located the /etc/* files
  -s, --shell SHELL             login shell of the new account
  -u, --uid UID                 user ID of the new account
  -U, --user-group              create a group with the same name as the user
  -Z, --selinux-user SEUSER     use a specific SEUSER for the SELinux user mapping
      --extrausers              Use the extra users database

```
### ตัวอย่างการใช้ usermod

### เพิ่ม comment -c
ใช้คำสั่ง Usermod -c comment username
``` bash
usermod -c "Tony Stark" ironman
```
![image](https://github.com/Piyanut012/User-Access-Management-3/assets/118871708/93f97e4a-0b75-422a-84d9-a40fbfaad76a)

### -L lock user
ใช้คำสั่ง usermod -L username เช่น
``` bash
usermod -L ironman
```
ทดลองเข้าด้วย ironman

![image](https://github.com/Piyanut012/User-Access-Management-3/assets/118871708/605e5331-a8b6-461c-94b2-f0e70a577005)

### -U unlock
ใช้คำสั่ง usermod -U username เช่น
``` bash
usermod -U ironman
```
## Groupmod
groupmod เป็นคำสั่งในระบบปฏิบัติการ Linux ที่ใช้สำหรับการแก้ไขหรือปรับเปลี่ยนค่าและตัวแปรต่างๆ ของกลุ่มในระบบ โดยสามารถใช้คำสั่ง groupmod เพื่อเปลี่ยนแปลงค่าต่างๆ ของกลุ่ม เช่น ชื่อกลุ่มหลัก, ชื่อกลุ่มวัสดุ, หรือ GID (Group ID) เป็นต้น

### OPTION
``` bash
Options:
  -g, --gid GID                 change the group ID to GID
  -h, --help                    display this help message and exit
  -n, --new-name NEW_GROUP      change the name to NEW_GROUP
  -o, --non-unique              allow to use a duplicate (non-unique) GID
  -p, --password PASSWORD       change the password to this (encrypted)
                                PASSWORD
  -R, --root CHROOT_DIR         directory to chroot into
  -P, --prefix PREFIX_DIR       prefix directory where are located the /etc/* files
```

### ตัวอย่างการใช้ groupmod

### เปลี่ยนชื่อกลุ่ม -n

ใช้คำสั่ง groupmod -n newgroupname oldgroupname เช่น
``` bash
groupmod -n newavengers avengers
```
![image](https://github.com/Piyanut012/User-Access-Management-3/assets/118871708/abe1684b-4bfc-4a97-8ff0-b335b2072550)

### เปลี่ยน GID
ใช้คำสั่ง groupmod -g GID groupname เช่น
``` bash
groupmod -n 2000 newavengers
```

![image](https://github.com/Piyanut012/User-Access-Management-3/assets/118871708/e9a50f62-c979-491e-91ae-a925a47c9d0a)



# Reference
* https://saixiii.com/groups-linux-command/
* https://youtu.be/19WOD84JFxA?si=QB8o0eLBt7IaE0U2
* https://youtu.be/iXUHWtgzPMY?si=HJzkgMGCgxuz6R97
* https://drive.google.com/file/d/0B6lwIJrH_EH4bGFqa0ZmSkl5SDA/view?resourcekey=0-S1J8qBHo1OxmGVj3_WnOyA
* https://youtu.be/gzNXittvyZ4?si=2y3tWfhy3NutaBFq
* https://youtu.be/nmqGGGdb5iA?si=b6XYnAHwdxkhrdwG











  



