# Дженерики (Generics) в Java

---

## 1. Введение

Для того чтобы объяснить что такое дженерики давайте создадим наш собственный `CustomList` которая будет основана на массиве, но которая будет избавлена от необходимости указывать индекс каждый раз при создании.

```java
public class CustomList {
    private Object[] objects; // вносим сюда самый базовый тип
    private int size;         // мы должны как то контролировать размер массива

    public CustomList(int initialSize) {
        this.objects = new Object[initialSize]; // создаем массив
    }

    public void add(T element) {
        objects[size++] = element;
        // благодаря постфиксной форме инкремента сначала возьмется текущий размер size,
        // а затем после операции он увеличится на 1, соответственно теперь
        // в отдельной строчке делать этого не нужно
        // в данном примере не будем реализовывать динамический массив,
        // так как текущая задача связана не с этим а с обобщенной типизацией
    }

    public Object get(int index) {
        // так как мы не знаем конкретный тип, то мы возвращаем Object
        return objects[index];
    }

    public int getSize() {
        return size;
    }
}
```

> **`Object`** — корневой класс в иерархии Java. Абсолютно все классы неявно наследуются от `Object`, поэтому переменная типа `Object` может хранить ссылку на объект любого класса. Именно по этой причине `Object` используется здесь как «универсальный контейнер».

Создадим новый класс для проверки:

```java
public class ListRunner {
    public static void main(String[] args) {
        CustomList list = new CustomList(10);
        list.add("String1");
        list.add("String2");
        list.add("String3");
        Object element = list.get(1);
        System.out.println(element);
    }
}
```

У нас есть проблема — сейчас наш лист возвращает не тот тип, который мы положили, а `Object` с базовым функционалом `Object`. Как вариант мы можем скастовать её к `String`:

```java
String element = (String) list.get(1);
```

> **Каст (приведение типов, casting)** — явное указание компилятору считать объект экземпляром другого типа. Запись `(String)` означает: «я знаю, что это строка, доверяй мне». Если объект на самом деле не является строкой, JVM бросит `ClassCastException` уже во время выполнения, а не на этапе компиляции — в этом и заключается главная опасность.

Но в таком случае мы можем добавить другой тип данных в `add` и никто нам не запретит, и мы получим `ClassCastException`:

```java
list.add("String1");
list.add(2);          // автоматически обернётся в Integer (autoboxing)
list.add("String3");
```

> **`ClassCastException`** — исключение времени выполнения (runtime exception). Оно возникает, когда программа пытается привести объект к несовместимому типу. Коварство здесь в том, что компилятор такую ошибку не видит — она проявится только при запуске. Именно для того, чтобы переносить подобные ошибки с этапа выполнения на этап компиляции, и были введены дженерики.

Для решения этой проблемы ввели дженерики. Перепишем наш лист с использованием дженериков:

```java
public class CustomList<T> {
    private T[] objects;
    private int size;

    public CustomList(int initialSize) {
        // мы не можем создавать новые объекты типа T, и массивы в том числе,
        // проблему можно решить тем что можно создать Object и скастовать его к T
        this.objects = (T[]) new Object[initialSize];
    }

    public void add(T element) {
        objects[size++] = element;
    }

    public T get(int index) {
        return objects[index];
    }

    public int getSize() {
        return size;
    }
}
```

> **`<T>`** — это объявление параметра типа (type parameter). Буква `T` — общепринятое соглашение (сокращение от *Type*), но технически это просто имя. Другие распространённые имена: `E` (Element — для коллекций), `K`/`V` (Key/Value — для словарей), `R` (Return — для результата). Запись `(T[]) new Object[initialSize]` вызывает предупреждение компилятора об непроверяемом касте (*unchecked cast*), потому что из-за механизма **type erasure** информация о типе `T` стирается в рантайме и массив на самом деле остаётся `Object[]`. Это известное ограничение дженериков в Java.

> **Type Erasure (стирание типов)** — механизм реализации дженериков в Java. Компилятор использует информацию о типе для проверок на этапе компиляции, но затем «стирает» её из байт-кода. В рантайме `CustomList<String>` и `CustomList<Integer>` — это один и тот же класс `CustomList`. Именно поэтому нельзя написать `new T[size]` — в момент создания массива JVM уже не знает, что такое `T`.

Теперь мы можем задать параметр и наш `List` больше не сможет принять другие типы:

```java
public class ListRunner {
    public static void main(String[] args) {
        CustomList<String> list = new CustomList<>(10);
        list.add("String1");
        list.add("String2");
        list.add("String3");
        String element = list.get(1); // уже не нужен каст — компилятор знает тип
        System.out.println(element);
    }
}
```

> **`<>`** (diamond operator, «оператор-ромб») — синтаксический сахар, введённый в Java 7. Позволяет не дублировать тип в правой части выражения: компилятор выводит его сам на основе левой части. `new CustomList<>(10)` эквивалентно `new CustomList<String>(10)`.

Можно не задавать параметр, но в таком случае он будет параметризован классом `Object`.

> Использование дженерик-класса без указания типа называется **raw type** (сырой тип). Компилятор выдаёт предупреждение, а все гарантии типобезопасности теряются — поведение становится аналогичным первой версии нашего `CustomList`. Raw types существуют только для обратной совместимости со старым кодом (до Java 5), и их использования следует избегать.

---

## 2. Обобщённая типизация на уровне класса

Создадим пакет `weapon`, в нём создадим интерфейс `Weapon` который будет иметь один метод:

```java
public interface Weapon {
    int getDamage();
}
```

Теперь создадим разные типы оружия в виде интерфейсов: `MagicWeapon`, `MeleeWeapon`, `RangeWeapon`:

```java
public interface MagicWeapon extends Weapon {
}
```

```java
public interface MeleeWeapon extends Weapon {
}
```

```java
public interface RangeWeapon extends Weapon {
}
```

И создадим само оружие: `Bow`, `Sword`, `Wand`:

```java
public class Bow implements RangeWeapon {
    @Override
    public int getDamage() {
        return 10;
    }
}
```

```java
public class Sword implements MeleeWeapon {
    @Override
    public int getDamage() {
        return 15;
    }
}
```

```java
public class Wand implements MagicWeapon {
    @Override
    public int getDamage() {
        return 20;
    }
}
```

А теперь создадим абстрактный класс героя с оружием:

```java
public abstract class Hero<T> {
    private T weapon;

    public T getWeapon() {
        return weapon;
    }

    public void setWeapon(T weapon) {
        this.weapon = weapon;
    }
}
```

Создадим трёх героев — `Archer`, `Mage`, `Warrior`:

```java
public class Archer extends Hero<Bow> {
}
```

```java
public class Mage extends Hero<Wand> {
}
```

```java
public class Warrior extends Hero<Sword> {
}
```

> Здесь при наследовании мы **конкретизируем** параметр типа. `Archer extends Hero<Bow>` означает: «Лучник — это Герой, у которого тип оружия зафиксирован как `Bow`». После этого `getWeapon()` в классе `Archer` будет возвращать именно `Bow`, а `setWeapon()` будет принимать только `Bow`. Параметр `T` в `Hero` «закрывается» конкретным типом на уровне дочернего класса.

Теперь создадим `WeaponRunner` и создадим в нём наших героев и дадим им оружие:

```java
public class WeaponRunner {
    public static void main(String[] args) {
        Archer archer = new Archer<>();
        archer.setWeapon(new Bow());
        // сейчас мы не сможем положить ничего другого кроме Bow,
        // так как задали это в параметре при создании объекта

        Warrior warrior = new Warrior<>();
        warrior.setWeapon(new Sword());
    }
}
```

Но сейчас мы сможем положить в нашего героя любой тип вместо оружия, даже `String`. Но мы можем ограничивать использование наших дженериков. Так же мы можем ограничить нашего героя чтобы он принимал только оружие, и теперь дочерние классы не могут ничего брать что не является оружием:

```java
public abstract class Hero<T extends Weapon> {
    // ...
}
```

```java
public class Mage extends Hero<Wand> {
}
```

```java
public class Archer extends Hero<Bow> {
}
```

```java
public class Warrior extends Hero<Sword> {
}
```

> **`<T extends Weapon>`** — это **upper bounded type parameter** (ограничение сверху). Оно означает: «тип `T` должен быть `Weapon` или любым его наследником / реализацией». Ключевое слово `extends` используется как для классов, так и для интерфейсов (несмотря на то что интерфейс формально «реализуется» через `implements` — в контексте дженериков всегда пишется `extends`). Теперь попытка написать `Hero<String>` вызовет ошибку компиляции.

Теперь мы получили возможность ограничивать использование наших дженериков. Кроме того каждый класс может быть параметризован несколькими типами через `&`:

```java
// Пример: тип должен быть одновременно Weapon и Serializable
public abstract class Hero<T extends Weapon & Serializable> {
    // ...
}
```

> **`&`** в параметре типа задаёт **пересечение ограничений** (intersection type bound). Можно указывать несколько интерфейсов, но не более одного класса, и класс (если есть) должен стоять первым. Пример: `<T extends AbstractWeapon & Serializable & Cloneable>`.

---

## 3. Параметризация на уровне методов

В `WeaponRunner` создадим теперь статический метод `printWeaponDamage`:

```java
public static void printWeaponDamage(Hero hero) {
    System.out.println(hero.getWeapon().getDamage());
}
```

Сейчас нам необходимо в аргументе `Hero` указывать тип которым он должен быть параметризован, но что указывать?

- `Hero<Sword> hero` — в таком случае мы сможем передать сюда только `Warrior`, в других будет ошибка.
- `Hero<Weapon> hero` — так тоже не работает, так как при такой записи нам нужно передавать только конкретный тип, то есть `Weapon`, а не его наследников.

> Второй момент требует пояснения: дженерики в Java **инвариантны**. Это означает, что `Hero<Sword>` **не является** подтипом `Hero<Weapon>`, даже если `Sword` является подтипом `Weapon`. Это принципиальное отличие от массивов (массивы в Java ковариантны: `Sword[]` можно присвоить `Weapon[]`). Именно поэтому `Hero<Weapon>` не принимает `Hero<Sword>` — с точки зрения системы типов это разные, несовместимые параметризации.

Есть два варианта как это исправить.

**Первый вариант** — сразу после модификаторов `public static` указать тип с ограничением, и затем использовать его в аргументах:

```java
public static <T extends Weapon> void printWeaponDamage(Hero<T> hero) {
    System.out.println(hero.getWeapon().getDamage());
}
```

> Это называется **обобщённый метод** (generic method). Параметр типа `<T extends Weapon>` объявляется до возвращаемого типа метода и действует только в пределах этого метода. Компилятор сам выводит конкретный тип при вызове (type inference): `printWeaponDamage(archer)` — и `T` автоматически станет `Bow`.

Но есть ещё один вариант — использовать **wildcards** — это знак `?`:

```java
public static void printWeaponDamage(Hero<? extends Weapon> hero) {
    System.out.println(hero.getWeapon().getDamage());
}
```

В таком случае мы можем передать любого героя, который параметризован чем-то, что наследуется от `Weapon`.

> **Wildcard (`?`)** — знак подстановки. Читается как «некий неизвестный тип». В отличие от обобщённого метода, `?` не создаёт именованного параметра — мы просто говорим «нам всё равно, что за конкретный тип, главное что он соответствует ограничению». Это делает сигнатуру метода более лаконичной, когда сам конкретный тип внутри метода не нужен.

Wildcards даёт ещё одно преимущество — мы можем использовать ограничение не только сверху, но и снизу:

```java
// upper bounded wildcard — "Weapon или любой наследник Weapon"
public static void printWeaponDamage(Hero<? extends Weapon> hero) {
    System.out.println(hero.getWeapon().getDamage());
}

// lower bounded wildcard — "Sword или любой предок Sword"
public static void printWeaponDamage(Hero<? super Sword> hero) {
    System.out.println(hero.getWeapon().getDamage());
}
```

Для чего это нужно? Иногда нам необходимо использовать наш тип как потребителя или как производителя:

```java
public static void printWeaponDamage(Hero<? extends Weapon> hero) {
    Sword weapon = hero.getWeapon();    // ? extends Weapon  →  Producer (производитель)
    hero.setWeapon(new Sword());        // ? super Weapon    →  Consumer (потребитель)
    System.out.println(hero.getWeapon().getDamage());
}
```

> Это знаменитый принцип **PECS** (Producer Extends, Consumer Super), сформулированный Джошуа Блохом:
>
> - **`? extends T`** (upper bounded wildcard) — используйте когда вы только **читаете** данные из структуры. Структура выступает **производителем** (producer): она отдаёт вам объекты типа `T` или его подтипа. Записать что-либо в такую структуру нельзя (кроме `null`), потому что компилятор не знает точный тип.
>
> - **`? super T`** (lower bounded wildcard) — используйте когда вы только **пишете** данные в структуру. Структура выступает **потребителем** (consumer): она принимает объекты типа `T` или его подтипа. Прочитать из неё можно только как `Object`, потому что конкретный тип неизвестен.
>
> Практическое правило: если коллекция/объект нужен вам **только для чтения** — пишите `extends`. Если **только для записи** — пишите `super`. Если нужно и то и другое — используйте конкретный тип без wildcard.
