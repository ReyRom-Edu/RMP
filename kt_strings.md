### Лекция: Работа со строками в Kotlin

#### 1. Введение
Строки (“String”) являются одним из базовых типов данных в языке Kotlin. Они широко используются для работы с текстовыми данными и предоставляют удобный интерфейс для выполнения различных операций, таких как форматирование, преобразование, разбиение и другие.

Kotlin предоставляет мощные возможности для работы со строками благодаря:
- Лаконичному синтаксису.
- Расширениям функций.
- Интеграции с Java API.

#### 2. Основные свойства строк
1. **Неизменяемость**: Объекты строк в Kotlin неизменяемы. Это означает, что после создания строка не может быть изменена.
2. **Хранение**: Строка в Kotlin представляет собой последовательность символов (Char).
3. **Объявление**:
   ```kotlin
   val str1 = "Пример строки"
   val str2 = "Еще одна строка"
   ```

#### 3. Основные операции со строками

1. **Конкатенация строк**:
   - Оператор `+`:
     ```kotlin
     val str1 = "Hello, "
     val str2 = "world!"
     val result = str1 + str2
     println(result) // Output: Hello, world!
     ```
   - Интерполяция строк (String Templates):
     ```kotlin
     val name = "Kotlin"
     val greeting = "Hello, $name!"
     println(greeting) // Output: Hello, Kotlin!
     ```
     - Вложенные выражения в интерполяции:
       ```kotlin
       val a = 5
       val b = 10
       println("Сумма: ${a + b}") // Output: Сумма: 15
       ```

2. **Доступ к символам**:
   ```kotlin
   val str = "Kotlin"
   println(str[0]) // Output: K
   println(str.last()) // Output: n
   ```

3. **Длина строки**:
   ```kotlin
   val str = "Hello"
   println(str.length) // Output: 5
   ```

4. **Сравнение строк**:
   - По значению:
     ```kotlin
     val str1 = "abc"
     val str2 = "abc"
     println(str1 == str2) // Output: true
     ```
   - Лексикографическое сравнение:
     ```kotlin
     val str1 = "abc"
     val str2 = "abd"
     println(str1 < str2) // Output: true
     ```
   - Сравнение без учета регистра:
     ```kotlin
     println("abc".equals("ABC", ignoreCase = true)) // Output: true
     ```

#### 4. Основные методы строк

1. **Изменение регистра**:
   ```kotlin
   val str = "Hello"
   println(str.uppercase()) // Output: HELLO
   println(str.lowercase()) // Output: hello
   ```

2. **Обрезка строк**:
   ```kotlin
   val str = "   Kotlin   "
   println(str.trim()) // Output: Kotlin
   println(str.trimStart()) // Обрезка слева
   println(str.trimEnd()) // Обрезка справа
   ```

3. **Поиск подстроки**:
   ```kotlin
   val str = "Kotlin is great!"
   println(str.contains("Kotlin")) // Output: true
   println(str.startsWith("Kot")) // Output: true
   println(str.endsWith("!")) // Output: true
   ```

4. **Разбиение строки**:
   ```kotlin
   val str = "apple,banana,grape"
   val list = str.split(",")
   println(list) // Output: [apple, banana, grape]
   ```

5. **Замена символов и подстрок**:
   ```kotlin
   val str = "Kotlin is amazing"
   println(str.replace("amazing", "great")) // Output: Kotlin is great
   ```

6. **Извлечение подстроки**:
   ```kotlin
   val str = "Kotlin programming"
   println(str.substring(0, 6)) // Output: Kotlin
   ```

#### 5. Расширенные возможности

1. **Регулярные выражения**:
   ```kotlin
   val regex = "\d+".toRegex()
   val str = "123 abc 456"
   println(regex.findAll(str).map { it.value }.toList()) // Output: [123, 456]
   ```

2. **Строки как коллекции**:
   Строка в Kotlin — это коллекция символов, поэтому можно использовать функции из стандартной библиотеки:
   ```kotlin
   val str = "Kotlin"
   println(str.filter { it.isUpperCase() }) // Output: K
   ```

3. **Строковые шаблоны для форматирования**:

В Kotlin можно создавать строки с форматированием, используя специальные функции и конструкции, схожие с языком Java.

1. **Метод format**:
   Метод `String.format` позволяет форматировать строки, вставляя значения в определенные места согласно шаблону:
   ```kotlin
   val format = "Прогресс: %d%% завершен"
   val result = String.format(format, 75)
   println(result) // Output: Прогресс: 75% завершен
   ```

   В шаблоне используются спецификаторы:
   - `%d` — для целых чисел.
   - `%f` — для чисел с плавающей точкой.
   - `%s` — для строк.

2. **Локализация форматирования**:
   Вы можете указать локаль для форматирования чисел и дат:
   ```kotlin
   import java.util.Locale
   val format = "Число с запятыми: %,d"
   val result = String.format(Locale.US, format, 1000000)
   println(result) // Output: Число с запятыми: 1,000,000
   ```

3. **Шаблоны с фиксированной шириной**:
   Для выравнивания текста можно задавать минимальную ширину поля:
   ```kotlin
   val format = "|%-10s|%10s|"
   println(String.format(format, "Kotlin", "Rocks"))
   // Output: |Kotlin    |     Rocks|
   ```

4. **Современный подход**: вместо использования `format`, можно применять string templates с расширенными выражениями, но `format` удобен для сложного форматирования.

Такие шаблоны особенно полезны при генерации отчетов или форматировании больших объемов данных.
   ```kotlin
   val format = "%sНа %d%%"
   println(format.format("Прогресс:", 80)) // Output: Прогресс: 80%
   ```

#### 6. Практическая часть
1. Напишите программу, которая принимает строку от пользователя, удаляет пробелы и выводит результат.
2. Реализуйте функцию, которая подсчитывает количество уникальных символов в строке.
3. Используя регулярные выражения, напишите программу для извлечения всех email-адресов из текста.

#### 7. Заключение
Работа со строками — это важный аспект программирования. В Kotlin строки интуитивно понятны и удобны в использовании благодаря богатой функциональности стандартной библиотеки. Изучение методов и возможностей строк позволит эффективно решать задачи обработки текста в ваших проектах.

