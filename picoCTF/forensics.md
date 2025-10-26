# 1. Trivial Flag Transfer Protocol
> Figure out how they moved the flag.

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


# 2. tunn3l v1s10n

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
