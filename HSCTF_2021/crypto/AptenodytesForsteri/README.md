# Challenge Name: aptenodytes-forsteri

## Description

Here's a warmup cryptography challenge. Reverse the script, decrypt the output, submit the flag.

- [aptenodytes-forsteri.py](aptenodytes-forsteri.py)  
- [output.txt](output.txt)

## Detailed solution  

We need to reverse the script and use the output to decrypt the flag  

- The flag is a string that start with flag{ and end with }   
- Uppercase letters used in encoding   
- We need to find the flag characters that it give us the encoded characters (output.txt) with letters[(letters.index(flag_chars)+18)%26]  

A python script to decrypt the output  

```python

letters = "ABCDEFGHIJKLMNOPQRSTUVWXYZ"
flag = ""
encoded = "IOWJLQMAGH"
for i in encoded:
    for j in letters:
      if letters[(letters.index(j)+18)%26] == i:
        flag+=j
print("flag{"+ flag + "}")
```
### Flag

```
flag{QWERTYUIOP}
```

## OR

We are given 2 file: 
First is aptenodytes-forsteri.py

```python
flag = open('flag.txt','r').read() #open the flag
    
assert flag[0:5]=="flag{" and flag[-1]=="}" #flag follows standard flag format
    
letters = "ABCDEFGHIJKLMNOPQRSTUVWXYZ"
    
encoded = ""
    
for character in flag[5:-1]:
    encoded+=letters[(letters.index(character)+18)%26] #encode each character
print(encoded)
```

second is output.txt

```
IOWJLQMAGH
```

I made a flag.txt file as the script takes in flag.txt file.
According to the script the script only takes in the characters between the curly brackets of the flag format i.e flag{}
And it also allows only capital letters.

So i made a flag.txt file with all charaters A-Z in the flag format.

```
flag{ABCDEFGHIJKLMNOPQRSTUVWXYZ}
```

![](https://i.imgur.com/1Aa0Lre.png)


Now i got the perspective encoded values of each letter so now I can compare it with the output.txt string.

```
A B C D E F G H I J K L M N O P Q R S T U V W X Y Z
S T U V W X Y Z A B C D E F G H I J K L M N O P Q R
    
I O W J L Q M A G H
Q W E R T Y U I O P
```

We can verify it by passing the flag we have in the flag.txt file and see if it matches with output.txt

![](https://i.imgur.com/EB638Lz.png)

It matches hence we have the right flag.

### Flag

```
flag{QWERTYUIOP}
```