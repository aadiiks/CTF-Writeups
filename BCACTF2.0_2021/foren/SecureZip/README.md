![question](https://i.imgur.com/SVXLUhw.png)
1) `unzip chall.zip` shows that it's password protected

2) Lets try and crack it with john
 
```bash
zip2john chall.zip > zippy
john zippy --wordlist=/usr/share/wordlists/rockyou.txt
```

3) Output gives password as "dogedoge".

4) Gives two items "homework.txt" "flag.txt".

5) After doing cat on the flag.txt we get our flag.
```bash
cat flag.txt
```

Flag: ```bcactf{cr4ck1ng_z1p_p455w0rd5_15_fun_a12ca37bdacef7}```
