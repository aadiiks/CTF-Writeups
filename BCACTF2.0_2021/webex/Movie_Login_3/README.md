![question](https://i.imgur.com/le9cXkk.png)
1) Another login page is at the link

2) looking at the deny list `[
    "and",
    "1",
    "0",
    "true",
    "false",
    "/",
    "*",
    "=",
    "xor",
    "null",
    "is",
    "<",
    ">"
]` it's a little more in depth than last time.

3) We can still bypass is with ```' or '5' - '4```
or we could use ```' OR 2 --```

4) Enter the above for the username/password

5) We got the flag!

![flag](https://i.imgur.com/aKhTjmw.png)

Flag: ```bcactf{gu3ss_th3r3s_n0_st0pp1ng_y0u!}```