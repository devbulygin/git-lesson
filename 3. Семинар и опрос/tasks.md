# Задачи для семинара по Java



## Задача 11: Максимум из двух чисел
**Условие:**
Даны два числа: 89 и 156. Выведите большее из них.
```
156
```

**Решение:**
```java
public class Max {
    public static void main(String[] args) {
        int a = 89;
        int b = 156;
        int max;
        
        if (a > b) {
            max = a;
        } else {
            max = b;
        }
        
        System.out.println(max);
    }
}
```

---

## Задача 2: Проверка на положительное число
**Условие:**
Дано число -15. Если число положительное, выведите 1, если отрицательное — выведите 0, если ноль — выведите 2.
```
0
```

**Решение:**
```java
public class CheckPositive {
    public static void main(String[] args) {
        int number = -15;
        int result;
        
        if (number > 0) {
            result = 1;
        } else if (number < 0) {
            result = 0;
        } else {
            result = 2;
        }
        
        System.out.println(result);
    }
}
```

---

## Задача 3: Чётное или нечётное
**Условие:**
Дано число 48. Если число чётное, выведите 1, если нечётное — выведите 0.
```
1
```

**Решение:**
```java
public class EvenOdd {
    public static void main(String[] args) {
        int number = 48;
        int result;
        
        if (number % 2 == 0) {
            result = 1;
        } else {
            result = 0;
        }
        
        System.out.println(result);
    }
}
```

---

## Задача 4: Среднее арифметическое
**Условие:**
Даны три числа: 10, 20, 30. Вычислите среднее арифметическое.
```
20.0
```

**Решение:**
```java
public class Average {
    public static void main(String[] args) {
        int a = 10;
        int b = 20;
        int c = 30;
        double average = (a + b + c) / 3.0;
        
        System.out.println(average);
    }
}
```

---

## Задача 5: Остаток от деления
**Условие:**
Даны два числа: 47 и 5. Выведите остаток от деления первого числа на второе.
```
2
```

**Решение:**
```java
public class Remainder {
    public static void main(String[] args) {
        int a = 47;
        int b = 5;
        int remainder = a % b;
        
        System.out.println(remainder);
    }
}
```

---

## Задача 6: Периметр и площадь квадрата
**Условие:**
Дана сторона квадрата: 7. Выведите периметр и площадь квадрата.
```
246
```

**Решение:**
```java
public class Square {
    public static void main(String[] args) {
        int side = 7;
        int perimeter = side * 4;
        int area = side * side;

        System.out.println(perimeter);
        System.out.println(area);
    }
}
```

---

## Задача 7: Абсолютное значение
**Условие:**
Дано число -78. Выведите его абсолютное значение (модуль).
```
78
```

**Решение:**
```java
public class Absolute {
    public static void main(String[] args) {
        int number = -78;
        int abs;
        
        if (number < 0) {
            abs = -number;
        } else {
            abs = number;
        }
        
        System.out.println(abs);
    }
}
```

---


## Задача 8 Проверка диапазона
**Условие:**
Дано число: 45. Проверьте, входит ли оно в диапазон от 30 до 60 включительно. Если да — выведите 1, если нет — выведите 0.
```
1
```

**Решение:**
```java
public class RangeCheck {
    public static void main(String[] args) {
        int number = 45;
        int result;

        if (number >= 30 && number <= 60) {
            result = 1;
        } else {
            result = 0;
        }

        System.out.println(result);
    }
}
```

---

## Задача 9  Количество секунд в заданном времени
**Условие:**
Дано: 2 часа, 15 минут, 30 секунд. Выведите общее количество секунд.
```
5
```

**Решение:**
```java
public class TimeToSeconds {
    public static void main(String[] args) {
        int hours = 2;
        int minutes = 15;
        int seconds = 30;
        int totalSeconds = hours * 3600 + minutes * 60 + seconds;

        System.out.println(totalSeconds);
    }
}
```

### Запасная задача в секции 1: Подсчёт калорий в заказе
**Условие:**
В ресторане быстрого питания: бургер — 500 калорий, картошка фри — 350 калорий, газировка — 150 калорий. Клиент заказал 2 бургера, 1 порцию картошки и 1 газировку. Программа должна вывести наименование товара, количество, и сколько всего калорий
```
Заказ:
2 бургера
1 картошка фри
1 газировка
Всего калорий: 1500
```

**Решение:**
```java
public class CalorieCounter {
    public static void main(String[] args) {
        int burgerCalories = 500;
        int friesCalories = 350;
        int sodaCalories = 150;
        
        int burgerCount = 2;
        int friesCount = 1;
        int sodaCount = 1;
        
        int totalCalories = (burgerCalories * burgerCount) + 
                           (friesCalories * friesCount) + 
                           (sodaCalories * sodaCount);
        
        System.out.println("Заказ:");
        System.out.println(burgerCount + " бургера");
        System.out.println(friesCount + " картошка фри");
        System.out.println(sodaCount + " газировка");
        System.out.println("Всего калорий: " + totalCalories);
    }
}
```


## Задача 10: Калькулятор чаевых в ресторане
**Условие:**
Вы работаете официантом в ресторане. Напишите программу, которая рассчитывает размер чаевых. Сумма счёта — 1250 рублей. Если сумма счёта больше 1000 рублей, чаевые составляют 15%, иначе — 10%. Программа должна вывести сумму чаевых, итого к оплате
```
Сумма чаевых: 187.5 рублей
Итого к оплате: 1437.5 рублей
```

**Решение:**
```java
public class TipCalculator {
    public static void main(String[] args) {
        int billAmount = 1250;
        double tipPercentage;
        
        if (billAmount > 1000) {
            tipPercentage = 0.15;
        } else {
            tipPercentage = 0.10;
        }
        
        double tipAmount = billAmount * tipPercentage;
        double totalAmount = billAmount + tipAmount;
        
        System.out.println("Сумма чаевых: " + tipAmount + " рублей");
        System.out.println("Итого к оплате: " + totalAmount + " рублей");
    }
}
```

---

## Задача 11: Проверка чётности номера места в автобусе
**Условие:**
В автобусе места с чётными номерами находятся у окна, с нечётными — у прохода. Напишите программу, которая проверяет, находится ли место №27 у окна или у прохода. Используйте оператор остатка от деления. Программа должна вывести номер места и где оно находится
```
Место 27 у прохода
```

**Решение:**
```java
public class BusSeat {
    public static void main(String[] args) {
        int seatNumber = 27;
        
        if (seatNumber % 2 == 0) {
            System.out.println("Место " + seatNumber + " у окна");
        } else {
            System.out.println("Место " + seatNumber + " у прохода");
        }
    }
}
```

---

## Задача 12: Конвертер температуры
**Условие:**
Вы разрабатываете приложение для метеостанции. Температура измеряется в градусах Цельсия, но нужно отображать её в Фаренгейтах. Формула: F = C × 1.8 + 32. Температура сейчас 25°C. Программа должна вывести температуру в цельсиях и в фаренгейтах
```
Температура: 25 градусов Цельсия
Температура: 77.0 градусов Фаренгейта
```

**Решение:**
```java
public class TemperatureConverter {
    public static void main(String[] args) {
        int celsius = 25;
        double fahrenheit = celsius * 1.8 + 32;
        
        System.out.println("Температура: " + celsius + " градусов Цельсия");
        System.out.println("Температура: " + fahrenheit + " градусов Фаренгейта");
    }
}
```

---

## Задача 13: Калькулятор скидки в магазине
**Условие:**
В магазине электроники проходит акция: при покупке на сумму от 10000 рублей — скидка 10%, от 20000 рублей — скидка 15%, от 30000 рублей — скидка 20%. Покупка составила 25000 рублей. Выведите сумму покупки, процент скидки, сумму скидки, итого к оплате.
```
Сумма покупки: 25000 рублей
Скидка: 15%
Сумма скидки: 3750.0 рублей
Итого к оплате: 21250.0 рублей
```

**Решение:**
```java
public class DiscountCalculator {
    public static void main(String[] args) {
        int purchaseAmount = 25000;
        int discountPercent;
        
        if (purchaseAmount >= 30000) {
            discountPercent = 20;
        } else if (purchaseAmount >= 20000) {
            discountPercent = 15;
        } else if (purchaseAmount >= 10000) {
            discountPercent = 10;
        } else {
            discountPercent = 0;
        }
        
        double discountAmount = purchaseAmount * discountPercent / 100.0;
        double finalAmount = purchaseAmount - discountAmount;
        
        System.out.println("Сумма покупки: " + purchaseAmount + " рублей");
        System.out.println("Скидка: " + discountPercent + "%");
        System.out.println("Сумма скидки: " + discountAmount + " рублей");
        System.out.println("Итого к оплате: " + finalAmount + " рублей");
    }
}
```

---

## Задача 14: Определение времени суток
**Условие:**
Вы разрабатываете умный будильник. Нужно определить время суток по часам: с 6 до 12 — утро, с 12 до 18 — день, с 18 до 23 — вечер, с 23 до 6 — ночь. Сейчас 14 часов. Программа должна вывести сколько сейчас времени и какое сейчас время суток
```
Сейчас 14 часов
Время суток: День
```

**Решение:**
```java
public class TimeOfDay {
    public static void main(String[] args) {
        int hour = 14;
        String timeOfDay;
        
        if (hour >= 6 && hour < 12) {
            timeOfDay = "Утро";
        } else if (hour >= 12 && hour < 18) {
            timeOfDay = "День";
        } else if (hour >= 18 && hour < 23) {
            timeOfDay = "Вечер";
        } else {
            timeOfDay = "Ночь";
        }
        
        System.out.println("Сейчас " + hour + " часов");
        System.out.println("Время суток: " + timeOfDay);
    }
}
```

---

## Задача 15: Проверка дня недели для скидки
**Условие:**
В кафе по понедельникам и вторникам действует скидка 20%.  День недели задан числом (1 — понедельник, 2 — вторник, и т.д.). Сегодня вторник (день = 2). Стоимость заказа — 800 рублей. Программа должна вывести стоимость заказа, процент скидки, итого к оплате
```
Стоимость заказа: 800 рублей
Сегодня действует скидка 20%!
Итого к оплате: 640.0 рублей
```

**Решение:**
```java
public class CafeDiscount {
    public static void main(String[] args) {
        int dayOfWeek = 2;
        int orderCost = 800;
        double finalCost;
        
        switch (dayOfWeek) {
            case 1:
            case 2:
                System.out.println("Стоимость заказа: " + orderCost + " рублей");
                System.out.println("Сегодня действует скидка 20%!");
                finalCost = orderCost * 0.8;
                System.out.println("Итого к оплате: " + finalCost + " рублей");
                break;
            default:
                System.out.println("Стоимость заказа: " + orderCost + " рублей");
                System.out.println("Сегодня скидок нет");
                System.out.println("Итого к оплате: " + orderCost + " рублей");
        }
    }
}
```

---




---

## Задача 17: Расчёт стоимости такси
**Условие:**
Такси берёт 50 рублей за посадку + 15 рублей за каждый километр. Если расстояние больше 10 км, применяется скидка 10% на всю поездку. Расстояние — 12 км. Программа должна вывести: расстояние, стоимость поездки, размер скидки, итого к оплате
```
Расстояние: 12 км
Базовая стоимость: 230.0 рублей
Скидка 10% применена
Итого к оплате: 207.0 рублей
```

**Решение:**
```java
public class TaxiCalculator {
    public static void main(String[] args) {
        int baseFare = 50;
        int pricePerKm = 15;
        int distance = 12;
        
        double totalCost = baseFare + (pricePerKm * distance);
        
        System.out.println("Расстояние: " + distance + " км");
        System.out.println("Базовая стоимость: " + totalCost + " рублей");
        
        if (distance > 10) {
            totalCost = totalCost * 0.9;
            System.out.println("Скидка 10% применена");
        }
        
        System.out.println("Итого к оплате: " + totalCost + " рублей");
    }
}
```

---


---

## Задача 18: Расчёт индекса массы тела (ИМТ)
**Условие:**
ИМТ рассчитывается по формуле: вес / (рост × рост), где вес в кг, рост в метрах. Значения ИМТ: меньше 18.5 — недостаточный вес, 18.5-25 — нормальный вес, 25-30 — избыточный вес, больше 30 — ожирение. Вес — 70 кг, рост — 1.75 м. Программа должна вывести вес, рост, значение имт, и результат имт
```
Вес: 70 кг
Рост: 1.75 м
ИМТ: 22.857142857142858
Категория: Нормальный вес
```

**Решение:**
```java
public class BMICalculator {
    public static void main(String[] args) {
        int weight = 70;
        double height = 1.75;
        double bmi = weight / (height * height);
        String category;
        
        if (bmi < 18.5) {
            category = "Недостаточный вес";
        } else if (bmi < 25) {
            category = "Нормальный вес";
        } else if (bmi < 30) {
            category = "Избыточный вес";
        } else {
            category = "Ожирение";
        }
        
        System.out.println("Вес: " + weight + " кг");
        System.out.println("Рост: " + height + " м");
        System.out.println("ИМТ: " + bmi);
        System.out.println("Категория: " + category);
    }
}
```

---

## Дополнительная Задача : Расчёт стоимости доставки
**Условие:**
Служба доставки берёт 200 рублей за доставку по городу и 50 рублей за каждый килограмм веса. Если вес больше 10 кг, за каждый килограмм сверх 10 кг берут только 30 рублей. Вес посылки — 15 кг. Программа должна вывести: Вес, базовую стоимость доставки, стоимость за первые 10 кг, стоимость за дополнительные кг, итого к оплате
```
Вес посылки: 15 кг
Базовая стоимость доставки: 200 рублей
Стоимость за первые 10 кг: 500 рублей (50 * 10)
Стоимость за дополнительные 5 кг: 150 рублей (30 * 5)
Итого: 850 рублей
```

**Решение:**
```java
public class DeliveryCalculator {
    public static void main(String[] args) {
        int weight = 15;
        int baseCost = 200;
        int totalCost = baseCost;
        
        System.out.println("Вес посылки: " + weight + " кг");
        System.out.println("Базовая стоимость доставки: " + baseCost + " рублей");
        
        if (weight <= 10) {
            int weightCost = weight * 50;
            totalCost = totalCost + weightCost;
            System.out.println("Стоимость за вес: " + weightCost + " рублей (50 * " + weight + ")");
        } else {
            int firstPartCost = 10 * 50;
            int extraWeight = weight - 10;
            int extraWeightCost = extraWeight * 30;
            totalCost = totalCost + firstPartCost + extraWeightCost;
            System.out.println("Стоимость за первые 10 кг: " + firstPartCost + " рублей (50 * 10)");
            System.out.println("Стоимость за дополнительные " + extraWeight + " кг: " + extraWeightCost + " рублей (30 * " + extraWeight + ")");
        }
        
        System.out.println("Итого: " + totalCost + " рублей");
    }
}
```

---

## Дополнительная Задача: Выбор напитка в кафе
**Условие:**
В меню кафе есть напитки: 1 — Эспрессо (100 руб), 2 — Капучино (150 руб), 3 — Латте (180 руб), 4 — Чай (80 руб). Используйте switch. Клиент выбрал напиток под номером 3. Программа должна вывести какой напиток выбрал клиент, и сколько он должен заплатить
```
Вы выбрали: Латте
Стоимость: 180 рублей
```

**Решение:**
```java
public class CafeDrink {
    public static void main(String[] args) {
        int drinkNumber = 3;
        String drinkName;
        int price;
        
        switch (drinkNumber) {
            case 1:
                drinkName = "Эспрессо";
                price = 100;
                break;
            case 2:
                drinkName = "Капучино";
                price = 150;
                break;
            case 3:
                drinkName = "Латте";
                price = 180;
                break;
            case 4:
                drinkName = "Чай";
                price = 80;
                break;
            default:
                drinkName = "Неизвестный напиток";
                price = 0;
        }
        
        System.out.println("Вы выбрали: " + drinkName);
        System.out.println("Стоимость: " + price + " рублей");
    }
}
```