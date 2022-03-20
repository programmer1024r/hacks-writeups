# Cryptohack write-ups
### You either know, XOR you don't
You given a hex string that was xored with a secret key. 
`0e0b213f26041e480b26217f27342e175d0e070a3c5b103e2526217f27342e175d0e077e263451150104` <br/>
and the hint is to remember the flag format (`crypto{}`).

First thing you need to convert the hex to bytes. 
The idea of this challenge is that the first 7 bytes will always be `crypto{`
meaning we have the solution for the first 7 bytes.<br>

**The equation is:**<br>
`message ^ key = xored message`. <br>
Message in range [0-7] bytes
- message = `crypto{`
- xored message = `0e0b213`
- key = `0e0b213` ^ `crypto{` = `myXORke`

if we try now to use this key we get `crypto{%r~n-LQCn\x05UaY6ifjtJ\x1aMvX\x0fb\x13l\x1eja`, this is because the key is not finish.
The key needs to be 8 bytes because the flage xored message is 42 bytes. 
If you trie to xor the xored message with the new key `myXORkey`, the result will be `crypto{1f_y0u_Kn0w_En0uGH_y0u_Kn0w_1t_4ll}`
Congratulations!

### Solution
```Python
from pwn import xor
HEADER_LEN = 7

message = bytes.fromhex("0e0b213f26041e480b26217f27342e175d0e070a3c5b103e2526217f27342e175d0e077e263451150104")
print(len(message))


key = xor(message[:HEADER_LEN], "crypto{".encode())
print("key that we got: ", key)


key = "myXORkey".encode()
print(xor(message, key).decode()) 
```
## Modular Arithmetics
Mod = modulus (%), 19 = 24 (mod 5). <br>

#### Fermat's little theorem [WIKI](https://en.wikipedia.org/wiki/Fermat%27s_little_theorem)
prime filed {-p+1,..-2, -1, 0, 1, ...., p-1}<br>
```
p = prime number
n^p mod p = n
n^p-1 mod p = 1
```

#### Multiplicative Inverse of `g`
```
g * d = 1 mod p
1 mod p = almost always 1
meaning when does g*d mod p = 1
```
#### Quadratic Residue
```
a^2 = x mod p
```
x is the quadratic residue


### python conversion
- `bytes.fromhex()`
- `bytes.hex()`
- `int.to_bytes()` 
- `int.from_bytes()`
- `bytes.decode()` 

Examples
```Python
integer_length = (int_var.bit_length() + 7) // 8
# little will make th bytes in reversed order
int_var.to_bytes(int_len, 'big')

from pwn import xor
```
