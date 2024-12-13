## Программная реализация алгоритма двойной перестановки.

Для этого открытый текст записывается в матрицу по определенному ключу k1, определяющему порядок записи открытого текста в строки матрицы при шифровании. Шифртекст образуется при считывании из этой матрицы по ключу k2, определяющему, в каком порядке записывается информация из столбцов матрицы. 

Программа осуществляет шифрование и расшифровку по приведенному выше алгоритму, вывод на экран незашифрованного, зашифрованного и расшифрованного сообщения.
Открытый текст: ШИФРОВАНИЕ_ПЕРЕСТАНОВКОЙ.
Ключи: [3, 1, 4, 2, 5]; [2, 1, 3, 5, 4].

<details close>
  <summary>Код</summary>
  
```python
    
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
</details>

## Программная реализация алгоритма замены Виженера.

Алгоритм шифрования Виженера использует ключ для циклического смещения букв сообщения на количество позиций, соответствующее символам ключа. Для расшифровки используется обратное смещение.

<details close>
  <summary>Код</summary>
  
```python
# Функция для шифрования сообщения с использованием шифра Виженера
def vigenere_encrypt(plaintext, key):
    encrypted_text = ""  # Инициализация строки для зашифрованного текста
    key_length = len(key)  # Длина ключа
    for i in range(len(plaintext)):
        # Получаем числовые значения текущего символа сообщения и ключа (A=0, ..., Z=25)
        p_char = ord(plaintext[i]) - ord('A')
        k_char = ord(key[i % key_length]) - ord('A')
        
        # Применяем формулу шифрования Виженера: (p + k) % 26
        encrypted_char = (p_char + k_char) % 26 + ord('A')
        
        # Преобразуем результат обратно в символ и добавляем в зашифрованный текст
        encrypted_text += chr(encrypted_char)
    
    return encrypted_text  # Возвращаем зашифрованный текст

# Функция для расшифровки сообщения с использованием шифра Виженера
def vigenere_decrypt(ciphertext, key):
    decrypted_text = ""  # Инициализация строки для расшифрованного текста
    key_length = len(key)  # Длина ключа
    for i in range(len(ciphertext)):
        # Получаем числовые значения текущего символа зашифрованного текста и ключа (A=0, ..., Z=25)
        c_char = ord(ciphertext[i]) - ord('A')
        k_char = ord(key[i % key_length]) - ord('A')
        
        # Применяем формулу расшифровки Виженера: (c - k + 26) % 26
        decrypted_char = (c_char - k_char + 26) % 26 + ord('A')
        
        # Преобразуем результат обратно в символ и добавляем в расшифрованный текст
        decrypted_text += chr(decrypted_char)
    
    return decrypted_text  # Возвращаем расшифрованный текст

# Исходные данные
plaintext = "SHIFROVANIE"
key = "CHAIR"

print(f"Открытый текст: {plaintext}")
print(f"Ключ: {key}")

# Шифрование
encrypted_text = vigenere_encrypt(plaintext, key)
print(f"Зашифрованное сообщение: {encrypted_text}")

# Расшифровка
decrypted_text = vigenere_decrypt(encrypted_text, key)
print(f"Расшифрованное сообщение: {decrypted_text}")
```
</details>

## Программная реализация алгоритма RSA (алгоритм асимметричного шифрования с открытым ключом).

Для шифрования используются открытый ключ (e, n), а для расшифровки — закрытый ключ (d, n).

Программа должна вычислять значения 
n=p×q, 
φ(n)=(p−1)×(q−1), 
открытый ключ (e), закрытый ключ (d).

По условию варианта p = 7, q = 11, открытое сообщение: CHAIR, однако при расшифровке с такими данными получаем "CHAI", похоже, что зашифрованный ASCII-код превышает диапазон символов, которые можно корректно расшифровать на этапе декодирования (в функции chr()), т.е. значение символа R после шифрования и последующего возведения в степень становится больше n. При вводных p = 17, q = 11 программа отработала без нареканий.

<details close>
  <summary>Код</summary>
  
```python
# Функция для вычисления наибольшего общего делителя (НОД)
def gcd(a, b):
    while b != 0:
        a, b = b, a % b  # Используем алгоритм Евклида для нахождения НОД
    return a

# Функция для нахождения мультипликативной обратной (d) для e по модулю φ(n)
# Это используется для нахождения закрытого ключа
def modinv(a, m):
    m0, x0, x1 = m, 0, 1
    if m == 1:
        return 0  # Обратного элемента не существует, если модуль равен 1
    while a > 1:
        q = a // m
        m, a = a % m, m  # Обновляем a и m по формуле расширенного алгоритма Евклида
        x0, x1 = x1 - q * x0, x0  # Переменные для вычисления обратного
    if x1 < 0:
        x1 += m0  # Делаем результат положительным
    return x1

# Генерация ключей RSA на основе простых чисел p и q
def rsa_keygen(p, q):
    # Шаг 1: Вычисляем n = p * q
    n = p * q
    # Шаг 2: Вычисляем функцию Эйлера φ(n) = (p-1)*(q-1)
    phi_n = (p - 1) * (q - 1)

    # Шаг 3: Выбираем e, чтобы 1 < e < φ(n) и gcd(e, φ(n)) = 1 (взаимно простое с φ(n))
    e = 2
    while gcd(e, phi_n) != 1:
        e += 1

    # Шаг 4: Вычисляем d, используя обратное значение e по модулю φ(n)
    d = modinv(e, phi_n)

    # Функция возвращает n, φ(n), e (открытый ключ) и d (закрытый ключ)
    return n, phi_n, e, d

# Функция для шифрования сообщения RSA с использованием открытого ключа (e, n)
def encrypt_rsa(plaintext, e, n):
    # Преобразуем каждый символ сообщения в его ASCII-код
    plaintext_integers = [ord(char) for char in plaintext]
    
    # Применяем формулу c = (m^e) % n для каждого символа (ASCII-кода)
    encrypted = [(char ** e) % n for char in plaintext_integers]
    
    return encrypted

# Функция для расшифровки зашифрованного текста с использованием закрытого ключа (d, n)
def decrypt_rsa(ciphertext, d, n):
    # Применяем формулу m = (c^d) % n для каждого символа (ASCII-кода)
    decrypted = [(char ** d) % n for char in ciphertext]
    
    # Преобразуем числа обратно в символы
    decrypted_message = ''.join([chr(char) for char in decrypted])
    
    return decrypted_message

# Начальные значения
p = 17
q = 11
plaintext = "CHAIR"

# Генерация открытого и закрытого ключей RSA
n, phi_n, e, d = rsa_keygen(p, q)

# Вывод промежуточных значений и ключей
print(f"Значения p: {p}, q: {q}")
print(f"Число n (произведение p и q): {n}")
print(f"Число φ(n): {phi_n}")
print(f"Открытый ключ (e, n): ({e}, {n})")
print(f"Закрытый ключ (d, n): ({d}, {n})")

# Шифрование сообщения
encrypted_message = encrypt_rsa(plaintext, e, n)
print(f"Открытое сообщение: {plaintext}")
print(f"Зашифрованное сообщение (в виде списка чисел): {encrypted_message}")

# Расшифровка сообщения
decrypted_message = decrypt_rsa(encrypted_message, d, n)
print(f"Расшифрованное сообщение: {decrypted_message}")
```
</details>

## Лабораторная работа №6

Реализация на Java программного модуля, создающего матрицу доступа пользователей к объектам доступа и осуществляющего передачу прав доступа. 

<details close>
  <summary>Код</summary>
  
```java
package org.example;

import java.util.HashMap;
import java.util.Map;
import java.util.Scanner;
import java.util.ArrayList;
import java.util.List;
import java.util.Random;

// Класс для представления пользователя
class User {
    private String name;
    private boolean isAdmin;

    public User(String name, boolean isAdmin) {
        this.name = name;
        this.isAdmin = isAdmin;
    }

    public String getName() {
        return name;
    }

    public boolean isAdmin() {
        return isAdmin;
    }

    @Override
    public boolean equals(Object obj) {
        if (this == obj) return true;
        if (obj == null || getClass() != obj.getClass()) return false;
        User user = (User) obj;
        return name.equals(user.name);
    }

    @Override
    public int hashCode() {
        return name.hashCode();
    }
}

// Класс для представления объекта доступа
class AccessObject {
    private String name;

    public AccessObject(String name) {
        this.name = name;
    }

    public String getName() {
        return name;
    }
}

// Класс для представления прав доступа
class Permission {
    private boolean read;
    private boolean write;
    private boolean transfer;

    public Permission(boolean read, boolean write, boolean transfer) {
        this.read = read;
        this.write = write;
        this.transfer = transfer;
    }

    public boolean canRead() {
        return read;
    }

    public boolean canWrite() {
        return write;
    }

    public boolean canTransfer() {
        return transfer;
    }

    @Override
    public String toString() {
        return "Чтение: " + read + ", Запись: " + write + ", Передача прав: " + transfer;
    }
}

// Класс для представления матрицы доступа
class AccessMatrix {
    private Map<User, Map<AccessObject, Permission>> matrix;
    private List<AccessObject> objects;
    private Random random;

    public AccessMatrix() {
        this.matrix = new HashMap<>();
        this.objects = new ArrayList<>();
        this.random = new Random();
    }

    public void addUser(User user) {
        matrix.put(user, new HashMap<>());
    }

    public void addObject(AccessObject object) {
        objects.add(object);
        for (Map<AccessObject, Permission> userPermissions : matrix.values()) {
            userPermissions.put(object, new Permission(false, false, false));
        }
    }

    public void setPermission(User user, AccessObject object, Permission permission) {
        matrix.get(user).put(object, permission);
    }

    public Permission getPermission(User user, AccessObject object) {
        return matrix.get(user).get(object);
    }

    public void displayPermissions(User user) {
        System.out.println("Права доступа для пользователя: " + user.getName());
        int i = 1;
        for (AccessObject object : objects) {
            System.out.println("Объект" + i + ": " + matrix.get(user).get(object));
            i++;
        }
    }

    public void transferPermission(User fromUser, User toUser, AccessObject object) {
        Permission fromPermission = matrix.get(fromUser).get(object);
        matrix.get(toUser).put(object, fromPermission);
    }

    public boolean hasUser(User user) {
        return matrix.containsKey(user);
    }

    public AccessObject getObjectByIndex(int index) {
        return objects.get(index - 1);
    }

    public void initializeAccessMatrix() {
        for (User user : matrix.keySet()) {
            for (AccessObject object : objects) {
                if (user.isAdmin()) {
                    setPermission(user, object, new Permission(true, true, true));
                } else {
                    setPermission(user, object, new Permission(random.nextBoolean(), random.nextBoolean(), random.nextBoolean()));
                }
            }
        }
    }
}

// Интерфейс пользователя
class UserInterface {
    private Scanner scanner;
    private AccessMatrix accessMatrix;
    private User currentUser;
    private Map<String, User> users;

    public UserInterface(AccessMatrix accessMatrix, Map<String, User> users) {
        this.scanner = new Scanner(System.in);
        this.accessMatrix = accessMatrix;
        this.users = users;
    }

    public void start() {
        while (true) {
            System.out.print("Введите имя пользователя для входа: ");
            String username = scanner.nextLine();
            currentUser = users.get(username);
            if (currentUser != null && accessMatrix.hasUser(currentUser)) {
                System.out.println("Идентификация прошла успешно, добро пожаловать в систему, " + currentUser.getName());
                System.out.println("Перечень прав доступа для пользователя: " + currentUser.getName());
                accessMatrix.displayPermissions(currentUser);

                while (true) {
                    System.out.print("Жду ваших указаний > ");
                    String command = scanner.nextLine();

                    if (command.equalsIgnoreCase("read")) {
                        System.out.print("Над каким объектом производится операция? ");
                        int objectIndex = scanner.nextInt();
                        scanner.nextLine(); // Consume newline
                        AccessObject object = accessMatrix.getObjectByIndex(objectIndex);
                        if (accessMatrix.getPermission(currentUser, object).canRead()) {
                            System.out.println("Операция прошла успешно.");
                        } else {
                            System.out.println("Отказ в выполнении операции. У вас нет прав для её осуществления.");
                        }
                    } else if (command.equalsIgnoreCase("write")) {
                        System.out.print("Над каким объектом производится операция? ");
                        int objectIndex = scanner.nextInt();
                        scanner.nextLine(); // Consume newline
                        AccessObject object = accessMatrix.getObjectByIndex(objectIndex);
                        if (accessMatrix.getPermission(currentUser, object).canWrite()) {
                            System.out.println("Операция прошла успешно.");
                        } else {
                            System.out.println("Отказ в выполнении операции. У вас нет прав для её осуществления.");
                        }
                    } else if (command.equalsIgnoreCase("grant")) {
                        System.out.print("Кому передать права? ");
                        String toUsername = scanner.nextLine();
                        User toUser = users.get(toUsername);
                        if (toUser == null || !accessMatrix.hasUser(toUser)) {
                            System.out.println("Пользователь не найден.");
                            continue;
                        }
                        System.out.print("Над каким объектом производится операция? ");
                        int objectIndex = scanner.nextInt();
                        scanner.nextLine(); // Consume newline
                        AccessObject object = accessMatrix.getObjectByIndex(objectIndex);
                        if (accessMatrix.getPermission(currentUser, object).canTransfer()) {
                            accessMatrix.transferPermission(currentUser, toUser, object);
                            System.out.println("Права успешно переданы пользователю " + toUsername + " на объект " + object.getName());
                        } else {
                            System.out.println("Отказ в выполнении операции. У вас нет прав для её осуществления.");
                        }
                    } else if (command.equalsIgnoreCase("quit")) {
                        System.out.println("Работа пользователя " + currentUser.getName() + " завершена.");
                        break;
                    } else {
                        System.out.println("Неизвестная команда. Попробуйте снова.");
                    }
                }
            } else {
                System.out.println("Пользователь не найден. Попробуйте снова.");
            }
        }
    }
}

public class Main {
    public static void main(String[] args) {
        AccessMatrix accessMatrix = new AccessMatrix();
        Map<String, User> users = new HashMap<>();

        // Добавление пользователей
        users.put("Администратор", new User("Администратор", true));
        users.put("Гость", new User("Гость", false));
        users.put("Пользователь_1", new User("Пользователь_1", false));

        // Добавление объектов доступа
        AccessObject object1 = new AccessObject("Файл_1");
        AccessObject object2 = new AccessObject("Файл_2");
        AccessObject object3 = new AccessObject("CD-RW");
        AccessObject object4 = new AccessObject("Дисковод");

        // Добавление пользователей и объектов в матрицу доступа
        for (User user : users.values()) {
            accessMatrix.addUser(user);
        }
        accessMatrix.addObject(object1);
        accessMatrix.addObject(object2);
        accessMatrix.addObject(object3);
        accessMatrix.addObject(object4);

        // Инициализация матрицы доступа
        accessMatrix.initializeAccessMatrix();

        // Запуск интерфейса пользователя
        UserInterface ui = new UserInterface(accessMatrix, users);
        ui.start();
    }
}

```
<details>
