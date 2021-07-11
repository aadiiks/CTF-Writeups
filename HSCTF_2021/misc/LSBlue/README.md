# Challenge Name: LSBlue

## Description

Orca watching is an awesome pastime of mine!

![lsblue.png](https://i.imgur.com/L279E4R.png)

## Detailed solution

Checking the image details 
```bash
┌──(kali㉿kali)-[~]
└─$ file lsblue.png
lsblue.png: PNG image data, 700 x 420, 8-bit/color RGB, non-interlaced
```
```bash
┌──(kali㉿kali)-[~]
└─$ exiftool lsblue.png
ExifTool Version Number         : 12.16
File Name                       : lsblue.png
Directory                       : .
File Size                       : 412 KiB
File Modification Date/Time     : 2021:06:16 22:16:38+00:00
File Access Date/Time           : 2021:06:19 20:04:27+00:00
File Inode Change Date/Time     : 2021:06:19 20:04:27+00:00
File Permissions                : rw-r--r--
File Type                       : PNG
File Type Extension             : png
MIME Type                       : image/png
Image Width                     : 700
Image Height                    : 420
Bit Depth                       : 8
Color Type                      : RGB
Compression                     : Deflate/Inflate
Filter                          : Adaptive
Interlace                       : Noninterlaced
Image Size                      : 700x420
Megapixels                      : 0.294
``` 

The filename has a hint LSB + Blue so we have LSB steganography in PNG. 
  
Let's use [zsteg](https://github.com/zed-0xff/zsteg) to find hidden data 

![image](https://i.imgur.com/yL8wUtK.png)

As we can see the payload **b1,b,lsb,xy** give us the flag  

### Flag

```
flag{0rc45_4r3nt_6lu3_s1lly_4895131}
```

## OR

open it via stegsolv ans use the LSB function
```bash
java -jar /opt/stegsolve.jar
```
![](https://i.imgur.com/TKZtfKM.png)


### Flag
```
flag{0rc 45_4r3nt_6lu3_s1 lly_4895131}
```