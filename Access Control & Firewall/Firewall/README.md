# Firewall
 Firewall คือระบบรักษาความปลอดภัยที่ปกป้องระบบของเราจากการรับส่งข้อมูลที่ไม่ต้องการ และการเข้าถึงโดยไม่ได้รับอนุญาต

## หลักการทำงาน
Firewall สามารถตรวจสอบและควบคุมการรับส่งข้อมูล network สามารถใช้เพื่อ block การเข้าถึง เช่น ที่อยู่ IP, Subnet เป็นต้น ซึ่ง
_หัวใจของความปลอดภัยขึ้นอยู่กับการกำหนดค่า Firewall_

## แนวทางการกำหนดค่า Firewall ที่ดี
* **เข้าใจนโยายความปลอดภัย** -> นโบายที่ชัดเจนในการกำหนดข้อมูลและบริการที่ต้องการป้องกัน จากภัยคุกคามต่างๆ block อะไรและจากใคร
* **การตั้งค่านโยบายเริ่มต้น** -> ให้ใช้จุดยืนที่มีความเสี่ยงน้อยที่สุด แนวทางปฏิบัติที่ดีที่สุดคือการปฏิเสธทั้งหมดโดยค่าเริ่มต้น และเปิดช่องทางการเข้าถึงเมื่อจำเป็นเท่านั้น คือสถาปัตยกรรมแบบ **Zero-Trust**
* **การจัดการ incoming และ outgoing traffic** -> เน้นไปที่การขัดขวางและตรวจสอบการรับส่งข้อมูลขาเข้า มีการตรวจสอบการรับส่งข้อมูลขาออก
* **การกำหนดค่ากฎเฉพาะบริการ** -> มีการปรับแต่ง Firewall ให้รองรับไม่ว่าจะเป็น SSH, HTTP หรือ FTP
* **ตรวจสอบและอัพเดต Firewall เป็นประจำ** -> ภัยคุมคามมีการพัฒนาตลอดเวลา การป้องกันก็ต้องอัพเดตเช่นกัน

## UFW (Uncomplicated Firewall)
![image](https://github.com/Piyanut012/User-Access-Management-3/assets/110012203/d977a13d-9caf-462f-9a3d-129324545769)

UFW คือ เครื่องมือสำหรับกำหนดค่า Firewall เริ่มต้น โดยมีการพัฒนาให้กำหนดค่าของ **iptables** ง่ายขึ้น มีวิธีการใช้งานที่ง่ายในการสร้าง Firewall บน host IPv4 หรือ IPv6

> ufw ไม่ได้มีจุดมุ่งหมายเพื่อให้ฟังก์ชันการทำงานของไฟร์วอลล์สมบูรณ์ผ่านทางอินเทอร์เฟซคำสั่ง แต่ให้วิธีง่ายๆ ในการเพิ่มหรือลบกฎง่ายๆ แทน ปัจจุบันส่วนใหญ่จะใช้สำหรับไฟร์วอลล์บนโฮสต์

## คำสั่งและการใช้ UFW
* ในกรณีที่ไม่มี UFW ใช้คำสั่งติดตั้ง

``` bash
sudo apt install ufw
```

* การตรวสอบสถานะ UFW

``` bash
sudo ufw status verbose
```

![image](https://github.com/Piyanut012/User-Access-Management-3/assets/110012203/79afdabd-6e84-442b-9af2-5737344cdfeb)

เห็นว่าสถานะ inactive คือยังไม่ทำงาน จากนั้นลองเข้า SSH

![image](https://github.com/Piyanut012/User-Access-Management-3/assets/110012203/022ceb9b-dcd3-465b-961c-784dbe6f3db8)

> เข้าได้ปกติ

* ใช้คำสั่งเปิดใช้งาน UFW แล้วเช็คสถานะ

``` bash
sudo ufw enable
```
> คำสั่งปิดใช้ disable

![image](https://github.com/Piyanut012/User-Access-Management-3/assets/110012203/df49caf8-e624-4ff0-81e4-5d681a3cc214)

หลังจากนั้นลองเข้า SSH ใหม่อีกครั้ง 

![image](https://github.com/Piyanut012/User-Access-Management-3/assets/110012203/b99b0935-20cb-4f68-bbdc-78e6414ce2d6)

> สังเกตว่าไม่สามารถเข้าได้ เนื่องจากนโยบายเริ่มต้นของ Firewall ที่กล่าวไปข้างต้น คือจะปฏิเสธทุกการเชื่อมต่อขาเข้าและอนุญาตเฉพาะการเชื่อมต่อขาออกทั้งหมดไปยังเซิร์ฟเวอร์ ซึ่งหมายความว่าจะไม่มีใครสามารถเข้าถึงเซิร์ฟเวอร์ของคุณได้ เว้นแต่คุณจะเปิดพอร์ตไว้โดยเฉพาะ ในขณะที่บริการหรือแอปพลิเคชันที่ทำงานอยู่ทั้งหมดบนเซิร์ฟเวอร์ของคุณสามารถเข้าถึงเครือข่ายภายนอกได้ โดยที่นโยบายจะเก็บไว้ที่ ```/etc/default/ufw```

* การสร้างกฎมีแค่ allow กับ deny

ตัวอย่างการอนุญาติ SSH 

``` bash
sudo ufw allow ssh
```

ตรวจสอบกฎที่เพิ่มมา

![image](https://github.com/Piyanut012/User-Access-Management-3/assets/110012203/7e6221fa-43cb-4ea9-b67e-61ae5b6bf191)

> สามารถเข้า SSH ได้แล้ว

![image](https://github.com/Piyanut012/User-Access-Management-3/assets/110012203/7a7e76c3-0c5b-40eb-b408-1e50e9549eff)


ตัวอย่างการปฏิเสธ port 22 แบบ tcp (SSH ที่ใช้อยู่)

``` bash
sudo ufw deny 22/tcp
```

ตรวจสอบกฎที่อัพเดต

![image](https://github.com/Piyanut012/User-Access-Management-3/assets/110012203/68024b9f-6685-462a-9781-49e97951c050)

> เข้าไม่ได้แล้ว

![image](https://github.com/Piyanut012/User-Access-Management-3/assets/110012203/cad524e9-a515-449b-ae8f-da4cb71b93fd)

_คำสั่งการเพิ่มกฎแบบอื่นๆ_

``` bash
sudo ufw deny from 8.8.8.8
```
``` bash
sudo ufw allow  from 172.16.0.0/24 to any port 22 proto tcp
```

* การแทรกกฏ

กฏปัจจุบัน

![image](https://github.com/Piyanut012/User-Access-Management-3/assets/110012203/f01b9ebd-7657-4095-b7c1-3b825a4a3900)

คำสั่ง

``` bash
sudo ufw insert 2 allow https
```

![Screenshot 2024-02-09 023802](https://github.com/Piyanut012/User-Access-Management-3/assets/110012203/7d5e53b3-70ef-4b92-90a6-8914bf991305)

* การลบกฏ

``` bash
sudo ufw delete deny from 8.8.8.8
```

![image](https://github.com/Piyanut012/User-Access-Management-3/assets/110012203/087cf805-55db-4258-8129-b90b027c56e8)

> 8.8.8.8 หายไปแล้ว

สามารถลบแบบบรรทัดได้

``` bash
sudo ufw delete 2
```

![image](https://github.com/Piyanut012/User-Access-Management-3/assets/110012203/87c8a376-f756-4bd3-8638-4011951eb7ad)

> https หายไปแล้ว

* การ reset

``` bash
sudo ufw reset
```

![image](https://github.com/Piyanut012/User-Access-Management-3/assets/110012203/cb69a59b-1f75-43bc-b5ad-d0cee138f711)

## Reference
* [Security - Firewall | Ubuntu](https://ubuntu.com/server/docs/security-firewallubuntu.com)
* [How to Configure a Linux Firewall: Complete Guide | NinjaOne](https://www.ninjaone.com/blog/how-to-configure-a-linux-firewall/www.ninjaone.com)
* [UFW - Community Help Wiki](https://help.ubuntu.com/community/UFWhelp.ubuntu.com)
* [ใช้งาน firewall บน ubuntu แบบง่ายๆ ด้วย ufw | by Titipat | Medium](https://titipat.medium.com/%E0%B9%83%E0%B8%8A%E0%B9%89%E0%B8%87%E0%B8%B2%E0%B8%99-firewall-%E0%B8%9A%E0%B8%99-ubuntu-%E0%B9%81%E0%B8%9A%E0%B8%9A%E0%B8%87%E0%B9%88%E0%B8%B2%E0%B8%A2%E0%B9%86-%E0%B8%94%E0%B9%89%E0%B8%A7%E0%B8%A2-ufw-9f39119a094btitipat.medium.com)
* [วิธีตั้งค่า UFW Firewall บน Ubuntu และ Debian](https://th.linux-console.net/?p=284th.linux-console.net)































  


