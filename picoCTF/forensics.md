# 1. Trivial Flag Transfer Protocol
> Figure out how they moved the flag.

## Solution:

I opened the given pcapng file in wireshark and found 2 text files along with 3 image files. The images were just sceneries and hills. The text had only characters, so I ran rot13 on them like so - 
```bash
sid@sidsAsusZenbook:~/cryptoTP$ echo "GSGCQBRFAGRAPELCGBHEGENSSVPFBJRZHFGQVFTHVFRBHESYNTGENAFSRE.SVTHERBHGNJNLGBUVQRGURSYNTNAQVJVYYPURPXONPXSBEGURCYNA" | rot13
TFTPDOESNTENCRYPTOURTRAFFICSOWEMUSTDISGUISEOURFLAGTRANSFER.FIGUREOUTAWAYTOHIDETHEFLAGANDIWILLCHECKBACKFORTHEPLAN
sid@sidsAsusZenbook:~/cryptoTP$ echo "VHFRQGURCEBTENZNAQUVQVGJVGU-QHRQVYVTRAPR.PURPXBHGGURCUBGBF" | rot13
IUSEDTHEPROGRAMANDHIDITWITH-DUEDILIGENCE.CHECKOUTTHEPHOTOS
```
The second text was crucial for the solve. It hinted about the program and a phrase, "DUEDILIGENCE". I tried installing the program to find out it was steghide.
```
sid@sidsAsusZenbook:~/cryptoTP/tftp$ sudo apt install ./program.deb
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
Note, selecting 'steghide' instead of './program.deb'
```
Then I ran steghide on each image with the passphrase as `DUEDILIGENCE`. The third image had the flag.

## Flag:

```
picoCTF{h1dd3n_1n_pLa1n_51GHT_18375919}
```

## Concepts learnt:

- `pcapng` or `pcap` files are used to store packet information
- `tftp` objects can be extracted using wireshark

## Notes:

- I wasted a lot of my time on this challenge simply because I didn't have the right version of wireshark. Apparently, wireshark 4.6.0 does not support extraction of `bmp` files as tftp objects but an earlier version, 4.2.2 does.

## Resources:

- Used [this](https://launchpad.net/~wireshark-dev/+archive/ubuntu/stable) to reinstall the correct version of wireshark.
- Used the steghide man page to figure out how its used.

<br><br><br>
***
<br><br><br>


# 2. tunn3l v1s10n

> We found this file. Recover the flag.

## Solution:
- I opened the file with my image viewer and get the error "bmp image has unsupported header size".
- I then opened it with sublime text and saw the header had a hex as "bad0" in the size data. After some digging, I changed it to 2800 and got an openable image but no flag.
- Then I changed the width data a bit but that result in a weird image, still no flag.
- Changing the height data finally from 3201 to 3203 revealed the flag.
  <img width="2554" height="1834" alt="image" src="https://github.com/user-attachments/assets/3a14cfd6-7566-4164-b001-2abd672eee51" />


## Flag:

```
picoCTF{qu1t3_a_v13w_2020}
```

## Concepts learnt:

- Image hex manipulation

## Notes:

- Fun challenge, learnt about the standard hex data for a BMP file.

## Resources:
- Learnt about BMP hex data [here.](https://www.donwalizerjr.com/understanding-bmp/)

<br><br><br>
***
<br><br><br>


# 3. m00nwalk

> Decode this message from the moon.

## Solution:
Couldn't find a proper place to start, so I read the first hint. It was about how images were sent from space during early days of space discovery, ie, Slow-Scan Television(SSTV) and it lead me a tool called `qsstv`. Then I created a virtual sink to qsstv to provide it the audio given in the challenge by running `pactl load-module module-null-sink sink_name=virtual-cable`. Then I played the audio by running `paplay -d virtual-cable message.wav` and obtained this image which had the flag.
<img width="2615" height="2111" alt="image" src="https://github.com/user-attachments/assets/b9ad9c4d-4cf4-4c98-b826-7cb778b4942e" />

## Flag:

```
picoCTF{beep_boop_im_in_space}
```

## Concepts learnt:
- Virtual cables for audio transfer using PulseAudio
- qsstv

## Notes:

I tried all sorts of things before reading the hint and I wouldn't have thought of qsstv if not for the hint. I tried seeing its spectogram, running it through DTMF checkers, etc. Once I found out it had to do with qsstv, it was quite a quick one.

## Resources:

- Found out about the DTMF thing [here](https://github.com/JohnHammond/ctf-katana?tab=readme-ov-file)
- Found it was sstv on its [wikipedia page.](https://en.wikipedia.org/wiki/Slow-scan_television)
- Used [this](https://askubuntu.com/questions/1338747/virtual-audio-sink-virtual-audio-cable-on-ubuntu) to make the virtual cable.
