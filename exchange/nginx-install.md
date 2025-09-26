


```
──(kali㉿kali)-[~/Markdown-Test]
└─$ systemctl status nginx
○ nginx.service - A high performance web server and a reverse proxy server
     Loaded: loaded (/usr/lib/systemd/system/nginx.service; disabled; preset>
     Active: inactive (dead)
       Docs: man:nginx(8)

                                                                             
┌──(kali㉿kali)-[~/Markdown-Test]
└─$ ss -tlnp | grep 80    
                                                                             
┌──(kali㉿kali)-[~/Markdown-Test]
└─$ nginx -v                                
nginx version: nginx/1.26.3
```

nginx ist also installiert, aber nicht gestartet

```sh
sudo systemctl start nginx   # starten
sudo systemctl enable nginx  # on startup
ss -tlnp | grep 80 # test
```

test
