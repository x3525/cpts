# Aircrack-ng

## Benchmark

```sh
my@attack:~$ aircrack-ng -S
```

```output title="Output"
5784.608 k/s
```

## Cracking WEP

```sh
my@attack:~$ aircrack-ng -K results.ivs
```

```output title="Output" hl_lines="19"
                                                                                                Aircrack-ng 1.6


                                                                                  [00:00:02] Tested 1995 keys (got 566693 IVs)

   KB    depth   byte(vote)
    0    0/  1   AE(  50) 11(  20) 71(  20) 0D(  12) 10(  12) 68(  12) 84(  12) 0A(   9) 31(   6) 90(   6) 9D(   6) 83(   5) BA(   5) 92(   4) 66(   3) 67(   3) 6B(   3)
    1    1/  2   5B(  31) BD(  18) F8(  17) E6(  16) 35(  15) 7A(  13) 7F(  13) 81(  13) CF(  13) D2(  13) 29(  12) 58(  12) B9(  12) BE(  12) 49(  10) BC(   8) DE(   7)
    2    0/  3   7F(  31) 74(  24) 54(  17) 1C(  13) 73(  13) 86(  12) 1B(  10) BF(  10) 31(   8) 56(   8) 5C(   8) FF(   8) 5D(   6) A2(   6) ED(   6) 06(   5) 4C(   5)
    3    0/  1   3A( 148) EC(  20) EB(  16) FB(  13) 81(  12) D7(  12) ED(  12) F0(  12) F9(  12) F8(  10) DD(   9) 02(   8) 72(   8) 8B(   8) B4(   8) 23(   6) 7A(   6)
    4    0/  1   03( 140) 90(  31) 4A(  15) 8F(  14) E9(  13) AD(  12) 86(  10) DB(  10) E2(  10) 99(   8) 59(   6) 93(   6) 21(   5) 27(   5) 2F(   5) 54(   5) 74(   5)
    5    0/  1   D0(  69) 04(  27) 60(  24) C8(  24) 26(  20) A1(  20) A0(  18) 4F(  17) B6(  16) 69(  14) 84(  14) A3(  13) D1(  12) 7B(  10) A8(  10) 3F(   9) 35(   8)
    6    0/  1   AF( 124) D4(  29) C8(  20) EE(  18) 3F(  12) 54(  12) 3C(  11) 90(  11) 76(  10) CF(  10) 3D(   9) 3E(   9) FE(   9) 4A(   8) 4C(   8) 72(   8) B2(   8)
    7    0/  1   9B( 168) 90(  24) 72(  22) F5(  21) 11(  20) F1(  20) 86(  17) FB(  16) 0E(  15) 12(  13) AD(  13) 17(  12) 36(  12) 8E(  10) FC(  10) 23(   9) 33(   9)
    8    0/  1   F6( 157) EE(  24) 66(  20) DA(  18) E0(  18) EA(  18) 82(  17) 11(  16) AD(  15) E4(  15) 4F(  13) 6A(  13) 74(  13) CC(  13) E9(  13) FC(  13) 71(  12)
    9    1/  2   7B(  44) E2(  30) 11(  27) DE(  23) A4(  20) 66(  19) E9(  18) 64(  17) E6(  17) 6F(  16) 16(  15) 2F(  14) 4D(  14) 6B(  14) FA(  14) E8(  13) 05(  12)
   10    1/  1   01(   0) 02(   0) 03(   0) 04(   0) 05(   0) 06(   0) 07(   0) 08(   0) 09(   0) 0A(   0) 0B(   0) 0C(   0) 0D(   0) 0E(   0) 0F(   0) 10(   0) 11(   0)

             KEY FOUND! [ AE:5B:7F:3A:03:D0:AF:9B:F6:8D:A5:E2:C7 ]
        Decrypted correctly: 100%
```

## Cracking WPA

```sh
my@attack:~$ aircrack-ng -e 'Coherer' -w wordlist.txt results.cap
```

```output title="Output" hl_lines="7"
                               Aircrack-ng 1.6

      [00:00:00] 547/10303727 keys tested (3733.88 k/s)

      Time left: 45 minutes, 59 seconds                          0.01%

                           KEY FOUND! [ Induction ]


      Master Key     : A2 88 FC F0 CA AA CD A9 A9 F5 86 33 FF 35 E8 99
                       2A 01 D9 C1 0B A5 E0 2E FD F8 CB 5D 73 0C E7 BC

      Transient Key  : 0E 92 23 FE 1C 0A ED 8D C9 89 5D D6 A6 E0 92 61
                       99 AC C6 E7 6D 4D F5 4A 18 D1 D1 00 00 00 00 00
                       00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
                       00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00

      EAPOL HMAC     : A4 62 A7 02 9A D5 BA 30 B6 AF 0D F3 91 98 8E 45
```
