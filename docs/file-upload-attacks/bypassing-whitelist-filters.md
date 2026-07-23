# Bypassing Whitelist Filters

## Double Extension

Dosya uzantısı, aşağıda verilen RegEx ile kontrol ediliyor olsun:

```php title="Source Code"
if (!preg_match("/^.*\.(gif|jpg|png)/", $fileName))
{
    echo "Only images are allowed!";
    die();
}
```

!!! success

    Eksik kontrol, .gif, .jpg, .png ile biten dosyalar kontrol edilmiyor.

Aşağıda verilen dosyalar beyaz listeyi atlatabilir:

* shell.gif.phar
* shell.jpg.phpt
* shell.png.phtm

## Reverse Double Extension

Dosya uzantısı, aşağıda verilen RegEx ile kontrol ediliyor olsun:

```php title="Source Code"
if (!preg_match("/^.*\.(gif|jpg|png)$/", $fileName))
{
    echo "Only images are allowed!";
    die();
}
```

!!! failure

    Doğru kontrol, .gif, .jpg, .png ile biten dosyalar kontrol ediliyor.

Aşağıda verilen dosyalar beyaz listeyi atlatamaz:

* shell.gif.phar
* shell.jpg.phpt
* shell.png.phtm

Web sunucusu, aşağıda verilen yapılandırma dosyasına sahip olsun:

```apache title="/etc/apache2/mods-enabled/php7.4.conf" linenums="1" hl_lines="1"
<FilesMatch ".+\.ph(ar|pt|tm)">
    SetHandler application/x-httpd-php
</FilesMatch>
```

!!! success

    Eksik kontrol, .phar, .phpt, .phtm ile biten dosyalar kontrol edilmiyor.

Aşağıda verilen dosyalar komut yürütme için kullanılabilir:

* shell.phar.gif
* shell.phpt.jpg
* shell.phtm.png

## Character Injection

```bash title="generate.sh" linenums="1"
#!/bin/bash

for character in "%00" "%20" "%0A" "%0D0A" "/" ".\\" "." "..." ":"
do
    for extension in ".pgif" ".phar" ".php" ".php3" ".php4" ".php5" ".php7" ".php8" ".phps" ".phpt" ".pht" ".phtm" ".phtml"
    do
        {
            echo "shell$character$extension.jpg"
            echo "shell$extension$character.jpg"
            echo "shell.jpg$character$extension"
            echo "shell.jpg$extension$character"
        } >> wordlist.txt
    done
done
```
