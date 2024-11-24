# CipherCracker 
 Information Security Management

---

## Storyline:
After finishing the first challenge without any great effort you feel like a crypto boss. Although Johnny can’t encrypt, you can.

You’re about to deregister for this course since there is nothing left to teach you, when you suddenly receive two encrypted text messages. Without hesitation you try to decrypt them with your PGP key, but the program throws an error message.

You stare at the scrambled letters on your screen, but it doesn’t make any sense to you. But you’re not just going to give up. You’re sure the codes can be cracked and you don’t mind getting your hands dirty.

You make yourself a strong cup of coffee and get ready for some cryptanalysis. You’re the next Alan Turing and there is no Enigma you couldn’t crack.

---

## Detailed Description:
Your task in this challenge is to crack the encryption keys that were used to encode the given ciphertexts: + stage_0 was encrypted by a substituion cipher. + stage_1 was encrypted by a one-time-pad algorithm (XOR).

---

## Questions for ciphercracker.md:
- Describe in detail how you decrypt the message.
- stage_0: Why was it possible to decrypt this code? Is there anything that can be done to prevent cracking of simple substituion ciphers?
- stage_1: Why were you able to crack the “unbreakable” one-time-pad? Which errors were made during the encryption process?
Points

---

## Hints for solving the challenge:
The plaintexts that were used to create the encrypted messages in both stages are in English language.
The secret messages were ciphered by assymetric encryption algorithms.
Substitution ciphers can be easily attacked by frequency analysis.
One-time-pads are only uncrackable when used the in the right way (i.e., long enough key, no key reusage).
Don’t be intimidated by the length of the texts! You know, the longer the text, the easier it is to decrypt.