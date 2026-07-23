# Joomla

## Footprinting

```sh
my@attack:~$ curl -s 'http://dev.inlanefreight.local' | grep Joomla
```

```html title="Output"
<meta name="generator" content="Joomla! - Open Source Content Management" />
```

## Version

```sh
my@attack:~$ curl -s 'http://dev.inlanefreight.local/administrator/manifests/files/joomla.xml' | grep -i version
```

```xml title="Output" hl_lines="4"
<?xml version="1.0" encoding="UTF-8"?>
<extension version="3.6" type="file" method="upgrade">
        <license>GNU General Public License version 2 or later; see LICENSE.txt</license>
        <version>3.9.4</version>
```

## Approx Version

```sh
my@attack:~$ curl -s 'http://dev.inlanefreight.local/plugins/system/cache/cache.xml' | grep -i version
```

```xml title="Output" hl_lines="4"
<?xml version="1.0" encoding="utf-8"?>
<extension version="3.1" type="plugin" group="system" method="upgrade">
        <license>GNU General Public License version 2 or later; see LICENSE.txt</license>
        <version>3.0.0</version>
```

## README.txt

```sh
my@attack:~$ curl -s 'http://dev.inlanefreight.local/README.txt' | head
```

```output title="Output" hl_lines="4"
1- What is this?
        * This is a Joomla! installation/upgrade package to version 3.x
        * Joomla! Official site: https://www.joomla.org
        * Joomla! 3.9 version history - https://docs.joomla.org/Special:MyLanguage/Joomla_3.9_version_history
        * Detailed changes in the Changelog: https://github.com/joomla/joomla-cms/commits/staging

2- What is Joomla?
        * Joomla! is a Content Management System (CMS) which enables you to build Web sites and powerful online applications.
        * It's a free and Open Source software, distributed under the GNU General Public License version 2 or later.
        * This is a simple and powerful web server application and it requires a server with PHP and either MySQL, PostgreSQL or SQL Server to run.
```

## [JoomScan](https://github.com/OWASP/joomscan)

```sh
my@attack:~$ joomscan.pl -u "http://dev.inlanefreight.local" --no-report
```

```output title="Output"
Processing http://dev.inlanefreight.local ...



[+] FireWall Detector
[++] Firewall not detected

[+] Detecting Joomla Version
[++] Joomla 3.9.4

[+] Core Joomla Vulnerability
[++] Target Joomla core is not vulnerable

[+] Checking Directory Listing
[++] directory has directory listing :
http://dev.inlanefreight.local/administrator/components
http://dev.inlanefreight.local/administrator/modules
http://dev.inlanefreight.local/administrator/templates
http://dev.inlanefreight.local/images/banners


[+] Checking apache info/status files
[++] Readable info/status files are not found

[+] admin finder
[++] Admin page : http://dev.inlanefreight.local/administrator/

[+] Checking robots.txt existing
[++] robots.txt is found
path : http://dev.inlanefreight.local/robots.txt

Interesting path found from robots.txt
http://dev.inlanefreight.local/joomla/administrator/
http://dev.inlanefreight.local/administrator/
http://dev.inlanefreight.local/bin/
http://dev.inlanefreight.local/cache/
http://dev.inlanefreight.local/cli/
http://dev.inlanefreight.local/components/
http://dev.inlanefreight.local/includes/
http://dev.inlanefreight.local/installation/
http://dev.inlanefreight.local/language/
http://dev.inlanefreight.local/layouts/
http://dev.inlanefreight.local/libraries/
http://dev.inlanefreight.local/logs/
http://dev.inlanefreight.local/modules/
http://dev.inlanefreight.local/plugins/
http://dev.inlanefreight.local/tmp/


[+] Finding common backup files name
[++] Backup files are not found

[+] Finding common log files name
[++] error log is not found

[+] Checking sensitive config.php.x file
[++] Readable config files are not found
```

## [droopescan](https://github.com/SamJoan/droopescan)

```sh
my@attack:~$ droopescan scan joomla -u "http://dev.inlanefreight.local" -e a
```

```output title="Output"
[+] Possible version(s):
    3.8.10
    3.8.11
    3.8.11-rc
    3.8.12
    3.8.12-rc
    3.8.13
    3.8.7
    3.8.7-rc
    3.8.8
    3.8.8-rc
    3.8.9
    3.8.9-rc

[+] Possible interesting urls found:
    Detailed version information. - http://dev.inlanefreight.local/administrator/manifests/files/joomla.xml
    Login page. - http://dev.inlanefreight.local/administrator/
    License file. - http://dev.inlanefreight.local/LICENSE.txt
    Version attribute contains approx version - http://dev.inlanefreight.local/plugins/system/cache/cache.xml

[+] Scan finished (0:00:01.821973 elapsed)
```

## [Joomla Scan](https://github.com/drego85/JoomlaScan)

```sh
my@attack:~$ python2 joomlascan.py -u "http://dev.inlanefreight.local"
```

```output title="Output"
-------------------------------------------
             Joomla Scan
   Usage: python joomlascan.py <target>
    Version 0.5beta - Database Entries 1235
         created by Andrea Draghetti
-------------------------------------------
Robots file found:               > http://dev.inlanefreight.local/robots.txt
No Error Log found

Start scan...with 10 concurrent threads!
Component found: com_actionlogs  > http://dev.inlanefreight.local/index.php?option=com_actionlogs
         On the administrator components
Component found: com_admin       > http://dev.inlanefreight.local/index.php?option=com_admin
         On the administrator components
Component found: com_ajax        > http://dev.inlanefreight.local/index.php?option=com_ajax
         But possibly it is not active or protected
         LICENSE file found      > http://dev.inlanefreight.local/administrator/components/com_actionlogs/actionlogs.xml
         LICENSE file found      > http://dev.inlanefreight.local/administrator/components/com_admin/admin.xml
         LICENSE file found      > http://dev.inlanefreight.local/administrator/components/com_ajax/ajax.xml
         Explorable Directory    > http://dev.inlanefreight.local/components/com_actionlogs/
         Explorable Directory    > http://dev.inlanefreight.local/administrator/components/com_actionlogs/
         Explorable Directory    > http://dev.inlanefreight.local/components/com_admin/
         Explorable Directory    > http://dev.inlanefreight.local/administrator/components/com_admin/
Component found: com_banners     > http://dev.inlanefreight.local/index.php?option=com_banners
         But possibly it is not active or protected
         Explorable Directory    > http://dev.inlanefreight.local/components/com_ajax/
         Explorable Directory    > http://dev.inlanefreight.local/administrator/components/com_ajax/
         LICENSE file found      > http://dev.inlanefreight.local/administrator/components/com_banners/banners.xml
         Explorable Directory    > http://dev.inlanefreight.local/components/com_banners/
         Explorable Directory    > http://dev.inlanefreight.local/administrator/components/com_banners/
Component found: com_config      > http://dev.inlanefreight.local/index.php?option=com_config
Component found: com_contact     > http://dev.inlanefreight.local/index.php?option=com_contact
Component found: com_content     > http://dev.inlanefreight.local/index.php?option=com_content
Component found: com_contenthistory      > http://dev.inlanefreight.local/index.php?option=com_contenthistory
         But possibly it is not active or protected
         LICENSE file found      > http://dev.inlanefreight.local/administrator/components/com_config/config.xml
         LICENSE file found      > http://dev.inlanefreight.local/administrator/components/com_content/content.xml
         LICENSE file found      > http://dev.inlanefreight.local/administrator/components/com_contact/contact.xml
         LICENSE file found      > http://dev.inlanefreight.local/administrator/components/com_contenthistory/contenthistory.xml
         Explorable Directory    > http://dev.inlanefreight.local/components/com_config/
         Explorable Directory    > http://dev.inlanefreight.local/administrator/components/com_config/
         Explorable Directory    > http://dev.inlanefreight.local/components/com_content/
         Explorable Directory    > http://dev.inlanefreight.local/components/com_contact/
         Explorable Directory    > http://dev.inlanefreight.local/components/com_contenthistory/
         Explorable Directory    > http://dev.inlanefreight.local/administrator/components/com_contact/
         Explorable Directory    > http://dev.inlanefreight.local/administrator/components/com_content/
         Explorable Directory    > http://dev.inlanefreight.local/administrator/components/com_contenthistory/
Component found: com_fields      > http://dev.inlanefreight.local/index.php?option=com_fields
         But possibly it is not active or protected
         LICENSE file found      > http://dev.inlanefreight.local/administrator/components/com_fields/fields.xml
         Explorable Directory    > http://dev.inlanefreight.local/components/com_fields/
         Explorable Directory    > http://dev.inlanefreight.local/administrator/components/com_fields/
Component found: com_installer   > http://dev.inlanefreight.local/index.php?option=com_installer
         On the administrator components
         LICENSE file found      > http://dev.inlanefreight.local/administrator/components/com_installer/installer.xml
         Explorable Directory    > http://dev.inlanefreight.local/components/com_installer/
         Explorable Directory    > http://dev.inlanefreight.local/administrator/components/com_installer/
Component found: com_joomlaupdate        > http://dev.inlanefreight.local/index.php?option=com_joomlaupdate
         On the administrator components
         LICENSE file found      > http://dev.inlanefreight.local/administrator/components/com_joomlaupdate/joomlaupdate.xml
         Explorable Directory    > http://dev.inlanefreight.local/components/com_joomlaupdate/
         Explorable Directory    > http://dev.inlanefreight.local/administrator/components/com_joomlaupdate/
Component found: com_mailto      > http://dev.inlanefreight.local/index.php?option=com_mailto
         But possibly it is not active or protected
Component found: com_media       > http://dev.inlanefreight.local/index.php?option=com_media
         But possibly it is not active or protected
         LICENSE file found      > http://dev.inlanefreight.local/components/com_mailto/mailto.xml
         LICENSE file found      > http://dev.inlanefreight.local/administrator/components/com_media/media.xml
         Explorable Directory    > http://dev.inlanefreight.local/components/com_mailto/
Component found: com_newsfeeds   > http://dev.inlanefreight.local/index.php?option=com_newsfeeds
         Explorable Directory    > http://dev.inlanefreight.local/components/com_media/
         Explorable Directory    > http://dev.inlanefreight.local/administrator/components/com_media/
         LICENSE file found      > http://dev.inlanefreight.local/administrator/components/com_newsfeeds/newsfeeds.xml
         Explorable Directory    > http://dev.inlanefreight.local/components/com_newsfeeds/
         Explorable Directory    > http://dev.inlanefreight.local/administrator/components/com_newsfeeds/
Component found: com_search      > http://dev.inlanefreight.local/index.php?option=com_search
         LICENSE file found      > http://dev.inlanefreight.local/administrator/components/com_search/search.xml
         Explorable Directory    > http://dev.inlanefreight.local/components/com_search/
         Explorable Directory    > http://dev.inlanefreight.local/administrator/components/com_search/
Component found: com_users       > http://dev.inlanefreight.local/index.php?option=com_users
         LICENSE file found      > http://dev.inlanefreight.local/administrator/components/com_users/users.xml
Component found: com_wrapper     > http://dev.inlanefreight.local/index.php?option=com_wrapper
         Explorable Directory    > http://dev.inlanefreight.local/components/com_users/
         Explorable Directory    > http://dev.inlanefreight.local/administrator/components/com_users/
         LICENSE file found      > http://dev.inlanefreight.local/components/com_wrapper/wrapper.xml
         Explorable Directory    > http://dev.inlanefreight.local/components/com_wrapper/
End Scanner
```
