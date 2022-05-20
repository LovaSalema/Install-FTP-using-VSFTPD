<div id="top"></div>

<br />
<div align="center">

  <h3 align="center">Install FTP using VSFTPD</h3>


</div>



<!-- TABLE OF CONTENTS -->
<details>
  <summary>Table of Contents</summary>
  <ul>
    <li>
        <a href="#Overview">overview</a>
    </li>
    <li>
      <a href="#Prerequisites">Prerequistes</a>
    </li>
    <li><a href="#Installing-VSFTPd">Installing VSFTPd</a></li>
    <li><a href="#Configuring-VSFTPd">Configuring VSFTPd</a></li>
    <li><a href="#Connecting-to-the-FTP-Server">Connecting to the FTP Server</a></li>
    <li><a href="#Conclusion">Conclusion</a></li>
  </ul>
</details>



<!-- ABOUT THE PROJECT 


[![Product Name Screen Shot][product-screenshot]](https://example.com)-->
## Overview
Have you ever wanted to setup a simple FTP server so that you can upload files to your remote server for yourself, or a few users? VSFTPd is the perfect option for you. It’s a powerful, open source FTP server that is designed to be quickly and easily configured.

In this article, we’ll show you how to setup VSFTPd on Ubuntu so that you can create your own personal FTP server.

### Prerequisites
In order to setup an FTP server, you’ll need a Hybrid, Cloud, or Dedicated Server from ServerMania. We have a variety of hosting solutions to choose from at ServerMania.com.

Not sure which server is best for you? Book an expert consultation today, and we’ll find the perfect server for your needs and budget, guaranteed.

This article was created using Ubuntu 16.04. Instructions may vary based on the version of Ubuntu you are running on your server.

## Installing VSFTPd
#### Step 1: Login to the server via SSH

`ssh ubuntu@SERVER-IP`

#### Step 2: Change into the root user
```sh
sudo su
```
#### Step 3: Install VSFTPd
```sh
apt-get install vsftpd -y
```

#### Step 4: Start VSFTPd and set it to start on boot
```sh
systemctl start vsftpd
systemctl enable vsftpd
```

#### Step 5: Create a user for FTP access
```sh
adduser vsftp
```

#### Step 6: Make an FTP directory and set permissions
```sh
mkdir /home/vsftp/ftp
chown nobody:nogroup /home/vsftp/ftp
chmod a-w /home/vsftp/ftp
```

#### Step 7: Create an upload directory and set permissions
```sh
mkdir /home/vsftp/ftp/test
chown vsftp:vsftp /home/vsftp/ftp/test
```
## Configuring VSFTPd

#### Step 1: Backup the configuration file

```sh
cp /etc/vsftpd.conf /etc/vsftpd.conf.bak
```

#### Step 2: Open the configuration file in your favourite text editor
```sh
vi /etc/vsftpd.conf
```

#### Step 3: Add the following lines to the file, then save and close the file:
```sh

listen=NO
listen_ipv6=YES
anonymous_enable=NO
local_enable=YES
write_enable=YES
local_umask=022
dirmessage_enable=YES
use_localtime=YES
xferlog_enable=YES
connect_from_port_20=YES
chroot_local_user=YES
secure_chroot_dir=/var/run/vsftpd/empty
pam_service_name=vsftpd
pasv_enable=Yes
pasv_min_port=10000
pasv_max_port=11000
user_sub_token=$USER
local_root=/home/$USER/ftp
userlist_enable=YES
userlist_file=/etc/vsftpd.userlist
userlist_deny=NO
rsa_cert_file=/etc/cert/vsftpd.pem
rsa_private_key_file=/etc/cert/vsftpd.pem
ssl_enable=YES
allow_anon_ssl=NO
force_local_data_ssl=YES
force_local_logins_ssl=YES
ssl_tlsv1=YES
ssl_sslv2=NO
ssl_sslv3=NO
require_ssl_reuse=NO
ssl_ciphers=HIGH
```
#### Step 4: Add the FTP user to VSFTP
```sh
vi /etc/vsftpd.userlist
```

Add the following line, then save and close the file:

`vsftp`
#### Step 5: Create a certificate to connect via SSL
```sh
mkdir /etc/cert
openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/cert/vsftpd.pem -out /etc/cert/vsftpd.pem
```

#### Step 6: Restart VSFTP
```sh
systemctl restart vsftpd
```



## Connecting to the FTP Server
You can now visit `ftp://YOUR-SERVER-IP` and login using the username and password you created earlier in order to view files uploaded.

In order to upload files, you can use an FTP client such as Filezilla. Remember to require explicit SSL connections when configuring Filezilla, otherwise the connection will fail.



## Conclusion
Your Ubuntu server is now setup with a simple FTP server for you to upload and store files. Congratulations!
#### End result

<!-- MARKDOWN LINKS & IMAGES -->
<!-- https://www.markdownguide.org/basic-syntax/#reference-style-links -->
[contributors-shield]: https://img.shields.io/github/contributors/othneildrew/Best-README-Template.svg?style=for-the-badge
[contributors-url]: https://github.com/othneildrew/Best-README-Template/graphs/contributors
[forks-shield]: https://img.shields.io/github/forks/othneildrew/Best-README-Template.svg?style=for-the-badge
[forks-url]: https://github.com/othneildrew/Best-README-Template/network/members
[stars-shield]: https://img.shields.io/github/stars/othneildrew/Best-README-Template.svg?style=for-the-badge
[stars-url]: https://github.com/othneildrew/Best-README-Template/stargazers
[issues-shield]: https://img.shields.io/github/issues/othneildrew/Best-README-Template.svg?style=for-the-badge
[issues-url]: https://github.com/othneildrew/Best-README-Template/issues
[license-shield]: https://img.shields.io/github/license/othneildrew/Best-README-Template.svg?style=for-the-badge
[license-url]: https://github.com/othneildrew/Best-README-Template/blob/master/LICENSE.txt
[linkedin-shield]: https://img.shields.io/badge/-LinkedIn-black.svg?style=for-the-badge&logo=linkedin&colorB=555
[linkedin-url]: https://linkedin.com/in/othneildrew
[product-screenshot]: images/screenshot.png
