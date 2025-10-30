# 1. rsa_oracle

> Can you abuse the oracle? An attacker was able to intercept communications between a bank and a fintech company. They managed to get the message (ciphertext) and the password that was used to encrypt the message. Additional details will be available after launching your challenge instance.

## Solution:

- First I connected through netcat to figure out the challenge
- I realised I can decrypt and encrypt anything, but ofc direct password decrypt didn't work lol
- So in order to leverage the oracle, I decided to understand how RSA even works
- `(m^e)^d ~= m (mod n)`, this is what makes RSA work
- I realised that it is multiplicative, i.e if I multiply 2 encrypted messages and later divide them after decrypting then I should get one input
- But there is a catch, it converts ASCII to Hex, so I need to convert hex to decimal, then divide and then convert it back to ASCII
- After I got the rough plan, I decided to follow all the steps
- First I inputted `2`, and the oracle gave out:
```
*****************************************
****************THE ORACLE***************
*****************************************
what should we do for you?
E --> encrypt D --> decrypt.
e
enter text to encrypt (encoded length must be less than keysize): 2
2

encoded cleartext as Hex m: 32

ciphertext (m ^ e mod n) 4707619883686427763240856106433203231481313994680729548861877810439954027216515481620077982254465432294427487895036699854948548980054737181231034760249505
```
- Then I used python script the multiply both encrypted texts (`2` and `password.enc`):
```python
a = 4707619883686427763240856106433203231481313994680729548861877810439954027216515481620077982254465432294427487895036699854948548980054737181231034760249505
b = 2336150584734702647514724021470643922433811330098144930425575029773908475892259185520495303353109615046654428965662643241365308392679139063000973730368839

result = a * b
print(result)
```
- This gave me another encrypted output: `10997708943982761084006315359417483254965299487204584192712335192036789472336196626179282134890223733758401125471056267054908321079024432384222437910457194483711112753102678178170094968585207806212096960492328042941752878907452001886104974213833155189826877814877017136978779880432127774578986380439317174695`
- I used the oracle again to decrypt this and got `11637011932000`
- Now I converted `2` as an ASCII to decimal, which is `50`
- Thus I divided that with decrypted number above and got `232740238640`
- Now I converted this back to hex and got `3630663530`
- I converted this hex to ASCII and got `60f50`, this was the password to `secret.enc`
- According to hint I was supposed to use `openssl` to unencrypt the secret
- Thus I used `openssl enc -aes-256-cbc -d -in secret.enc`, this asked me for a password and I inputted `60f50` and got the flag!

## Flag:

```
picoCTF{su((3ss_(r@ck1ng_r3@_60f50766}
```

## Concepts learnt:

- I learnt how RSA works and that it is multiplicative

## Notes:

- This was the HARDEST one yet
- The key to solve it was figuring out RSA was multiplicative
- There were other catches to like messing between hex, decimal and ascii values of outputs and inputs

## Resources:

- CTF101 page on [RSA](https://ctf101.org/cryptography/what-is-rsa/)
- [Hex to ASCII](https://www.rapidtables.com/convert/number/hex-to-ascii.html) and visa-verca

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
