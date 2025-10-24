# 1. GDB baby step 1

> Can you figure out what is in the eax register at the end of the main function? Put your answer in the picoCTF flag format: picoCTF{n} where n is the contents of the eax register in the decimal number base. If the answer was 0x11 your flag would be picoCTF{17}. Disassemble this.

## Solution:

- I first used `file` to figure out file type
- Found it is an ELF file
- Searched up online what is ELF file, then later searched how to reverse it back to C code
- Found a stackoverflow thread where a user told how to reverse it to assemble code
- So I used `objdump --disassemble debugger0_a` this gave me the full assembly code
- Description only asked me to find `eax` inside `main`, so I used `grep`
- So I ran: `objdump --disassemble debugger0_a | grep eax`
- I got the following output: `   1138:       b8 42 63 08 00          mov    $0x86342,%eax`
- Converted hexadecimal `0x86342` to decimal and got the flag

## Flag:

```
picoCTF{549698}
```

## Concepts learnt:

- Learnt how to convert ELF file to assembly using `objdump`
- Also reminder to find file type using `file` command

## Notes:

- We can also solve this by opening the file in IDA
- Then we can go to main function and find hexadecimal:
<img width="1280" height="568" alt="ida_ChFPJsnO5J" src="https://github.com/user-attachments/assets/b8a5f494-d2b2-4d64-8a7b-80a72960ffb8" />

- We can even use online assembly to C converter and check the return value which gives hexadecimal value


## Resources:

- [Hexadecimal Convertor](https://www.rapidtables.com/convert/number/hex-to-decimal.html?x=86342)
- [Stackoverflow](https://stackoverflow.com/questions/17247505/how-to-convert-an-elf-executable-to-c-code-the-generated-c-code-need-not-be-hum)
- [ELF file info](https://en.wikipedia.org/wiki/Executable_and_Linkable_Format)


***

# 2. ARMssembly1

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


***
# 3. Challenge name

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


***
