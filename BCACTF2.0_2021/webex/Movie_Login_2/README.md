![question](https://i.imgur.com/iRgBL7S.png)
1) At the link we are greeted with another login page.

2)  the deny list shows that we can't use `[
    "1",
    "0",
    "/",
    "="
]`. So our technique of `' or '1' = '1` won't work this time. 

2) We can achieve the same affect with ```' or '5' > '4```
another way was ```' OR true --```

3) enter the above for both the username and password.

4) We got the flag!

![flag](https://i.imgur.com/9kpJFlh.png)

5) Flag: ```bcactf{h0w_d1d_y0u_g3t_h3r3_th1s_t1m3?!?}```