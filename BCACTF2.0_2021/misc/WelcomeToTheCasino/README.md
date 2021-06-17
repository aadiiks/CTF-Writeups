# Welcome to the Casino

#### Category : misc
#### Points : 125
#### Author : micpap25

## Challenge
Can you get three-of-a-kind on this slot machine? Let's find out!

```nc misc.bcactf.com 49156```

## Solution
When we connect to the given server, it tells us to send a random lower case alphabet and then spins the Lotto. 

It is clear from the challenge description, that the goal is to get 3 same letters in a row after spinning. So, we need to make a script to achieve the same.

Our script first needs to parse the letter that we need to send, send the letter and wait till the flag which starts with `bcactf{` comes. If it doesn't come, then it needs to connect again.

I came up with the following script :
```python
#!/usr/bin/python2
from pwn import *
host = "misc.bcactf.com"
port = 49156
while True:
    try:
        s = remote(host, port)
    except:
        continue
    print(s.recvuntil("Let's see"))
    print(s.recv())
    print(s.recv())
    key = (s.recv().split('"')[1])
    s.sendline(key)
    try:
        print(s.recvuntil("bcactf"))
        print(s.recv())
        break
    except:
        continue

```

Link to the script : [solve.py](https://github.com/p1xxxel/ctf-writeups/blob/main/2021/BCACTF%202.0/Welcome%20to%20the%20Casino/solve.py)
(While running the script, keep in mind it is in python2)

To speed the process, I had to run this script in seven terminals and I got the following flag :
![flag](https://github.com/p1xxxel/ctf-writeups/blob/main/2021/BCACTF%202.0/Welcome%20to%20the%20Casino/flag.png)

Flag : ```bcactf{y0u_g0t_1ucKy_af23dd97g64n}```

#### Further explanation of the script :
It first tries to connect to the server. If it can't connect it keeps retrying.
Then it gets they key to send.
After that it recieves text till `bcactf` and if it doesn't and exception occurs due to which it continues the loop.
If it does recieve `bcactf`, it prints the whole flag and then breaks the loop.


---

### 2nd Method

# Welcome to the Casino:misc:150pts

Can you get three-of-a-kind on this slot machine? Let's find out!  
```nc misc.bcactf.com 49156```

# Solution
When I try to access it with, I am told to press a key and a mysterious spin begins.  
```bash
$ nc misc.bcactf.com 49156
 /$$                           /$$
| $$                          | $$
| $$       /$$   /$$  /$$$$$$$| $$   /$$ /$$   /$$
| $$      | $$  | $$ /$$_____/| $$  /$$/| $$  | $$
| $$      | $$  | $$| $$      | $$$$$$/ | $$  | $$
| $$      | $$  | $$| $$      | $$_  $$ | $$  | $$
| $$$$$$$$|  $$$$$$/|  $$$$$$$| $$ \  $$|  $$$$$$$
|________/ \______/  \_______/|__/  \__/ \____  $$
                                         /$$  | $$
                                        |  $$$$$$/
                                         \______/
 /$$                   /$$     /$$
| $$                  | $$    | $$
| $$        /$$$$$$  /$$$$$$ /$$$$$$    /$$$$$$
| $$       /$$__  $$|_  $$_/|_  $$_/   /$$__  $$
| $$      | $$  \ $$  | $$    | $$    | $$  \ $$
| $$      | $$  | $$  | $$ /$$| $$ /$$| $$  | $$
| $$$$$$$$|  $$$$$$/  |  $$$$/|  $$$$/|  $$$$$$/
|________/ \______/    \___/   \___/   \______/



Welcome to the Lucky Lotto Slot Machine!
Let's see if you're today's big winner!
Enter the letter "d" to pull the lever...
d
Spinning...
           [[[ e ]]]
           [[[ p ]]]
           [[[ i ]]]

You didn't win anything. Try matching more letters next time!

Come back next time!
```
I think it should be aligned, but it seems that it will take some time from execution to the result.
Also, the key seems to be specified in any lowercase alphabet for each execution.
It can be executed multiple times at once using the following spin.sh.

```sh:spin.sh
echo -e "abcdefghijklmnopqrstuvwxyz\n" | nc misc.bcactf.com 49156 | grep ctf &
echo -e "abcdefghijklmnopqrstuvwxyz\n" | nc misc.bcactf.com 49156 | grep ctf &
~~~
echo -e "abcdefghijklmnopqrstuvwxyz\n" | nc misc.bcactf.com 49156 | grep ctf &
echo -e "abcdefghijklmnopqrstuvwxyz\n" | nc misc.bcactf.com 49156 | grep ctf &
```
Execute  

```bash
$ ./spin.sh
$ bcactf{y0u_g0t_1ucKy_af23dd97g64n}
```


Flag - ```bcactf{y0u_g0t_1ucKy_af23dd97g64n}```