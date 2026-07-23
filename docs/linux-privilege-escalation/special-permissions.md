# Special Permissions

## Primer

### Change Owner

```sh
my@attack:~$ sudo chown root:root script.py
```

### Change Permissions

```sh
my@attack:~$ sudo chmod 754 script.py
```

| VALUE | SUM | PERMISSION |
|---|---|---|
| 7 | 4 + 2 + 1 | rwx |
| 6 | 4 + 2 + 0 | rw- |
| 5 | 4 + 0 + 1 | r-x |
| 4 | 4 + 0 + 0 | r-- |
| 3 | 0 + 2 + 1 | -wx |
| 2 | 0 + 2 + 0 | -w- |
| 1 | 0 + 0 + 1 | --x |
| 0 | 0 + 0 + 0 | --- |

### Change Permissions (Letters)

```sh
my@attack:~$ sudo chmod a+r script.py
```

| VALUE | MEANING |
|---|---|
| u | Owner |
| g | Group |
| o | Others |
| a | All |

### SUID

Bir dosyada SUID etkin ise, herhangi bir kullanıcı ilgili dosyayı, o dosyanın sahibi gibi çalıştırabilir.

* Değer s ise SUID aktif ve çalıştırılabilir izin var.
* Değer S ise SUID aktif ve çalıştırılabilir izin yok.

```sh
my@attack:~$ sudo chmod 4755 script.py
```

| VALUE | PERMISSION |
|---|---|
| 4 | SUID |

### SGID

Bir dosyada SGID etkin ise, herhangi bir kullanıcı ilgili dosyayı, o dosyanın sahibi gibi çalıştırabilir.

* Değer s ise SGID aktif ve çalıştırılabilir izin var.
* Değer S ise SGID aktif ve çalıştırılabilir izin yok.

```sh
my@attack:~$ sudo chmod 2755 script.py
```

| VALUE | PERMISSION |
|---|---|
| 2 | SGID |

### Sticky Bit

Bir dizinde Sticky Bit etkin ise, dizin altında bulunan dosyalar yalnızca dizin sahibi, dosya sahibi veya kök kullanıcı tarafından silinebilir.

* Değer t ise Sticky Bit aktif ve çalıştırılabilir izin var.
* Değer T ise Sticky Bit aktif ve çalıştırılabilir izin yok.

```sh
my@attack:~$ sudo chmod 1754 scripts
```

| VALUE | PERMISSION |
|---|---|
| 1 | Sticky Bit |

## Searching for SUID

```sh
htb-student@NIX02:~$ find / -user root -perm /4000 -ls 2> /dev/null
```

## Searching for SGID

```sh
htb-student@NIX02:~$ find / -user root -perm /6000 -ls 2> /dev/null
```
