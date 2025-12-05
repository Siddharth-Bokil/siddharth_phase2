# 1. All Signs Align

> *None*


## Solution:

- Initially, I read the contents out of `out.txt` as stored it as a list.
- The line `if pow(x,(p-1)//2,p) == 1:` in the provided code revealed that `x` was always a quadratic residue.
- Now, the only difference between the output for bit 0 and bit 1 was that it was multiplied by -1 for bit 1. Since -1 is a non-quadratic residue of the given p, the bit could be decrypted by using the multiplicity property of quadratic and non-quadratic residues.
- So `legs[]` contains either `1` or `p-1` based on which the correspoing bit can be found.
- So there were 2 possible mappings for each bit, based on the Legrande symbol of `a`.
- If leg(a) = 1 then outputs are 1 (for bit 0) and p-1 (for bit 1) and if leg(a) = p-1 then outputs are p-1 (for bit 0) and 1 (for bit 1).
- Trying both configurations, the latter produced readable text.
- The code below gave the flag.

```python
from Crypto.Util.number import *

p = 9129026491768303016811207218323770273047638648509577266210613478726929333106121387323539916009107476349319902011390210650434835260358014251332047605739279


s = open("out.txt",'r').read()
s = s.strip()[1:-1]
out = list(map(int,s.split(',')))       # Take each individual number in list

legs=[]
for output in out:
    legs.append(pow(output, (p - 1) // 2, p))  # Legendre symbol

def bitConvert(legs):
    bits = ""
    for l in legs:
        if l==p-1:
            bits = bits + '0'
        elif l==1:
            bits = bits + '1'
    return bits
            
bits = bitConvert(legs)
bytes = long_to_bytes(int(bits,2))
print(bytes.decode())
```


## Flag:

```
nite{r3s1du35_f4ll1ng_1nt0_pl4c3}
```


## Concepts learnt:

- If `c^2 = x mod p`, then x is a quadratic residue of p given that p is prime and is not divisible by x.
- Euler's theorem for quadratic residues - `pow(x,p-1//2)` is `1 mod p` if x is a QR and `-1 mod p = p-1` if x is a NQR.
- Multiplicative property - `QR * QR = QR`, `NQR * QR = NQR`.
- Legrande symbol - If x is a QR, leg(x) = 1. If X is a NQR, leg(x) = -1.


## Notes:

This challenge was more about the math. It took me a long time to solve this challenge but the books provided in the resources are pretty helpful. It was fun to finally see the flag but the challenge is too mathematically technical, don't think I would remember this math after 5 days from now.


## Resources:

- An introduction to mathematical cryptography [here](https://www.scribd.com/document/906877978/An-Introduction-to-Mathematical-Cryptography-2nd-Edition-Hoffstein)
- Serious cryptography [here](https://www.kea.nu/files/textbooks/humblesec/seriouscrytography.pdf)




<br><br><br>
***
<br><br><br>




# 2. Residue Refinery

> *None*

## Solution:

- The multiplication part of `class Num` suggested it is like a linear transformation.
- The byte order is reversed for every 2 bytes so I had to simply reverse the bytes everytime.
- Since we already had the ciphertext for the first 2 values of plaintext, it was quite mathematical to figure out the coefficient matrix in the linear transformation.
- Once the coefficient matrix was found, it could be applied to reverse every other ciphertext and the plaintext in readable english was the flag.

```python
p = 257
ct_hex = "9813d3838178abd17836f3e2e752a99d5cd3fba291205f90c1d0a78b6eca"
ct = bytes.fromhex(ct_hex)
x1, x2 = 0x31, 0x6d

def modinv(a, m):
    return pow(a, -1, m)

a = x1
b = x2
C2_init, C1_init = ct[0], ct[1]

det = (a*a - 3*b*b) % p
inv_det = modinv(det, p)
k1 = (inv_det * ((a*C1_init - 3*b*C2_init) % p)) % p
k2 = (inv_det * ((-b*C1_init + a*C2_init) % p)) % p

coeffA, coeffB, coeffC, coeffD = k1, (3*k2) % p, k2, k1
detA = (coeffA * coeffD - coeffB * coeffC) % p
invA = modinv(detA, p)

plain = bytearray()
for i in range(0, len(ct), 2):
    C2, C1 = ct[i], ct[i+1]
    f0 = (invA * ((coeffD * C1 - coeffB * C2) % p)) % p
    f1 = (invA * ((-coeffC * C1 + coeffA * C2) % p)) % p
    plain.extend([f0, f1])

print(plain.decode())
```


## Flag:

```
nite{1mp0r7_m0dul3?_1_4M_7h3_m0dul3}
```


## Concepts learnt:

- Block ciphers - Similar to matirx multiplications and linear trnansformations
- Finding coefficients from given input and corresponding output


## Notes:

Really had to dig through the book pdfs to find some help. I was doing it right but I did not swap the 2 values otherwise would've finished it much quicker.


## Resources:

- [Intro to crypto](https://www.scribd.com/document/906877978/An-Introduction-to-Mathematical-Cryptography-2nd-Edition-Hoffstein)
- [Serious cryptography](https://www.kea.nu/files/textbooks/humblesec/seriouscrytography.pdf)
- Python functions [here](https://www.geeksforgeeks.org/python/python-os-urandom-method/)




<br><br><br>
***
<br><br><br>




# 3. Quixorte

> *None*


## Solution:

- The png file has been encrypted using a sliding XOR along with byte rotation.
- The png is decryptable since we know the first 8 bytes of a png are always fixed. This allows us to figure out the key and this key is used throughout the entire png and hence can be decrypted.
- The code for finding the key is given below.

```python
png = bytes([0x89, 0x50, 0x4E, 0x47, 0x0D, 0x0A, 0x1A, 0x0A])

with open("quote.png.enc", "rb") as f:
    enc = f.read(8)
    
def ror(b, i):
    s = i % 8
    return ((b >> s) | (b << (8 - s)) & 0xFF) & 0xFF

rot = bytes(ror(png[i], i) for i in range(8))
S = [enc[i] ^ rot[i] for i in range(8)]

key = [S[0]]
for i in range(1, 8):
    key.append(S[i] ^ S[i-1])

print(bytes(key).hex())
```

- Now that the key is known, we can use the property `ciphertext ^ key = plaintext` to find out the original bytes and hence decrypt the png.
- Since the bytes were rotated to the right, rotating to the left after XORing gives us the correct bytes.
 
```python
key = bytes.fromhex("ec95e0220a3d5ab7")
enc = bytearray(open("quote.png.enc", "rb").read())

def rol(b, i):
    s = i % 8
    return ((b << s) & 0xFF) | (b >> (8 - s))

for i in reversed(range(len(enc) - len(key) + 1)): # Undo XOR
    for j in range(len(key)):
        enc[i + j] ^= key[j]

flag = bytearray(rol(enc[i], i) for i in range(len(enc))) # Undo Rotation

image = open("quote.png", "wb")
image.write(flag)
```
<img width="457" height="450" alt="image" src="https://github.com/user-attachments/assets/83784fd4-8d41-457d-a671-427ebc672c89" />


## Flag:

```
nite{t0_b3_X0R_n0t_t0_b3333}
```


## Concepts learnt:

- Xor Properties -
      - Plaintext ^ Key = Ciphertext
      - Ciphertext ^ Key = Plaintext
      - Plaintext ^ Ciphertext = Key
- Byte rotation - Left rotate and right rotate
- Magic numbers

## Notes:

Initially I thought the flag was written as text in the png bytes in some way, instead of being written in the image itself. The sliding-window XOR was confusig initially but it felt easier to decrypt than the previous challenge. XOR seems to have a lot of properties that make it a bad method of encryption.


## Resources:

- Basic XOR properties [video](https://www.youtube.com/watch?v=xK_SqWG9w-Y)
- Common file signatures [here](https://en.wikipedia.org/wiki/List_of_file_signatures)
- [Intro to crypto](https://www.scribd.com/document/906877978/An-Introduction-to-Mathematical-Cryptography-2nd-Edition-Hoffstein)
- [Serious cryptography](https://www.kea.nu/files/textbooks/humblesec/seriouscrytography.pdf)




<br><br><br>
***
<br><br><br>




# 4. Willy's Chocolate Experience

> *None*


## Solution:

- The challenge converts the flag into a large integer using bytes_to_long. This integer is called the ticket.
- Since ticket is unknown, the goal is to recover it from two consecutive values of a function `f(m) = 13^m + 37^m (mod p)`
- We simplify the expression and solve the discrete log problem using Baby Step Giant Step and Pohlig-Hellman algorithms.
- All the partial results are then combined using the Chinese Remainder Theorem, which gives the unique value of (m-1) inside the group.
- Adding 1 to this value and converting to bytes gives us the flag.

```python
from collections import Counter
from Crypto.Util.number import inverse, long_to_bytes

p = 396430433566694153228963024068183195900644000015629930982017434859080008533624204265038366113052353086248115602503012179807206251960510130759852727353283868788493357310003786807
A = 208271276785711416565270003674719254652567820785459096303084135643866107254120926647956533028404502637100461134874329585833364948354858925270600245218260166855547105655294503224
B = 124499652441066069321544812234595327614165778598236394255418354986873240978090206863399216810942232360879573073405796848165530765886142184827326462551698684564407582751560255175
g = 37

x = ((A - 13 * B) * inverse(24, p)) % p

def factorTrial(n, bound=2000000):
    f = Counter()
    d = 2
    while d * d <= n and d <= bound:
        while n % d == 0:
            f[d] += 1
            n //= d
        d = 3 if d == 2 else d + 2
    return f, n

def bsgs(base, target, modp, limit):
    m = int(pow(limit,0.5)) + 1
    table = {}
    cur = 1
    for j in range(m):
        if cur not in table:
            table[cur] = j
        cur = (cur * base) % modp

    step = pow(inverse(base, modp), m, modp)
    gamma = target
    for i in range(m + 1):
        if gamma in table:
            return (i * m + table[gamma]) % limit
        gamma = (gamma * step) % modp
    return None

def hellman(base, target, modp):
    N = modp - 1
    facs, rem = factorTrial(N)
    if rem != 1:
        facs[int(rem)] += 1

    parts = []
    for q, e in facs.items():
        pe = q ** e
        g0 = pow(base, N // pe, modp)
        h0 = pow(target, N // pe, modp)
        t = bsgs(g0, h0, modp, pe)
        parts.append((t, pe))

    M = 1
    for _, ni in parts:
        M *= ni

    xcrt = 0
    for ai, ni in parts:
        mi = M // ni
        xcrt = (xcrt + ai * mi * inverse(mi, ni)) % M
    return xcrt

k = hellman(g, x, p)
m = k + 1
print(long_to_bytes(m).decode())
```


## Flag:

```
nite{g0ld3n_t1ck3t_t0_gl4sg0w}
```


## Concepts learnt:

- Pohlig-Hellman algorithm for computing discrete logs
- Baby step giant step algorithm for computing discrete logs
- Chinese remainder theorem


## Notes:
This challenge took a very long time, almost 2 days and there's a lot of math in this challenge. I had really search through the internet to find different algorithms and piece them together to make the code for recovering the flag. The book provided in the resources told me what I needed to do and then I found algorithms till something worked. There's still a lot of math used in this challenge that I don't fully understand but it worked so i'm happy. However, this challenge has made me rethink my choice of cryptography as a domain.


## Resources:

- [Intro to crypto](https://www.scribd.com/document/906877978/An-Introduction-to-Mathematical-Cryptography-2nd-Edition-Hoffstein)
- BSGS algo [here](https://mvpworkshop.co/baby-step-giant-step-algorithm/)
- Pohlig-Hellman algo [here](https://github.com/christiankrug/pohlig_hellman/blob/master/main.py)



<br><br><br>
***
<br><br><br>




# 5. spAES Oddity

> *None*


## Solution:

- Include as many steps as you can with your thought process
- You **must** include images such as screenshots wherever relevant.

```
put codes & terminal outputs here using triple backticks

you may also use ```python for python codes for example
```


## Flag:

```

```


## Concepts learnt:

- Include the new topics you've come across and explain them in brief
- 


## Notes:

- Include any alternate tangents you went on while solving the challenge, including mistakes & other solutions you found.
- 


## Resources:

- Include the resources you've referred to with links. [example hyperlink](https://google.com)
- 


