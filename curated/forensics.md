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
- SecLists installation for rockyou.txt [here.](https://github.com/danielmiessler/SecLists)




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
<img width="429" height="421" alt="image" src="https://github.com/user-attachments/assets/3f5598a5-8c03-42ea-bff3-d17c4bf3e478" />

## Flag:

```
nite{n0w_y0u_kn0w_ab0ut_PNG_chunk5}
```


## Concepts learnt:

- Magic numbers
- PNG chunks(IHDR/IDAT/IEND)


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

- Opened the pcap in wireshark
- On going to Statistics-->Conversations, I found a bunch of packets out of which the TCP ones had readable english.
- It was a conversation between 2 philosophers, it went as -
```
Camus: One must imagine Sisyphus happy but are we happy ?
Nietzsche: You will be happy after reading my latest work
Camus: whats the password ?
Nietzsche: b3y0ndG00dand3vil
Camus: thanks
```
- Along with this I found a file that had the header of a RAR file, relating to the name of challenge. I then downloaded that file as a raw `.bin` file and used unrar to extract and find the flag, using `b3y0ndG00dand3vil` as the password.
```
sid@sidsAsusZenbook:~/cryptoTP/curated/RARofTheAbyss$ unrar x output.bin
UNRAR 7.00 freeware      Copyright (c) 1993-2024 Alexander Roshal
Enter password (will not be echoed) for output.bin: 

Extracting from output.bin
Extracting  flag.txt                                                  OK 
All OK
sid@sidsAsusZenbook:~/cryptoTP/curated/RARofTheAbyss$ 

```


## Flag:

```
nite{thus_sp0k3_th3_n3tw0rk_f0r3ns1cs_4n4lyst}
```


## Concepts learnt:

- Conversations in Wireshark
- RAR file headers
- tcpflow (mentioned in notes)

## Notes:

- I initally tried exporting objects but didnt find anything. Later on I stumbled upon a part of the conversation which didnt have the password for the rar file. I only saw the part `you will be happy after you read my latest book` so for about an hour I was trying all sorts of words relating to Nietzsche and his latest book to extract the flag. I even tried to crack it using rockyou.txt. Eventually I realised just guessing could not be the answer and went back to wireshark to play with it a bit more and found the entire conversation which had the password. Later on after the challenge, I found this tool called `tcpflow` which would've basically done the wireshark part for me from the cli itself.


## Resources:

- Came across the conversations [here.](https://www.wireshark.org/docs/man-pages/wireshark.html)
- unrar man page
- Learnt about tcpflow [here.](https://www.tecmint.com/tcpflow-analyze-debug-network-traffic-in-linux/)




<br><br><br>
***
<br><br><br>




# 4. NineTails

> Looks like I got a little too clever and hid the flag as a password in Firefox, tucked away like one of NineTails’ many tails. Recover the "logins" and the "key4" and let it guide you to the flag. Hint: I named my Ninetails "j4gjesg4", quite a peculiar name isn't it?


## Solution:

- I extracted the ad1 file from the given rar file, but could not find much support for an ad1 file on my linux system.
- I then booted into Windows, opened the file in FTK Imager. On messing with the file tree I found `logins.json` and `key4.db` in `GIC2024/AppData/Roaming/Mozilla/Firefox/Profiles/j4gjesg4.default-release` confirming the name given in the hint.
- Then I installed the `firefox_decrypt` package from github and ran it.
- Doing that revealed the flag.

```
python "C:\Users\Siddharth\OneDrive\Desktop\firefox_ctf\firefox_decrypt.py" -d "C:\Users\Siddharth\OneDrive\Desktop\firefox_ctf"
```
<img width="720" height="621" alt="image" src="https://github.com/user-attachments/assets/8517394c-7e1f-4154-845f-9a7413e6eb1a" />


## Flag:

```
GCTF{m0zarella_f1ref0x_p4ssw0rd}
```


## Concepts learnt:

- ad1 files, memory forensics
- FTK Imager
- Firefox password decryption


## Notes:

- It took me a while because I was trying to open up the ad1 on linux for a long time but nothing worked. I tried ewf-tools, autopsy and finally I realised I could've used wine all along. Anyways doing it in windows was quite quick. Fun challenge.


## Resources:

- Used [this](https://www.youtube.com/watch?v=CPup3ClC7nE) video as a guide for FTK Imager.
- FOllowed [this](https://dev.to/higordiego/cracking-firefox-encryption-and-rescuing-saved-passwords-pfl) to find the right file path.




<br><br><br>
***
<br><br><br>




# 5. Re:Draw

> Her screen went black and a strange command window flickered to life, lines of text flashed across before everything went silent. Moments later, the system crashed. By sheer luck, we recovered a memory dump. Note: There are three stages to this challenge and you will find three flags. What we know: just before the crash, a black command window flickered across the screen, something in its output might still be visible if you dig through memory. She was drawing when it happened, and remnants of a painting program linger, which could reveal more if inspected in the right way. Finally, a mysterious archive hides deeper in memory, likely holding the last piece of her work. Hint: Learn up on volatility 2 and its various plugins and what they are used for.


## Solution:

- Extracted the given `.7z` file to get a memory dump in the form of a `.raw` file.
- Ran a kdbgscan on the .raw file and it was a Win7SP1x64 profile file.
- Using pslist showed 3 main processes, winRAR, mspaint, and cmd.
- Running consoles on the cmd process revealed a base64 encoded string which was the flag for stage 1.
- Using memdump on the winRAR process I obtained a .dmp file which I later converted to a .data file so I could open it using GIMP.
- To find the offset, I checked the hex of the file and found repeating `FF FF FF FF` units which indicated that it was an RGBA type of image. The byte position of the first `FF FF FF FF` block is the offset and I found it to be `276`. On playing with the height filter, I found `1230` revealed some inverted text which was the flag for stgae 2.
- For the 3rd stage, I found a `.rar` file was being provided as an argument to winRAR.
- To unrar this file I needed an NTLM hash of the owner's password. Upon learnning of the structer of these hashes, I was able to find the NTLM hash of the owner via the hashdump command.
- The flag for the 3rd stage was in the `.png` obtained on unrar-ing the file.

```
sid@sidsAsusZenbook:~/cryptoTP/curated/reDraw$ 7za x Re_Draw.7z
------------------------STAGE 1--------------------------
vol@03ca48285010:/data$ python2 volatility/vol.py kdbgscan -f MemoryDump_Lab1.raw 
Volatility Foundation Volatility Framework 2.6.1
**************************************************
Instantiating KDBG using: /data/MemoryDump_Lab1.raw WinXPSP2x86 (5.1.0 32bit)
Offset (P)                    : 0x28100a0
KDBG owner tag check          : True
Profile suggestion (KDBGHeader): Win7SP1x64
PsActiveProcessHead           : 0x2846b90
PsLoadedModuleList            : 0x2864e90
KernelBase                    : 0xfffff8000261f000

vol@03ca48285010:/data$ python2 volatility/vol.py -f MemoryDump_Lab1.raw --profile=Win7SP1x64 pslist
....
vol@03ca48285010:/data$ python2 volatility/vol.py -f MemoryDump_Lab1.raw --profile=Win7SP1x64 consoles -p 1984
...
ZmxhZ3t0aDFzXzFzX3RoM18xc3Rfc3Q0ZzMhIX0=
...
vol@03ca48285010:/data$ echo 'ZmxhZ3t0aDFzXzFzX3RoM18xc3Rfc3Q0ZzMhIX0=' | base64 -d
flag{th1s_1s_th3_1st_st4g3!!}


------------------------STAGE 2--------------------------
vol@5711d5656df0:/volatility$ python2 vol.py -f /data/MemoryDump_Lab1.raw --profile=Win7SP1x64 memdump -p 2424 -D .
Volatility Foundation Volatility Framework 2.6.1
************************************************************************
Writing mspaint.exe [  2424] to 2424.dmp
sid@sidsAsusZenbook:~/cryptoTP/curated/reDraw$ sudo docker cp 5711d5656df0:/volatility/2424.dmp .
Successfully copied 266MB to /home/sid/cryptoTP/curated/reDraw/.
sid@sidsAsusZenbook:~/cryptoTP/curated/reDraw$ file 2424.dmp
2424.dmp: data
sid@sidsAsusZenbook:~/cryptoTP/curated/reDraw$ mv 2424.dmp 2424.data
sid@sidsAsusZenbook:~/cryptoTP/curated/reDraw$ gimp 2424.data
sid@sidsAsusZenbook:~/cryptoTP/curated/reDraw$ hexedit 2424.data


------------------------STAGE 3--------------------------
vol@81f895bfcf44:/data$ python2 volatility/vol.py -f MemoryDump_Lab1.raw --profile=Win7SP1x64 cmdline -p 1512
Volatility Foundation Volatility Framework 2.6.1
************************************************************************
WinRAR.exe pid:   1512
Command line : "C:\Program Files\WinRAR\WinRAR.exe" "C:\Users\Alissa Simpson\Documents\Important.rar"
vol@81f895bfcf44:/data$ python2 volatility/vol.py -f MemoryDump_Lab1.raw --profile=Win7SP1x64 filescan | grep Important.rar
Volatility Foundation Volatility Framework 2.6.1
0x000000003fa3ebc0      1      0 R--r-- \Device\HarddiskVolume2\Users\Alissa Simpson\Documents\Important.rar
0x000000003fac3bc0      1      0 R--r-- \Device\HarddiskVolume2\Users\Alissa Simpson\Documents\Important.rar
0x000000003fb48bc0      1      0 R--r-- \Device\HarddiskVolume2\Users\Alissa Simpson\Documents\Important.rar
vol@5711d5656df0:/volatility$ python2 vol.py -f /data/MemoryDump_Lab1.raw --profile=Win7SP1x64 dumpfiles -Q 0x000000003fa3ebc0 -D ./outputDirectory/
Volatility Foundation Volatility Framework 2.6.1
DataSectionObject 0x3fa3ebc0   None   \Device\HarddiskVolume2\Users\Alissa Simpson\Documents\Important.rar
vol@5711d5656df0:/volatility$ ls outputDirectory/
file.None.0xfffffa8001034450.dat
vol@5711d5656df0:/volatility$ cd outputDirectory/
vol@5711d5656df0:/volatility/outputDirectory$ file file.None.0xfffffa8001034450.dat 
file.None.0xfffffa8001034450.dat: RAR archive data, v5
vol@5711d5656df0:/volatility/outputDirectory$ mv file.None.0xfffffa8001034450.rar /data/file.rar
sid@sidsAsusZenbook:~/cryptoTP/curated/reDraw$ sudo docker cp 5711d5656df0:/volatility/outputDirectory/file.None.0xfffffa8001034450.rar .
[sudo] password for sid: 
Successfully copied 46.6kB to /home/sid/cryptoTP/curated/reDraw/.
vol@5711d5656df0:/volatility$ python2 vol.py -f /data/MemoryDump_Lab1.raw --profile=Win7SP1x64 hashdump     
Volatility Foundation Volatility Framework 2.6.1
Administrator:500:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
Guest:501:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
SmartNet:1001:aad3b435b51404eeaad3b435b51404ee:4943abb39473a6f32c11301f4987e7e0:::
HomeGroupUser$:1002:aad3b435b51404eeaad3b435b51404ee:f0fc3d257814e08fea06e63c5762ebd5:::
Alissa Simpson:1003:aad3b435b51404eeaad3b435b51404ee:f4ff64c8baac57d22f22edc681055ba6:::
sid@sidsAsusZenbook:~/cryptoTP/curated/reDraw$ unrar e file.None.0xfffffa8001034450.rar 
UNRAR 7.11 beta 1 freeware      Copyright (c) 1993-2025 Alexander Roshal
Archive comment:
Password is NTLM hash(in uppercase) of Alissa's account passwd.
Extracting from file.None.0xfffffa8001034450.rar
Enter password (will not be echoed) for flag3.png: 
Extracting  flag3.png                                                 OK 
All OK
sid@sidsAsusZenbook:~/cryptoTP/curated/reDraw$
```

<img width="976" height="439" alt="image" src="https://github.com/user-attachments/assets/46bd6cee-f705-4dd9-83eb-fd8b6c0dbbda" />
<img width="489" height="495" alt="image" src="https://github.com/user-attachments/assets/96981fbe-e5da-4ce6-82fc-8b45686344dd" />


## Flag:

```
flag{th1s_1s_th3_1st_st4g3!!}
flag{Good_Boy_good_girl}
flag{w3ll_3rd_stage_was_easy}
```


## Concepts learnt:

- Include the new topics you've come across and explain them in brief
- 


## Notes:

- This was a really long challenge and had a lot to learn. It was quite fun learning about the various plugins of volatility but their github page came in handy. I had some issues installing volatility properly since I had python3 and not python2.7 which volatility2 needs so I ended up doing the challenge on docker instead. The 2nd stage in my opinion was quite tough compared to the other 2 because it had that image-based forensics thing as well.


## Resources:

- Include the resources you've referred to with links. [example hyperlink](https://google.com)
- 

