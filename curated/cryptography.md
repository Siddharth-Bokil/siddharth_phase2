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

```
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

- This challenge was more about the math. It took me a long time to solve this challenge but the books provided in the resources are pretty helpful. It was fun to finally see the flag but the challenge is too mathematically technical, don't think I would remember this math after 5 days from now.


## Resources:

- An introduction to mathematical cryptography [here](file:///home/sid/Downloads/An%20Introduction%20to%20Mathematical%20Cryptography%20%5BHoffstein-Pipher-Silverman%5D%20(2014).pdf)
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

```
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

- Really had to dig through the book pdfs to find some help. I was doing it right but I did not swap the 2 values otherwise would've finished it much quicker.


## Resources:

- [Intro to crypto](file:///home/sid/Downloads/An%20Introduction%20to%20Mathematical%20Cryptography%20%5BHoffstein-Pipher-Silverman%5D%20(2014).pdf)
- [Serious cryptography](https://www.kea.nu/files/textbooks/humblesec/seriouscrytography.pdf)
- Python functions [here](https://www.geeksforgeeks.org/python/python-os-urandom-method/)




<br><br><br>
***
<br><br><br>




# 3. Quixorte

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




<br><br><br>
***
<br><br><br>




# 4. Willy's Chocolate Experience

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


