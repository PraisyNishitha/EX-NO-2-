## EX. NO:2 IMPLEMENTATION OF PLAYFAIR CIPHER

 
## Name : praisy Nishitha J
## Reg n0 : 212224100042
## AIM:
To write a C program to implement the Playfair Substitution technique.

## DESCRIPTION:

The Playfair cipher starts with creating a key table. The key table is a 5×5 grid of letters that will act as the key for encrypting your plaintext. Each of the 25 letters must be unique and one letter of the alphabet is omitted from the table (as there are 25 spots and 26 letters in the alphabet).

To encrypt a message, one would break the message into digrams (groups of 2 letters) such that, for example, "HelloWorld" becomes "HE LL OW OR LD", and map them out on the key table. The two letters of the diagram are considered as the opposite corners of a rectangle in the key table. Note the relative position of the corners of this rectangle. Then apply the following 4 rules, in order, to each pair of letters in the plaintext:
1.	If both letters are the same (or only one letter is left), add an "X" after the first letter
2.	If the letters appear on the same row of your table, replace them with the letters to their immediate right respectively
3.	If the letters appear on the same column of your table, replace them with the letters immediately below respectively
4.	If the letters are not on the same row or column, replace them with the letters on the same row respectively but at the other pair of corners of the rectangle defined by the original pair.
## EXAMPLE:
![image](https://github.com/Hemamanigandan/EX-NO-2-/assets/149653568/e6858d4f-b122-42ba-acdb-db18ec2e9675)

 

## ALGORITHM:

STEP-1: Read the plain text from the user.
STEP-2: Read the keyword from the user.
STEP-3: Arrange the keyword without duplicates in a 5*5 matrix in the row order and fill the remaining cells with missed out letters in alphabetical order. Note that ‘i’ and ‘j’ takes the same cell.
STEP-4: Group the plain text in pairs and match the corresponding corner letters by forming a rectangular grid.
STEP-5: Display the obtained cipher text.




Program:

```

#include <stdio.h>
#include <stdlib.h>
#include <string.h>

#define SIZE 100

void toLowerCase(char s[], int n) {
    for (int i = 0; i < n; i++)
        if (s[i] >= 'A' && s[i] <= 'Z')
            s[i] += 32;
}

int removeSpaces(char s[], int n) {
    int i, j = 0;
    for (i = 0; i < n; i++)
        if (s[i] != ' ')
            s[j++] = s[i];
    s[j] = '\0';
    return j;
}

void generateKeyTable(char key[], int ks, char t[5][5]) {
    int used[26] = {0}, i = 0, j = 0, k;
    used['j' - 'a'] = 1;

    for (k = 0; k < ks; k++) {
        int x = key[k] - 'a';
        if (x >= 0 && x < 26 && !used[x]) {
            t[i][j++] = key[k];
            used[x] = 1;
            if (j == 5) i++, j = 0;
        }
    }

    for (k = 0; k < 26; k++) {
        if (!used[k]) {
            t[i][j++] = k + 'a';
            if (j == 5) i++, j = 0;
        }
    }
}

void search(char t[5][5], char a, char b, int p[]) {
    int i, j;
    if (a == 'j') a = 'i';
    if (b == 'j') b = 'i';

    for (i = 0; i < 5; i++)
        for (j = 0; j < 5; j++) {
            if (t[i][j] == a) p[0] = i, p[1] = j;
            if (t[i][j] == b) p[2] = i, p[3] = j;
        }
}

void encrypt(char s[], char t[5][5], int n) {
    int i, p[4];

    for (i = 0; i < n; i += 2) {
        search(t, s[i], s[i + 1], p);

        if (p[0] == p[2]) {
            s[i] = t[p[0]][(p[1] + 1) % 5];
            s[i + 1] = t[p[2]][(p[3] + 1) % 5];
        } else if (p[1] == p[3]) {
            s[i] = t[(p[0] + 1) % 5][p[1]];
            s[i + 1] = t[(p[2] + 1) % 5][p[3]];
        } else {
            s[i] = t[p[0]][p[3]];
            s[i + 1] = t[p[2]][p[1]];
        }
    }
    s[n] = '\0';
}

int main() {
    char key[SIZE], s[SIZE];
    int ks, n, i;

    printf("Enter key: ");
    fgets(key, SIZE, stdin);
    key[strcspn(key, "\n")] = '\0';

    printf("Enter plaintext: ");
    fgets(s, SIZE, stdin);
    s[strcspn(s, "\n")] = '\0';

    ks = removeSpaces(key, strlen(key));
    n = removeSpaces(s, strlen(s));
    toLowerCase(key, ks);
    toLowerCase(s, n);

    for (i = 0; i < n; i++)
        if (s[i] == 'j') s[i] = 'i';

    generateKeyTable(key, ks, (char[5][5]){{0}});

    {
        char t[5][5];
        generateKeyTable(key, ks, t);

        for (i = 0; i < n - 1; ) {
            if (s[i] == s[i + 1]) {
                memmove(s + i + 2, s + i + 1, n - i);
                s[i + 1] = 'x';
                n++;
            }
            i += 2;
        }

        if (n % 2 != 0) s[n++] = 'x';
        s[n] = '\0';

        encrypt(s, t, n);
    }
}
```





## Output:
<img width="1613" height="1012" alt="image" src="https://github.com/user-attachments/assets/58397de6-5432-4d55-8cf3-bd0a5d48406b" />

## Result
The Playfair Cipher program was executed successfully. The plaintext was encrypted into ciphertext using the given key.
