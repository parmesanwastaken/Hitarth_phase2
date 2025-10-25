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

# 2. ARMssembly 1

> For what argument does this program print `win` with variables 58, 2 and 3? File: chall_1.S Flag format: picoCTF{XXXXXXXX} -> (hex, lowercase, no 0x, and 32 bits. ex. 5614267 would be picoCTF{0055aabb})

## Solution:

- Downloaded the file and used `file` on it, which made it clear it was assembly code
- Used `cat` on it
- Got the assembly code and converted it to C using online convertor:
```c
#include <stdio.h>
#include <stdlib.h>

int func(int input) {
    int a = input;
    int b = 58;
    int c = 2;
    int d = 3;

    int temp = b << c;
    temp = temp / d;
    temp = temp - a;

    return temp;
}

int main(int argc, char **argv) {
    int val = atoi(argv[1]);
    int result = func(val);

    if (result == 0) {
        puts("You win!");
    } else {
        puts("You Lose :(");
    }

    return 0;
}
```
- Analysed the code and got `a = (b*2^c)/d` cause we need `temp = 0`
- After putting given variable values, I approximately got `a = 77`
- Converting this to hexadecimal gives `0000004d`


## Flag:

```
picoCTF{0000004d}

```

## Concepts learnt:

- Learnt how to convert assembly code to C online and then analyse the C code

## Notes:

- Got confused by `atoi()` at first then learnt about it online 

## Resources:

- [Online assembly to C converter](https://www.codeconvert.ai/assembly-to-c-converter)
- [Hexadecimal Convertor](https://www.rapidtables.com/convert/number/decimal-to-hex.html)


***
# 3. Vault door 3 

> This vault uses for-loops and byte arrays. The source code for this vault is here: VaultDoor3.java

## Solution:

- I downloaded the file and decided to look source code
- I opened the `VaultDoor3.java` in vscode and got:
```java
import java.util.*;

class VaultDoor3 {
    public static void main(String args[]) {
        VaultDoor3 vaultDoor = new VaultDoor3();
        Scanner scanner = new Scanner(System.in);
        System.out.print("Enter vault password: ");
        String userInput = scanner.next();
	String input = userInput.substring("picoCTF{".length(),userInput.length()-1);
	if (vaultDoor.checkPassword(input)) {
	    System.out.println("Access granted.");
	} else {
	    System.out.println("Access denied!");
        }
    }

    // Our security monitoring team has noticed some intrusions on some of the
    // less secure doors. Dr. Evil has asked me specifically to build a stronger
    // vault door to protect his Doomsday plans. I just *know* this door will
    // keep all of those nosy agents out of our business. Mwa ha!
    //
    // -Minion #2671
    public boolean checkPassword(String password) {
        if (password.length() != 32) {
            return false;
        }
        char[] buffer = new char[32];
        int i;
        for (i=0; i<8; i++) {
            buffer[i] = password.charAt(i);
        }
        for (; i<16; i++) {
            buffer[i] = password.charAt(23-i);
        }
        for (; i<32; i+=2) {
            buffer[i] = password.charAt(46-i);
        }
        for (i=31; i>=17; i-=2) {
            buffer[i] = password.charAt(i);
        }
        String s = new String(buffer);
        return s.equals("jU5t_a_sna_3lpm12g94c_u_4_m7ra41");
    }
}

```
- I analysed the loops:
- `for (i=0; i<8; i++) {
            buffer[i] = password.charAt(i);` meant it just copy pastes first 8 characters
- `for (; i<16; i++) {
            buffer[i] = password.charAt(23-i);` meant it reversed the characters (from 8-15)
- `for (; i<32; i+=2) {
            buffer[i] = password.charAt(46-i);` meant reversed characters at even intervals (from 16-30)
- `for (i=31; i>=17; i-=2) {
            buffer[i] = password.charAt(i);` meant reversed characters (from 17-31)

## Flag:

```
picoCTF{jU5t_a_s1mpl3_an4gr4m_4_u_c79a21}
```

## Concepts learnt:

- Learnt how to see source code of java file
- Leanrt how to understand loops in java and reverse the algorithm

## Notes:

- At first I was confused how exactly to reverse the loops, then searched a bit on basics of Java loops, understood them and figured it out.

## Resources:

- [Java For Loop Basics](https://www.w3schools.com/java/java_for_loop.asp)
- [VS Code](https://code.visualstudio.com/)


***
