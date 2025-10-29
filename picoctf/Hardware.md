# 1. IQ Test

> let your input x = 30478191278. wrap your answer with nite{ } for the flag. As an example, entering x = 34359738368 gives (y0, ..., y11), so the flag would be nite{010000000011}.

## Solution:

- Opened image
- It had combination of XOR, XNOR, NAND, AND, OR, NOR and NOT gate
- So I converted `x=30478191278` to binary and got `11100011000101001000100101010101110` which I used as input
- Then I tried to write and solve it manually:
<img width="1225" height="2176" alt="iqtest(1)" src="https://github.com/user-attachments/assets/fbe0a07d-6443-42eb-a82d-9f15d2fd89e8" />


- Thus I got the output and flag


## Flag:

```
nite{100010011000}
```

## Concepts learnt:

- Manual Labour

## Notes:

- 35 inputs are exhausting

## Resources:

NA


***

# 2. I like Logic

> i like logic and i like files, apparently, they have something in common, what should my next step be.

## Solution:

- First I searched up about `.sal` files
- Found a tool similar to title called Logic, which can help me decode the signal
- Installed it and looked around, channel three had huge yellow color block, after zooming it there were different waveforms
<img width="1779" height="1115" alt="image" src="https://github.com/user-attachments/assets/fdcbd7f7-33c4-4b96-a601-b461220aab6f" />

- These looked similar to morse code but had thousands of it, so I decided to assume they aren't morse code and I should stick to decrypting using logic
- I searched around a bit how to use Logic 2 and it turns out we can simple view raw text or ASCII values from waveform using `ANalyzers Tab > Async Serials`
- I was confused how to configure it so I searched up about what bit rate to set, and other settings but didn't find much promising results, except few plugins that can give me some values
- At the end I just selected let's select channel 3 and go with default values to see if it outputs anything
<img width="780" height="867" alt="image" src="https://github.com/user-attachments/assets/d1982bb6-5685-45ed-b2b9-309f89d79f58" />

- Surprisingly it worked it showed me the table of ASCII values, then after messing a bit I realised I can properly view the whole text by just pressing the terminal button
- I got the following text:
```
on't think he makes any claims of
that kind.  But I do believe he has got something new."

"Then for Heaven's sake, man, write it up!"

"I'm longing to, but all I know he gave me in confidence and on
condition that I didn't."  I condensed into a few sentences the
Professor's narrative.  "That's how it stands."

McArdle looked deeply incredulous.

"Well, Mr. Malone," he said at last, "about this scientific meeting
to-night; there can be no privacy about that, anyhow.  I don't suppose
any paper will want to report it, for Waldron has been reported already
a dozen times, and no one is aware that Challenger will speak.  We may
get a scoop, if we are lucky.  You'll be there in any case, so you'll
just give us a pretty full report.  I'll keep space up to midnight."

My day was a busy one, and I had an early dinner at the Savage Club
with Tarp Henry, to whom I gave some account of my adventures.  He
listened with a sceptical smile on his gaunt face, and roared with
laughter on hearing that the Professor had convinced me.

"My dear chap, things don't happen like that in real life.  People
don't stumble upon enormous discoveries and then lose their evidence.
Leave that to the novelists.  The fellow is as full of tricks as the
monkey-house at the Zoo.  It's all bosh."

"But the American poet?"

"He never existed."

"I saw his sketch-book."

"Challenger's sketch-book."

"You think he drew that animal?"

"Of course he did.  Who else?"

"Well, then, the photographs?"

"There was nothing in the photographs.  By your own admission you only
saw a bird."

FCSC{b1dee4eeadf6c4e60aeb142b0b486344e64b12b40d1046de95c89ba5e23a9925}

"A pterodactyl."

"That's what HE says.  He put the pterodactyl into your head."

"Well, then, the bones?"

"First one out of an Irish stew.  Second one vamped up for the
occasion.  If you are clever and know your business you can fake a bone
as easily as you can a photograph."

I began to feel uneasy.  Perhaps, after all, I had been premature in my
acquiescence.  Then I had a sudden happy thought.

"Will you come to the meeting?" I asked.

Tarp Henry looked thoughtful.

"He is not a popular person, the genial Challenger," said he.  "A lot
of people have accounts to settle with him.  I should say he is about
the best-hated man in London.  If the medical students turn out there
will be no end of a rag.  I don't want to get into a bear-garden."

"You might at least do him the justice to hear him state his own case."

"Well, perhaps it's only fair.  All right.  I'm your man for the
evening."

When we arrived at the hall we found a much greater concourse than I
had expected.  A line of electric broughams discharged their little
cargoes of white-bearded professors, while the dark stream of humbler
pedestrians, who crowded through the arched door-way, showed that the
audience would be popular as well as scientific.  Indeed, it became
evident to us as soon as we had taken our seats that a youthful and
even boyish spirit was abroad in the gallery and the back portions of
the hall.  Looking behind me, I could see rows of faces of the familiar
medical student type.  Apparently the great hospitals had each sent
down their contingent.  The behavior of the audience at present was
good-humored, but mischievous.  Scraps of popular songs were chorused
with an enthusiasm which was a strange prelude to a scientific lecture,
and there was already a tendency to personal chaff which promised a
jovial evening to others, however embarrassing it might be to the
recipients of these dubious honors.

Thus, when old Doctor Meldrum, with his well-known curly-brimmed
opera-hat, appeared upon the platform, there was such a universal query
of "Where DID you get that tile?" that he hurriedly removed it, and
concealed it furtively under his chair.  When gouty Professor Wadley
limped down to his seat there were general affectionate inquiries from
all parts of the hall as to the exact state of his poor toe, which
caused him obvious embarrassment.  The greatest demonstration of all,
however, was at the entrance of my new acquaintance, Professor
Challenger, when he passed down to take his place at the extreme end of
the front row of the platform.  Such a yell of welcome broke forth when
his black beard first protruded round the corner that I began to
suspect Tarp Henry was right in his surmise, and that this assemblage
was there not merely for the sake of the lecture, but because it had
got rumored abroad that the famous Professor would take part in the
proceedings.

There was some sympathetic laughter on his entrance among the front
benches of well-dressed spectators, as though the demonstration of the
students in this instance was not unwelcome to them.  That greeting
was, indeed, a frightful outburst of sound, the uproar of the carnivora
cage when the step of the bucket-bearing keeper is heard in the
distance.  There was an offensive tone in it, perhaps, and yet in the
main it struck me as mere riotous outcry, the noisy reception of one
who amused and interested them, rather than of one they disliked or
despised.  Challenger smiled with weary and tolerant contempt, as a
kindly man would meet the yapping of a litter of puppies.  He sat
slowly down, blew out his chest, passed his hand caressingly down his
beard, and looked with drooping eyelids and supercilious eyes at the
crowded hall before him.  The uproar of his advent had not yet died
away when Professor Ronald Murray, the chairman, and Mr. Waldron, the
lecturer, threaded their way to the front, and the proceedings began.

Professor Murray will, I am sure, excuse me if I say that he has the
common fault of most Englishmen of being inaudible.  Why on earth
people who have something to say which is worth hearing should not take
the slight trouble to learn how to make it heard is one of the strange
mysteries of modern life.  Their methods are as reasonable as to try to
pour some precious stuff from the spring to the reservoir through a
non-conducting pipe, which could by the least effort be opened.
Professor Murray made several profound remarks to his white tie and to
the water-carafe upon the table, with a humorous, twinkling aside to
the silver candlestick upon his right.  Then he sat down, and Mr.
Waldron, the famous popular lecturer, rose amid a general murmur of
applause.  He was a stern, gaunt man, with a harsh voice, and an
aggressive manner, but he had the merit of knowing how to assimilate
the ideas of other men, and to pass them on in a way which was
intelligible and even interesting to the lay public, with a happy knack
of being funny about the most unlikely objects, so that the precession
of the Equinox or the formation of a vertebrate became a highly
humorous process as treated by him.

It was a bird's-eye view of creation, as interpreted by science, which,
in language always clear and sometimes picturesque, he unfolded before
us.  He told us of ton't think he makes any claims of
that kind.  But I do believe he has got something new."

"Then for Heaven's sake, man, write it up!"

"I'm longing to, but all I know he gave me in confidence and on
condition that I didn't."  I condensed into a few sentences the
Professor's narrative.  "That's how it stands."

McArdle looked deeply incredulous.

"Well, Mr. Malone," he said at last, "about this scientific meeting
to-night; there can be no privacy about that, anyhow.  I don't suppose
any paper will want to report it, for Waldron has been reported already
a dozen times, and no one is aware that Challenger will speak.  We may
get a scoop, if we are lucky.  You'll be there in any case, so you'll
just give us a pretty full report.  I'll keep space up to midnight."

My day was a busy one, and I had an early dinner at the Savage Club
with Tarp Henry, to whom I gave some account of my adventures.  He
listened with a sceptical smile on his gaunt face, and roared with
laughter on hearing that the Professor had convinced me.

"My dear chap, things don't happen like that in real life.  People
don't stumble upon enormous discoveries and then lose their evidence.
Leave that to the novelists.  The fellow is as full of tricks as the
monkey-house at the Zoo.  It's all bosh."

"But the American poet?"

"He never existed."

"I saw his sketch-book."

"Challenger's sketch-book."

"You think he drew that animal?"

"Of course he did.  Who else?"

"Well, then, the photographs?"

"There was nothing in the photographs.  By your own admission you only
saw a bird."

FCSC{b1dee4eeadf6c4e60aeb142b0b486344e64b12b40d1046de95c89ba5e23a9925}

"A pterodactyl."

"That's what HE says.  He put the pterodactyl into your head."

"Well, then, the bones?"

"First one out of an Irish stew.  Second one vamped up for the
occasion.  If you are clever and know your business you can fake a bone
as easily as you can a photograph."

I began to feel uneasy.  Perhaps, after all, I had been premature in my
acquiescence.  Then I had a sudden happy thought.

"Will you come to the meeting?" I asked.

Tarp Henry looked thoughtful.

"He is not a popular person, the genial Challenger," said he.  "A lot
of people have accounts to settle with him.  I should say he is about
the best-hated man in London.  If the medical students turn out there
will be no end of a rag.  I don't want to get into a bear-garden."

"You might at least do him the justice to hear him state his own case."

"Well, perhaps it's only fair.  All right.  I'm your man for the
evening."

When we arrived at the hall we found a much greater concourse than I
had expected.  A line of electric broughams discharged their little
cargoes of white-bearded professors, while the dark stream of humbler
pedestrians, who crowded through the arched door-way, showed that the
audience would be popular as well as scientific.  Indeed, it became
evident to us as soon as we had taken our seats that a youthful and
even boyish spirit was abroad in the gallery and the back portions of
the hall.  Looking behind me, I could see rows of faces of the familiar
medical student type.  Apparently the great hospitals had each sent
down their contingent.  The behavior of the audience at present was
good-humored, but mischievous.  Scraps of popular songs were chorused
with an enthusiasm which was a strange prelude to a scientific lecture,
and there was already a tendency to personal chaff which promised a
jovial evening to others, however embarrassing it might be to the
recipients of these dubious honors.

Thus, when old Doctor Meldrum, with his well-known curly-brimmed
opera-hat, appeared upon the platform, there was such a universal query
of "Where DID you get that tile?" that he hurriedly removed it, and
concealed it furtively under his chair.  When gouty Professor Wadley
limped down to his seat there were general affectionate inquiries from
all parts of the hall as to the exact state of his poor toe, which
caused him obvious embarrassment.  The greatest demonstration of all,
however, was at the entrance of my new acquaintance, Professor
Challenger, when he passed down to take his place at the extreme end of
the front row of the platform.  Such a yell of welcome broke forth when
his black beard first protruded round the corner that I began to
suspect Tarp Henry was right in his surmise, and that this assemblage
was there not merely for the sake of the lecture, but because it had
got rumored abroad that the famous Professor would take part in the
proceedings.

There was some sympathetic laughter on his entrance among the front
benches of well-dressed spectators, as though the demonstration of the
students in this instance was not unwelcome to them.  That greeting
was, indeed, a frightful outburst of sound, the uproar of the carnivora
cage when the step of the bucket-bearing keeper is heard in the
distance.  There was an offensive tone in it, perhaps, and yet in the
main it struck me as mere riotous outcry, the noisy reception of one
who amused and interested them, rather than of one they disliked or
despised.  Challenger smiled with weary and tolerant contempt, as a
kindly man would meet the yapping of a litter of puppies.  He sat
slowly down, blew out his chest, passed his hand caressingly down his
beard, and looked with drooping eyelids and supercilious eyes at the
crowded hall before him.  The uproar of his advent had not yet died
away when Professor Ronald Murray, the chairman, and Mr. Waldron, the
lecturer, threaded their way to the front, and the proceedings began.

Professor Murray will, I am sure, excuse me if I say that he has the
common fault of most Englishmen of being inaudible.  Why on earth
people who have something to say which is worth hearing should not take
the slight trouble to learn how to make it heard is one of the strange
mysteries of modern life.  Their methods are as reasonable as to try to
pour some precious stuff from the spring to the reservoir through a
non-conducting pipe, which could by the least effort be opened.
Professor Murray made several profound remarks to his white tie and to
the water-carafe upon the table, with a humorous, twinkling aside to
the silver candlestick upon his right.  Then he sat down, and Mr.
Waldron, the famous popular lecturer, rose amid a general murmur of
applause.  He was a stern, gaunt man, with a harsh voice, and an
aggressive manner, but he had the merit of knowing how to assimilate
the ideas of other men, and to pass them on in a way which was
intelligible and even interesting to the lay public, with a happy knack
of being funny about the most unlikely objects, so that the precession
of the Equinox or the formation of a vertebrate became a highly
humorous process as treated by him.

It was a bird's-eye view of creation, as interpreted by science, which,
in language always clear and sometimes picturesque, he unfolded before
us.  He told us of t
```
- The flag is hidden inside it

## Flag:

```
FCSC{b1dee4eeadf6c4e60aeb142b0b486344e64b12b40d1046de95c89ba5e23a9925}
```

## Concepts learnt:

- I learnt about `.sal` files and how we can view waveform and signals using Logic
- We can further analyze this in detail too and decode them to binary, hex or ASCII
- Thus we can plugin specific hardware devices and check different channels to see different signals

## Notes:

- This was pretty unique challenge and I had no clue about `.sal` at first
- After searching a bit, it became easier and I was able to figure out what to do next and how to decode waveform

## Resources:

- Pentesting guide for `.sal`: https://exploit-notes.hdks.org/exploit/hardware/sal-logic-analysis/


***

# 3. Bare Metal Alchemist

> my friend recommended me this anime but i think i've heard a wrong name.

## Solution:

- First I installed the file `firmware` and used `file` on it
- Found out it is: `firmware: ELF 32-bit LSB executable, Atmel AVR 8-bit, version 1 (SYSV), statically linked, with debug_info, not stripped`
- This meant it is an executable file (for linux), and debug info is not stripped (didn't have use for this info tbh)
- I now used `cat`, `string` and `binwalk` but that didn't help at all
- So I decided to use a online decompiler to check source code (it had multiple decompilers: ghidra, binary ninja, reko etc.)
- Reko showed the following:
```
byte g_b006E = 222; // 006E
byte g_b007A = 0x96; // 007A
byte g_b0080 = 0xC9; // 0080
byte g_b0081 = 0x96; // 0081
byte g_b00B0 = 0x0D; // 00B0
byte g_b00B1 = 0x92; // 00B1
byte g_b00C1 = 0x92; // 00C1
// 00D4: void z1()
```
- This kind of looked like keys of XOR encryption
- Single byte XOR is pretty easy to bruteforce, so I decided to try that
- So we can use a python script for this:
```
import re

def xor_data(binary_data, key_byte):
    """XORs each byte in binary_data with a single key_byte."""
    return bytes(b ^ key_byte for b in binary_data)

def find_flag():
    filename = "./firmware"
    flag_pattern = re.compile(rb"[A-Za-z0-9_]{1,30}\{[A-Za-z0-9_\-\+\=\/\\\.\s]{4,200}\}")

    try:
        with open(filename, "rb") as f:
            encrypted_content = f.read()
    except FileNotFoundError:
        print(f"'{filename}' not found.")
        return
        
    for key in range(1, 256):
        decrypted_content = xor_data(encrypted_content, key)
        match = flag_pattern.search(decrypted_content)
        if match:
            print(f"This key matches: {key}")
            print(f"Flag: {match.group().decode()}")

if __name__ == "__main__":
    find_flag()
```
This gives the output:
```
This key matches: 9
Flag: mfVjelh{VkzzVz}
This key matches: 11
Flag: T{ydlyjf
              TT}
This key matches: 19
Flag: LL{vrcLv}
This key matches: 24
Flag: GG{lwjkG}
This key matches: 26
Flag: lh5{lh/5vsx}
This key matches: 30
Flag: Aqh{lxrqiA}
This key matches: 46
Flag: ojc{v.ojm.ojm}
This key matches: 52
Flag: 4kkqqdf{ykfqs}
This key matches: 56
Flag: yt8{tshj8kj}
This key matches: 57
Flag: 9mnxtk9mn{k9mnzk9mnjk9mn}
This key matches: 66
Flag: HBB{vBALxIyI
                  Q}
This key matches: 68
Flag: W{HFNDD}
This key matches: 71
Flag: TxKEMGG{sGDI}
This key matches: 107
Flag: mzjyjhcpcNcxnkkkjzk{mzjyjhcpcNcxnkkk}
This key matches: 109
Flag: hmmm{mmmom}
This key matches: 118
Flag: vv{BvuxL}
This key matches: 121
Flag: yy{yyyyy}
This key matches: 122
Flag: zzzzzzzzxuzzz{rymzzzx}
This key matches: 165
Flag: TFCCTF{Th1s_1s_som3_s1mpl3_4rdu1no_f1rmw4re}
```
- Thus we find the flag



## Flag:

```
TFCCTF{Th1s_1s_som3_s1mpl3_4rdu1no_f1rmw4re}
```

## Concepts learnt:

- Learnt how to bruteforce XOR encryption (especially when it is single byte)
- Learnt more about ELF files

## Notes:

- This was extremely confusing at first as I tried literally everything I could think of, which includes stegnography, reading its strings or compiled code
- I even went against my own morals and ethics and decided to give it execution perms and executed it, but that gave me an error

## Resources:

- Info about XOR: https://ctf101.org/cryptography/what-is-xor/
- Online decompiler (file is retained in url): https://dogbolt.org/?id=3eaba4d3-407f-4b76-b3b0-de51612bf74d#Reko=7&BinaryNinja=25
