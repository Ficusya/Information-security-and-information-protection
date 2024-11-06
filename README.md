## Программная реализация алгоритма двойной перестановки.

Для этого открытый текст записывается в матрицу по определенному ключу k1, определяющему порядок записи открытого текста в строки матрицы при шифровании. Шифртекст образуется при считывании из этой матрицы по ключу k2, определяющему, в каком порядке записывается информация из столбцов матрицы. 

Программа осуществляет шифрование и расшифровку по приведенному выше алгоритму, вывод на экран незашифрованного, зашифрованного и расшифрованного сообщения.
Открытый текст: ШИФРОВАНИЕ_ПЕРЕСТАНОВКОЙ.
Ключи: [3, 1, 4, 2, 5]; [2, 1, 3, 5, 4].
```
def print_matrix(matrix):    
    for row in matrix:
        print(' '.join(row))

def encode(text, k1, k2):
    # Определение размеров матрицы    
    rows = len(k1)
    cols = len(k2)
    # Заполнение матрицы по ключу k1    
    matrix = [['_' for _ in range(cols)] for _ in range(rows)]
    text_index = 0    
    for i in k1:
        for j in range(cols):            
            if text_index < len(text):
                matrix[i - 1][j] = text[text_index]                
                text_index += 1
    # Печать матрицы перед шифрованием
    print("Полученная матрица:")    
    print_matrix(matrix)
    # Шифрование по ключу k2
    ciphertext = ''    
    for j in k2:
        for i in range(rows):            
            ciphertext += matrix[i][j - 1]
    return ciphertext

def decode(ciphertext, k1, k2):    
    # Определение размеров матрицы
    rows = len(k1)    
    cols = len(k2)
    # Заполнение матрицы по ключу k2
    matrix = [['_' for _ in range(cols)] for _ in range(rows)]    
    text_index = 0
    for j in k2:        
        for i in range(rows):
            if text_index < len(ciphertext):                
                matrix[i][j - 1] = ciphertext[text_index]
                text_index += 1
    # Расшифровка по ключу k1    
    plaintext = ''
    for i in k1:        
        for j in range(cols):
            plaintext += matrix[i - 1][j]
    return plaintext

# Ключи
k1 = [3, 1, 4, 2, 5]
k2 = [2, 1, 3, 5, 4]
# Открытый текст
plaintext = 'ШИФРОВАНИЕ_ПЕРЕСТАНОВКОЙ'
# Шифрование
ciphertext = encode(plaintext, k1, k2)
print("Открытый текст:", plaintext)
print("Зашифрованный текст:", ciphertext)
# Расшифровка
decrypted_text = decode(ciphertext, k1, k2)
print("Расшифрованный текст:", decrypted_text)
```
