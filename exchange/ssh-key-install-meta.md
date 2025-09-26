# Pubkey Zugriff auf Metasploitable

Key muss im "alten" Format erzeugt werden.

```sh
$ ssh-keygen -t rsa
```

Key kopieren, beim Hostcheck auf das zurückstufen was der Server kann.

```sh
$ ssh-copy-id -i .ssh/id_rsa -oHostKeyAlgorithms=+ssh-rsa msfadmin@$META
```

Zum Login muss die Server-Authentifizierung und Pubkey Prüfung auf RSA zuückgestellt werden. Default wird das vom Client nicht mehr akzeptiert, der SSH auf Metaspoitable2 bietet aber nichts besseres an.

```sh
$ ssh -oHostKeyAlgorithms=+ssh-rsa -oPubkeyAcceptedAlgorithms=+ssh-rsa msfadmin@$META
```



