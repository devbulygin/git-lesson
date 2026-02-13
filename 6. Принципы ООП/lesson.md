
# Принципы ООП в Java

## 1. Инкапсуляция

### Что такое инкапсуляция

Инкапсуляция — это:

1. Объединение данных и методов работы с ними в одном классе.
2. Сокрытие внутренней реализации и защита состояния объекта.

Изначальное значение слова «инкапсуляция» в программировании — объединение данных и методов работы с этими данными в одной упаковке («капсуле»).

---

## Объединение данных и поведения

```java
public class Cat {

    private String name;
    private int age;
    private double weight;

    public void meow() {
        System.out.println("Мяу!");
    }
}
````

Класс содержит:

* состояние (поля)
* поведение (методы)

Это и есть инкапсуляция в базовом смысле.

---

## Сокрытие данных

Плохой вариант:

```java
public class Cat {
    public String name;
    public int age;
}
```

Можно сделать:

```java
Cat cat = new Cat();
cat.age = -100;
```

Это нарушает корректность состояния.

Правильный вариант:

```java
public class Cat {

    private String name;
    private int age;
    private double weight;

    public Cat(String name, int age, double weight) {
        setName(name);
        setAge(age);
        setWeight(weight);
    }

    public void setAge(int age) {
        if (age < 0) {
            throw new IllegalArgumentException("Возраст не может быть отрицательным");
        }
        this.age = age;
    }

    public int getAge() {
        return age;
    }

    // аналогично для остальных полей
}
```

Теперь объект всегда находится в корректном состоянии.

---

## Модификаторы доступа

| Модификатор | Доступ                |
| ----------- | --------------------- |
| `private`   | Только внутри класса  |
| `protected` | В пакете + наследники |
| `default`   | Только в пакете       |
| `public`    | Везде                 |

---

## Сокрытие реализации

Пользователь не должен знать, как реализован метод:

```java
public void feed(double foodWeight) {
    weight += foodWeight * 0.8; // внутренняя логика
}
```

Снаружи важен только контракт:

```java
cat.feed(0.2);
```

---

## Что дает инкапсуляция

* Контроль состояния
* Гарантии корректности
* Возможность менять реализацию без изменения интерфейса
* Снижение связанности

---

# 2. Наследование

## Все классы наследуются от Object

В Java любой класс **неявно наследуется от `Object`**:

```java
public class Cat {
}
```

Фактически это:

```java
public class Cat extends Object {
}
```

---

## Что такое Object?

`Object` — корневой класс иерархии Java.

Он содержит базовые методы:

```java
public boolean equals(Object obj)
public int hashCode()
public String toString()
protected Object clone()
protected void finalize()
public final Class<?> getClass()
public final void wait()
public final void notify()
public final void notifyAll()
```

### Основные методы:

* `toString()` — строковое представление
* `equals()` — сравнение объектов
* `hashCode()` — хеш-код
* `getClass()` — получение типа во время выполнения

Пример переопределения:

```java
@Override
public String toString() {
    return "Cat{name='" + name + "', age=" + age + "}";
}
```

---

## Что такое наследование

Наследование — механизм расширения поведения через `extends`.

```java
public class WildCat extends Cat {

    private boolean aggressive;

    public WildCat(String name, int age, double weight, boolean aggressive) {
        super(name, age, weight);
        this.aggressive = aggressive;
    }
}
```

---

## Как работает super

`super` используется:

1. Для вызова конструктора родителя
2. Для обращения к методам родителя
3. Для обращения к полям родителя

### Вызов конструктора

```java
super(name, age, weight);
```

Важно:

* Вызов `super()` должен быть первой строкой конструктора.
* Если не указать явно — будет вызван `super()` без параметров.
* Если у родителя нет конструктора без параметров — компилятор выдаст ошибку.

---

## Переопределение методов

```java
@Override
public void meow() {
    System.out.println("Дикий рык!");
}
```

`@Override`:

* помогает избежать ошибок
* гарантирует, что метод действительно переопределяется

---

## Что нельзя переопределить

* `final`
* `private`
* `static`

---

## Принцип is-a

Наследование допустимо только если соблюдается отношение **"является"**.

WildCat является Cat ✔
Cat не является Food ✘

Если отношение "содержит" — используется композиция:

```java
public class Cat {

    private Collar collar; // has-a
}
```

---

# 3. Полиморфизм

## 3. Полиморфизм

**Полиморфизм** — это способность объектов с одинаковым базовым типом вести себя по-разному.
Проще говоря:

> Один тип (`Cat`) — много форм поведения.

Полиморфизм позволяет работать с объектами через **общий родительский тип**, не зная их конкретного класса.

---

# 3.1. Полиморфизм времени выполнения (Runtime Polymorphism)

Это основной вид полиморфизма в Java.
Он реализуется через:

* наследование
* переопределение методов (overriding)
* динамическое связывание (dynamic dispatch)

---

### Базовый класс

```java
class Cat {
    void makeSound() {
        System.out.println("Some cat sound");
    }
}
```

---

### Дочерние классы

```java
class HomeCat extends Cat {
    @Override
    void makeSound() {
        System.out.println("Meow");
    }
}
```

```java
class WildCat extends Cat {
    @Override
    void makeSound() {
        System.out.println("Roar");
    }
}
```

---

### Использование полиморфизма

```java
Cat cat1 = new HomeCat();
Cat cat2 = new WildCat();

cat1.makeSound(); // Meow
cat2.makeSound(); // Roar
```

---

### Что здесь происходит?

* Тип переменной: `Cat`
* Реальный объект: `HomeCat` или `WildCat`
* JVM во время выполнения определяет, какой метод вызвать

Это называется **динамическое связывание**.

Метод выбирается по **реальному объекту**, а не по типу ссылки.

---

# 3.2. Как это работает внутри

Когда метод не `static`, не `private`, не `final`, он является **виртуальным**.

JVM:

1. Смотрит на реальный тип объекта
2. Ищет переопределённый метод
3. Вызывает его

Это и есть механизм runtime polymorphism.

---

# 3.3. Важно: поля не полиморфны

```java
class Cat {
    String type = "Cat";
}

class HomeCat extends Cat {
    String type = "HomeCat";
}

Cat cat = new HomeCat();
System.out.println(cat.type); // Cat
```

Поля определяются типом ссылки (`Cat`),
методы — типом объекта (`HomeCat`).

---

# 3.4. Правила переопределения (Overriding)

Чтобы метод считался переопределённым:

* имя совпадает
* параметры совпадают
* возвращаемый тип совпадает (или более узкий)
* уровень доступа нельзя уменьшать
* нельзя переопределить `final` метод
* `static` методы не переопределяются (они скрываются)

---

### Пример ошибки

```java
class Cat {
    final void sleep() {}
}

class HomeCat extends Cat {
    void sleep() {} // ошибка — нельзя переопределить final
}
```

---

# 3.5. Static методы — не полиморфизм

```java
class Cat {
    static void info() {
        System.out.println("Cat");
    }
}

class HomeCat extends Cat {
    static void info() {
        System.out.println("HomeCat");
    }
}

Cat cat = new HomeCat();
cat.info(); // Cat
```

Static методы выбираются по типу ссылки.

Это не runtime polymorphism.

---

# 3.6. Upcasting и Downcasting

### Upcasting — безопасно

```java
Cat cat = new HomeCat();
```

Ссылка родителя указывает на дочерний объект.

---

### Downcasting — потенциально опасно

```java
Cat cat = new HomeCat();
HomeCat homeCat = (HomeCat) cat; // безопасно
```

Но:

```java
Cat cat = new WildCat();
HomeCat homeCat = (HomeCat) cat; // ClassCastException
```

Проверка:

```java
if (cat instanceof HomeCat) {
    HomeCat homeCat = (HomeCat) cat;
}
```

---

# 3.7. Перегрузка (Overloading) — полиморфизм компиляции

```java
class Cat {

    void feed() {
        System.out.println("Feeding cat");
    }

    void feed(String food) {
        System.out.println("Feeding cat with " + food);
    }

    void feed(String food, int amount) {
        System.out.println("Feeding " + amount + " portions of " + food);
    }
}
```

Метод выбирается **во время компиляции**.

Это compile-time polymorphism.

---

# 3.8. Зачем нужен полиморфизм

Он позволяет писать расширяемый код.

```java
void makeCatSpeak(Cat cat) {
    cat.makeSound();
}
```

Метод не знает:

* `HomeCat`
* `WildCat`
* любой другой наследник

Но работает с любым.

Добавим новый класс:

```java
class StreetCat extends Cat {
    @Override
    void makeSound() {
        System.out.println("Hiss");
    }
}
```

Код `makeCatSpeak` менять не нужно.

Это основа:

* гибкой архитектуры
* расширяемости
* принципа подстановки Лисков (LSP)

---

# 3.9. Главная идея

Полиморфизм — это:

* один общий тип
* много различных реализаций
* выбор поведения во время выполнения

Он работает только для методов и основан на наследовании.


---

## 4. Абстракция

**Абстракция** — это принцип ООП, при котором мы скрываем детали реализации и оставляем только существенные характеристики объекта.

Проще:

> Мы описываем, **что умеет делать `Cat`**, но не обязательно говорим, **как именно это реализовано**.

Абстракция позволяет работать с общим понятием, не вникая в конкретные детали.

---

# 4.1. Зачем нужна абстракция

Представим, что у нас есть разные виды `Cat`:

* домашний
* дикий
* уличный

У всех есть общее поведение:

* издавать звук
* есть
* спать

Но реализация может отличаться.

Абстракция позволяет:

* задать общий контракт
* скрыть конкретную реализацию
* заставить наследников реализовать обязательные методы

---

# 4.2. Абстрактный класс

Абстрактный класс — это класс, который:

* объявляется с ключевым словом `abstract`
* **нельзя создать напрямую**
* может содержать абстрактные методы
* может содержать обычные методы
* может содержать поля
* может иметь конструктор

---

### Пример

```java
abstract class Cat {

    protected String name;

    Cat(String name) {
        this.name = name;
    }

    abstract void makeSound();

    void sleep() {
        System.out.println(name + " is sleeping");
    }
}
```

---

### Что здесь важно

1. Метод `makeSound()` — абстрактный
   → у него нет реализации
   → каждый наследник обязан его реализовать

2. Метод `sleep()` — обычный
   → общая логика для всех котов

3. Есть поле `name`
   → у всех котов есть имя

---

# 4.3. Реализация абстрактного класса

```java
class HomeCat extends Cat {

    HomeCat(String name) {
        super(name);
    }

    @Override
    void makeSound() {
        System.out.println("Meow");
    }
}
```

---

### Использование

```java
Cat cat = new HomeCat("Barsik");
cat.makeSound();
cat.sleep();
```

Мы работаем через абстракцию `Cat`.

---

# 4.4. Почему нельзя создать абстрактный класс

```java
Cat cat = new Cat("Test"); // ошибка
```

Потому что:

* у него есть абстрактный метод
* он не полностью реализован
* это шаблон, а не конкретный объект

---

# 4.5. Когда использовать абстрактный класс

Используем, когда:

* есть общее состояние (поля)
* есть общая логика
* нужно частично реализованное поведение
* нужно заставить наследников реализовать обязательные методы

---

# 4.6. Интерфейс как форма абстракции

Интерфейс — это чистый контракт.

Он описывает, что должен уметь `Cat`, но не хранит состояние.

---

### Пример интерфейса

```java
interface Huntable {
    void hunt();
}
```

---

### Реализация

```java
class WildCat extends Cat implements Huntable {

    WildCat(String name) {
        super(name);
    }

    @Override
    void makeSound() {
        System.out.println("Roar");
    }

    @Override
    public void hunt() {
        System.out.println(name + " is hunting");
    }
}
```

---

# 4.7. Абстрактный класс vs Интерфейс

| Абстрактный класс          | Интерфейс                   |
| -------------------------- | --------------------------- |
| Может иметь поля           | Нет состояния               |
| Может иметь конструктор    | Нет конструктора            |
| Может иметь обычные методы | Методы — контракт           |
| Наследование только одно   | Можно реализовать несколько |

---

# 4.8. Абстракция и уровень проектирования

Абстракция помогает:

* уменьшить связанность кода
* скрыть детали реализации
* писать расширяемые системы
* работать через контракты

Пример:

```java
void processCat(Cat cat) {
    cat.makeSound();
}
```

Метод не знает:

* домашний это кот
* дикий
* уличный

Он работает с абстракцией.

---

# 4.9. Связь абстракции и полиморфизма

Абстракция отвечает:

> Какие действия доступны?

Полиморфизм отвечает:

> Какая конкретная реализация будет выполнена?

Вместе они образуют основу объектно-ориентированного программирования.

---

# 4.10. Ключевая мысль

Абстракция — это способ описать общий тип (`Cat`)
без привязки к конкретной реализации.

Это инструмент проектирования, который позволяет строить гибкие и расширяемые системы.


```
```
