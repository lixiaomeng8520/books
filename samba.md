1. yum install -y samba
2. smbpasswd -a lxm (添加一个linux用户,并输入密码)
3. systemctl start smb
4. 直接可以访问home目录,windows: \\\\192.168.56.20\lxm