# 1. Hide and Seek

> Sakamoto’s at it again with a game of hide and seek, but this time, it’s not with Shin or his daughter. An old friend hid some secret data in this image. Can you find it before the others do? Hint:
Even in retirement, Sakamoto never loses at hide and seek. Maybe stegseek can help you keep up.

## Solution:

- Read the description and it was clear this was steganography challenge
- The hint clearly mentioned to use stegseek, which is a stehide bruteforce using wordlists
- So I went to stegseek repo and installed the debian package and used `sudo apt install ./stegseek_0.6-1.deb`
- Then I went to terminal and used `stegseek sakamoto.jpg rockyou.txt`, where `rockyou.txt` is a wordlist that contains many leaked passwords
- It cracked it immedietaly:
```
╰─ stegseek sakamoto.jpg rockyou.txt
StegSeek 0.6 - https://github.com/RickdeJager/StegSeek

[i] Found passphrase: "iloveyou1"
[i] Original filename: "flag.txt".
[i] Extracting to "sakamoto.jpg.out".
```
- The file name was messed up so I just changed extension to .txt and got the flag!!

## Flag:

```
nite{h1d3_4nd_s33k_but_w1th_st3g_sdfu9s8}
```

## Concepts learnt:

- Learnt that we can easily crack steghide if it uses common password

## Notes:

- Even though it was obvious we have to crack steghide using stegseek, I still tried binwalk on the file
- Surprisingly it just showed 1 jpg instead of showing anything embedded inside it
- So I just used stegseek and got the flag

## Resources:

- [stegseek](https://github.com/RickdeJager/stegseek) repository


***


# 2. Nutrela Chunks

> One of my favorite foods is soya chunks. But as I was enjoying some Nutrela today, I noticed a few chunks weren’t quite right. Seems like something’s off with their structure. Could you help me fix these broken chunks so I can enjoy my meal again?

## Solution:

- Read the description and realised I need to edit the hex of this .png file
- At first I was very confused what to change, whether it was image height width, header or some random thing
- Then I opened the file and it was corrupted, also decided to search about png chunks
- They talked about how there are 3 very important chunks, IHDR, ILAT and IEND
- I also found out about `pngcheck` command, so I installed the package and ran it on `nutrela.png`
```
╰─ pngcheck nutrela.png
zlib warning:  different version (expected 1.2.13, using 1.3)

nutrela.png  this is neither a PNG or JNG image nor a MNG stream
ERROR: nutrela.png
```
- I realised the header itself is incorrect so I fixed it by changing first 4 bytes:
```
original 4 bytes: 89 70 6E 67
new 4 bytes: 89 50 4E 47
```
- Then I again ran `pngcheck`
```
╰─ pngcheck nutrela.png
zlib warning:  different version (expected 1.2.13, using 1.3)

nutrela.png  first chunk must be IHDR
ERROR: nutrela.png
```
- I fixed IHDR by changing 4 bytes next to its string:
```
IHDR:
original: 69 68 64 72
new: 49 48 44 52
```
- Then I again ran command:
```
╰─ pngcheck nutrela.png
zlib warning:  different version (expected 1.2.13, using 1.3)

nutrela.png  illegal reserved-bit-set chunk idat
ERROR: nutrela.png
```
- I fixed IDAT by changing 4 bytes next to its string:
```
IDAT:
original: 69 64 61 74
new: 49 44 41 54
```
- Then I again ran command:
```
╰─ pngcheck nutrela.png
zlib warning:  different version (expected 1.2.13, using 1.3)

nutrela.png  illegal reserved-bit-set chunk iend
ERROR: nutrela.png
```
- I fixed IEND by changing 4 bytes next to its string:
```
IEND:
original: 69 65 6E 64
new: 49 45 4E 44
```
- Finally the file was fixed:
```
╰─ pngcheck nutrela.png
zlib warning:  different version (expected 1.2.13, using 1.3)

OK: nutrela.png (1000x1000, 24-bit RGB, non-interlaced, 82.0%).
```
- After I opened the file I found the flag!!

## Flag:

```
nite{n0w_y0u_kn0w_ab0ut_PNG_chunk5}
```

## Concepts learnt:

- I learnt about different png chunks
- Also that chunks are case sensitive
- Also learnt about `pngcheck` command

## Notes:

- At first I was confused what to change, but focusing just on chunks part as mentioned in description helped me a lot

## Resources:

- PNG chunk doc: https://www.w3.org/TR/PNG-Chunks.html
- `man pngcheck`


***

# 3. RAR of the Abyss

> Two philosophers peer into the networked abyss and swap a secret. Use the secret to decrypt the Abyss’ RAwR and pull your flag from the void.

## Solution:

- It was a `.pcap` file so I opened it in wireshark
<img width="2560" height="1128" alt="image" src="https://github.com/user-attachments/assets/06b24133-54ac-4368-8607-b8660d89aed9" />

- It showed multiple protocols and packets
- I scrolled through them and some of them had conversations in them, so I found the password in a TCP packet's strings
<img width="1257" height="251" alt="image" src="https://github.com/user-attachments/assets/969b9e7f-c4d2-4caa-8b67-e7795c3e7517" />

- Also found packet which contained RAR file
<img width="852" height="231" alt="image" src="https://github.com/user-attachments/assets/a69ec120-88cc-44e9-99ef-c3ae124295ac" />

- Then I selected the RAR part and clicked on
<img width="2131" height="796" alt="image" src="https://github.com/user-attachments/assets/8841337a-0d95-48d0-855d-085051662b30" />

- Then I selected show as RAW and finally saved it as `ctf.rar`
- When I tried to extract it, it asked for password, so I entered `b3y0ndG00dand3vil`
- It extracted the RAR and I got `flag.txt` which had flag in it


## Flag:

```
nite{thus_sp0k3_th3_n3tw0rk_f0r3ns1cs_4n4lyst}

```

## Concepts learnt:

- I learnt how to extract RAR file from TCP packets
- Also learnt how to get info from packets and read strings/hex from it

## Notes:

- First got confused by overwhelming packets, but after looking into all the data inside them, I realised I can just extract RAR and open it with given password

## Resources:

- Wireshark: https://www.wireshark.org/#download


***

# 4. NineTails

> Description: Looks like I got a little too clever and hid the flag as a password in Firefox, tucked away like one of NineTails’ many tails. Recover the "logins" and the "key4" and let it guide you to the flag. Hint: I named my Ninetails "j4gjesg4", quite a peculiar name isn't it?

## Solution:

- Extracted the rar and got a ad1 file
- Searched about it and found out it is supposed to be opened in a program called `FTK imager`
- After openening I looked through all the files and got lost, then properly read the description and realised I have to look for password in firefox folder
- So I went in appdata and local, there I found firefox under mozilla folder but there was no sign of passwords or anything useful under username `j4gjesg4`
- I searched and found they are actually stored in roaming, so I went there and got the db and json file
<img width="1902" height="1108" alt="image" src="https://github.com/user-attachments/assets/55cd1a56-2234-4882-b38d-3b58ff65e5e6" />

- Then I searched for a tool that can crack passwords using db file from the json
- A tool called `firepwd` worked for me
- It ccracked and showed me the output
```
clearText b'ab1ccd54c13b1573648a347a6e4c73dcc27af8cb8ad3c74c0808080808080808'
decrypting login/password pairs
https://www.rehack.xyz:b'warlocksmurf',b'GCTF{m0zarella'
 https://ctftime.org:b'ilovecheese',b'CHEEEEEEEEEEEEEEEEEEEEEEEEEESE'
https://www.reddit.com:b'bluelobster',b'_f1ref0x_'
https://www.facebook.com:b'flag',b'SIKE'
https://warlocksmurf.github.io:b'Man I Love Forensics',b'p4ssw0rd}'
```
- This showed the flag


## Flag:

```
GCTF{m0zarella_f1ref0x_p4ssw0rd}
```

## Concepts learnt:

- Learnt how passwords are unsecurily stored in browsers
- Learnt how to crack them if I just get access to read files on a computer
- Also learnt about `ad1` image archives, which can be opened in programs to look through files
- ~~learnt how annoying python can get~~

## Notes:

- Wasted 30-45 minutes on exploring local folder, wondering why db and json files is not there, finally mozilla's own page came in clutch
- Also spent ~1hr just finding a proper decryptor, some only scanned appdata folder and not specific directory, others had some version issues with deprecated module
- `firepwd` didn't work locally either, but then after almost an hour of trying I ran it in virtual environment on WSL, finally that gave me the flag

## Resources:

- Mozilla's page on where passwords are stored: https://support.mozilla.org/en-US/kb/profiles-where-firefox-stores-user-data
- [firepwd](https://github.com/lclevy/firepwd) tool


***

# 5. Challenge name

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
