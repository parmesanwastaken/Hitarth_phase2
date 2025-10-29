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

# 2. Custom encryption

> Can you get sense of this code file and write the function that will decode the given encrypted file content. Find the encrypted file here flag_info and code file might be good to analyze and get the flag. 

## Solution:

- First I installed both the files
- Then I looked through the python file to understand the encryption (as mentioned in hint)
- We notice that the shared key is encrypted using `Diffie-Hellman: (g^(a*b)) % p`
- Also we can reverse outer 'encrypt' function by sending RHS to LHS and vica versa
- The reversed plaintext is XOR'd by by 'dynamic_xor_encrypt', by reversing this, we get reversed flag (this works since XOR is its own inverse)
- Now we can simply reverse the result and get the flag
- We can create a python script for this whole process:
```python

p = 97
g = 31
a = 89
b = 27
text_key = "trudeau"
cipher = [33588, 276168, 261240, 302292, 343344, 328416, 242580, 85836, 82104, 156744, 0, 309756, 78372, 18660, 253776, 0, 82104, 320952, 3732, 231384, 89568, 100764, 22392, 22392, 63444, 22392, 97032, 190332, 119424, 182868, 97032, 26124, 44784, 63444]


shared_key = pow(g, a * b, p)
print(f"[*] Calculated shared key: {shared_key}")

semi_cipher = ""
for num in cipher:
    ord_val = num // (shared_key * 311)
    semi_cipher += chr(ord_val)

print(f"[*] Recovered semi_cipher: {semi_cipher}")

key_length = len(text_key)
reversed_plaintext = ""
for i, char in enumerate(semi_cipher):
    key_char = text_key[i % key_length]
    decrypted_char = chr(ord(char) ^ ord(key_char))
    reversed_plaintext += decrypted_char

print(f"[*] Recovered reversed plaintext: {reversed_plaintext}")

final_plaintext = reversed_plaintext[::-1]

print("\n" + "="*40)
print(f"Final flag is: {final_plaintext}")
print("="*40)
```

## Flag:

```
picoCTF{custom_d2cr0pt6d_dc499538}
```

## Concepts learnt:

- Kind of learnt how to read an encryption algorithm and thus reverse the whole thing

## Notes:

- Took me a lot of time, as there was no documentation on the given encryption. The whole process requires understanding the python code line by line and thus reversing this to get the flag from values in `enc_flag` file

## Resources:

- [Python basics](https://w3schools.com/python/)


***

# 3. miniRSA

> Let's decrypt this: ciphertext? Something seems a bit small.

## Solution:

- It was an RSA so I opened CTF101 page on it
- The exploitation section talked about RsaCtfTool, so I decided to look into it
- Also the wikipedia page had a section which said `e=3` is vulnerable and easy to crack using `eth root`
- After glancing on RsaCtfTool, I realised there is a cube root attack, that takes input of `N` and `ciphertext`
- So I used the command:
```
RsaCtfTool -n 29331922499794985782735976045591164936683059380558950386560160105740343201513369939006307531165922708949619162698623675349030430859547825708994708321803705309459438099340427770580064400911431856656901982789948285309956111848686906152664473350940486507451771223435835260168971210087470894448460745593956840586530527915802541450092946574694809584880896601317519794442862977471129319781313161842056501715040555964011899589002863730868679527184420789010551475067862907739054966183120621407246398518098981106431219207697870293412176440482900183550467375190239898455201170831410460483829448603477361305838743852756938687673 -e 3 --attack cube_root --decrypt 2205316413931134031074603746928247799030155221252519872650073010782049179856976080512716237308882294226369300412719995904064931819531456392957957122459640736424089744772221933500860936331459280832211445548332429338572369823704784625368933
```
- This gave me the output:
```
['/tmp/tmpvf7skubq']

[*] Testing key /tmp/tmpvf6ksubq.
[*] Performing cube_root attack on /tmp/tmpvf7skubq.
[+] Time elapsed: 0.0008 sec.
[+] Total time elapsed min,max,avg: 0.0008/0.0008/0.0008 sec.

Results for /tmp/tmpvf7skubq:

Decrypted data :
HEX : 0x7069636f4354467b6e3333645f615f6c41726733725f655f63636161373737367d
INT (big endian) : 13016382529449106065894479374027604750406953699090365388203708028670029596145277
INT (little endian) : 14498533606165685119718956852327439022714916090771037807901302412009841646332272
utf-8 : picoCTF{n33d_a_lArg3r_e_ccaa7776}
STR : b'picoCTF{n33d_a_lArg3r_e_ccaa7776}'
```
- Thus I got the flag!



## Flag:

```
picoCTF{n33d_a_lArg3r_e_ccaa7776}
```

## Concepts learnt:

- Learnt a lot about how RSA actually works
- Learnt about different RSA vulnerabilities too
- Even found out tools to automate this

## Notes:

- Was extremely confused for a long time about what to do, as RSA seemed complex
- Made mistakes such as finding and trying to attack using way more complex and time consuming method, thus wasted many hours on understanding RSA and attacks
- Once I fixed that `e=3` should be the perfec method, I was still confused on which tool to use
- After finding RsaCtfTool and reading the doc, it became simpler and automatically cracked it

## Resources:

- Wikipedia page on [RSA](https://en.wikipedia.org/wiki/RSA_cryptosystem)
- [RsaCtfTool](https://github.com/RsaCtfTool/RsaCtfTool)
- CTF101 page briefing [RSA](https://ctf101.org/cryptography/what-is-rsa/)
