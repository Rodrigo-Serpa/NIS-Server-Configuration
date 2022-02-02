# NIS-Server-Configuration

## NIS Server Configuration Ubuntu

☁️ # **AWS**

👉 1 VPC (default is enough)
👉 2 Instances ( 1 Server 1 Client)
👉 2 Elastic IPs (1 for each one of the Instances)


🖥️ ## **Termius**

▶️ ## **Server**

◽Define your hosts ❗ (its need for the configuration)

```
nano /etc/hosts -> important step if you dont put your hostname with you server ip, its not going to work
add -> ipserver hostname@domain hostname domain (ex: 1.1.1.1 nis.server.pt nis server.pt)

```
◽Install NIS ➡️ `apt -y install nis`

◽Define if the Instance is the master nis server or not

```
nano /etc/default/nis
NISSERVER=false ->change to NISSERVER=master ->this change is done so the machine knows that he is the NIS server

```
◽Define which are the Subnets allowed in the NIS server

```
# in the end add mask and network
ex: 255.255.255.0		10.0.0.0

```
➡️dont comment 0.0.0.0 0.0.0.0 until you have all nis configuration working when ou have all configs done ❗

```
nano /etc/var/Makefile
line 52 change false ->true
line 56 change false ->true

```
◽Do `/usr/lib/yp/ypinit -m` ➡️ to define which are the hosts for the nis ➡️ after you put all your hosts do "Ctrl+d" and "yes" 

◽Restart nis `systemctl restart nis`


▶️## **Client**

◽Define your hosts ❗ (its need for the configuration) ➡️ the same as the server ❗

```
nano /etc/hosts -> important step if you dont put your hostname with you server ip, its not going to work
add -> ipserver hostname@domain hostname domain (ex: 1.1.1.1 nis.server.pt nis server.pt)

```
◽Install NIS ➡️ `apt -y install nis`

◽Define which is the server ➡️ nano /etc/yp.conf ➡️ add in the end -> domain ______ server hostname.domain (ex domain server.pt server nis.server.pt)

```
nano /etc/nsswitch.conf
passwd:         files systemd nis -> add nis
group:          files systemd nis -> add nis
shadow:         files nis -> add nis
gshadow:        files nis -> add nis

hosts:          files dns nis -> add nis

```

```
nano /etc/pam.d/common-session
add in the end -> session optional        pam_mkhomedir.so skel=/etc/skel umask=077

```

◽ Restart nis ➡️ `systemctl restart nis` 


▶️ ## **Server**

◽to add the new users to the configuration

```
cd /var/yp
make

```

✔️to check if the configuration is working login in both (server and cliente) 
