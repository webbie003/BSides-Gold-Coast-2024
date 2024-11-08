# BSides-Gold-Coast-2024
CTF Write-up for Skipper, This is one way of completing the challenge I found by Googling another way but I liked mine :)

## Overview
**Description:**
The given binary will give you the password... if you meet its criteria!

**Binary:**
https://github.com/webbie003/BSides-Gold-Coast-2024/blob/main/skipper-32

**Type:**
Reverse Engineering

## TL; DR
First thought was maybe there was a help section or arguments to pass!
I quickly bailed on those ideas and run it through Strings, STrace & LTrace looking for clues. The clue found gave the impression it was time to fire up R2 which is where I was able to patch the file and gain the flag.

## Analysis
**Strings** was pretty clean but did uncover the following, which at the time was very ambiguous to me and I didn’t notice the three answers where in front of me.

![alt text](https://github.com/webbie003/BSides-Gold-Coast-2024/blob/main/images/strings.png)

**STrace** gave up some clues, where the file when executing was reading the name of my VM “BRETT” and writing output that the computer name was wrong, so there must be some logic being performed.

![alt text](https://github.com/webbie003/BSides-Gold-Coast-2024/blob/main/images/strace.png)

**LTrace** also gave up some information and confirmed there is a ‘strcmp’ hence confirming the suspicion that the program is performing logic functions.

![alt text](https://github.com/webbie003/BSides-Gold-Coast-2024/blob/main/images/ltrace.png)

The next bit was a steep learning curve using Radare2 to examine the file. Specifically focusing on the ‘main’ function where I found that there are actually three separate logic tests being performed that were daisy-chained. Each test jumping to a new address if true before until the **fcn.08048a63** function is called that looks to perform some complex mathematic operation that went way over my head but would if executed produce the flag. 

## Solution
Opened the **skipper-32** file as read/write under Radare2, adjusted the jump conditions under each of the three identified logic tests from JE (jump if true, known as 0x74) to JNE (jump if not true, known as 0x75) and execute the file again giving me the flag..!

**The Result**

SPOILER WARNING...!
<p>
  <details>
    <summary>Reveal Here</summary>

  ![img](https://github.com/webbie003/BSides-Gold-Coast-2024/blob/main/images/Result.png)
  </details>
</p>
