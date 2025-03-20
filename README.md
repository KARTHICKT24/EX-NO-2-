## EX. NO:2 IMPLEMENTATION OF PLAYFAIR CIPHER

## NAME : KARTHICK KISHORE T
## REG NO : 212223220042
## DATE : 20-03-2025

## AIM:
 

 

To write a Python program to implement the Playfair Substitution technique.

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




## Program:
```
def generate_playfair_matrix(key):
    key = "".join(dict.fromkeys(key.upper().replace("J", "I"))) 
    alphabet = "ABCDEFGHIKLMNOPQRSTUVWXYZ"
    matrix = "".join([char for char in key if char in alphabet] + [char for char in alphabet if char not in key])
    return [list(matrix[i:i+5]) for i in range(0, 25, 5)]

def find_position(matrix, letter):
    for i, row in enumerate(matrix):
        if letter in row:
            return i, row.index(letter)

def playfair_cipher(text, key, encrypt=True):
    matrix = generate_playfair_matrix(key)
    text = text.upper().replace("J", "I").replace(" ", "")
    pairs = []
    i = 0
    while i < len(text):
        a = text[i]
        b = text[i + 1] if i + 1 < len(text) and text[i] != text[i + 1] else "X"
        pairs.append((a, b))
        i += 2 if b != "X" else 1
    
    result = ""
    for a, b in pairs:
        row1, col1 = find_position(matrix, a)
        row2, col2 = find_position(matrix, b)
        if row1 == row2:  # Same row
            col1 = (col1 + (1 if encrypt else -1)) % 5
            col2 = (col2 + (1 if encrypt else -1)) % 5
        elif col1 == col2:  # Same column
            row1 = (row1 + (1 if encrypt else -1)) % 5
            row2 = (row2 + (1 if encrypt else -1)) % 5
        else:  # Rectangle swap
            col1, col2 = col2, col1
        result += matrix[row1][col1] + matrix[row2][col2]
    
    return result


text = input("Enter text: ")
key = input("Enter key: ")
mode = input("Encrypt or Decrypt? (e/d): ").strip().lower()
print("Result:", playfair_cipher(text, key, encrypt=(mode == 'e')))
```



## Output:

# ENCRYPTION
![encrypt 2 py](https://github.com/user-attachments/assets/5e7800c5-9742-4f18-a6ce-bfc736a4a9cb)

# DECRYPTION
![decrypt 2](https://github.com/user-attachments/assets/291f616b-d0e0-4b75-8e48-fa176cadaed9)
