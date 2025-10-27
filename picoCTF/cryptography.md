# 1. RSA Oracle
> Put in the challenge's description here

## Solution:

- Include as many steps as you can with your thought process
- You **must** include images such as screenshots wherever relevant.

```
put codes & terminal outputs here using triple backticks

you may also use ```python for python codes for example
```

## Flag:

```
picoCTF{}
```

## Concepts learnt:

- Include the new topics you've come across and explain them in brief
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

- Include as many steps as you can with your thought process
- You **must** include images such as screenshots wherever relevant.

```
put codes & terminal outputs here using triple backticks

you may also use ```python for python codes for example
```

## Flag:

```
picoCTF{}
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

