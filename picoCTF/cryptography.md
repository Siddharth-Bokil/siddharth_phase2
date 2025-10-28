# 1. RSA Oracle
> Can you abuse the oracle? An attacker was able to intercept communications between a bank and a fintech company. They managed to get the message (ciphertext) and the password that was used to encrypt the message. After some intensive reconassainance they found out that the bank has an oracle that was used to encrypt the password and can be found here nc titan.picoctf.net 51777. Decrypt the password and use it to decrypt the message. The oracle can decrypt anything except the password.

## Solution:

- This one took a lot of time and I would say should be a hard rated challenge. On connecting to the nc port, you can either encrypt or decrypt and obviously decrypting the password didn't work. After doing some researching and checking the hints, I found a property of the RSA algo that helps decrypt the flag.
- The property is basically if I have `m^e mod n` (which is given) and I can find `x^e mod n` (which can be found by encrypting through the nc), then multiplying the two numbers gives `(x*m)^e mod n` which can then be decrypted and divided by x to find m.
```math
m^e mod n =  2575135950983117315234568522857995277662113128076071837763492069763989760018604733813265929772245292223046288098298720343542517375538185662305577375746934

2^e mod n = 5067313465613043651275429665315895824157755779222372979446076012356324498190828210335763979330272318657269048435311897896433721115606764442199497891219230
```

- I chose x as 2 and this is where I got my first big problem. I wanted to input a sequence whose hex is 2, since the nc was encrypting the hex instead of raw input. Then I found a solution to this as providing the input through `echo -e` and `sleep` which I learnt during Clutter Overflow in Binary Exploitation.
```
{ printf 'e\n'; sleep 3; echo -e '\x02\n'; } | nc titan.picoctf.net 61347
```

- After figuring out the math and the input, the last big issue was what was to be done after I got m. Giving some samples values to the nc and cross-checking via ascii tables online, I found out that m is a hex and must be converted to ASCII to use to decrypt the secret.enc file.

```
m = 215627228002
>>> hex(215627228002)
'0x3234626362'
>>> bytes.fromhex('3234626362').decode()
'24bcb'
```


- After finding the final ASCII, the third hint had the openssl command for the decryption of the secret.enc file and hence I got the flag.

```
openssl enc -aes-256-cbc -d -in secret.enc 
enter AES-256-CBC decryption password:
*** WARNING : deprecated key derivation used.
Using -iter or -pbkdf2 would be better.
picoCTF{su((3ss_(r@ck1ng_r3@_24bcbc66}
```

## Flag:

```
picoCTF{su((3ss_(r@ck1ng_r3@_24bcbc66}
```

## Concepts learnt:

- RSA encryption/decryption
- RSA multiplicative property
- 

## Notes:

- Include any alternate tangents you went on while solving the challenge, including mistakes & other solutions you found.
- 

## Resources:

- Include the resources you've referred to with links. [example hyperlink](https://google.com)

<br><br><br>
***
<br><br><br>


# 2. Custom Encryption

> Can you get sense of this code file and write the function that will decode the given encrypted file content. Find the encrypted file here flag_info and code file might be good to analyze and get the flag. 

## Solution:

I looked at the `code file` which was the file used to encrypt the flag, and going over it line by line I made a python file to decrypt the cipher.

```python
cipher = [61578, 109472, 437888, 6842, 0, 20526, 129998, 526834, 478940, 287364, 0, 567886, 143682, 34210, 465256, 0, 150524, 588412, 6842, 424204, 164208, 184734, 41052, 41052, 116314, 41052, 177892, 348942, 218944, 335258, 177892, 47894, 82104, 116314]

p = 97
g = 31
a = 90
b = 26

def generator(g, x, p):
    return pow(g, x) % p

v = generator(g, b, p)
key = generator(v, a, p)

semi_cipher = []
flag = ""

for i in cipher:
    char = i//(311*key)
    semi_cipher.append(chr(char))
    
for i, char in enumerate(semi_cipher):
    key_char = "trudeau"[i % 7]
    decrypted_char = chr(ord(char) ^ ord(key_char))
    flag += decrypted_char

print(flag[::-1])
```
Running this code gave me the flag.

## Flag:

```
picoCTF{custom_d2cr0pt6d_49fbee5b}
```

## Concepts learnt:

- XOR, if a ^ b = c, then c ^ b = a

## Notes:

This felt more like a reverse engineering challenge than a cryptography challenge. Took me some time to figure out the code to decrypt it, especially the last part because I wasn't aware that if a ^ b = c, then c ^ b = a.

## Resources:

- Learnt about XOR [here](https://www.geeksforgeeks.org/dsa/xor-cipher/)

<br><br><br>
***
<br><br><br>


# 3. MiniRSA

> Let's decrypt this: ciphertext? Something seems a bit small.

## Solution:
Took a lot of time and researching.
- The first hint guided me to the wikipedia page of RSA and and the second one told me to check for the case in which e is super small, in this case, 3.
- On the wikipedia page I found,
  > When encrypting with low encryption exponents (e.g., e = 3) and small values of the m (i.e., m < n1/e), the result of me is strictly less than the modulus n. In this case, ciphertexts can be decrypted easily by taking the eth root of the ciphertext over the integers.
- Based on this, I make a funciton with a binary search algorithm to decrypt c and give m. Then, decoding the hex form on m gave the flag.


```python
c = 2205316413931134031074603746928247799030155221252519872650073010782049179856976080512716237308882294226369300412719995904064931819531456392957957122459640736424089744772221933500860936331459280832211445548332429338572369823704784625368933 

start = 1
end = c
while end - start > 1:
    mid = (start + end) // 2

    if (mid ** 3) > c:
        end = mid
    else:
        start = mid
    
    if start ** 3 == c:
        m = start
    elif end ** 3 == c:
        m = end

h = hex(m)
flag = bytes.fromhex(h[2:]).decode()
print(flag) 
```

## Flag:

```
picoCTF{n33d_a_lArg3r_e_ccaa7776}
```

## Concepts learnt:

- RSA Encryption/Decryption

## Notes:

- The biggest mistake I made was assuming the cube root logic will work if only e is small regardless of n. However, m = pow(c,1/e) only when both e and n are considerably small. Figuring this out took way longer than it should've.
- decoding the flag frmo hex in python was tough due to different ways of doing so in previous versions of python.

## Resources:

- The [wikipedia page](https://en.wikipedia.org/wiki/RSA_cryptosystem).
- Code for [cubic root](https://www.geeksforgeeks.org/python/python-program-for-find-cubic-root-of-a-number/) of large numbers.

