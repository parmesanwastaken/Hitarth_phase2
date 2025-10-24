# 1. trivial flag transfer protocol

> Figure out how they moved the flag.

## Solution:

- Downloaded the file, it was `.pcapng`. This can be opened in wireshark
- So I opened it in wireshark and found 2 protocols mainly: `TFTP` and `ARP`
- The file name and challenge name both hinted that `TFTP` is significant
- When I went to `Export Objects` it showed me option for `TFTP` to directly extract files from it and thus I recieved few pngs, a unknown file, txt and debian package file
<img width="356" height="419" alt="Wireshark_5Esl8lXYs9" src="https://github.com/user-attachments/assets/422e6de5-6860-4d95-b099-174ff77d95d8" />
<img width="1125" height="780" alt="image" src="https://github.com/user-attachments/assets/c59ad6e6-4262-4055-9931-418f6bceffba" />

- I catted both `instructions.txt` and `plan.txt`, thought they were ceasar cipher and tried to decode them, turns out they were decoded at 13 offset so they were ROT13
- `instructions.txt` was redundant:
```
GSGCQBRFAGRAPELCGBHEGENSSVPFBJRZHFGQVFTHVFRBHESYNTGENAFSRE.SVTHERBHGNJNLGBUVQRGURSYNTNAQVJVYYPURPXONPXSBEGURCYNA > IUSEDTHEPROGRAMANDHIDITWITH-TFTPDOESNTENCRYPTOURTRAFFICSOWEMUSTDISGUISEOURFLAGTRANSFER.FIGUREOUTAWAYTOHIDETHEFLAGANDIWILLCHECKBACKFORTHEPLAN
```
- `plan` was kinda useful:
```
VHFRQGURCEBTENZNAQUVQVGJVGU-QHRQVYVTRAPR.PURPXBHGGURCUBGBF > IUSEDTHEPROGRAMANDHIDITWITH-DUEDILIGENCE.CHECKOUTTHEPHOTOS
```
- So I decided to `exiftool` all 3 `.bmp` files and it was useless
- Then I tried `binwalk`, `zsteg` and `steghide` (with empty passphrase) on all 3 files and it didn't work
- I decided to checkout the `program.deb` package
- I used:
```
 dpkg -I program.deb
 new Debian package, version 2.0.
 size 138310 bytes: control archive=1250 bytes.
     826 bytes,    18 lines      control
    1184 bytes,    17 lines      md5sums
 Package: steghide
 Source: steghide (0.5.1-9.1)
 Version: 0.5.1-9.1+b1
 Architecture: amd64
 Maintainer: Ola Lundqvist <opal@debian.org>
 Installed-Size: 426
 Depends: libc6 (>= 2.2.5), libgcc1 (>= 1:4.1.1), libjpeg62-turbo (>= 1:1.3.1), libmcrypt4, libmhash2, libstdc++6 (>= 4.9), zlib1g (>= 1:1.1.4)
 Section: misc
 Priority: optional
 Description: A steganography hiding tool
  Steghide is steganography program which hides bits of a data file
  in some of the least significant bits of another file in such a way
  that the existence of the data file is not visible and cannot be proven.
  .
  Steghide is designed to be portable and configurable and features hiding
  data in bmp, wav and au files, blowfish encryption, MD5 hashing of
  passphrases to blowfish keys, and pseudo-random distribution of hidden bits
  in the container data.
```
- Turns out it was indeed `steghide` all along
- Thought a bit and realised `plan` did hint about `DUEDILIGENCE` being passphrase
- Tried it on all 3 files and FINALLY THIRD FILE GAVE `flag.txt` which had flag in it

## Flag: 

```
picoCTF{h1dd3n_1n_pLa1n_51GHT_18375919}

```

## Concepts learnt:

- Learnt how to extract unencrypted data from `.pcapng` file
- Learnt how to use stegnoggraphy tools

## Notes:

- `binwalk` mislead at first as it showed lot of hidden files inside second image and only png in third file. This caused me to focus more on second file, but I decided to try `steghide` on all 3 files which helped
- Even `zsteg` showed pgp keys in 2 files which was kinda confusing, but had same issue in another CTF challenge so just ignored them and started focusing on other methods

## Resources:

- [Ceaser Cipher Decoder](https://cryptii.com/pipes/caesar-cipher)


***

# 2. tunn3l v1s10n

> We found this file. Recover the flag.

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
