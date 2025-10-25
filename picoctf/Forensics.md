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

- I downloaded the file and tried using `file` on it
- It didn't give any info so I tried catting it, which gave redundant stuff too
- Decided to check metadata using `exiftool`, this showed it is a `BMP` file
- Even strings gave out useless results
- So I decided to hexdump and found few changes from a normal BMP file header
- Decided to open file using `hexedit` and checked the DIB header part specifically
- Wikipedia mentions it should be `28 00 00 00`, but here it was:
<img width="1718" height="162" alt="image" src="https://github.com/user-attachments/assets/1a81fd8d-cc44-4a82-9ea3-f518467b06c2" />

- So I changed that
- Later noticed similar issue in BMP header part:
<img width="1698" height="111" alt="image" src="https://github.com/user-attachments/assets/0cd9571c-1a07-4e36-a055-8cc0c6856df9" />

- So I changed it to `36 00 00 00` according to wikipedia
- Even after all this, nothing worked :(
- I decided to mess again with headers but it didn't help
- The opened image did look a bit weird and smaller, so I decided to recheck the metadata
- The size in metadata and photos app weren't matching
- Metadata mentioned `1134x1074` but photos app showed `1134x306`
- Decided to change width a bit here and there:
<img width="1602" height="72" alt="image" src="https://github.com/user-attachments/assets/42501ef9-18a0-4671-a9ae-1a1660b15fd7" />

- Used a decimal to hex convertor and thus changed `32 01` to `32 04` for 1074 value. But this gave error too ;-;
- Decided to do a little smaller and went with `32 03`
- This worked and showed me the flag:
[tunn3l_v1s10n.bmp](https://github.com/user-attachments/files/23144287/tunn3l_v1s10n.bmp)


## Flag:

```
picoCTF{qu1t3_a_v13w_2020}
```

## Concepts learnt:

- Learnt how to use `hexedit`
- Learnt how to read basic hex and edit it a bit here and there
- Learnt a bit about header of files

## Notes:

- This challenge in particular was very tricky cause I didn't understand hex codes at all
- Nothing was very obvious, so had to take help from lot of searchese online
- Wikipedia was extremely helpful!!

## Resources:

- [Wikipedia page on BMP file](https://en.wikipedia.org/wiki/BMP_file_format)
- Man page of `hexedit`

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
