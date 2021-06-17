# Wasm Protected Site 2:webex:250pts
- Similar to wasm protected site 1, but this time there is no password, only the flag.  
- Enter the flag, and the program will check it for you  

[http://web.bcactf.com:49158/](http://web.bcactf.com:49158/)  
  
Hint 1 of 1  
What does the wasm do to compare each byte  

# Solution
When I visit the site, it seems that the login form is running in wasm as well as [Wasm Protected Site 1] (../Wasm_Protected_Site_1).
Looking at the network, it seems that `http://web.bcactf.com:49158/code.wasm` is also the main body (you can read js).
You can't get anything with wget and strings.
Looking at the debugger, it was as follows.
! [wasm.png] (images / wasm.png)
It seems that the character string is encrypted, so read it in order from the top.
```wasm
(module
  (memory $memory0 1)
  (export "memory" (memory $memory0))
  (export "checkFlag" (func $checkFlag))
  (func $cmp (;0;) (param $v0 (;0;) i32) (param $v1 (;1;) i32) (result i32)
    (local $v2 (;2;) i32)
    loop $label0
      local.get $v2
      local.get $v0
      i32.add
      i32.load8_u
      local.get $v2
      local.get $v1
      i32.add
      i32.load8_u
      local.get $v2
      i32.const 9
      i32.mul
      i32.const 127
      i32.and
      i32.xor
      i32.ne
      local.get $v2
      i32.const 27
      i32.ne
      i32.and
      if
        i32.const 0
        return
      end
      local.get $v2
      i32.const 1
      i32.add
      local.tee $v2
      i32.const 1
      i32.sub
      local.get $v0
      i32.add
      i32.load8_u
      i32.eqz
      if
        i32.const 1
        return
      end
      br $label0
    end $label0
    i32.const 0
    return
  )
  (func $checkFlag (;1;) (param $a (;0;) i32) (result i32)
    local.get $a
    i32.const 1000
    call $cmp
    return
  )
  (data (i32.const 1000) "bjsxPKMH|\227N\1bD\043b]PR\19e%\7f/;\17")
)
```
In summary, the ciphertext character ^ ((ciphertext character index * 9) & 127) `is decrypted and compared to the input character.
Since the ciphertext is hard to see, use the console to execute a part of js in the site and see the contents of the memory numerically. 
```js
> console.log(memory = new Uint8Array(wasm.instance.exports.memory.buffer).toString())
> 0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,98,106,115,120,80,75,77,72,124,34,55,78,27,68,4,51,98,93,80,82,25,101,37,127,47,59,23,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0

```
Decrypt with the following wasm_dec.py.  
```python
:wasm_dec.py
crypto = [98,106,115,120,80,75,77,72,124,34,55,78,27,68,4,51,98,93,80,82,25,101,37,127,47,59,23]

for i in range(len(crypto)):
    print(chr(crypto[i] ^ ((i * 9) & 127)), end="")

print()
```

Execute.  
```bash
$ python wasm_dec.py
bcactf{w4sm-w1z4rDry-Xc0wZ}
```

Flag: ```bcactf{w4sm-w1z4rDry-Xc0wZ}```