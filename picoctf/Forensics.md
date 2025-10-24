# 1. Challenge name

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

# 1. Challenge name

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

# 3. m00nwalk

> Decode this message from the moon.

## Solution:

- First I downloaded the file
- It was a `.wav` file and hint talked about transmission from moon (similar to desc and title)
- So I used [this](https://sstv-decoder.mathieurenaud.fr/) online tool to decode sstv to image
- It gave me the following image after inputting `.wav` file:

<img width="320" height="256" alt="decoded-image" src="https://github.com/user-attachments/assets/6225bd8c-9188-46e6-94ec-a05fd49bdb20" />

- This has flag in it

## Flag:

```
picoCTF{beep_boop_im_in_space}
```

## Concepts learnt:

- Learnt how to convert SSTV transmission to image

## Notes:

- Already did similar challenge in an event called `cryptic finds` recently, so this was easy to figure out (both had same hints too lol)
- qsstv has some issues with .wav file so used online convertor instead

## Resources:

- [Online SSTV decoder](https://sstv-decoder.mathieurenaud.fr/)
