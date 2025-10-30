# 1. IQ Test
> let your input x = 30478191278.
wrap your answer with nite{ } for the flag.
As an example, entering x = 34359738368 gives (y0, ..., y11), so the flag would be nite{010000000011}.


## Solution:

- I converted the given number to binary using an online tool.
- This binary was of 35 digits but the question asked for 36 inputs, so I added a 0 at the MSB.
- Solving the logic circuit manually gave me the values for y0,y1...,y11 which then revealed the flag.

## Flag:

```
nite{100010011000}
```

## Concepts learnt:

- Logic gates

## Notes:

- Initially I thought some not operators were to be interpreted differently, but later found it they were normal not/chaining operators.

## Resources:

- Decimal to Binary convertor [here.](https://www.rapidtables.com/convert/number/decimal-to-binary.html)
  
<br><br><br>
***
<br><br><br>


# 2. I Like Logic

> i like logic and i like files, apparently, they have something in common, what should my next step be.

## Solution:

- Ran `strings digital-x.bin` on every file and found the same output in each other than digital-3.bin which had some additional stuff that didn't make sense.
```
sid@sidsAsusZenbook:~/cryptoTP/iLikeLogic$ strings digital-0.bin 
<SALEAE>
WAAF
sid@sidsAsusZenbook:~/cryptoTP/iLikeLogic$ 
```
- Searched about SALEAE and found a software of theirs called 'Logic 2', installed it.
- On opening the given .sal file in Logic 2, I started adding each analyser till I got something readable. Luckily the right one, Async Serial, was the second analyser.
- I selected the Input channel as 3 as it was the only one showing something significant and I got story-type text in the Data box.
- I found the flag hidden between the lines of the story.

<img width="3830" height="2195" alt="image" src="https://github.com/user-attachments/assets/89afc657-eccd-474d-911f-ce42b7322505" />

## Flag:

```
FCSC{b1dee4eeadf6c4e60aeb142b0b486344e64b12b40d1046de95c89ba5e23a9925}
```

## Concepts learnt:

- Logic 2 Capture
- Async Serial

## Notes:

- Went in a lot of different direction with this one. Tried to find stuff in the json file, tried to convert the binary in the .bin files to get something readable, then learning a bit about SALEAE helped get to the flag. I didn't think this was the final flag since it doesn't have anything readable inside the {} which is something that was there in the previous challenge.

## Resources:

- [This](https://discuss.saleae.com/t/logic-2-capture-format-sal/1858) guided me to downloading the software.
- Learnt the basics of Logic 2 [here.](https://www.youtube.com/watch?v=Ak9R4yxQPhs)



<br><br><br>
***
<br><br><br>


# 3. Bare Metal Alchemist

> my friend recommended me this anime but i think i've heard a wrong name.

## Solution:

- Initally I ran the `file` command to understand a bit more about the .elf file.
- I then tried to decompile using `avr-gdb` but that supposedly does not support the architecture of the given file. I had come across another decompiling software, `Ghidra`, in Citadel CTF and hence opened the file in it.
- It took me a while to fully familiarize myself with Ghidra but after a while I was comfortable with it. I opened the main function from the symbol tree and opened the decompiled version which looked similar to C with weird variable names. There were a lot of memory address which were tough to go about.
- Also, the function `z1()` was being called again and again, on reading it in Ghidra and doing some digging, I found it it was replacing certain bytes, ie, masking it.
- Going further down the main function, a key thing I found out was that the code was reading bytes from a memory address, `0x68`, and then was XORing it with `0xA5`. Also, the loop was being broken if the current byte was the key, `0xA5`, or 0.
- The exact XOR loop:
  ```
  R15R14 = 0x68;
  R16 = 0;
  while( true ) {
         Z = (byte *)R15R14;
         R25R24._0_1_ = *(byte *)(uint3)R15R14;
         if ((byte)R25R24 == 0) break;
         Z._1_1_ = (undefined1)(R15R14 >> 8);
         Z._0_1_ = (byte)R25R24 ^ R11;
         if ((byte)R25R24 == 0xa5) break;
         R25R24._0_1_ = DAT_mem_0029;
         R25R24._0_1_ = (byte)R25R24 ^ (byte)R25R24 * '\x02';
         if (((byte)R25R24 & 4) == 0) {
               auStack_7 = (undefined1  [3])0x131;
               z1();
               break;
         }
  ```
- Then, I went to the memory location `0x68`, took a few bytes from there and converted to ascii but nothing meaningful showed up.
- After some more digging, I went to the memory map and saw .data starting at `0100`.
- Then using 68 as an offset, I took a few bytes from there and converted to ASCII.
- This was the hex from `0x0168`,
  ```
  f1 e3 e6 e6 f1 e3 de f1 cd 94 d6 fa 94 d6 fa d6 ca c8 96 fa d6 94 c8 d5 c9 96 fa 91 d7 c1 d0 94 cb ca fa c3 94 d7 c8 d2 91 d7 c0 d8
  ```
- XORing each byte with `0xA5` we get,
  ```
  54 46 43 43 54 46 7b 54 68 31 73 5f 31 73 5f 73 6f 6d 33 5f 73 31 6d 70 6c 33 5f 34 72 64 75 31 6e 6f 5f 66 31 72 6d 77 34 72 65 7d
  ```
- Converting these decoded bytes to ASCII, we finally get,
  `TFCCTF{Th1s_1s_som3_s1mpl3_4rdu1no_f1rmw4re}`

## Flag:

```
TFCCTF{Th1s_1s_som3_s1mpl3_4rdu1no_f1rmw4re}
```

## Concepts learnt:

- Ghidra
- XOR
- Data byte offsets

## Notes:

- Probably the toughest challenge. Getting to the main funciton was do-able, but reading and understanding the main function and the memory addresses was the actual task. This took a lot of time, but in hindsight I could've probably done it quicker. I had already tried reading the main function before getting the hint, but I did not go into much depth. After getting the hint I knew the main function was the right path and hence eventually got the flag but I still don't see how the name or the anime would connect to the solution.

## Resources:

- Ghidra [tutorial](https://www.youtube.com/watch?v=OWEZQMVLMPs), watched specific parts.
- Simple [bytes to ascii convertor.](https://onlinetools.com/ascii/convert-bytes-to-ascii)
- Xor calculator [here.](https://xor.pw/)

