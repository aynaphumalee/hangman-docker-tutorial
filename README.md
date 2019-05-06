# hangman-docker-tutorial
###  อันนี้สำหรับ Client ที่ไม่ได้ทำเป็น GUI 


ก่อนอื่นไปอ่านนี้มาก่อน ว่า docker, image, container คืออะไร [Link](https://www.hostpacific.com/using-docker-on-centos7/)

1. ก่อนอื่นสร้าง โฟร์เดอร์ ขึ้นมาก่อน สมมตชื่อ `hangman-project`

2. จากนั้นก็อปไฟล์ java ทั้งหมดไปไว้ใน โฟร์เดอร์นั้น

3. จากนั้นสร้างไฟล์ชื่อ `Dockerfile-server` ขึ้นมา ไฟล์นี้จะเอาไว้สร้าง image ของ server

4. จากนั้น เปิด `Dockerfile-server` ขึ้นพิมพ์ตามนี้

    	FROM openjdk:12
        COPY . /usr/src/hangman-server/
        WORKDIR /usr/src/hangman-server/
        RUN javac -encoding UTF-8 *.java
        CMD ["java", "Server"]
		
5. `CMD ["java", "Server"]` บรรทัดนี้แก้ **"Server"** เป็นชื่อ class ที่ะรัน
![](https://i.imgur.com/9Agw4eX.png)

6. จากนั้น รันคำสั่ง `docker build -f Dockerfile-server -t hangman-server-image .`
![](https://i.imgur.com/cb6nyZq.png)

7. จากนั้น รันคำสั่ง `docker run -dit --name hangman-server hangman-server-image`

8. จากนั้น `docker ps` ดูจะเห็นว่ามี container มันรันอยู่ ชื่อว่า **"hangman-server"** 
![](https://i.imgur.com/a6tntcX.png)


#### ถ้าเห็น ตัว hangman-server รันอยู่แสดงว่ามันโอเคล่ะ ต่อไป ไปทำตัว Client ต่อ

ก่อนอื่นเลย ให้เอา คำว่า `server` ไปใส่แทน ip บนตัวเกม Code Client.java 
![](https://imgur.com/JPEjJMw.png)

1. จากนั้นสร้างไฟล์ชื่อ `Dockerfile-client` ขึ้นมา ไฟล์นี้จะเอาไว้สร้าง image ของ client

2. จากนั้น เปิด `Dockerfile-client` ขึ้นพิมพ์ตามนี้

    	FROM openjdk:12
        COPY . /usr/src/hangman-client/
        WORKDIR /usr/src/hangman-client/
        RUN javac -encoding UTF-8 *.java
        CMD ["java", "Client"]
		
3. `CMD ["java", "Client"]` บรรทัดนี้แก้ **"Client"** เป็นชื่อ class ที่ะรัน

4. จากนั้น รันคำสั่ง `docker build -f Dockerfile-client -t hangman-client-image .`

5. จากนั้น รันคำสั่ง `docker run -it --name hangman-client --link hangman-server:server hangman-client-image` 
คำสั่ง `--link` เป็นการเชื่อม **hangman-server **เข้าไปใน **hangman-client** โดยใช้ชื่อเป็น `server`  เหมือนที่บอกไว้ข้างบน ที่ให้เอา คำวา `server` ไปใส่แทน **ip**

**ถ้าทำสำเร็จ มันจะขึ้นตัวเกมมาให้เล่น**

ถ้าสมมุต run ตัว docker ไปแล้ว แล้วจะรันใหม่ แล้วมันขึ้นประมาณนี้
![](https://i.imgur.com/1OpYoOz.png)
แสดง ว่ามันมี docker ชื่อนี้แล้ว ให้ลบออกก่อน หรือถ้าไม่อยากลบ ก็ให้เปลี่ยนชื่อตรง `--name`
คำสั่งลบ `docker rm -f docker-name`  ตรง docker-name ให้ใส่เป็นชื่อ ของ docker

**ปล.1 ถ้าหากมีการแก้ไข code ให้ทำใหม่เลยตั้งแต่ `docker build -f .......  `** 


**คำสั่งต่างๆ**
- `docker ps` แสดง container ที่ ทำงานอยู่
- `docker ps -a` แสดง container ทั้งหมด ทั้งที่ทำงาน และไม่ทำงาน
- `docker rm -f "name or id" ` ลบ container
- `docker start "name or id" ` start container ที่ไม่ได้ทำงานอยู่

