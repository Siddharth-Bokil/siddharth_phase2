# 1. Hide And Seek

> Sakamoto’s at it again with a game of hide and seek, but this time, it’s not with Shin or his daughter. An old friend hid some secret data in this image. Can you find it before the others do? Hint: Even in retirement, Sakamoto never loses at hide and seek. Maybe stegseek can help you keep up.



## Solution:

- Ran stegseek on the image to get the flag.

```bash
sid@sidsAsusZenbook:~/cryptoTP/curated/hideAndSeek$ stegseek sakamoto.jpg rockyou.txt 
StegSeek 0.6 - https://github.com/RickdeJager/StegSeek

[i] Found passphrase: "iloveyou1"
[i] Original filename: "flag.txt".
[i] Extracting to "sakamoto.jpg.out".

sid@sidsAsusZenbook:~/cryptoTP/curated/hideAndSeek$ cat sakamoto.jpg.out 
nite{h1d3_4nd_s33k_but_w1th_st3g_sdfu9s8}
sid@sidsAsusZenbook:~/cryptoTP/curated/hideAndSeek$ 
```


## Flag:

```
nite{h1d3_4nd_s33k_but_w1th_st3g_sdfu9s8}
```


## Concepts learnt:

- Stegseek


## Notes:

- The challenge was pretty straightforward, since stegseek is literally mentioned in the description.


## Resources:

- Stegseek man page
- SecLists installation for rockyou.txt (here.)[https://github.com/danielmiessler/SecLists]




<br><br><br>
***
<br><br><br>




# 2. Nutrela Chunks

> One of my favorite foods is soya chunks. But as I was enjoying some Nutrela today, I noticed a few chunks weren’t quite right. Seems like something’s off with their structure. Could you help me fix these broken chunks so I can enjoy my meal again?


## Solution:

- Tried to open the file, that didn't work.
- Read its magic numbers and changed it to standard ones for PNGs.
- Took me some time, but I was browsing the website on PNG chunks I realised there was a difference in upper and lower case in the human-readable part. 
```
sid@sidsAsusZenbook:~/cryptoTP/curated/nutrelaChunks$ xxd nutrela.png | les
00000000: 8970 6e67 0d0a 1a0a 0000 000d 6968 6472  .png........ihdr
00000010: 0000 03e8 0000 03e8 0802 0000 00c2 c143  ...............C
00000020: b300 0837 ed69 6461 7478 9cec bd79 2055  ...7.idatx...y U
00000030: db1f f7bf 1d8e cc84 084d 6e33 4952 48ba  .........Mn3IRH.
sid@sidsAsusZenbook:~/cryptoTP/curated/nutrelaChunks$ hexedit nutrela.png
sid@sidsAsusZenbook:~/cryptoTP/curated/nutrelaChunks$ xxd nutrela.png | less
00000000: 8950 4e47 0d0a 1a0a 0000 000d 4948 4452  .PNG........IHDR
00000010: 0000 03e8 0000 03e8 0802 0000 00c2 c143  ...............C
00000020: b300 0837 ed49 4441 5478 9cec bd79 2055  ...7.IDATx...y U

```
<img width="982" height="1002" alt="image" src="https://github.com/user-attachments/assets/1715267a-8e60-428d-a31b-4323947817d1" />


## Flag:

```
nite{n0w_y0u_kn0w_ab0ut_PNG_chunk5}
```


## Concepts learnt:

- Magic numbers
- PNG chunks(IDHR/IDAT/IEND)


## Notes:

- Nice challenge. Initally I just changed the magic numbers, but then later saw that the `IHDR` and `IDAT` chunks were in lowercase.


## Resources:

- Found common file signatures [here.](https://en.wikipedia.org/wiki/List_of_file_signatures)
- Used [this](https://medium.com/@0xwan/png-structure-for-beginner-8363ce2a9f73) to learn about chunks and their hex representation.



<br><br><br>
***
<br><br><br>




# 3. RAR Of The Abyss

> Two philosophers peer into the networked abyss and swap a secret. Use the secret to decrypt the Abyss’ RAwR and pull your flag from the void.


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




# 4. NineTails

> Looks like I got a little too clever and hid the flag as a password in Firefox, tucked away like one of NineTails’ many tails. Recover the "logins" and the "key4" and let it guide you to the flag. Hint: I named my Ninetails "j4gjesg4", quite a peculiar name isn't it?


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




# 5. Re:Draw

> Her screen went black and a strange command window flickered to life, lines of text flashed across before everything went silent. Moments later, the system crashed. By sheer luck, we recovered a memory dump. Note: There are three stages to this challenge and you will find three flags. What we know: just before the crash, a black command window flickered across the screen, something in its output might still be visible if you dig through memory. She was drawing when it happened, and remnants of a painting program linger, which could reveal more if inspected in the right way. Finally, a mysterious archive hides deeper in memory, likely holding the last piece of her work. Hint: Learn up on volatility 2 and its various plugins and what they are used for.


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

