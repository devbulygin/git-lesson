# Java Exceptions

## Иерархия исключений

Исключения в Java — это обычные классы с общим родителем `Throwable`.

```
Throwable
├── Error        — системные ошибки JVM (нельзя перехватить)
└── Exception    — можно перехватить и обработать
    ├── RuntimeException   — необязательно обрабатывать
    └── Прочие             — обязательно обрабатывать (Checked)
```

- **Error** — серьёзные сбои JVM: нехватка памяти, переполнение стека. Обычно не перехватываются.
- **Exception** — ошибки, которые можно поймать и обработать.
- **RuntimeException** — подвид Exception, обработка остаётся на усмотрение разработчика.

---

## Checked исключения (проверяемые)

Это исключения, которые **обязательно** нужно либо обработать, либо пробросить дальше. Компилятор не даст запустить код без этого.

### Объявление и выброс исключения

```java
public static void unsafeMethod(int value) throws FileNotFoundException {
    if (value > 0) {
        throw new FileNotFoundException();
    }
}
```

Ключевое слово `throws` в сигнатуре метода сообщает вызывающему коду, что метод может выбросить исключение.

### Несколько исключений

```java
public static void unsafeMethod(int value) throws FileNotFoundException, TimeoutException {
    if (value > 0) {
        throw new FileNotFoundException();
    }
}
```

---

## Обработка исключений: try-catch-finally

### Базовый синтаксис

```java
try {
    unsafeMethod(1);
} catch (FileNotFoundException exception) {
    exception.printStackTrace(); // выводит стектрейс ошибки
}
```

### Несколько catch-блоков

```java
// Вариант 1: объединить в один блок через |
try {
    unsafeMethod(1);
} catch (FileNotFoundException | TimeoutException exception) {
    exception.printStackTrace();
}

// Вариант 2: отдельные блоки (от частного к общему!)
try {
    unsafeMethod(1);
} catch (FileNotFoundException e) {
    e.printStackTrace();
} catch (Exception e) {
    e.printStackTrace();
}
```

> **Важно:** catch-блоки проверяются сверху вниз. Если поставить родительский класс (`Exception`) первым — дочерние блоки станут недостижимыми. Всегда обрабатывай частные случаи **перед** общими.

### finally

Блок `finally` выполняется **всегда** — независимо от того, возникло исключение или нет. Используется для освобождения ресурсов: закрытия соединений с БД, файлов и т.д.

```java
try {
    unsafeMethod(1);
} catch (Exception exception) {
    exception.printStackTrace();
} finally {
    System.out.println("Этот код выполнится в любом случае");
}
```

Можно использовать `try-finally` без `catch` — чтобы выполнить код при пробросе исключения выше:

```java
try {
    unsafeMethod(1);
} finally {
    System.out.println("Выполняется даже при проброске исключения");
}
```

### Приоритет finally над return

`finally` выполняется даже если в `try` или `catch` есть `return`. Если в `finally` тоже есть `return` — он **перекрывает** предыдущий:

```java
public static int finallyTest() {
    try {
        return 1;  // не вернётся
    } catch (Throwable throwable) {
        return 2;  // не вернётся
    } finally {
        return 4;  // вернётся именно это
    }
}
// Результат: 4
```

---

## Unchecked исключения (RuntimeException)

Эти исключения **не нужно** объявлять через `throws` и не обязательно обрабатывать. Компилятор не требует их обработки.

```java
public static void unsafeMethod(int value) {
    if (value > 0) {
        throw new RuntimeException();
    }
}
```

Если не перехватить — программа завершится с ошибкой:

```
main start
unsafe start
Exception in thread "main" java.lang.RuntimeException
    at ExceptionExample.unsafeMethod(ExceptionExample.java:15)
    at ExceptionExample.main(ExceptionExample.java:7)

Process finished with exit code 1
```

**Когда использовать RuntimeException?** Для ошибок программиста — ситуаций, которые не должны возникать при правильном использовании кода (неверные аргументы, нарушение инварианта и т.д.). Примеры из JDK: `NullPointerException`, `ArrayIndexOutOfBoundsException`, `IllegalArgumentException`.

---

## Создание собственных исключений

Иногда стандартных исключений недостаточно — нужно создать своё, чтобы точнее описать ошибку в предметной области.

### Checked исключение

Наследуйся от `Exception`:

```java
public class InsufficientFundsException extends Exception {

    private final double amount;

    public InsufficientFundsException(double amount) {
        super("Недостаточно средств. Не хватает: " + amount);
        this.amount = amount;
    }

    public double getAmount() {
        return amount;
    }
}
```

### Unchecked исключение

Наследуйся от `RuntimeException`:

```java
public class InvalidAgeException extends RuntimeException {

    public InvalidAgeException(int age) {
        super("Недопустимый возраст: " + age);
    }
}
```

### Использование

```java
public class BankAccount {

    private double balance;

    public BankAccount(double balance) {
        this.balance = balance;
    }

    // Checked — вызывающий код обязан обработать
    public void withdraw(double amount) throws InsufficientFundsException {
        if (amount > balance) {
            throw new InsufficientFundsException(amount - balance);
        }
        balance -= amount;
    }

    // Unchecked — ошибка программиста, передан некорректный возраст
    public static void setAge(int age) {
        if (age < 0 || age > 150) {
            throw new InvalidAgeException(age);
        }
    }
}

// Обработка:
BankAccount account = new BankAccount(100.0);
try {
    account.withdraw(200.0);
} catch (InsufficientFundsException e) {
    System.out.println(e.getMessage());       // Недостаточно средств. Не хватает: 100.0
    System.out.println(e.getAmount());        // 100.0
}
```

### Правила именования и проектирования

- Название должно заканчиваться на `Exception`: `UserNotFoundException`, `PaymentFailedException`.
- Всегда вызывай `super(message)` — чтобы стектрейс содержал понятное описание.
- Добавляй поля с дополнительным контекстом (как `amount` выше) — это упрощает диагностику.
- Выбирай между Checked и Unchecked осознанно:
    - **Checked** — если вызывающий код реально может и должен восстановиться после ошибки.
    - **Unchecked** — если ошибка вызвана неправильным использованием кода.

