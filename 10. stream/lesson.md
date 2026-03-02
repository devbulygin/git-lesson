1. сегодня мы с вами познакомимся про функциональное программирование в языке java. не смотря на то что java у нас является объектно ориентированным языком, у нас так же могут применяться другие подходы, в том числе функциональное программирование
   
   создадим свой компаратор 
   и давайте напишем свой компаратор - это специальный класс для сравнения двух объектов - не по equals,  а по величине - это нам нужно иногда для сортировки IntegerComparator


```
public static class IntegerComparator implements Comparator<Integer> {

        @Override
        //[модиф.] возвр название([параметры])

        (Integer o1, Integer o2) -> {
            return Integer.compare(o1, o2);
        }
   }
```

   
   что делает компаратор - в данном случае он принимает в себя два числа, и возвращает 0, если они равны, -1 если первое число меньше второго и 1 если первое  число больше второго
   


```
import java.util.Comparator;

public class LambdaExample {

    public static void main(String[] args) {

        Comparator<Integer> comparator = new IntegerComparator();

        System.out.println(comparator.compare(25, 100));
    }

    private static class IntegerComparator implements Comparator<Integer> {

        @Override
//        //[модиф.] возвр название([параметры])

public int compare(Integer o1, Integer o2) {
return Integer.compare(o1, )}


    }
}
```


если так подумал то модификторы доступа можно не учитывать так как для понимания логики самого метода они не нужны. возвращаемый тип мы тоже можем определить из логики метода. название нас тоже не интересует, так как  интерфейс компаратор  является функциональным интерфейсом - то есть интерфейс у которого только один метод. 

соответственно если в интерфейсе только один метод то и название нам не нужно, мы просто беерм его по факту.

в итоге мы уже по факту получаем примерно такую структуру - лямбда выражение

```
        (Integer o1, Integer o2) -> {
            return Integer.compare(o1, o2);
        }
```

 и мы можем это лямбда выражение использовать уже сразу
public class LambdaExample {


```
    public static void main(String[] args) {

        Comparator<Integer> comparator = (Integer o1, Integer o2) -> {
            return Integer.compare(o1, o2);
        };

        System.out.println(comparator.compare(25, 100));
    }
```

таким образом мы уменьшаем количество кода

более того, так как мы знаем что наш компаратор параметризован Integer, мы можем еще уменьшить даже так


```
      Comparator<Integer> comparator = ( o1,  o2) -> {
             return Integer.compare(o1, o2);
             };
```

так как тут наша функция состоит из одной строчки, то мы тут можем убрать и фигурные скобки и слово return

```
      Comparator<Integer> comparator = ( o1,  o2) -> 
             Integer.compare(o1, o2);
```

Раньше для создания подобных конструкций использовались анонимные классы. как это выглядело:



```
public static void main(String[] args) {

        Comparator<Integer> comparator = new Comparator<Integer>(){
                @Override
public int compare(Integer o1, Integer o2) {
return Integer.compare(o1, )}


    }
        };

        System.out.println(comparator.compare(25, 100));
    }
```

их в целом и сейчас можно встретить, но сейчас используются в основном лямбда выражения

кроме того  мы можем увидеть что Integer.compare принимает два параметра и возвращет один. и в comparator так же вы видим что у него в аргументах два метода. таким образом мы можем использовать метод референс 


```
        Comparator<Integer> comparator = Integer::compare;
```

кроме компаратора у нас есть несколько еще основным функциональных интерфейсов: 


```
Function<T, R> {
R apply(T t);
}
```


эта функция принимает один параметр на вход и возвращает один параметр на выход


```
Predicate<T> {
boolean test(T t);
}
```


принимает одно значение и возвращает true или false
 
кроме того есть потребитель 

```
Cunsumer<T> {
void accept(T t);
}
```


принимает что то и ничего не возвращает


```
Supplier<T> {
T get();
}
```


ничего не потребляет но что то возвращает

функциональных интерфейсов вообще очень много и со многими мы познакомимся в процессе работы


2. stream api 

каждая коллекция соджержит дефолтный метод stream. - он представляет нашу коллекцию в виде потока данных или конвейра,

рассмотрим на практике. к примеру у нас есть такой список строк


```
        List<String> strings = List.of("88", "11", "22", "33", "44", "55", "66");
        
        
```

 у нас есть задача: каждый элемент конкатенировать друг с другом, и затем перевести в числа и вывести каждое четное число на экран

```
 
         for (String string : strings) {
            String value = string + string;
            Integer intValue = Integer.valueOf(value);
            if (intValue % 2 == 0) {
                System.out.println(intValue);
            }
        }
```
 в основном таких задач очень много

```
         strings.stream()
                .map(string -> string + string)
                .map(Integer::valueOf)
                .filter(value -> value % 2 == 0)
                .forEach(System.out::println);
```
тут происходит что то вроде цикла foreach с разными функциями. рассмотрим некоторые:
типом map - мы преобразуем  данные
filter фильтруем нашу коллекцию по заданном условию
forEach - терминальная операция выходим из потока и применяет функцию к каждому элементу потока 

```

кроме того мы можем отсортировать наши элементы
         strings.stream()
                .map(string -> string + string)
                .map(Integer::valueOf)
                .filter(value -> value % 2 == 0)
                .sorted()
                .forEach(System.out::println);
```
в том случае если у нас уже есть компаратор по умолчанию мы можем ничего не передавать. но если нам нужна какая то особая сортировка, то нам необходимо передать в sirted интерфейс компаратор, например по убыванию


```
strings.stream()
    .map(string -> string + string)
    .map(Integer::valueOf)
    .filter(value -> value % 2 == 0)
    .sorted((a, b) -> b.compareTo(a))
    .forEach(System.out::println);
```
дальше 

```
strings.stream()
    .map(string -> string + string)
    .map(Integer::valueOf)
    .filter(value -> value % 2 == 0)
    .sorted((a, b) -> b.compareTo(a))
	    .skip(1)
	    .limit(2)
    .forEach(System.out::println);
```
skip пропускает первый элемент, limit ограничивает вывод до двух элементов

3.  Stream для примитивных типов
   
   для работы с примитивными типами у нас есть три разных видов стримов
   `IntStream`
   `DoubleStream`
   `LongStream`
   
   
   
мы можем преобразовать свой stream в IntStream
```
   strings.stream()
    .map(string -> string + string)
    .map(Integer::valueOf)
    .filter(value -> value % 2 == 0)
    .sorted((a, b) -> b.compareTo(a))
	    .skip(1)
	    .limit(2)
	              .mapToInt(Integer::intValue)
    .forEach(System.out::println);
```
после этого у нас появляется новые функции  - такие как max без компаратора,
average, min, summaryStatistics();


```
        List<String> strings = List.of("88", "11", "22", "33", "44", "55", "66");
        IntSummaryStatistics intSummaryStatistics = strings.stream()
                .map(string -> string + string)
                .map(Integer::valueOf)
                .filter(value -> value % 2 == 0)
                .sorted()
//                .skip(1)
                .limit(2)
                .mapToInt(Integer::intValue)

                .summaryStatistics();
        System.out.println(intSummaryStatistics); 
```

        
        кроме того мы можем преобразовывать наш IntStream обратно в обычный stream mapToObj
        
        Стримы можно еще создавать таким образом
        

```
           List<String> collect = Stream.of("88", "11", "22", "33", "44", "55", "66")
                .peek(System.out::println)
                .collect(Collectors.toList());     
```

метод peek в этом случае аналогичен методу forEach, Но он не терминальный - то есть выполнение стрима не прекращается, просто к каждому элементу можно применить метод без возвращаемого значения.

кроме того есть еще вот такой метод, который позволяет просто перечислить заданный диапазон
```
        IntStream.range(0, 10)
                .forEach(System.out::println);
```


или более продвинутый вариант - перечиление с условием
```

        IntStream.iterate(0, operand -> operand + 3)
                .skip(10)
                .limit(20)
                .forEach(System.out::println);
```


   
   
   
Практика 




Дан список целых чисел. Найти среднее всех
нечётных чисел, делящихся на 5.

```
    public static void main(String[] args) {
        List<Integer> integers = List.of(1, 3, 4, 6, 8, 20, 10);
        OptionalDouble optionalDouble = integers.stream()
                .filter(value -> value % 2 != 0)
                .filter(value -> value % 5 == 0)
                .mapToInt(Integer::intValue)
                .average();
        optionalDouble.ifPresent(System.out::println);
```

Дан список строк. Найти количество уникальных
строк длиной более 8 символов.

```
    public static void main(String[] args) {
        List<String> strings = List.of(
                "string-1",
                "string-2",
                "string-3",
                "string-4",
                "string-10",
                "string-10",
                "string-10",
                "string-12",
                "string-16"
        );
        int result = strings.stream()
                .filter(value -> value.length() > 8)
                .collect(Collectors.toSet())
                .size();
        System.out.println(result);

        long result2 = strings.stream()
                .filter(value -> value.length() > 8)
                .distinct()
                .count();
        System.out.println(result2);
    }
```

Дана Map<String, Integer>. Найти сумму всех значений, длина ключей которых меньше 7
   символов.


```
    public static void main(String[] args) {
        Map<String, Integer> map = Map.of(
                "string1", 1,
                "strin2", 2,
                "string3", 3,
                "string4", 5,
                "strin5", 5
        );
        int result = map.entrySet().stream()
                .filter(entry -> entry.getKey().length() < 7)
         //       .mapToInt(entry -> entry.getValue().intValue())
                .mapToInt(Map.Entry::getValue)
                .sum();
        System.out.println(result);
    }
```
Дан список целых чисел, вывести строку,
представляющую собой конкатенацию
строковых представлений этих чисел.
Пример: список {5, 2, 4, 2, 1}
Результирующая строка: "52421"

    public static void main(String[] args) {
        List<Integer> integers = List.of(5, 2, 4, 2, 1);
        String result = integers.stream()
                .map(String::valueOf)
                //.collect(Collectors.joining());
                .collect(Collectors.joining(",", "Prefix: ", " end"));
        System.out.println(result);
    }
    
Дан класс Person с полями firstName, lastName,
age.
Вывести полное имя самого старшего человека, у
которого длина этого имени не превышает 15
символов.


```
public class Person {

    private String firstName;
    private String lastName;
    private int age;

    public Person(String firstName, String lastName, int age) {
        this.firstName = firstName;
        this.lastName = lastName;
        this.age = age;
    }

    public String getFullName() {
        return firstName + " " + lastName;
    }

    public String getFirstName() {
        return firstName;
    }

    public String getLastName() {
        return lastName;
    }

    public int getAge() {
        return age;
    }

    @Override
    public String toString() {
        return "Person{" +
                "firstName='" + firstName + '\'' +
                ", lastName='" + lastName + '\'' +
                ", age=" + age +
                '}';
    }
}
```



```
    public static void main(String[] args) {
        List<Person> persons = List.of(
                new Person("Ivan", "Ivanov", 20),
                new Person("Petr", "Petrov", 25),
                new Person("Sveta", "Svetikova", 33),
                new Person("Kate", "Ivanova", 25),
                new Person("Slava", "Slavikov", 18),
                new Person("Arni", "Kutuzov12324", 56)
        );

        persons.stream()
                .filter(person -> person.getFullName().length() < 15)
                .max(Comparator.comparing(Person::getAge))
                .map(Person::getFullName)
                .ifPresent(System.out::println);
    }
```
