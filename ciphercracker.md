# **Ciphercracker.md**

## **Stage 0: Decrypting the Substitution Cipher**

### **Description of the Process**
The encrypted message in Stage 0 was encoded using a substitution cipher. This type of cipher replaces each letter of the plaintext with a unique letter or symbol. To decrypt it, I utilized **frequency analysis**, a method that leverages the statistical properties of the English language. Here's a step-by-step description of how I decrypted the ciphertext:

---

### **Steps to Decrypt:**

1. **Analyze Letter Frequencies:**
   - Count the occurrences of each character in the ciphertext.
   - Compare the frequency distribution of characters in the ciphertext to standard English letter frequency distributions (e.g., 'E' is the most common letter, followed by 'T', 'A', etc.).

   Python snippet for frequency analysis:
   ```python
   from collections import Counter

   def frequency_analysis(text):
       filtered_text = ''.join(filter(str.isalpha, text))
       frequencies = Counter(filtered_text)
       sorted_frequencies = sorted(frequencies.items(), key=lambda x: x[1], reverse=True)
       return sorted_frequencies
   ```

2. **Initial Mapping of Characters:**
   - Using the frequency analysis, I mapped the most frequent characters in the ciphertext to their likely plaintext counterparts based on English frequency data.
   - In my case, the most frequent character in the ciphertext was mapped to 'E'.

3. **Iterative Refinement:**
   - Applied the initial mapping to decrypt the ciphertext.
   - Identified partially or fully correct words, such as "HIGH," "MISSION," and "I'M."
   - Adjusted the mapping iteratively to make more words intelligible. For example:
     - Corrected 'U' -> 'M' to decode "I'U" as "I'M."
     - Fixed 'F' -> 'I' to improve "CESAR."
   - This step required context-based reasoning to improve the decryption progressively.

   Python snippet to apply and refine the mapping:
   ```python
   def decrypt_with_mapping(text, mapping):
       decrypted_text = []
       for char in text:
           if char.isalpha():
               if char in mapping:
                   decrypted_text.append(mapping[char.lower()].upper() if char.isupper() else mapping[char])
               else:
                   decrypted_text.append(char)
           else:
               decrypted_text.append(char)
       return ''.join(decrypted_text)

   initial_mapping = {'i': 'E', 'b': 'T', 'x': 'A', 'z': 'O', ...}  
   refined_text = decrypt_with_mapping(ciphertext, initial_mapping)
   ```

4. **Finalize the Decryption:**
   - After multiple iterations, the decrypted message became coherent and matched the expected plaintext structure.
   - The final output revealed the plaintext and confirmed the success of the decryption process.

---

5. **Found Flag:**

   FLG_AOWVGYZFABCAQQNIL3PGXYQ=

### **Why Was It Possible to Decrypt This Code?**

Substitution ciphers are vulnerable to frequency analysis because:
- Each letter in the plaintext maps to a unique ciphertext letter, preserving the overall frequency distribution.
- Common patterns in English, such as single letters ("A," "I") or common digraphs ("TH," "HE"), help identify mappings quickly.
- The ciphertext was long, providing a large dataset for frequency analysis. Longer texts make it easier to identify patterns and refine mappings.

---

### **How to Prevent Cracking of Substitution Ciphers?**

To protect against attacks like frequency analysis:
1. **Use Polyalphabetic Ciphers:**
   - Vary the substitution rules (e.g., the Vigenère cipher) to ensure letters in the plaintext map to different ciphertext letters depending on their position.
2. **Apply Modern Encryption:**
   - Employ robust encryption algorithms such as AES or RSA that resist frequency-based attacks.
3. **Minimize Redundancy:**
   - Avoid predictable language patterns by obfuscating the plaintext.

---


## **Stage 1: Decrypting Messages Using a One-Time Pad***

## 1. How I Decrypted the Message

### Step-by-Step Description process
The process of decrypting messages encrypted using a one-time pad involves reversing the XOR operation used during encryption. Below are the detailed steps:

1. **Understanding the Input**:
   - Each ciphertext is given as a hexadecimal string.
   - A key (also in hexadecimal format) is provided, which is used for XOR operations.

2. **Preparation**:
   - Convert both the ciphertext and key from hexadecimal to byte arrays using `bytes.fromhex()`. This ensures that the data is in a form suitable for XOR operations.

3. **Perform XOR Operation**:
   - For decryption, XOR the ciphertext byte by byte with the corresponding key bytes. Since XOR is its own inverse, this operation recovers the original plaintext if the key matches the one used for encryption.

4. **Convert Decrypted Bytes to Text**:
   - The resulting XOR output is converted to a string using UTF-8 decoding. If the output does not represent valid UTF-8 text, it is displayed as a hexadecimal string to ensure data integrity.

5. **Automated Decryption of Multiple Ciphertexts**:
   - A function (`decrypt_all`) iterates through all ciphertexts and applies the decryption method up to the length of the key, ensuring alignment between the key and ciphertext.

6. **Output the Results**:
   - The decrypted messages are displayed, showing either plaintext or hexadecimal output.

### Relevant Source Code Snippets
#### **Performing XOR and Hexadecimal Conversion**
```python
# Convert hex strings to byte arrays
data_bytes = bytes.fromhex(hex_string)
key_bytes = bytes.fromhex(key_hex)

# Perform XOR operation
xored_bytes = bytes(a ^ b for a, b in zip(data_bytes, key_bytes))

# Convert the result to hexadecimal
xored_hex = xored_bytes.hex()
print(f"Result after XOR with '{key_hex}': {xored_hex}")
```

#### **Decrypting a Single Ciphertext**
```python
def one_time_pad_decrypt(ciphertext_hex, key_hex):
    """
    Decrypt a ciphertext using a one-time pad.

    :param ciphertext_hex: Hexadecimal representation of the ciphertext
    :param key_hex: Hexadecimal representation of the key
    :return: Decrypted plaintext
    """
    # Convert hex to integers for XOR
    ciphertext_int = int(ciphertext_hex, 16)
    key_int = int(key_hex, 16)
    
    # XOR the ciphertext with the key
    decrypted_int = ciphertext_int ^ key_int
    
    # Convert the result back to bytes
    decrypted_bytes = decrypted_int.to_bytes((decrypted_int.bit_length() + 7) // 8, byteorder='big')
    
    # Decode bytes to a string
    try:
        plaintext = decrypted_bytes.decode("utf-8")
    except UnicodeDecodeError:
        plaintext = decrypted_bytes.hex()  # If it's not valid UTF-8, return the hex
    
    return plaintext
```

#### **Decrypting Multiple Ciphertexts**
```python
def decrypt_all(ciphertexts, key):
    """
    Decrypt all ciphertexts in the list using a one-time pad.

    :param ciphertexts: List of hexadecimal ciphertexts
    :param key: Hexadecimal key
    :return: List of decrypted plaintexts
    """
    # Ensure the key length
    key_len = len(key)
    
    # Decrypt each ciphertext up to the length of the key
    decrypted_messages = [
        one_time_pad_decrypt(ciphertext[:key_len], key)
        for ciphertext in ciphertexts
    ]
    
    return decrypted_messages
```

---

## 2. Cracking the "Unbreakable" One-Time Pad

### The Vulnerability in the Process
The one-time pad encryption is theoretically unbreakable **only if the following conditions are met**:
1. The key is truly random.
2. The key is as long as the plaintext.
3. The key is used only once and kept secret.

In this scenario, the encryption process failed to satisfy one or more of these criteria:

1. **Reusing the Key**:
   - A fundamental flaw in the encryption process was the reuse of the key across multiple ciphertexts. When a one-time pad is reused, the XOR of two ciphertexts eliminates the key, leaving behind a relationship between the two plaintexts, making decryption possible.

2. **Key Shorter Than Ciphertext**:
   - The provided key was shorter than some ciphertexts. During encryption, this likely caused the key to wrap around or be padded in some predictable manner, introducing patterns exploitable during decryption.

3. **Potential Non-Random Key**:
   - If the key is not truly random and instead generated using a predictable process, it can be guessed or derived through cryptanalysis.

---

### Exploitation of Errors
The decryption succeeded because:
- Reusing the key allowed partial or complete recovery of the plaintext, leveraging the predictability of XOR operations.
- The predictable wrapping or padding of the key enabled aligning parts of the ciphertext with known plaintext patterns (e.g., common words or phrases).
- Analyzing the structure of the ciphertext provided clues about the plaintext, especially if the plaintext had repetitive or recognizable elements.


