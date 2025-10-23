# 1. GDB Baby Step 1

> Can you figure out what is in the eax register at the end of the main function? Put your answer in the picoCTF flag format: picoCTF{n} where n is the contents of the eax register in the decimal number base. If the answer was 0x11 your flag would be picoCTF{17}. Disassemble (file).


## Solution:
For this first challenge, my first action was to run the file but that gave no output. Then I focused on the 'disassemble' part of the challenge and tried using `gdb` on the file. With some help from a blogpost I ran the following lines and got the flag.

```
gdb debugger0_a
(gdb) info functions
All defined functions:

Non-debugging symbols:
0x0000000000001000  _init
0x0000000000001030  __cxa_finalize@plt
0x0000000000001040  _start
0x0000000000001070  deregister_tm_clones
0x00000000000010a0  register_tm_clones
0x00000000000010e0  __do_global_dtors_aux
0x0000000000001120  frame_dummy
0x0000000000001129  main
0x0000000000001140  __libc_csu_init
0x00000000000011b0  __libc_csu_fini
0x00000000000011b8  _fini
(gdb) set disassembly-flavor intel
(gdb) disassemble main
Dump of assembler code for function main:
   0x0000000000001129 <+0>:	endbr64
   0x000000000000112d <+4>:	push   rbp
   0x000000000000112e <+5>:	mov    rbp,rsp
   0x0000000000001131 <+8>:	mov    DWORD PTR [rbp-0x4],edi
   0x0000000000001134 <+11>:	mov    QWORD PTR [rbp-0x10],rsi
   0x0000000000001138 <+15>:	mov    eax,0x86342
   0x000000000000113d <+20>:	pop    rbp
   0x000000000000113e <+21>:	ret
End of assembler dump.
(gdb) print 0x86342
$1 = 549698
(gdb) 
```

## Flag:

```
picoCTF{549698}
```

## Concepts learnt:
gdb(GNU Debugger) can be used to disassemble executable files. Running `info functions` in gdb shows all the functions that are present in the disassembled code.
`print anyBaseNum` returns the decimal equivalent of the number.

## Notes:
Given that the challenge was named gdb and explicitly mentioned disassembly, trying to run the executable was foolish.

## Resources:
[Solution I followed along](https://medium.com/@Oscar404/cracking-picoctf-challenge-gdb-baby-step-1-2d77e8eab818)


<br><br><br>
***
<br><br><br>


# 2. ARMssembly 1

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


# 3. Vault Door 3

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
