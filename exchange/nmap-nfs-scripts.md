# nmap NFS

Verwenden von nfs bezogenen nmap Scripts ()

nfs benutzt Port 2049, allerdings wird dieser Dienst in Metasploitable 2 über den portmapper zur Verfügung gestellt, dieser hat Port 111 und muss als Ziel für `nfs-showmount.nse` verwendet werden.

```sh
$ nmap --script nfs-showmount -p 111  $META
```
```
Starting Nmap 7.95 ( https://nmap.org ) at 2025-09-25 05:18 EDT
Nmap scan report for 192.168.40.129
Host is up (0.00041s latency).

PORT    STATE SERVICE
111/tcp open  rpcbind
| nfs-showmount: 
|_  / *
MAC Address: 00:0C:29:94:7A:DD (VMware)

Nmap done: 1 IP address (1 host up) scanned in 0.24 seconds
```

Alle nfs bezogenen Skripts laufen lassen:

```sh
$ nmap --script "nfs-*" -p 111,2049  $META
```
```
Starting Nmap 7.95 ( https://nmap.org ) at 2025-09-25 05:47 EDT
Nmap scan report for 192.168.40.129
Host is up (0.00054s latency).

PORT     STATE SERVICE
111/tcp  open  rpcbind
| nfs-showmount: 
|_  / *
| nfs-statfs: 
|   Filesystem  1K-blocks  Used       Available  Use%  Maxfilesize  Maxlink
|_  /           7282168.0  1477784.0  5437384.0  22%   2.0T         32000
| nfs-ls: Volume /
|   access: Read Lookup Modify Extend Delete NoExecute
| PERMISSION  UID  GID  SIZE   TIME                 FILENAME
| drwxr-xr-x  0    0    4096   2012-05-14T03:35:33  bin
| drwxr-xr-x  0    0    4096   2010-04-16T06:16:02  home
| drwxr-xr-x  0    0    4096   2010-03-16T22:57:40  initrd
| lrwxrwxrwx  0    0    32     2010-04-28T20:26:18  initrd.img
| drwxr-xr-x  0    0    4096   2012-05-14T03:35:22  lib
| drwx------  0    0    16384  2010-03-16T22:55:15  lost+found
| drwxr-xr-x  0    0    4096   2010-03-16T22:55:52  media
| drwxr-xr-x  0    0    4096   2010-04-28T20:16:56  mnt
| drwxr-xr-x  0    0    4096   2012-05-14T01:54:53  sbin
| drwxr-xr-x  0    0    4096   2010-04-28T04:06:37  usr
|_
2049/tcp open  nfs
MAC Address: 00:0C:29:94:7A:DD (VMware)

Nmap done: 1 IP address (1 host up) scanned in 0.33 seconds
```

Skripts Ergebnisse nur am Port 111.
