# WordPress

## Version

```sh
my@attack:~$ curl -s 'http://blog.inlanefreight.local' | grep WordPress
```

```html title="Output"
<meta name="generator" content="WordPress 5.8" />
```

## Themes

```sh
my@attack:~$ curl -s 'http://blog.inlanefreight.local' | grep themes
```

```html title="Output"
<link rel='stylesheet' id='bootstrap-css'  href='http://blog.inlanefreight.local/wp-content/themes/business-gravity/assets/vendors/bootstrap/css/bootstrap.min.css' type='text/css' media='all' />
<link rel='stylesheet' id='kfi-icons-css'  href='http://blog.inlanefreight.local/wp-content/themes/business-gravity/assets/vendors/kf-icons/css/style.css' type='text/css' media='all' />
<link rel='stylesheet' id='owlcarousel-css'  href='http://blog.inlanefreight.local/wp-content/themes/business-gravity/assets/vendors/OwlCarousel2-2.2.1/assets/owl.carousel.min.css' type='text/css' media='all' />
<link rel='stylesheet' id='owlcarousel-theme-css'  href='http://blog.inlanefreight.local/wp-content/themes/business-gravity/assets/vendors/OwlCarousel2-2.2.1/assets/owl.theme.default.min.css' type='text/css' media='all' />
<link rel='stylesheet' id='business-gravity-blocks-css'  href='http://blog.inlanefreight.local/wp-content/themes/business-gravity/assets/css/blocks.min.css' type='text/css' media='all' />
<link rel='stylesheet' id='business-gravity-style-css'  href='http://blog.inlanefreight.local/wp-content/themes/transport-gravity/style.css' type='text/css' media='all' />
<link rel='stylesheet' id='transport-gravity-style-parent-css'  href='http://blog.inlanefreight.local/wp-content/themes/business-gravity/style.css?ver=5.8' type='text/css' media='all' />
<link rel='stylesheet' id='transport-gravity-style-css'  href='http://blog.inlanefreight.local/wp-content/themes/transport-gravity/style.css?ver=1.0.0' type='text/css' media='all' />
<script type='text/javascript' src='http://blog.inlanefreight.local/wp-content/themes/business-gravity/assets/vendors/bootstrap/js/bootstrap.min.js' id='bootstrap-js'></script>
<script type='text/javascript' src='http://blog.inlanefreight.local/wp-content/themes/business-gravity/assets/js/skip-link-focus-fix.min.js' id='business-gravity-skip-link-focus-fix-js'></script>
<script type='text/javascript' src='http://blog.inlanefreight.local/wp-content/themes/business-gravity/assets/js/navigation.js' id='business-gravity-navigation-js'></script>
<script type='text/javascript' src='http://blog.inlanefreight.local/wp-content/themes/business-gravity/assets/vendors/OwlCarousel2-2.2.1/owl.carousel.min.js' id='owlcarousel-js'></script>
<script type='text/javascript' src='http://blog.inlanefreight.local/wp-content/themes/business-gravity/assets/js/main.min.js' id='business-gravity-script-js'></script>
<script type='text/javascript' src='http://blog.inlanefreight.local/wp-content/themes/transport-gravity/js/custom.js?ver=1.0.0' id='transport-gravity-script-js'></script>
```

## Plugins

```sh
my@attack:~$ curl -s 'http://blog.inlanefreight.local' | grep plugins
```

```html title="Output"
<link rel='stylesheet' id='contact-form-7-css'  href='http://blog.inlanefreight.local/wp-content/plugins/contact-form-7/includes/css/styles.css?ver=5.4.2' type='text/css' media='all' />
<link rel='stylesheet' id='mm_frontend-css'  href='http://blog.inlanefreight.local/wp-content/plugins/mail-masta/lib/css/mm_frontend.css?ver=5.8' type='text/css' media='all' />
<script type='text/javascript' src='http://blog.inlanefreight.local/wp-content/plugins/mail-masta/lib/subscriber.js?ver=5.8' id='subscriber-js-js'></script>
<script type='text/javascript' src='http://blog.inlanefreight.local/wp-content/plugins/mail-masta/lib/jquery.validationEngine-en.js?ver=5.8' id='validation-engine-en-js'></script>
<script type='text/javascript' src='http://blog.inlanefreight.local/wp-content/plugins/mail-masta/lib/jquery.validationEngine.js?ver=5.8' id='validation-engine-js'></script>
<script type='text/javascript' src='http://blog.inlanefreight.local/wp-content/plugins/contact-form-7/includes/js/index.js?ver=5.4.2' id='contact-form-7-js'></script>
```

## Directory Listing

```sh
my@attack:~$ curl -s 'http://blog.inlanefreight.local/wp-content/plugins/mail-masta/' | html2text
```

```output title="Output"
# Index of /wp-content/plugins/mail-masta

![\[ICO\]](/icons/blank.gif)| [Name](?C=N;O=D)| [Last modified](?C=M;O=A)|
[Size](?C=S;O=A)| [Description](?C=D;O=A)
---|---|---|---|---

* * *

![\[PARENTDIR\]](/icons/back.gif)| [Parent Directory](/wp-content/plugins/)|  |  \- |
![\[DIR\]](/icons/folder.gif)| [amazon_api/](amazon_api/)| 2021-08-25 14:52 |  \- |
![\[DIR\]](/icons/folder.gif)| [inc/](inc/)| 2021-08-25 14:52 |  \- |
![\[DIR\]](/icons/folder.gif)| [lib/](lib/)| 2021-08-25 14:52 |  \- |
![\[   \]](/icons/unknown.gif)| [plugin-interface.php](plugin-interface.php)| 2021-08-25 14:52 |  88K|
![\[TXT\]](/icons/text.gif)| [readme.txt](readme.txt)| 2021-08-25 14:52 | 2.2K|

* * *

Apache/2.4.41 (Ubuntu) Server at blog.inlanefreight.local Port 80
```

## Username Enumeration

### Valid

```sh
my@attack:~$ curl -s 'http://blog.inlanefreight.local/wp-login.php' -d 'log=doug&pwd=password' | html2text | grep Error
```

```output title="Output"
**Error** : The password you entered for the username **doug** is incorrect.
```

### Invalid

```sh
my@attack:~$ curl -s 'http://blog.inlanefreight.local/wp-login.php' -d 'log=john&pwd=password' | html2text | grep Error
```

```output title="Output"
**Error** : The username **john** is not registered on this site. If you are
```

## WPScan

### Installation

```sh
my@attack:~$ sudo gem install wpscan
```

### Users

```sh
my@attack:~$ sudo wpscan --url "http://blog.inlanefreight.local" --enumerate u
```

### All Themes and All Plugins

```sh
my@attack:~$ sudo wpscan --url "http://blog.inlanefreight.local" --enumerate at,ap
```

```output title="Output" hl_lines="38 63 76 92 112 133 152 171"
Interesting Finding(s):

[+] Headers
 | Interesting Entry: Server: Apache/2.4.41 (Ubuntu)
 | Found By: Headers (Passive Detection)
 | Confidence: 100%

[+] XML-RPC seems to be enabled: http://blog.inlanefreight.local/xmlrpc.php
 | Found By: Direct Access (Aggressive Detection)
 | Confidence: 100%
 | References:
 |  - http://codex.wordpress.org/XML-RPC_Pingback_API
 |  - https://www.rapid7.com/db/modules/auxiliary/scanner/http/wordpress_ghost_scanner/
 |  - https://www.rapid7.com/db/modules/auxiliary/dos/http/wordpress_xmlrpc_dos/
 |  - https://www.rapid7.com/db/modules/auxiliary/scanner/http/wordpress_xmlrpc_login/
 |  - https://www.rapid7.com/db/modules/auxiliary/scanner/http/wordpress_pingback_access/

[+] WordPress readme found: http://blog.inlanefreight.local/readme.html
 | Found By: Direct Access (Aggressive Detection)
 | Confidence: 100%

[+] Upload directory has listing enabled: http://blog.inlanefreight.local/wp-content/uploads/
 | Found By: Direct Access (Aggressive Detection)
 | Confidence: 100%

[+] The external WP-Cron seems to be enabled: http://blog.inlanefreight.local/wp-cron.php
 | Found By: Direct Access (Aggressive Detection)
 | Confidence: 60%
 | References:
 |  - https://www.iplocation.net/defend-wordpress-from-ddos
 |  - https://github.com/wpscanteam/wpscan/issues/1299

[+] WordPress version 5.8 identified (Insecure, released on 2021-07-20).
 | Found By: Rss Generator (Passive Detection)
 |  - http://blog.inlanefreight.local/?feed=rss2, <generator>https://wordpress.org/?v=5.8</generator>
 |  - http://blog.inlanefreight.local/?feed=comments-rss2, <generator>https://wordpress.org/?v=5.8</generator>

[+] WordPress theme in use: transport-gravity
 | Location: http://blog.inlanefreight.local/wp-content/themes/transport-gravity/
 | Latest Version: 1.0.1 (up to date)
 | Last Updated: 2020-08-02T00:00:00.000Z
 | Readme: http://blog.inlanefreight.local/wp-content/themes/transport-gravity/readme.txt
 | [!] Directory listing is enabled
 | Style URL: http://blog.inlanefreight.local/wp-content/themes/transport-gravity/style.css
 | Style Name: Transport Gravity
 | Style URI: https://keonthemes.com/downloads/transport-gravity/
 | Description: Transport Gravity is an enhanced child theme of Business Gravity. Transport Gravity is made for tran...
 | Author: Keon Themes
 | Author URI: https://keonthemes.com/
 |
 | Found By: Css Style In Homepage (Passive Detection)
 | Confirmed By: Urls In Homepage (Passive Detection)
 |
 | Version: 1.0.1 (80% confidence)
 | Found By: Style (Passive Detection)
 |  - http://blog.inlanefreight.local/wp-content/themes/transport-gravity/style.css, Match: 'Version: 1.0.1'

[+] Enumerating All Plugins (via Passive Methods)
[+] Checking Plugin Versions (via Passive and Aggressive Methods)

[i] Plugin(s) Identified:

[+] contact-form-7
 | Location: http://blog.inlanefreight.local/wp-content/plugins/contact-form-7/
 | Last Updated: 2025-04-10T06:47:00.000Z
 | [!] The version is out of date, the latest version is 6.0.6
 |
 | Found By: Urls In Homepage (Passive Detection)
 |
 | Version: 5.4.2 (90% confidence)
 | Found By: Query Parameter (Passive Detection)
 |  - http://blog.inlanefreight.local/wp-content/plugins/contact-form-7/includes/css/styles.css?ver=5.4.2
 | Confirmed By: Readme - Stable Tag (Aggressive Detection)
 |  - http://blog.inlanefreight.local/wp-content/plugins/contact-form-7/readme.txt

[+] mail-masta
 | Location: http://blog.inlanefreight.local/wp-content/plugins/mail-masta/
 | Latest Version: 1.0 (up to date)
 | Last Updated: 2014-09-19T07:52:00.000Z
 |
 | Found By: Urls In Homepage (Passive Detection)
 |
 | Version: 1.0 (80% confidence)
 | Found By: Readme - Stable Tag (Aggressive Detection)
 |  - http://blog.inlanefreight.local/wp-content/plugins/mail-masta/readme.txt

[+] Enumerating All Themes (via Passive and Aggressive Methods)
[+] Checking Theme Versions (via Passive and Aggressive Methods)

[i] Theme(s) Identified:

[+] business-gravity
 | Location: http://blog.inlanefreight.local/wp-content/themes/business-gravity/
 | Latest Version: 1.2.6 (up to date)
 | Last Updated: 2021-05-04T00:00:00.000Z
 | Readme: http://blog.inlanefreight.local/wp-content/themes/business-gravity/readme.txt
 | Style URL: http://blog.inlanefreight.local/wp-content/themes/business-gravity/style.css
 | Style Name: Business Gravity
 | Style URI: https://keonthemes.com/downloads/business-gravity
 | Description: Business Gravity is a clean, responsive theme that&apos;s versatile and easy to use. Suitable for bo...
 | Author: Keon Themes
 | Author URI: https://keonthemes.com
 |
 | Found By: Urls In Homepage (Passive Detection)
 | Confirmed By: Known Locations (Aggressive Detection)
 |  - http://blog.inlanefreight.local/wp-content/themes/business-gravity/, status: 500
 |
 | Version: 1.2.6 (80% confidence)
 | Found By: Style (Passive Detection)
 |  - http://blog.inlanefreight.local/wp-content/themes/business-gravity/style.css, Match: 'Version: 1.2.6'

[+] transport-gravity
 | Location: http://blog.inlanefreight.local/wp-content/themes/transport-gravity/
 | Latest Version: 1.0.1 (up to date)
 | Last Updated: 2020-08-02T00:00:00.000Z
 | Readme: http://blog.inlanefreight.local/wp-content/themes/transport-gravity/readme.txt
 | [!] Directory listing is enabled
 | Style URL: http://blog.inlanefreight.local/wp-content/themes/transport-gravity/style.css
 | Style Name: Transport Gravity
 | Style URI: https://keonthemes.com/downloads/transport-gravity/
 | Description: Transport Gravity is an enhanced child theme of Business Gravity. Transport Gravity is made for tran...
 | Author: Keon Themes
 | Author URI: https://keonthemes.com/
 |
 | Found By: Urls In Homepage (Passive Detection)
 | Confirmed By: Known Locations (Aggressive Detection)
 |  - http://blog.inlanefreight.local/wp-content/themes/transport-gravity/, status: 200
 |
 | Version: 1.0.1 (80% confidence)
 | Found By: Style (Passive Detection)
 |  - http://blog.inlanefreight.local/wp-content/themes/transport-gravity/style.css, Match: 'Version: 1.0.1'

[+] twentynineteen
 | Location: http://blog.inlanefreight.local/wp-content/themes/twentynineteen/
 | Last Updated: 2025-04-15T00:00:00.000Z
 | Readme: http://blog.inlanefreight.local/wp-content/themes/twentynineteen/readme.txt
 | [!] The version is out of date, the latest version is 3.1
 | Style URL: http://blog.inlanefreight.local/wp-content/themes/twentynineteen/style.css
 | Style Name: Twenty Nineteen
 | Style URI: https://wordpress.org/themes/twentynineteen/
 | Description: Our 2019 default theme is designed to show off the power of the block editor. It features custom sty...
 | Author: the WordPress team
 | Author URI: https://wordpress.org/
 |
 | Found By: Known Locations (Aggressive Detection)
 |  - http://blog.inlanefreight.local/wp-content/themes/twentynineteen/, status: 500
 |
 | Version: 2.1 (80% confidence)
 | Found By: Style (Passive Detection)
 |  - http://blog.inlanefreight.local/wp-content/themes/twentynineteen/style.css, Match: 'Version: 2.1'

[+] twentytwenty
 | Location: http://blog.inlanefreight.local/wp-content/themes/twentytwenty/
 | Last Updated: 2025-04-15T00:00:00.000Z
 | Readme: http://blog.inlanefreight.local/wp-content/themes/twentytwenty/readme.txt
 | [!] The version is out of date, the latest version is 2.9
 | Style URL: http://blog.inlanefreight.local/wp-content/themes/twentytwenty/style.css
 | Style Name: Twenty Twenty
 | Style URI: https://wordpress.org/themes/twentytwenty/
 | Description: Our default theme for 2020 is designed to take full advantage of the flexibility of the block editor...
 | Author: the WordPress team
 | Author URI: https://wordpress.org/
 |
 | Found By: Known Locations (Aggressive Detection)
 |  - http://blog.inlanefreight.local/wp-content/themes/twentytwenty/, status: 500
 |
 | Version: 1.8 (80% confidence)
 | Found By: Style (Passive Detection)
 |  - http://blog.inlanefreight.local/wp-content/themes/twentytwenty/style.css, Match: 'Version: 1.8'

[+] twentytwentyone
 | Location: http://blog.inlanefreight.local/wp-content/themes/twentytwentyone/
 | Last Updated: 2025-04-15T00:00:00.000Z
 | Readme: http://blog.inlanefreight.local/wp-content/themes/twentytwentyone/readme.txt
 | [!] The version is out of date, the latest version is 2.5
 | Style URL: http://blog.inlanefreight.local/wp-content/themes/twentytwentyone/style.css
 | Style Name: Twenty Twenty-One
 | Style URI: https://wordpress.org/themes/twentytwentyone/
 | Description: Twenty Twenty-One is a blank canvas for your ideas and it makes the block editor your best brush. Wi...
 | Author: the WordPress team
 | Author URI: https://wordpress.org/
 |
 | Found By: Known Locations (Aggressive Detection)
 |  - http://blog.inlanefreight.local/wp-content/themes/twentytwentyone/, status: 500
 |
 | Version: 1.4 (80% confidence)
 | Found By: Style (Passive Detection)
 |  - http://blog.inlanefreight.local/wp-content/themes/twentytwentyone/style.css, Match: 'Version: 1.4'
```
