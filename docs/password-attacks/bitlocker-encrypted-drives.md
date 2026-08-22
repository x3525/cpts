# BitLocker Encrypted Drives

## Extracting

```sh
my@attack:~$ bitlocker2john -i Private.vhd | tee output.txt
```

```output title="Output" hl_lines="45"
Signature found at 0x10003
Version: 8
Invalid version, looking for a signature with valid version...

Signature found at 0x2200000
Version: 2 (Windows 7 or later)

VMK entry found at 0x22000bb

VMK encrypted with User Password found at 22000dc
VMK encrypted with AES-CCM

VMK entry found at 0x220019b

VMK encrypted with Recovery Password found at 0x22001bc
Searching AES-CCM from 0x22001d8
Trying offset 0x220026b....
VMK encrypted with AES-CCM!!
Encrypted device Private.vhd opened, size 64MB
UP Nonce: b020fe18bbb1db0103000000
UP MAC: e9c6b548788aeff190e517b0d85ada5d
UP VMK: aad7a0a3f40c4467307011ac17f79f8c99768419903025fd7072ee78b15a729afcf54b8c2e3af05bb18d4ba0

Salt: 1d06e8b3aa5f14ddcb5ad7415c1d24f5
RP Nonce: b020fe18bbb1db0106000000
RP MAC: 8ff2e9cdd6ec25df9060e95e3c490a9a
RP VMK: efd5601f4cefa47da810339f809a90187cbbd907bd1077565293727114f3c218c078e03f2f8baa214f1de800


Signature found at 0x2956000
Version: 2 (Windows 7 or later)

VMK entry found at 0x29560bb

VMK entry found at 0x295619b

Signature found at 0x30ab000
Version: 2 (Windows 7 or later)

VMK entry found at 0x30ab0bb

VMK entry found at 0x30ab19b

User Password hash:
$bitlocker$0$16$b3c105c7ab7faaf544e84d712810da65$1048576$12$b020fe18bbb1db0103000000$60$e9c6b548788aeff190e517b0d85ada5daad7a0a3f40c4467307011ac17f79f8c99768419903025fd7072ee78b15a729afcf54b8c2e3af05bb18d4ba0
Hash type: User Password with MAC verification (slower solution, no false positives)
$bitlocker$1$16$b3c105c7ab7faaf544e84d712810da65$1048576$12$b020fe18bbb1db0103000000$60$e9c6b548788aeff190e517b0d85ada5daad7a0a3f40c4467307011ac17f79f8c99768419903025fd7072ee78b15a729afcf54b8c2e3af05bb18d4ba0
Hash type: Recovery Password fast attack
$bitlocker$2$16$1d06e8b3aa5f14ddcb5ad7415c1d24f5$1048576$12$b020fe18bbb1db0106000000$60$8ff2e9cdd6ec25df9060e95e3c490a9aefd5601f4cefa47da810339f809a90187cbbd907bd1077565293727114f3c218c078e03f2f8baa214f1de800
Hash type: Recovery Password with MAC verification (slower solution, no false positives)
$bitlocker$3$16$1d06e8b3aa5f14ddcb5ad7415c1d24f5$1048576$12$b020fe18bbb1db0106000000$60$8ff2e9cdd6ec25df9060e95e3c490a9aefd5601f4cefa47da810339f809a90187cbbd907bd1077565293727114f3c218c078e03f2f8baa214f1de800
```

## Cracking the Hash

```sh
my@attack:~$ hashcat --hwmon-disable -a 0 -m 22100 "$(grep -F '$bitlocker$0$' output.txt)" /usr/local/share/wordlists/rockyou.txt
```

## Connect/Disconnect

=== "CRYPTSETUP"

    ```sh
    my@attack:~$ sudo modprobe nbd
    my@attack:~$ sudo qemu-nbd -c /dev/nbd0 Private.vhd -f raw
    my@attack:~$ sudo cryptsetup open /dev/nbd0p1 PRIVATE --type bitlk
    my@attack:~$ sudo mount /dev/mapper/PRIVATE /mnt
    my@attack:~$ sudo umount /mnt
    my@attack:~$ sudo cryptsetup close PRIVATE
    my@attack:~$ sudo qemu-nbd -d /dev/nbd0
    my@attack:~$ sudo modprobe nbd -r
    ```

=== "DISLOCKER"

    ```sh
    my@attack:~$ sudo losetup --find --partscan Private.vhd
    my@attack:~$ sudo mkdir /media
    my@attack:~$ sudo mkdir /media/BitLocker
    my@attack:~$ sudo mkdir /media/BitLockerMount
    my@attack:~$ sudo dislocker /dev/loop3p1 -u'francisco' -- /media/BitLocker
    my@attack:~$ sudo mount -o loop /media/BitLocker/dislocker-file /media/BitLockerMount
    my@attack:~$ sudo umount /media/BitLockerMount
    my@attack:~$ sudo umount /media/BitLocker
    ```
