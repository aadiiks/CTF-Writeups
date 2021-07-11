# Challenge Name: pallets-of-gold

## Description

It doesn't really look like gold to me...

![pallets-of-gold.png](https://i.imgur.com/4Bcc1ZP.png)

## Detailed solution

Checking the image file 

```bash
┌──(kali㉿kali)-[~]
└─$ file pallets-of-gold.png
pallets-of-gold.png: PNG image data, 3191 x 227, 8-bit colormap, non-interlaced
``` 

```bash
┌──(kali㉿kali)-[~]
└─$ exiftool pallets-of-gold.png
ExifTool Version Number         : 12.16
File Name                       : pallets-of-gold.png
Directory                       : .
File Size                       : 683 KiB
File Modification Date/Time     : 2021:06:16 21:50:39+00:00
File Access Date/Time           : 2021:06:20 00:53:55+00:00
File Inode Change Date/Time     : 2021:06:20 00:53:55+00:00
File Permissions                : rw-r--r--
File Type                       : PNG
File Type Extension             : png
MIME Type                       : image/png
Image Width                     : 3191
Image Height                    : 227
Bit Depth                       : 8
Color Type                      : Palette
Compression                     : Deflate/Inflate
Filter                          : Adaptive
Interlace                       : Noninterlaced
Palette                         : (Binary data 768 bytes, use -b option to extract)
Image Size                      : 3191x227
Megapixels                      : 0.724
``` 
Challenge name pallets is refering to colour palette (bitmap) 

Let's use stegonline to check for colour palette https://stegonline.georgeom.net/upload 

![image](https://i.imgur.com/rIhaxrr.png)

Let's browse colour palette

![image](https://i.imgur.com/o1dEqpD.png)

![image](https://i.imgur.com/5DNpOmA.png)

We can see our flag 




### Flag

```
flag{plte_chunks_remind_me_of_gifs}
```


## OR

open it via stegsolv
```bash
java -jar /opt/stegsolve.jar
```
![](https://i.imgur.com/C1DyoU2.png)

###Flag

```
flag{plte_chunks_remins_me_of_qils}
```