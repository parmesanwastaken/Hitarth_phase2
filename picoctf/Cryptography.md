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
