
# Исключения в Java

Исключения — это по сути обычные классы, у которых есть один общий родитель — `Throwable`.

У `Throwable` есть два основных наследника:

- `Error`
- `Exception`

## Error

`Error` — это системные ошибки в JVM, которые обычно нельзя перехватить и корректно обработать.  
Это серьёзные сбои, например нехватка памяти (`OutOfMemoryError`).

Такие ошибки сигнализируют о проблемах на уровне среды выполнения.

## Exception

`Exception` — это исключения, которые можно перехватить и обработать.

Они делятся на:

- **Checked (проверяемые)** — обязаны быть обработаны или проброшены выше
- **Unchecked (Runtime)** — обрабатывать необязательно

Мы уже сталкивались с некоторыми исключениями, например:
- `NullPointerException`
- `IndexOutOfBoundsException`

---

# Пример checked-исключения

Создадим класс и поместим в него небезопасный метод:

```java
public class ExceptionExample {

    public static void main(String[] args) {

    }

    public static void unsafeMethod(int value) {
        if (value > 0) {

        }
    }
}
````

Раньше мы просто писали `System.out.println`, но можем воспользоваться конструкцией `throw` и выбросить исключение.

```java
public static void unsafeMethod(int value) throws FileNotFoundException {
    if (value > 0) {
        throw new FileNotFoundException();
    }
}
```

Теперь попробуем вызвать метод:

```java
public static void main(String[] args) {
    unsafeMethod(1);
}
```

Java сообщает, что исключение нужно обработать.

Мы можем:

* обработать его в `main`
* либо пробросить выше (до JVM)

## Обработка через try-catch

```java
public static void main(String[] args) {
    try {
        unsafeMethod(1);
    } catch (FileNotFoundException exception) {
        exception.printStackTrace();
    }
}
```

Мы увидим стек вызовов — откуда произошла ошибка.

---

# Прерывание выполнения метода

Если в `unsafeMethod` произойдёт исключение, всё что после `throw` выполнено не будет.

```java
public class ExceptionExample {

    public static void main(String[] args) {
        System.out.println("main start");

        try {
            unsafeMethod(-1);
        } catch (FileNotFoundException exception) {
            exception.printStackTrace();
        }

        System.out.println("main end");
    }

    public static void unsafeMethod(int value) throws FileNotFoundException {
        System.out.println("unsafe start");

        if (value > 0) {
            throw new FileNotFoundException();
        }

        System.out.println("unsafe end");
    }
}
```

Вывод:

```
main start
unsafe start
main end
java.io.FileNotFoundException
	at ExceptionExample.unsafeMethod(ExceptionExample.java:17)
	at ExceptionExample.main(ExceptionExample.java:7)
```

Почему стек выводится "после"?

Потому что:

* `System.out.println` пишет в поток `out`
* исключение выводится в поток `err`

Если использовать `System.err.println`, порядок будет последовательным.

---

# Несколько исключений

Метод может пробрасывать несколько исключений:

```java
public static void unsafeMethod(int value)
        throws FileNotFoundException, TimeoutException {

    System.out.println("unsafe start");

    if (value > 0) {
        throw new FileNotFoundException();
    }

    System.out.println("unsafe end");
}
```

Вызывающий метод обязан обработать их все.

## Multi-catch

```java
try {
    unsafeMethod(1);
} catch (FileNotFoundException | TimeoutException exception) {
    exception.printStackTrace();
}
```

---

# Полиморфизм в catch

Можно ловить родителя:

```java
try {
    unsafeMethod(1);
} catch (Exception exception) {
    exception.printStackTrace();
}
```

Важно: более общий тип должен идти ПОСЛЕ более конкретного.

Неправильно:

```java
catch (Exception e) { }
catch (TimeoutException e) { } // недостижимый код
```

Правильно:

```java
catch (TimeoutException e) { }
catch (Exception e) { }
```

---

# finally

`finally` выполняется всегда.

```java
try {
    unsafeMethod(1);
} catch (Exception exception) {
    exception.printStackTrace();
} finally {
    System.out.println("finally");
}
```

Даже если исключение пробрасывается дальше:

```java
try {
    unsafeMethod(1);
} finally {
    System.out.println("finally");
}
```

Чаще всего используется для:

* закрытия файлов
* закрытия соединений с БД
* освобождения ресурсов

---

# Особенность return в finally

```java
public static void main(String[] args) {
    System.out.println(finallyTest());
}

public static int finallyTest() {
    try {
        return 1;
    } catch (Throwable throwable) {
        return 2;
    } finally {
        return 4;
    }
}
```

Вернётся `4`.

`finally` перезаписывает результат.

⚠️ Возвращать значение из `finally` — плохая практика.

---

# RuntimeException (unchecked)

```java
public static void unsafeMethod(int value) {
    System.out.println("unsafe start");

    if (value > 0) {
        throw new RuntimeException();
    }

    System.out.println("unsafe end");
}
```

Мы не обязаны:

* ловить это исключение
* указывать `throws`

Пример:

```java
public static void main(String[] args) {
    System.out.println("main start");
    unsafeMethod(1);
    System.out.println("main end");
}
```

Вывод:

```
main start
unsafe start
Exception in thread "main" java.lang.RuntimeException
	at ExceptionExample.unsafeMethod(ExceptionExample.java:15)
	at ExceptionExample.main(ExceptionExample.java:7)
```

Программа завершается с кодом 1.

---

# Для чего используются RuntimeException?

Для ошибок программиста:

* обращение к `null`
* выход за границы массива
* некорректная логика
* неправильные аргументы метода

Это ситуации, которые **не должны происходить при корректной работе программы**.

Обработка таких исключений остаётся на усмотрение разработчика.

---

# Создание собственных исключений


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

```
