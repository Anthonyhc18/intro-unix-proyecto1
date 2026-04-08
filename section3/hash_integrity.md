INTEGRITY COMPARISON

File Status
	

Command Executed
	

Hash value (SHA-256)

Original
	

sha256sum prueba.txt
	

788abd23ac761c0f4ec08e75b7869de54646105b63473b797dd1385a73a3431d

Modified
	

sha256sum prueba.txt
	

e6b0070fe694dd250fad32a33f49be44f2afac1fd66499099b02f2271c03404e


 

Explanation of the screenshots:

Observation of the change: Although only one character ("1") was added to the end of the file using the echo command, the resulting hash changed by more than 90% of its characters.

Avalanche effect: This demonstrates the avalanche effect, where a tiny change in the input generates a completely different and unpredictable output.

Technical conclusion: It is confirmed that hash functions are infallible tools for detecting alterations. If this file were a software installer, the user would immediately know that the file had been tampered with because the hash would not match the official one.

RESEARCH QUESTIONS

1.      What is a cryptographic hash function and its four properties?

A hash function is an algorithm that processes an input of any size and returns a unique string of fixed length. It works like a "fingerprint" of the original data. Its properties are: Preimage resistance: It is impossible to reverse the hash to obtain the original message. Second preimage resistance: If you have a message, you cannot find another different message that produces the same hash. Collision resistance: It is extremely difficult to find two different messages that share the same hash. Avalanche effect: A minimal change in the input completely alters the resulting hash.

2.      Real-world cases of malware due to lack of verification:

In 2016, attackers compromised the Linux Mint website by replacing the download ISO with a version containing a backdoor. Many users installed the system without verifying the MD5 hash, allowing hackers to remotely control their computers. Another relevant case is the distribution of modified versions of CCleaner in 2017. Although the attack occurred in the supply chain, it highlights the importance of validation. Users who downloaded the software from unofficial mirror sites without checking signatures or hashes facilitated the mass infection of devices with malware.

3.      Dictionary attack, brute force, rainbow tables, and the 'salt'

The dictionary attack tries common words and leaked passwords, while brute force attempts all possible combinations of characters, which is slow but exhaustive. Rainbow tables are pre-calculated databases that allow hashes to be translated into plain text almost instantly. The 'salt' is a unique random value that is added to the password before the hash is applied. This mitigates rainbow tables because the attacker can no longer use a general table; they would have to calculate a new table for each individual user, making the attack impractical due to the time and resources required.

4.      Entropy and Crack Resistance:

The password "12345678" has low entropy because it is predictable and short; an attacker can crack it in milliseconds. In contrast, a medium-entropy password (using symbols and uppercase letters) increases the number of possible combinations exponentially. The conclusion is that resistance is not linear. As entropy increases, the cracking time doesn't just increase by a few minutes, but scales to years. This demonstrates that length and randomness are the most effective defenses against modern high-speed processors.

5.      /etc/shadow Format in Linux and Identifiers:

The /etc/shadow file stores hashes with the structure $id$salt$hash. The $id field tells the system which algorithm to use to process the key.

 

 

 

 

The most common identifiers are:

1: MD5 (no longer recommended because it is insecure).

5: SHA-256 (secure and efficient).

6: SHA-512 (the current standard due to its high resistance).

y: Yescrypt (used in modern systems to resist attacks with specialized hardware).

 

BIBLIOGRAPHIC REFERENCES

National Institute of Standards and Technology (NIST). (2015). Secure Hash Standard (SHS) (FIPS PUB 180-4). https://nvlpubs.nist.gov/nistpubs/FIPS/NIST.FIPS.180-4.pdf

Paar, C., & Pelzl, J. (2010). Understanding Cryptography: A Textbook for Students and Practitioners. Springer. https://www.crypto-textbook.com/

Stallings, W. (2023). Cryptography and Network Security: Principles and Practice (8th ed.). Pearson. https://www.pearson.com/en-us/subject-catalog/p/cryptography-and-network-security-principles-and-practice/P200000003290/9780136764038

The Linux Information Project. (2024). Shadow Password Suite Definition. http://www.bellevuelinux.org/shadow.html

Ward, J. (2016). The Linux Mint Hack: What Happened and What to Do. LinuxSecurity. https://blog.linuxmint.com/?p=2994
