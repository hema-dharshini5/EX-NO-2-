## EX. NO:2 IMPLEMENTATION OF PLAYFAIR CIPHER

 ```
Name:Hema Dharshini N
Reg no:212223220034
```
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


```
import itertools

def generate_playfair_matrix(key):
    key = key.upper().replace("J", "I")  # Convert key to uppercase and replace J with I
    key = "".join(dict.fromkeys(key))  # Remove duplicate letters
    alphabet = "ABCDEFGHIKLMNOPQRSTUVWXYZ"
    key += "".join(c for c in alphabet if c not in key)
    
    matrix = [list(key[i:i+5]) for i in range(0, 25, 5)]
    return matrix

def find_position(matrix, letter):
    for row, line in enumerate(matrix):
        if letter in line:
            return row, line.index(letter)
    return None

def playfair_cipher(text, key, mode='encrypt'):
    matrix = generate_playfair_matrix(key)
    text = text.upper().replace("J", "I").replace(" ", "")  # Convert text to uppercase
    pairs = []
    i = 0

    while i < len(text):
        a = text[i]
        if i + 1 < len(text) and text[i] != text[i + 1]:
            b = text[i + 1]
            i += 2
        else:
            b = 'X'  # Add 'X' if the letter is repeated or if it's a single letter at the end
            i += 1

        pairs.append((a, b))

    result = ""
    for a, b in pairs:
        pos_a = find_position(matrix, a)
        pos_b = find_position(matrix, b)

        if pos_a is None or pos_b is None:
            continue  # Skip if letter is not found (shouldn't happen)

        row_a, col_a = pos_a
        row_b, col_b = pos_b

        if row_a == row_b:  # Same row
            col_a = (col_a + 1) % 5 if mode == 'encrypt' else (col_a - 1) % 5
            col_b = (col_b + 1) % 5 if mode == 'encrypt' else (col_b - 1) % 5
        elif col_a == col_b:  # Same column
            row_a = (row_a + 1) % 5 if mode == 'encrypt' else (row_a - 1) % 5
            row_b = (row_b + 1) % 5 if mode == 'encrypt' else (row_b - 1) % 5
        else:  # Rectangle swap
            col_a, col_b = col_b, col_a

        result += matrix[row_a][col_a] + matrix[row_b][col_b]

    return result

# Get user input
message = input("Enter the message: ").strip()
key = input("Enter the key: ").strip()

encrypted_text = playfair_cipher(message, key, mode='encrypt')
decrypted_text = playfair_cipher(encrypted_text, key, mode='decrypt')

print("Encrypted:", encrypted_text)
print("Decrypted:", decrypted_text)

```


# Output:

<img width="1680" alt="Playfair" src="https://github.com/user-attachments/assets/e2011979-7419-4e4d-b7ab-fccc19eec54d" />

## Result
The program is executed successfully.
