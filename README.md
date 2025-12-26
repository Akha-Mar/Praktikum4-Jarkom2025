[![Review Assignment Due Date](https://classroom.github.com/assets/deadline-readme-button-22041afd0340ce965d47ae6ef1cefeee28c7c493a6346c4f15d667ab976d596c.svg)](https://classroom.github.com/a/oYnIPZ_t)
| Name           | NRP        | Kelas     |
| ---            | ---        | ----------|
| Akhamar Elnath Chaniago | 5025231281 | B |



## Put your topology config image here!

<img width="1133" height="677" alt="image" src="https://github.com/user-attachments/assets/cce31488-0936-494b-a654-643cfb234aa0" />


## Put your GNS3 Project file here!

https://drive.google.com/file/d/1fAI2ny3_ToWrw6WUHhaASE6CSXjNdH90/view?usp=sharing

<br>

## Soal 1

> Lakukan subnetting pada topologi diatas menggunakan metode VLSM: [Referensi](https://github.com/arsitektur-jaringan-komputer/Modul-Jarkom/tree/master/Modul-4/Subnetting#2-vlsm-variable-length-subnet-masking)  
*Cantumkan juga tabel dan diagram pembagian subnet pada laporan praktikum*.


> _Subnet the topology above using the VLSM method: [Reference](https://github.com/arsitektur-jaringan-komputer/Modul-Jarkom/tree/master/Modul-4/Subnetting#2-vlsm-variable-length-subnet-masking)_  
_Also include the subnet table and diagram in the lab report._

**Answer:**

- Screenshot

- Explanation

  Untuk menghitung subnetting untuk setiap grup subnet, kita perlu mempertimbangkan jumlah host yang menggunakan subnet tersebut. Untuk melakukannya, kita merujuk pada tabel lenghts and addresses.
  
  ![jarkomsubnet](https://github.com/arsitektur-jaringan-komputer/Modul-Jarkom/blob/master/Modul-4/assets/1.png?raw=true)  
  
  | Subnet | Hosts | Label |
  | --- | --- | --- |
  | A1 | 115 | /25 |
  | A2 | 2 | /30 |
  | A3 | 2 | /30 |
  | A4 | 25 | /27 |
  | A5 | 20 | /27 |
  | A6 | 2 | /30 |
  | A7 | 450 | /23 |
  | A8 | 2 | /30 |
  | A9 | 12 | /28 |
  | A10 | 18 | 27/ |
  | **Total** | **648** | **22/** |

  Dengan itu, kita dapat mulai menghitung cara membagi subnet menggunakan pohon. Kita ambil subnet dengan label terbesar dan mulai membagi dari itu.

  | Subnet | Hosts | Label | Network ID | Netmask | Broadcast Address |
  | ------ | ----- | ----- | ---------- | ------- | ----------------- |
  | A1 | 115 | /25 | 10.86.3.0 | 255.255.255.128 | 10.86.3.127 |
  | A2 | 2 | /30 | 10.86.3.240 | 255.255.255.252 | 10.86.3.243 |
  | A3 | 2 | /30 | 10.86.3.244 | 255.255.255.252 | 10.86.3.247 |
  | A4 | 25 | /27 | 10.86.3.128 | 255.255.255.224 | 10.86.3.159 |
  | A5 | 20 | /27 | 10.86.3.160 | 255.255.255.224 | 10.86.3.191 |
  | A6 | 2 | /30 | 10.86.3.248 | 255.255.255.252 | 10.86.3.251 |
  | A7 | 450 | /23 | 10.86.0.0 | 255.255.254.0 | 10.86.1.255 |
  | A8 | 2 | /30 | 10.86.3.252 | 255.255.255.252 | 10.86.3.255 |
  | A9 | 12 | /28 | 10.86.3.224 | 255.255.255.240 | 10.86.3.239 |
  | A10 | 18 | /27 | 10.86.3.192 | 255.255.255.224 | 10.86.3.223 |
  | **Total** | **648** | **/22** |  |  |  |

  Setelah mengatur ulang semua Network ID untuk setiap subnet, kita dapat mulai mengalokasikan IP setiap node secara statis.
  ### Routers
  - **router-2**
    ```
    auto eth0
    iface eth0 inet dhcp
        post-up apt install iptables -y
        post-up iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE
        post-up echo 1 > /proc/sys/net/ipv4/ip_forward

    auto eth1
    iface eth1 inet static
        address 10.86.3.241
        netmask 255.255.255.252

    auto eth3
    iface eth3 inet static
        address 10.86.3.245
        netmask 255.255.255.252

    auto eth2
    iface eth2 inet static
        address 10.86.3.253
        netmask 255.255.255.252
    ```

  - **router-1**
    ```
    auto eth0
    iface eth0 inet static
        address 10.86.3.242
        netmask 255.255.255.252
        up echo nameserver 8.8.8.8 > /etc/resolv.conf

    auto eth1
    iface eth1 inet static
        address 10.86.3.1
        netmask 255.255.255.128
    ```
  
  - **router-3**
    ```
    auto eth0
    iface eth0 inet static
        address 10.86.3.254
        netmask 255.255.255.252
        up echo nameserver 8.8.8.8 > /etc/resolv.conf

    auto eth1
    iface eth1 inet static
        address 10.86.3.225
        netmask 255.255.255.240

    auto eth2
    iface eth2 inet static
        address 10.86.3.193
        netmask 255.255.255.224
    ```

  - **router-4**
    ```
    auto eth0
    iface eth0 inet static
        address 10.86.3.246
        netmask 255.255.255.252
        up echo nameserver 8.8.8.8 > /etc/resolv.conf

    auto eth1
    iface eth1 inet static
        address 10.86.3.129
        netmask 255.255.255.224

    auto eth2
    iface eth2 inet static
        address 10.86.3.161
        netmask 255.255.255.224

    auto eth3
    iface eth3 inet static
        address 10.86.3.249
        netmask 255.255.255.252
    ```

  - **router-5**
    ```
    # Connects to Router-4 (Link A6)
    auto eth0
    iface eth0 inet static
        address 10.86.3.250
        netmask 255.255.255.252
        up echo nameserver 8.8.8.8 > /etc/resolv.conf

    # HR LAN Gateway (Subnet A7)
    auto eth1
    iface eth1 inet static
        address 10.86.0.1
        netmask 255.255.254.0
    ```

  ### IT Systems
  - **IT-PC-1**
    ```
    auto eth0
    iface eth0 inet static
        address 10.86.3.2
        netmask 255.255.255.128
        gateway 10.86.3.1
        up echo nameserver 8.8.8.8 > /etc/resolv.conf
    ```

  - **IT-PC-2**
    ```
    auto eth0
    iface eth0 inet static
        address 10.86.3.3
        netmask 255.255.255.128
        gateway 10.86.3.1
        up echo nameserver 8.8.8.8 > /etc/resolv.conf
    ```

  - **IT-PC-3**
    ```
    auto eth0
    iface eth0 inet static
        address 10.86.3.4
        netmask 255.255.255.128
        gateway 10.86.3.1
        up echo nameserver 8.8.8.8 > /etc/resolv.conf
    ```

  ### Web Servers
  - **Web-Server-1**
    ```
    auto eth0
    iface eth0 inet static
        address 10.86.3.130
        netmask 255.255.255.224
        gateway 10.86.3.129
        up echo nameserver 8.8.8.8 > /etc/resolv.conf
    ```

  - **Web-Server-2**
    ```
    auto eth0
    iface eth0 inet static
        address 10.86.3.162
        netmask 255.255.255.224
        gateway 10.86.3.161
        up echo nameserver 8.8.8.8 > /etc/resolv.conf
    ```

  ### HR Systems
  - **HR-PC-1**
    ```
    auto eth0
    iface eth0 inet static
        address 10.86.0.2
        netmask 255.255.254.0
        gateway 10.86.0.1
        up echo nameserver 8.8.8.8 > /etc/resolv.conf
    ```

  - **HR-PC-2**
    ```
    auto eth0
    iface eth0 inet static
        address 10.86.0.3
        netmask 255.255.254.0
        gateway 10.86.0.1
        up echo nameserver 8.8.8.8 > /etc/resolv.conf
    ```

  ### DB Servers
  - **DB-Server-1**
    ```
    auto eth0
    iface eth0 inet static
        address 10.86.3.226
        netmask 255.255.255.240
        gateway 10.86.3.225
        up echo nameserver 8.8.8.8 > /etc/resolv.conf
    ```

  - **DB-Server-2**
    ```
    auto eth0
    iface eth0 inet static
        address 10.86.3.194
        netmask 255.255.255.224
        gateway 10.86.3.193
        up echo nameserver 8.8.8.8 > /etc/resolv.conf
    ```

<br>

## Soal 2

> Buatlah agar router-2 dapat melakukan koneksi ke internet. [Dapat menggunakan static routing].

> _Make sure router-2 can connect to the internet. [Can use static routing]._

**Answer:**

- Screenshot

<img width="567" height="178" alt="image" src="https://github.com/user-attachments/assets/d44adb6d-f68f-4e3d-bdb2-a05bc78a155f" />

- Explanation

  Router-2 sudah terhubung ke NAT instance.

<br>

## Soal 3

> Setelah mengimplementasi subnetting, buatlah agar seluruh topologi dapat terhubung. Lakukan Dynamic Routing pada topologi tersebut.
*Pastikan seluruh node yang ada dapat mengakses internet*.

> _After implementing subnetting, ensure the entire topology is connected. Perform dynamic routing on the topology._  
_Ensure all existing nodes can access the internet._

**Answer:**

- Screenshot
  <img width="577" height="185" alt="image" src="https://github.com/user-attachments/assets/a93652d5-03ac-4407-9246-473401b2384f" />
- Explanation

  Task 3 dapat dicapai dengan mengkonfigurasi dynamic routing pada setiap router. Saya memilih OSPF dan kita dapat mulai dengan mengaktifkannya di `/etc/frr/daemons`
  ```diff
  - ospfd=no
  + ospfd=yes
  ```
  Setelah itu, kita dapat memulainya dengan menjalankan ini.
  ```sh
  /usr/lib/frr/frrinit.sh start
  ```

  Kita sekarang dapat configure dynamic routing. Kita mulai dari router-2.
  ```sh
  vtysh
    conf t
      router ospf
        network 10.86.0.0/22 area 0
        default-information originate
        exit
      exit
    wr
  exit
  ```

  Setelah itu, kita dapat lanjut dengan router 1, 3, 4, dan 5.
  ```sh
  vtysh
    conf t
      router ospf
        network 10.86.0.0/22 area 0
        exit
      exit
    wr
  exit
  ```

<br>

## Soal 4

> Lakukan setup web server dengan file html di attachment berikut: [ Attachment ](https://drive.google.com/file/d/199qwfTNJCkxDV7mdO-MsaDdApkmKsnAG/view?usp=sharing)  menggunakan nginx pada “Web-Server-1” dan “Web-Server-2”.  
*Config dibebaskan kepada praktikkan dengan catatan menggunakan port 80*.

> _Set up a web server with the HTML file in the following attachment: [ Attachment ](https://drive.google.com/file/d/199qwfTNJCkxDV7mdO-MsaDdApkmKsnAG/view?usp=sharing) using nginx on “Web-Server-1” and “Web-Server-2”._
_Configuration is free to practice, but note that it uses port 80._

**Answer:**

- Screenshot  
<img width="692" height="407" alt="image" src="https://github.com/user-attachments/assets/90632450-3d7a-4d8a-84d3-8111e0eb93e2" />


- Explanation

  Deploying this can be done by simply editing the `/var/www/html/index.debian-default.html` and starting nginx by running this command.
  `/var/www/html/index.debian-default.html`
  ```html
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <meta name="viewport" content="width=device-width, initial-scale=1.0">
      <title>Simple Page</title>
      <style>
          body {
              margin: 0;
              padding: 0;
              font-family: Arial, sans-serif;
              background: #f5f5f5;
              display: flex;
              justify-content: center;
              align-items: center;
              height: 100vh;
              color: #333;
          }
          .container {
              text-align: center;
              padding: 20px 30px;
              background: white;
              border-radius: 8px;
              box-shadow: 0 2px 10px rgba(0,0,0,0.1);
          }
          h1 {
              margin-bottom: 10px;
              font-size: 24px;
              font-weight: 600;
          }
          p {
              margin: 0;
              font-size: 14px;
              color: #555;
          }
      </style>
  </head>
  <body>
      <div class="container">
          <h1>Hello!</h1>
          <p>Web Server | Jarkom Praktikum Modul 4 </p>
      </div>
  </body>
  </html>
  ```

  ```sh
  service nginx start
  ```

<br>

## Soal 5

> Kalian diminta untuk melakukan drop semua paket TCP yang masuk  ke subnet HR dengan port 1337 dan 4444. Lakukan testing dengan netcat.

> _You are asked to drop all incoming TCP packets to the HR subnet with ports 1337 and 4444. Test with netcat._

**Answer:**

- Screenshot

  ![image](/images/5.png)

- Explanation

  We can do this by blocking packets that fit our criteria using iptables. To further simplify this process, we can implement this on `router-5`.

  ```sh
  apt install iptables -y
  iptables -A FORWARD -p tcp -d 10.86.0.0/23 -dport 1337 -j DROP
  iptables -A FORWARD -p tcp -d 10.86.0.0/23 -dport 4444 -j DROP
  ```

  After which, we can try and test whether this works or not using netcat. First we setup a listener on one of the HR nodes.
  ```sh
  nc -lvp 1337
  ```
  And then, we can try and connect from a node outside of the HR subnet to test it.
  ```
  nc -v 10.86.0.2 1337
  ```
  We can see that the attempt doesn't connect both nodes and end up timing out, which validates the blocking.

<br>

## Soal 6

> Lakukan pembatasan sehingga koneksi SSH pada semua Web Server hanya dapat dilakukan oleh user yang berada pada node IT-PC-1, IT-PC-2, dan IT-PC-3. 

> _Implement restrictions so that SSH connections to all Web Servers can only be made by users on nodes IT-PC-1, IT-PC-2, and IT-PC-3._

**Answer:**

- Screenshot

  ![images](/images/6.png)

- Explanation

  Generally speaking, SSH connections are done via port 22. Thus, we need to configure that specifically that port's access clearance for IPs belonging to the IT System Subnet (`10.86.3.0/25`). We can attempt this on `router-4`.
  ```sh
  apt install iptables -y
  iptables -A FORWARD -o eth1 -p tcp -s 10.86.3.0/25 --dport 22 -j ACCEPT
  iptables -A FORWARD -o eth2 -p tcp -s 10.86.3.0/25 --dport 22 -j ACCEPT

  iptables -A FORWARD -o eth1 -p tcp --dport 22 -j DROP
  iptables -A FORWARD -o eth2 -p tcp --dport 22 -j DROP
  ```

<br>

## Soal 7

> Semua subnet hanya dapat mengakses semua DB-Server pada port 80 dan 443 (DB-Server-1 dan DB-Server-2) pada hari Senin-Sabtu, pukul 07:00- 22:00.

> _All subnets can only access all DB-Servers on ports 80 and 443 (DB-Server-1 and DB-Server-2) on Monday-Saturday, 07:00-22:00._

**Answer:**

- Screenshot

  ![image](/images/7.png)

- Explanation

  Similarly to the previous tasks, we can implement this by adding a firewall to `router-3`. This can be done by running the following code.
  ```sh
  apt install iptables -y
  iptables -A FORWARD -o eth1 -p tcp -m multiport --dports 80,443 -m time --timestart 07:00 --timestop 22:00 --weekdays Mon,Tue,Wed,Thu,Fri,Sat -j ACCEPT
  iptables -A FORWARD -o eth2 -p tcp -m multiport --dports 80,443 -m time --timestart 07:00 --timestop 22:00 --weekdays Mon,Tue,Wed,Thu,Fri,Sat -j ACCEPT

  iptables -A FORWARD -o eth1 -p tcp -j DROP
  iptables -A FORWARD -o eth2 -p tcp -j DROP
  ```

<br>

## Soal 8

> Kemudian, buat agar “Web-Server-1” dan “Web-Server-2” hanya memperbolehkan traffic bertipe HTTP.

> _Then, make sure that “Web-Server-1” and “Web-Server-2” only allow HTTP type traffic._

**Answer:**

- Screenshot

  ![image](/images/8.png)

- Explanation

  To implement this, we can add the constraint onto `router-4` and target the web server's network interface. You can add this to your existing configuration.
  ```sh
  iptables -F

  # Task 8
  iptables -A FORWARD -o eth1 -p tcp --dport 80 -j ACCEPT
  iptables -A FORWARD -o eth2 -p tcp --dport 80 -j ACCEPT

  # Task 6
  iptables -A FORWARD -o eth1 -p tcp -s 10.86.3.0/25 --dport 22 -j ACCEPT
  iptables -A FORWARD -o eth2 -p tcp -s 10.86.3.0/25 --dport 22 -j ACCEPT

  # Task 8
  iptables -A FORWARD -o eth1 -p tcp -j DROP
  iptables -A FORWARD -o eth2 -p tcp -j DROP
  ```

<br>

## Soal 9

> Pilih salah satu Subnet dan lakukan blokir terhadap semua request protokol ICMP (ping) dari luar subnet terhadap subnet tersebut.

> _Select one of the Subnets and block all ICMP protocol requests (ping) from outside the subnet to that subnet._

**Answer:**

- Screenshot

  ![image](/images/9.png)

- Explanation

  For this task, i will be chosing the HR subnets. We can do this by configuring `router-5`. Additional note to reset / flush your previous configurations before adding new ones to layer them properly.
  ```sh
  iptables -F

  # Task 5
  iptables -A FORWARD -p tcp -d 10.86.0.0/23 --dport 1337 -j DROP
  iptables -A FORWARD -p tcp -d 10.86.0.0/23 --dport 4444 -j DROP

  # Task 9
  iptables -A FORWARD -p icmp --icmp-type echo-request -d 10.86.0.0/23 -j DROP
  ```

<br>

## Soal 10

> Konfigurasikan fitur logging untuk melakukan log terhadap seluruh paket yang di-DROP pada lalu lintas setiap node.

> _Configure the logging feature to log all dropped packets on each node's traffic._

**Answer:**

- Screenshot

  - DB Servers Subnet (`router-3`)
    ![image](/images/10_1.png)
  - Web Servers Subnet (`router-4`)
    ![image](/images/10_2.png)
  - HR Systems Subnet (`router-5`)
    ![image](/images/10_3.png)

- Explanation

  Enabling logging for all the routers we added iptables configurations are simple. For each drop we attempt, we add a `LOG` command beforehand. Do this for all routers affected (`router-3`, `router-4`, `router-5`).

  - `router-3`: DB (Tasks 7)
    ```sh
    # Task 7
    iptables -A FORWARD -o eth1 -p tcp -m multiport --dports 80,443 -m time --timestart 07:00 --timestop 22:00 --weekdays Mon,Tue,Wed,Thu,Fri,Sat -j ACCEPT
    iptables -A FORWARD -o eth2 -p tcp -m multiport --dports 80,443 -m time --timestart 07:00 --timestop 22:00 --weekdays Mon,Tue,Wed,Thu,Fri,Sat -j ACCEPT

    # Task 10
    iptables -A FORWARD -o eth1 -p tcp -j LOG --log-prefix "DB1-DROP: "
    iptables -A FORWARD -o eth2 -p tcp -j LOG --log-prefix "DB2-DROP"

    # Task 7
    iptables -A FORWARD -o eth1 -p tcp -j DROP
    iptables -A FORWARD -o eth2 -p tcp -j DROP
    ```
  - `router-4`: Web (Tasks 6, 8)
    ```sh
    # Task 8
    iptables -A FORWARD -o eth1 -p tcp --dport 80 -j ACCEPT
    iptables -A FORWARD -o eth2 -p tcp --dport 80 -j ACCEPT

    # Task 6
    iptables -A FORWARD -o eth1 -p tcp -s 10.86.3.0/25 --dport 22 -j ACCEPT
    iptables -A FORWARD -o eth2 -p tcp -s 10.86.3.0/25 --dport 22 -j ACCEPT

    # Task 10
    iptables -A FORWARD -o eth1 -p tcp -j LOG --log-prefix "WEB1-DROP:"
    iptables -A FORWARD -o eth2 -p tcp -j LOG --log-prefix "WEB2-DROP:"

    # Task 8
    iptables -A FORWARD -o eth1 -p tcp -j DROP
    iptables -A FORWARD -o eth2 -p tcp -j DROP
    ```
  - `router-5`: HR (Tasks 5, 9)
    ```sh
    # Task 10
    iptables -A FORWARD -p tcp -d 10.86.0.0/23 --dport 1337 -j LOG --log-prefix "HR-Suspicious: " --log-level 4
    iptables -A FORWARD -p tcp -d 10.86.0.0/23 --dport 4444 -j LOG --log-prefix "HR-Suspicious: " --log-level 4
    iptables -A FORWARD -p icmp --icmp-type echo-request -d 10.86.0.0/23 -j LOG --log-prefix "HR-Ping-Drop: " --log-level 4

    # Task 5
    iptables -A FORWARD -p tcp -d 10.86.0.0/23 --dport 1337 -j DROP
    iptables -A FORWARD -p tcp -d 10.86.0.0/23 --dport 4444 -j DROP

    # Task 9
    iptables -A FORWARD -p icmp --icmp-type echo-request -d 10.86.0.0/23 -j DROP
    ```

<br>
  
## Problems

## Revisions (if any)
