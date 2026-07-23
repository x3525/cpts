# Catching Files over HTTPS

Yüklenen dosyaları yönetebilmek için bir dizin oluştur:

```sh
htb-student@nix04:~$ sudo mkdir -p /var/www/uploads/SecretUploadDirectory
```

Boş bir index.html dosyası oluştur:

```sh
htb-student@nix04:~$ sudo touch /var/www/uploads/SecretUploadDirectory/index.html
```

Oluşturulan dizinin kullanıcı ve grup sahipliğini www-data olarak değiştir:

```sh
htb-student@nix04:~$ sudo chown -R www-data:www-data /var/www/uploads/SecretUploadDirectory
```

Nginx yapılandırma dosyasını oluştur:

```nginx title="/etc/nginx/sites-available/upload.conf" linenums="1" hl_lines="2"
server {
    listen 9001;

    location /SecretUploadDirectory/ {
        root    /var/www/uploads;
        dav_methods PUT;
    }
}
```

Yapılandırma dosyası için bir sembolik link oluştur:

```sh
htb-student@nix04:~$ sudo ln -s /etc/nginx/sites-available/upload.conf /etc/nginx/sites-enabled/
```

Nginx servisini yeniden başlat:

```sh
htb-student@nix04:~$ sudo systemctl restart nginx
```

!!! tip

    Hata alınması durumunda /var/log/nginx/error.log günlüğü kontrol edilebilir.

80 numaralı port aşağıdaki komut ile kontrol edilir:

```sh
htb-student@nix04:~$ sudo ss -lntp | grep 80
```

```output title="Output"
LISTEN 0      100          0.0.0.0:80        0.0.0.0:*    users:(("python",pid=2811,fd=3),("python",pid=2070,fd=3),("python",pid=1968,fd=3),("python",pid=1856,fd=3))
```

PID numarasına göre kontrol etmek için aşağıdaki komutu kullan:

```sh
htb-student@nix04:~$ sudo ps -ef | grep 2811
```

```output title="Output"
user65      2811    1856  0 16:05 ?        00:00:04 python -m websockify 80 localhost:5901 -D
root        6720    2226  0 16:14 pts/0    00:00:00 grep --color=auto 2811
```

Çıktıda görüldüğü üzere 80 numaralı portu dinleyen bir modül var.

Durumu düzeltmek için Nginx yapılandırma dosyasını sil:

```sh
htb-student@nix04:~$ sudo rm /etc/nginx/sites-enabled/default
```

Bir dosya yükle:

```sh
htb-student@nix04:~$ sudo curl -s 'http://localhost:9001/SecretUploadDirectory/file.txt' -T file.txt
```
