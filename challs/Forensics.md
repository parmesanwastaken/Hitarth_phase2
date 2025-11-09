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

# 5. Re:Draw

> Her screen went black and a strange command window flickered to life, lines of text flashed across before everything went silent. Moments later, the system crashed. By sheer luck, we recovered a memory dump. Note: There are three stages to this challenge and you will find three flags. What we know: just before the crash, a black command window flickered across the screen, something in its output might still be visible if you dig through memory. She was drawing when it happened, and remnants of a painting program linger, which could reveal more if inspected in the right way. Finally, a mysterious archive hides deeper in memory, likely holding the last piece of her work. Hint: Learn up on volatility 2 and its various plugins and what they are used for.

## Solution:

- First downloaded the rar and extracted it, and got `MemoryDump_Lab1.raw`
- According to the hint I was supposed to use volatility 2
- So I installed the linux version from [github](https://github.com/volatilityfoundation/volatility/releases/tag/2.6.1)
- Now I glanced a bit over volatility [docs](https://github.com/volatilityfoundation/volatility/wiki/Command-Reference) for basic commands
- Although I still found it confusing so decided to search more and found an [article](https://infosecwriteups.com/memory-dump-analysis-by-using-volatility-framework-742d70663d41) which explained basic commands for analysis much better
- So I first used `imageinfo`
```
╰─ ./volatility_2.6_lin64_standalone -f MemoryDump_Lab1.raw imageinfo
Volatility Foundation Volatility Framework 2.6
INFO    : volatility.debug    : Determining profile based on KDBG search...
          Suggested Profile(s) : Win7SP1x64, Win7SP0x64, Win2008R2SP0x64, Win2008R2SP1x64_23418, Win2008R2SP1x64, Win7SP1x64_23418
                     AS Layer1 : WindowsAMD64PagedMemory (Kernel AS)
                     AS Layer2 : FileAddressSpace (/mnt/c/Users/mins_/Downloads/custom_ctf/volatility_2.6_lin64_standalone/MemoryDump_Lab1.raw)
                      PAE type : No PAE
                           DTB : 0x187000L
                          KDBG : 0xf800028100a0L
          Number of Processors : 1
     Image Type (Service Pack) : 1
                KPCR for CPU 0 : 0xfffff80002811d00L
             KUSER_SHARED_DATA : 0xfffff78000000000L
           Image date and time : 2019-12-11 14:38:00 UTC+0000
     Image local date and time : 2019-12-11 20:08:00 +0530
```
- Now I knew the image and I could use it in command to properly look through raw dump
- So from now on I appended `--profile=Win7SP1x64` in my command

### Part 1

- From description we know, before the crash a black command window flashed, which is command line
- I decided to confirm this by running `pslist`
```
╰─ ./volatility_2.6_lin64_standalone -f MemoryDump_Lab1.raw --profile=Win7SP1x64 pslist
Volatility Foundation Volatility Framework 2.6
Offset(V)          Name                    PID   PPID   Thds     Hnds   Sess  Wow64 Start                          Exit 
------------------ -------------------- ------ ------ ------ -------- ------ ------ ------------------------------ ------------------------------
0xfffffa8000ca0040 System                    4      0     80      570 ------      0 2019-12-11 13:41:25 UTC+0000        
0xfffffa800148f040 smss.exe                248      4      3       37 ------      0 2019-12-11 13:41:25 UTC+0000        
0xfffffa800154f740 csrss.exe               320    312      9      457      0      0 2019-12-11 13:41:32 UTC+0000        
0xfffffa8000ca81e0 csrss.exe               368    360      7      199      1      0 2019-12-11 13:41:33 UTC+0000        
0xfffffa8001c45060 psxss.exe               376    248     18      786      0      0 2019-12-11 13:41:33 UTC+0000        
0xfffffa8001c5f060 winlogon.exe            416    360      4      118      1      0 2019-12-11 13:41:34 UTC+0000        
0xfffffa8001c5f630 wininit.exe             424    312      3       75      0      0 2019-12-11 13:41:34 UTC+0000        
0xfffffa8001c98530 services.exe            484    424     13      219      0      0 2019-12-11 13:41:35 UTC+0000        
0xfffffa8001ca0580 lsass.exe               492    424      9      764      0      0 2019-12-11 13:41:35 UTC+0000        
0xfffffa8001ca4b30 lsm.exe                 500    424     11      185      0      0 2019-12-11 13:41:35 UTC+0000        
0xfffffa8001cf4b30 svchost.exe             588    484     11      358      0      0 2019-12-11 13:41:39 UTC+0000        
0xfffffa8001d327c0 VBoxService.ex          652    484     13      137      0      0 2019-12-11 13:41:40 UTC+0000        
0xfffffa8001d49b30 svchost.exe             720    484      8      279      0      0 2019-12-11 13:41:41 UTC+0000        
0xfffffa8001d8c420 svchost.exe             816    484     23      569      0      0 2019-12-11 13:41:42 UTC+0000        
0xfffffa8001da5b30 svchost.exe             852    484     28      542      0      0 2019-12-11 13:41:43 UTC+0000        
0xfffffa8001da96c0 svchost.exe             876    484     32      941      0      0 2019-12-11 13:41:43 UTC+0000        
0xfffffa8001e1bb30 svchost.exe             472    484     19      476      0      0 2019-12-11 13:41:47 UTC+0000        
0xfffffa8001e50b30 svchost.exe            1044    484     14      366      0      0 2019-12-11 13:41:48 UTC+0000        
0xfffffa8001eba230 spoolsv.exe            1208    484     13      282      0      0 2019-12-11 13:41:51 UTC+0000        
0xfffffa8001eda060 svchost.exe            1248    484     19      313      0      0 2019-12-11 13:41:52 UTC+0000        
0xfffffa8001f58890 svchost.exe            1372    484     22      295      0      0 2019-12-11 13:41:54 UTC+0000        
0xfffffa8001f91b30 TCPSVCS.EXE            1416    484      4       97      0      0 2019-12-11 13:41:55 UTC+0000        
0xfffffa8000d3c400 sppsvc.exe             1508    484      4      141      0      0 2019-12-11 14:16:06 UTC+0000        
0xfffffa8001c38580 svchost.exe             948    484     13      322      0      0 2019-12-11 14:16:07 UTC+0000        
0xfffffa8002170630 wmpnetwk.exe           1856    484     16      451      0      0 2019-12-11 14:16:08 UTC+0000        
0xfffffa8001d376f0 SearchIndexer.          480    484     14      701      0      0 2019-12-11 14:16:09 UTC+0000        
0xfffffa8001eb47f0 taskhost.exe            296    484      8      151      1      0 2019-12-11 14:32:24 UTC+0000        
0xfffffa8001dfa910 dwm.exe                1988    852      5       72      1      0 2019-12-11 14:32:25 UTC+0000        
0xfffffa8002046960 explorer.exe            604   2016     33      927      1      0 2019-12-11 14:32:25 UTC+0000        
0xfffffa80021c75d0 VBoxTray.exe           1844    604     11      140      1      0 2019-12-11 14:32:35 UTC+0000        
0xfffffa80021da060 audiodg.exe            2064    816      6      131      0      0 2019-12-11 14:32:37 UTC+0000        
0xfffffa80022199e0 svchost.exe            2368    484      9      365      0      0 2019-12-11 14:32:51 UTC+0000        
0xfffffa8002222780 cmd.exe                1984    604      1       21      1      0 2019-12-11 14:34:54 UTC+0000        
0xfffffa8002227140 conhost.exe            2692    368      2       50      1      0 2019-12-11 14:34:54 UTC+0000        
0xfffffa80022bab30 mspaint.exe            2424    604      6      128      1      0 2019-12-11 14:35:14 UTC+0000        
0xfffffa8000eac770 svchost.exe            2660    484      6      100      0      0 2019-12-11 14:35:14 UTC+0000        
0xfffffa8001e68060 csrss.exe              2760   2680      7      172      2      0 2019-12-11 14:37:05 UTC+0000        
0xfffffa8000ecbb30 winlogon.exe           2808   2680      4      119      2      0 2019-12-11 14:37:05 UTC+0000        
0xfffffa8000f3aab0 taskhost.exe           2908    484      9      158      2      0 2019-12-11 14:37:13 UTC+0000        
0xfffffa8000f4db30 dwm.exe                3004    852      5       72      2      0 2019-12-11 14:37:14 UTC+0000        
0xfffffa8000f4c670 explorer.exe           2504   3000     34      825      2      0 2019-12-11 14:37:14 UTC+0000        
0xfffffa8000f9a4e0 VBoxTray.exe           2304   2504     14      144      2      0 2019-12-11 14:37:14 UTC+0000        
0xfffffa8000fff630 SearchProtocol         2524    480      7      226      2      0 2019-12-11 14:37:21 UTC+0000        
0xfffffa8000ecea60 SearchFilterHo         1720    480      5       90      0      0 2019-12-11 14:37:21 UTC+0000        
0xfffffa8001010b30 WinRAR.exe             1512   2504      6      207      2      0 2019-12-11 14:37:23 UTC+0000        
0xfffffa8001020b30 SearchProtocol         2868    480      8      279      0      0 2019-12-11 14:37:23 UTC+0000        
0xfffffa8001048060 DumpIt.exe              796    604      2       45      1      1 2019-12-11 14:37:54 UTC+0000        
0xfffffa800104a780 conhost.exe            2260    368      2       50      1      0 2019-12-11 14:37:54 UTC+0000
```
- `cmd.exe` confirms that command line ran
- The article mentioned that we can get command history using `consoles command`
```
╰─ ./volatility_2.6_lin64_standalone -f MemoryDump_Lab1.raw --profile=Win7SP1x64 consoles
Volatility Foundation Volatility Framework 2.6
**************************************************
ConsoleProcess: conhost.exe Pid: 2692
Console: 0xff756200 CommandHistorySize: 50
HistoryBufferCount: 1 HistoryBufferMax: 4
OriginalTitle: %SystemRoot%\system32\cmd.exe
Title: C:\Windows\system32\cmd.exe - St4G3$1
AttachedProcess: cmd.exe Pid: 1984 Handle: 0x60
----
CommandHistory: 0x1fe9c0 Application: cmd.exe Flags: Allocated, Reset
CommandCount: 1 LastAdded: 0 LastDisplayed: 0
FirstCommand: 0 CommandCountMax: 50
ProcessHandle: 0x60
Cmd #0 at 0x1de3c0: St4G3$1
----
Screen 0x1e0f70 X:80 Y:300
Dump:
Microsoft Windows [Version 6.1.7601]
Copyright (c) 2009 Microsoft Corporation.  All rights reserved.

C:\Users\SmartNet>St4G3$1
ZmxhZ3t0aDFzXzFzX3RoM18xc3Rfc3Q0ZzMhIX0=
Press any key to continue . . .
**************************************************
ConsoleProcess: conhost.exe Pid: 2260
Console: 0xff756200 CommandHistorySize: 50
HistoryBufferCount: 1 HistoryBufferMax: 4
OriginalTitle: C:\Users\SmartNet\Downloads\DumpIt\DumpIt.exe
Title: C:\Users\SmartNet\Downloads\DumpIt\DumpIt.exe
AttachedProcess: DumpIt.exe Pid: 796 Handle: 0x60
----
CommandHistory: 0x38ea90 Application: DumpIt.exe Flags: Allocated
CommandCount: 0 LastAdded: -1 LastDisplayed: -1
FirstCommand: 0 CommandCountMax: 50
ProcessHandle: 0x60
----
Screen 0x371050 X:80 Y:300
Dump:
  DumpIt - v1.3.2.20110401 - One click memory memory dumper
  Copyright (c) 2007 - 2011, Matthieu Suiche <http://www.msuiche.net>
  Copyright (c) 2010 - 2011, MoonSols <http://www.moonsols.com>


    Address space size:        1073676288 bytes (   1023 Mb)
    Free space size:          24185389056 bytes (  23064 Mb)

    * Destination = \??\C:\Users\SmartNet\Downloads\DumpIt\SMARTNET-PC-20191211-
143755.raw

    --> Are you sure you want to continue? [y/n] y
    + Processing...
```
- This gave me the first flag in base64, so I [decoded](https://www.base64decode.org/) it and got the flag

### Part 2

- The description also talked about, painting program lingering and drawing
- We can confirm this as `mspaint.exe` is seen in running processes before crash
- I searched around more and found an [article](https://w00tsec.blogspot.com/2015/02/extracting-raw-pictures-from-memory.html) which discussed how we can recover display from memory dumps
- Before I found the article I used `procdump` to dump files, which gave redundant results
- I then followed the article and used `memdump`
```
╰─ ./volatility_2.6_lin64_standalone -f MemoryDump_Lab1.raw --profile=Win7SP1x64 memdump -p 2424 -D ./output
Volatility Foundation Volatility Framework 2.6
************************************************************************
Writing mspaint.exe [  2424] to 2424.dmp
```
- I renamed the file with `.data` extension and opened it in [GIMP](https://www.gimp.org/downloads/) as raw data
<img width="1277" height="1406" alt="image" src="https://github.com/user-attachments/assets/4b68ab13-6413-4efa-8d3d-f0b3ce27c119" />

- Then I wasted hours on this, messing with offset and dimensions but had not luck
- Decided to ask mentor for help and after a few hints I was able to reach the point where characters were visible
<img width="1277" height="1406" alt="image" src="https://github.com/user-attachments/assets/fe7105aa-ca7d-462d-bc53-d792ff039fa5" />

- I opened this in gimp and rotated it 180 degree, then flipped it vartically
- This gave me the flag

![mspaint2](https://github.com/user-attachments/assets/cfa633b5-f06d-4950-933c-3df1c422df64)

### Part 3

- While I was extracting mspaint stuff, I also did `filescan` commands multiple times with png, jpg, paint as piped grep
- This didn;t give anything useful, but I also tried rar, which gave me:
```
╰─ ./volatility_2.6_lin64_standalone -f MemoryDump_Lab1.raw --profile=Win7SP1x64 filescan | grep -E '\.rar'
Volatility Foundation Volatility Framework 2.6
0x000000003fa3ebc0      1      0 R--r-- \Device\HarddiskVolume2\Users\Alissa Simpson\Documents\Important.rar
0x000000003fac3bc0      1      0 R--r-- \Device\HarddiskVolume2\Users\Alissa Simpson\Documents\Important.rar
0x000000003fb48bc0      1      0 R--r-- \Device\HarddiskVolume2\Users\Alissa Simpson\Documents\Important.rar
```
- I decided to dump `Important.rar` file using `╰─ ./volatility_2.6_lin64_standalone -f MemoryDump_Lab1.raw --profile=Win7SP1x64 dumpfiles -Q 0x000000003eb0b770 -D ./output`
- This gave me a `.dat` file which I renamed to `.rar` (btw all 3 above files were same rar)
- Then I tried to unrar it:
```
╰─ unrar e output/test.rar

UNRAR 7.00 freeware      Copyright (c) 1993-2024 Alexander Roshal

Archive comment:
Password is NTLM hash(in uppercase) of Alissa's account passwd.


Extracting from output/test.rar

Enter password (will not be echoed) for flag3.png:
```
- It was password locked and I was told the password was hash of Alissa's account password
- I looked into docs again and found `hashdump` which can give me hashes of various accounts
```
╰─ ./volatility_2.6_lin64_standalone -f MemoryDump_Lab1.raw --profile=Win7SP1x64 hashdump
Volatility Foundation Volatility Framework 2.6
Administrator:500:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
Guest:501:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
SmartNet:1001:aad3b435b51404eeaad3b435b51404ee:4943abb39473a6f32c11301f4987e7e0:::
HomeGroupUser$:1002:aad3b435b51404eeaad3b435b51404ee:f0fc3d257814e08fea06e63c5762ebd5:::
Alissa Simpson:1003:aad3b435b51404eeaad3b435b51404ee:f4ff64c8baac57d22f22edc681055ba6:::
```
- Thus I found the password for rar: `F4FF64C8BAAC57D22F22EDC681055BA6`
- After extracting it, I got the third flag!!
<img width="500" height="500" alt="flag3" src="https://github.com/user-attachments/assets/07d8f60d-b383-4c67-a519-a5c1e8bae3de" />


## Flag:

```
flag{th1s_1s_th3_1st_st4g3!!}
flag{G00d_BoY_good_girL}
flag{w3ll_3rd_stage_was_easy}
```

## Concepts learnt:

- This challenge alone taught me a lot
- I learnt how to use volatality, which seems extremely useful for memory forensics
- I had no idea I can access screens, clipboard and literally everything on computer including sensitive data by a simple memory dump and it is easy to analyse offline too
- One of the most useful part was that one can see display of any process by simply dumping it and opening it as raw image data in a program such as `GIMP`

## Notes:

- This was extremely confusing and spent a lot of time on it
- Before I looked through docs, I just wasted time on help command output
- Later I spent 3-4 hours on offsetting `procdump` mspaint file
- The output image was slanted at first and that confused me too
- And many more trivial mistakes that wasted a lot of time when grouped together
- Glad that after spending so much time, this is over!

## Resources:

- Attatched em in solution this time
