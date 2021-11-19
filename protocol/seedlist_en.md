English | [中文](seedlist_cn.md)

#### I. Principles
1. Cryptography algorithms should choose mature, time-precipitated and mathematically proven algorithms to be safe;
2. Try to deal with confusion as much as possible, and improve privacy by increasing randomness;
3. In the time that the user can tolerate, the algorithm is used to extend the encryption time to increase the attack cost of the adversary;
4. Keep it simple;

#### II. Overview

The security of the private key/mnemonic phrase is fundamentally an offensive and defensive game between the storage party, the user, and the hacker; from a defensive perspective, we should assume:

1. The storage party is completely reliable;
2. The user's encryption key is strong enough;
3. The encryption process is sufficiently secure;

How to make this assumption become reality? Before expanding the following discussion, we need to define three initial entropy values:

spaceName: The name used to mark the user key space, which needs to be provided by the user; for example, you can use an impressive verse, one of your anniversaries, the ID of one of your certificates, or even one of your most commonly used ones Website login name, etc.;

label: Used to mark the alias of each storage object, theoretically it can be any value and needs to be provided by the user; for example, ethereum-privkey, bitcoin-privkey, etc.;

password: The initial password value used by the user to generate the key, which needs to be provided by the user, but this value will not be used directly as the encryption key value; the real encryption key will be used by combining space name, label and password. Column KDF operation, and randomly add salt value to the result. This will be explained below.



#### III. The storage party is completely reliable

The storage party of the private key/mnemonic phrase here is the storage party in the general sense. It includes common centralized cloud storage, local disk storage, hardware wallet storage, paper storage, decentralized storage and even brain memory, etc.;
Similarly, we need to define reliability: as long as the user completes the storage behavior, at any subsequent time, the original content that is indistinguishable from the stored content can be obtained. Therefore: the core appeal is that it is available at any time and the content is not different;
Based on the definition of reliable private key/mnemonic storage, in contrast to the above-mentioned storage parties, there will always be more or less potential threats to reliability, which cannot reach 100%;

Considering that the storage volume of the private key/mnemonic phrase is very small (even if it is encrypted, it will remain in a few hundred bytes). At present, the Ethereum contract storage area can fully meet the above-mentioned reliability requirements;

#### IV. Encryption Strength

Generally speaking, if the encryption algorithm is reliable, the encryption strength depends on two aspects:

1. Key length: the number of common key bits entered;

   ```
   Therefore, when dealing with the key length, length verification is required to prompt the user to reach a certain length; at this point, we need users to work with us to complete this request in order to move towards the purpose of high-strength keys;
   From the perspective of the encryption process, the seedlist will add salt obfuscation to promote the final encryption key to reach a reliable encryption length, which is 98 characters temporarily;
   ```

2. The complexity of the key: that is, the input key should contain as many characters as possible, such as English capitalization, numbers, characters, etc.;

   ```
   The complexity of the key, the encryption process of the seedlist will add a number of characters based on a random algorithm by obfuscating the salt value to ensure the complexity;
   ```



#### V. Encryption process

Based on the definition of spaceName, label and password in the "Overview", we will gradually carry out the following encryption derivation process:

1. Generate the key space ID

   ```
   a. Perform string splicing on spaceName and label, and hash the spliced string;
   
   b. Take the 4 bytes before and after the hash value to obtain an Int32 integer; based on the Int32 integer, obtain a random value rand0 located in [32, 64];
    
   c. Find the hash twice for spaceName and label again to get spacename_hash2 and label_hash2;
    
   d. Rotate and splice spacename_hash2 and label_hash2 to obtain two new strings s1 and s2; and generate a public and private key pair p1 and p2 based on s1 and s2;
   
   e. Use p1 and p2 to sign s2 and s1 respectively, and assign the signature results to spacename_hash2 and label_hash2 respectively;
   
   f. Cycle through the above c~e process and sign rand0 times (generated in step b);
   
   g. After step f, we will get two strings, after concatenating the two strings, we get s3;
   
   h. Based on s3, obtain the random salt string salt with a random length of 32 from the following random strings:
      CHARS="1qaz!QAZ2w?sx@WSX.(=]3ec#EDC/)P:4rfv$RF+V5t*IK<9og}b%TGB6OL>yhn^YHN-[d'_7ujm&UJ0p;{M8ik,l|" ;
   
   i. Take s3 as the slow hash object, take h step salt as the salt value, and obtain the final hash as the key value based on the scrypt algorithm; among them, N, r, p, dkLen are set to: 32, 64, 16, 64;

   j. Based on step i, generate an Ethereum wallet address, which is the key space indicator;
   ```



2. Encrypted label

   ```
   a. Take the hash of the spaceName string;
   
   b. Take the 4 bytes before and after the hash value to obtain an Int32 integer; based on the Int32 integer, obtain a random value rand0 located in [8, 16];
    
   c. After recursively hashing spaceName rand0 times, the result obtained is hash0;
   
   d. Using hash0 as the basic entropy value, extract 32 characters from the following random strings and concatenate them into a string as the salt value:
   CHARS="1qaz!QAZ2w?sx@WSX.(=]3ec#EDC/)P:4rfv$RF+V5t*IK<9og}b%TGB6OL>yhn^YHN-[d'_7ujm&UJ0p;{M8ik,l|" ;
    
   e. Take hash0 as the slow hash object, take the salt of step d as the salt value, and obtain the final hash as the key value based on the scrypt algorithm; among them, N, r, p, dkLen are set to: 32, 64, 16, 64;
    
   f. Based on the final key of step e, perform AES encryption on label to obtain label ciphertext;
   ```



3. Encrypted plaintext

   ```
   a. Perform string splicing on spaceName, label and password, and hash the spliced string;
   
   b. Take the 4 bytes before and after the hash value to obtain an Int32 integer; based on the Int32 integer, obtain a random value rand0 located in [0, 8];
    
   c. Double-hash the spaceName, label and password to obtain h1, h2, and h3;
    
   d. Based on h1+h2, h2+h3 and h3+h1 as seed values, generate three public and private key pairs p1 p2 p3;
   
   e. Use p1 p2 p3 to sign h2+h3, h3+h1 and h1+h2 respectively, and assign the signed results to h1 h2 h3 respectively
   
   f. Repeat the above c~e process rand0 times;
   
   g. Splice the h1 h2 h3 obtained after rand0 operations, and calculate the hash value after the splicing, and record it as hash0;
   
   h. Based on hash0 and the following random string, obtain a random substring with a length of 32 as the salt of scrypt:
     CHARS="1qaz!QAZ2w?sx@WSX.(=]3ec#EDC/)P:4rfv$RF+V5t*IK<9og}b%TGB6OL>yhn^YHN-[d'_7ujm&UJ0p;{M8ik,l|" ;
   
   i. Take the 4 bytes before and after the hash0 value to get an Int32 integer; based on the Int32 integer, get a random value rand1 located in [0, 8];
   
   j. Based on hash0, take salt as the salt value, perform scrypt slow hash calculation, obtain the hash value, and re-assign it to hash0;
   
   k. Use rand1 as the step size, take the following string length modulo, insert the character at the modulus value into hash0; the calculation method of the insertion position is: calculate the hash for hash0, and repeat the algorithm process of step i to get the random value rand2;
       rand2 is the insertion position;
      CHARS="1qaz!QAZ2w?sx@WSX.(=]3ec#EDC/)P:4rfv$RF+V5t*IK<9og}b%TGB6OL>yhn^YHN-[d'_7ujm&UJ0p;{M8ik,l|" ;
   
   l. Repeat step i~k 32 times;
   
   m. Use the final string (98 characters) as the key to encrypt user data with AES;
   ```

