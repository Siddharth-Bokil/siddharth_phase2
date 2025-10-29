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

- Decimal to Binary [convertor](https://www.rapidtables.com/convert/number/decimal-to-binary.html)
  
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

