# EX-NO-7-Implement-DES-Encryption
# Name:Mahasri D
# Reg no:212224220058
## Aim:

To use the Data Encryption Standard (DES) algorithm for a practical application, such as securing sensitive data transmission in financial transactions.

## ALGORITHM:

1. DES is based on a symmetric key encryption technique that encrypts data in 64-bit blocks.
2. DES uses a Feistel network structure with 16 rounds of processing for encryption.
3. DES has a 64-bit key, but only 56 bits are used for encryption (the remaining 8 bits are for parity).
4. DES applies initial and final permutations along with 16 rounds of substitution and permutation transformations to produce ciphertext.

## Program:
```
#include <stdio.h>
#include <stdint.h>

uint32_t feistel(uint32_t right, uint32_t key)
{
    return right ^ key;
}

uint64_t encrypt(uint64_t plaintext, uint64_t key)
{
    uint32_t left, right;
    uint32_t roundKey;
    uint64_t result;

    left = (uint32_t)(plaintext >> 32);
    right = (uint32_t)plaintext;

    /* 16 Feistel rounds */
    for (int i = 0; i < 16; i++)
    {
        roundKey = (uint32_t)(key >> (i % 32));

        uint32_t newRight = left ^ feistel(right, roundKey);

        left = right;
        right = newRight;
    }

    /* Swap after the final round */
    result = ((uint64_t)right << 32) | left;

    return result;
}

uint64_t decrypt(uint64_t ciphertext, uint64_t key)
{
    uint32_t left, right;
    uint32_t roundKey;
    uint64_t result;

    left = (uint32_t)(ciphertext >> 32);
    right = (uint32_t)ciphertext;

    /* 16 rounds in reverse order */
    for (int i = 15; i >= 0; i--)
    {
        roundKey = (uint32_t)(key >> (i % 32));

        uint32_t newLeft = right ^ feistel(left, roundKey);

        right = left;
        left = newLeft;
    }

    result = ((uint64_t)left << 32) | right;

    return result;
}

int main()
{
    char text[9];
    char keyText[9];

    uint64_t plaintext = 0;
    uint64_t key = 0;
    uint64_t ciphertext;
    uint64_t decrypted;

    printf("Enter 8-character plaintext: ");
    scanf("%8s", text);

    printf("Enter 8-character key: ");
    scanf("%8s", keyText);

    /* Convert plaintext into 64-bit value */
    for (int i = 0; i < 8; i++)
    {
        plaintext = (plaintext << 8) |
                    (unsigned char)text[i];
    }

    /* Convert key into 64-bit value */
    for (int i = 0; i < 8; i++)
    {
        key = (key << 8) |
              (unsigned char)keyText[i];
    }

    /* Encryption */
    ciphertext = encrypt(plaintext, key);

    printf("\nPlaintext  : %s", text);
    printf("\nKey        : %s", keyText);

    printf("\nCiphertext : %016llX",
           (unsigned long long)ciphertext);

    /* Decryption */
    decrypted = decrypt(ciphertext, key);

    printf("\nDecrypted  : ");

    for (int i = 7; i >= 0; i--)
    {
        printf("%c",
               (char)(decrypted >> (i * 8)));
    }

    printf("\n");

    return 0;
}
```

## Output:

<img width="1395" height="862" alt="image" src="https://github.com/user-attachments/assets/4813c5d9-d114-4a3e-87ed-c8dfc4de8ce6" />


## Result:
  The program is executed successfully.
