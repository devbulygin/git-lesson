# Задачи для семинара по Java

## Задача 1: Калькулятор чаевых в ресторане
**Условие:**
Вы работаете официантом в ресторане. Напишите программу, которая рассчитывает размер чаевых. Сумма счёта — 1250 рублей. Если сумма счёта больше 1000 рублей, чаевые составляют 15%, иначе — 10%. Программа должна вывести:
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

## Задача 2: Проверка чётности номера места в автобусе
**Условие:**
В автобусе места с чётными номерами находятся у окна, с нечётными — у прохода. Напишите программу, которая проверяет, находится ли место №27 у окна или у прохода. Используйте оператор остатка от деления. Программа должна вывести:
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

## Задача 3: Конвертер температуры
**Условие:**
Вы разрабатываете приложение для метеостанции. Температура измеряется в градусах Цельсия, но нужно отображать её в Фаренгейтах. Формула: F = C × 1.8 + 32. Температура сейчас 25°C. Программа должна вывести:
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

## Задача 4: Калькулятор скидки в магазине
**Условие:**
В магазине электроники проходит акция: при покупке на сумму от 10000 рублей — скидка 10%, от 20000 рублей — скидка 15%, от 30000 рублей — скидка 20%. Покупка составила 25000 рублей. Программа должна вывести:
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

## Задача 5: Определение времени суток
**Условие:**
Вы разрабатываете умный будильник. Нужно определить время суток по часам: с 6 до 12 — утро, с 12 до 18 — день, с 18 до 23 — вечер, с 23 до 6 — ночь. Сейчас 14 часов. Программа должна вывести:
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

## Задача 6: Проверка дня недели для скидки
**Условие:**
В кафе по понедельникам и вторникам действует скидка 20%. Используйте оператор switch. День недели задан числом (1 — понедельник, 2 — вторник, и т.д.). Сегодня вторник (день = 2). Стоимость заказа — 800 рублей. Программа должна вывести:
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

## Задача 7: Банкомат - выдача купюр
**Условие:**
Банкомат выдаёт деньги только купюрами по 100 рублей. Клиент хочет снять 1750 рублей. Определите, сколько купюр можно выдать и какую сумму не удастся выдать (остаток). Программа должна вывести:
```
Запрошенная сумма: 1750 рублей
Выдано купюр по 100 рублей: 17
Выдано денег: 1700 рублей
Остаток (не выдан): 50 рублей
```

**Решение:**
```java
public class ATM {
    public static void main(String[] args) {
        int requestedAmount = 1750;
        int billValue = 100;
        int numberOfBills = requestedAmount / billValue;
        int dispensedAmount = numberOfBills * billValue;
        int remainder = requestedAmount % billValue;
        
        System.out.println("Запрошенная сумма: " + requestedAmount + " рублей");
        System.out.println("Выдано купюр по 100 рублей: " + numberOfBills);
        System.out.println("Выдано денег: " + dispensedAmount + " рублей");
        System.out.println("Остаток (не выдан): " + remainder + " рублей");
    }
}
```

---

## Задача 8: Определение сезона года
**Условие:**
По номеру месяца определите сезон года. Используйте switch. Месяцы: 12, 1, 2 — зима; 3, 4, 5 — весна; 6, 7, 8 — лето; 9, 10, 11 — осень. Сейчас месяц 7 (июль). Программа должна вывести:
```
Месяц: 7
Сезон: Лето
```

**Решение:**
```java
public class SeasonDetector {
    public static void main(String[] args) {
        int month = 7;
        String season;
        
        switch (month) {
            case 12:
            case 1:
            case 2:
                season = "Зима";
                break;
            case 3:
            case 4:
            case 5:
                season = "Весна";
                break;
            case 6:
            case 7:
            case 8:
                season = "Лето";
                break;
            case 9:
            case 10:
            case 11:
                season = "Осень";
                break;
            default:
                season = "Неизвестный месяц";
        }
        
        System.out.println("Месяц: " + month);
        System.out.println("Сезон: " + season);
    }
}
```

---

## Задача 9: Подсчёт калорий в заказе
**Условие:**
В ресторане быстрого питания: бургер — 500 калорий, картошка фри — 350 калорий, газировка — 150 калорий. Клиент заказал 2 бургера, 1 порцию картошки и 1 газировку. Программа должна вывести:
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

---

## Задача 10: Расчёт стоимости такси
**Условие:**
Такси берёт 50 рублей за посадку + 15 рублей за каждый километр. Если расстояние больше 10 км, применяется скидка 10% на всю поездку. Расстояние — 12 км. Программа должна вывести:
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

## Задача 11: Определение високосного года
**Условие:**
Год является високосным, если он делится на 4 без остатка, НО не делится на 100, ИЛИ делится на 400. Проверьте год 2024. Программа должна вывести:
```
Год: 2024
Високосный год: да
```

**Решение:**
```java
public class LeapYear {
    public static void main(String[] args) {
        int year = 2024;
        boolean isLeap = (year % 4 == 0 && year % 100 != 0) || (year % 400 == 0);
        
        System.out.println("Год: " + year);
        System.out.println("Високосный год: " + (isLeap ? "да" : "нет"));
    }
}
```

---

## Задача 12: Расчёт индекса массы тела (ИМТ)
**Условие:**
ИМТ рассчитывается по формуле: вес / (рост × рост), где вес в кг, рост в метрах. Значения ИМТ: меньше 18.5 — недостаточный вес, 18.5-25 — нормальный вес, 25-30 — избыточный вес, больше 30 — ожирение. Вес — 70 кг, рост — 1.75 м. Программа должна вывести:
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

## Задача 13: Калькулятор стоимости парковки
**Условие:**
Парковка стоит 100 рублей за первый час, 80 рублей за второй час и 60 рублей за каждый последующий час. Машина простояла 5 часов. Программа должна вывести:
```
Время парковки: 5 часов
Стоимость:
1 час: 100 рублей
2 час: 80 рублей
3-5 часы: 180 рублей (60 * 3)
Итого: 360 рублей
```

**Решение:**
```java
public class ParkingCalculator {
    public static void main(String[] args) {
        int hours = 5;
        int totalCost = 0;
        
        System.out.println("Время парковки: " + hours + " часов");
        System.out.println("Стоимость:");
        
        if (hours >= 1) {
            totalCost = totalCost + 100;
            System.out.println("1 час: 100 рублей");
        }
        
        if (hours >= 2) {
            totalCost = totalCost + 80;
            System.out.println("2 час: 80 рублей");
        }
        
        if (hours > 2) {
            int additionalHours = hours - 2;
            int additionalCost = additionalHours * 60;
            totalCost = totalCost + additionalCost;
            System.out.println("3-5 часы: " + additionalCost + " рублей (60 * " + additionalHours + ")");
        }
        
        System.out.println("Итого: " + totalCost + " рублей");
    }
}
```

---

## Задача 14: Проверка треугольника
**Условие:**
Проверьте, может ли существовать треугольник со сторонами a, b, c. Условие: сумма любых двух сторон должна быть больше третьей стороны. Стороны: a = 5, b = 7, c = 10. Программа должна вывести:
```
Стороны: 5, 7, 10
Может ли существовать треугольник: да
```

**Решение:**
```java
public class TriangleValidator {
    public static void main(String[] args) {
        int a = 5;
        int b = 7;
        int c = 10;
        
        boolean isValid = (a + b > c) && (a + c > b) && (b + c > a);
        
        System.out.println("Стороны: " + a + ", " + b + ", " + c);
        System.out.println("Может ли существовать треугольник: " + (isValid ? "да" : "нет"));
    }
}
```

---

## Задача 15: Выбор тарифа мобильной связи
**Условие:**
У оператора связи есть тарифы: 1 — "Базовый" (300 руб/мес), 2 — "Комфорт" (500 руб/мес), 3 — "Премиум" (800 руб/мес). Используйте switch для выбора тарифа. Клиент выбрал тариф 2. Программа должна вывести:
```
Выбран тариф: Комфорт
Стоимость: 500 рублей в месяц
```

**Решение:**
```java
public class MobilePlan {
    public static void main(String[] args) {
        int planNumber = 2;
        String planName;
        int cost;
        
        switch (planNumber) {
            case 1:
                planName = "Базовый";
                cost = 300;
                break;
            case 2:
                planName = "Комфорт";
                cost = 500;
                break;
            case 3:
                planName = "Премиум";
                cost = 800;
                break;
            default:
                planName = "Неизвестный";
                cost = 0;
        }
        
        System.out.println("Выбран тариф: " + planName);
        System.out.println("Стоимость: " + cost + " рублей в месяц");
    }
}
```

---

## Задача 16: Определение категории водительских прав
**Условие:**
Определите категорию прав по типу транспорта: 'A' — мотоцикл, 'B' — легковой автомобиль, 'C' — грузовой автомобиль, 'D' — автобус. Используйте switch. Категория — 'C'. Программа должна вывести:
```
Категория прав: C
Тип транспорта: Грузовой автомобиль
```

**Решение:**
```java
public class DriverLicense {
    public static void main(String[] args) {
        char category = 'C';
        String vehicleType;
        
        switch (category) {
            case 'A':
                vehicleType = "Мотоцикл";
                break;
            case 'B':
                vehicleType = "Легковой автомобиль";
                break;
            case 'C':
                vehicleType = "Грузовой автомобиль";
                break;
            case 'D':
                vehicleType = "Автобус";
                break;
            default:
                vehicleType = "Неизвестная категория";
        }
        
        System.out.println("Категория прав: " + category);
        System.out.println("Тип транспорта: " + vehicleType);
    }
}
```

---

## Задача 17: Расчёт стоимости доставки
**Условие:**
Служба доставки берёт 200 рублей за доставку по городу и 50 рублей за каждый килограмм веса. Если вес больше 10 кг, за каждый килограмм сверх 10 кг берут только 30 рублей. Вес посылки — 15 кг. Программа должна вывести:
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

## Задача 18: Выбор напитка в кафе
**Условие:**
В меню кафе есть напитки: 1 — Эспрессо (100 руб), 2 — Капучино (150 руб), 3 — Латте (180 руб), 4 — Чай (80 руб). Используйте switch. Клиент выбрал напиток под номером 3. Программа должна вывести:
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

---

# Дополнительные задачи на проблемные темы

## Задача 19 (Инкремент и декремент): Счётчик посетителей магазина
**Условие:**
В магазин зашло 10 посетителей. Затем зашёл ещё 1 человек (используйте постфиксный инкремент), вышло 2 человека (используйте префиксный декремент дважды). Программа должна вывести текущее количество посетителей и значения счётчика после каждой операции. Программа должна вывести:
```
Начальное количество посетителей: 10
Зашёл посетитель (a++): значение до инкремента = 10, после = 11
Вышел посетитель (--a): значение после декремента = 10
Вышел посетитель (--a): значение после декремента = 9
Итого посетителей в магазине: 9
```

**Решение:**
```java
public class VisitorCounter {
    public static void main(String[] args) {
        int visitors = 10;
        System.out.println("Начальное количество посетителей: " + visitors);
        
        int temp = visitors++;
        System.out.println("Зашёл посетитель (a++): значение до инкремента = " + temp + ", после = " + visitors);
        
        System.out.println("Вышел посетитель (--a): значение после декремента = " + --visitors);
        System.out.println("Вышел посетитель (--a): значение после декремента = " + --visitors);
        
        System.out.println("Итого посетителей в магазине: " + visitors);
    }
}
```

---

## Задача 20 (Целочисленное деление): Распределение пиццы
**Условие:**
На вечеринке 23 человека. Каждая пицца разделена на 8 кусков. Сколько целых пицц нужно заказать, чтобы каждому досталось минимум по 1 куску? Используйте целочисленное деление. Программа должна вывести:
```
Количество гостей: 23
Кусков в одной пицце: 8
Минимально нужно пицц: 3
Всего кусков будет: 24
Останется кусков: 1
```

**Решение:**
```java
public class PizzaCalculator {
    public static void main(String[] args) {
        int guests = 23;
        int slicesPerPizza = 8;
        
        int pizzasNeeded = guests / slicesPerPizza;
        
        if (guests % slicesPerPizza != 0) {
            pizzasNeeded++;
        }
        
        int totalSlices = pizzasNeeded * slicesPerPizza;
        int leftoverSlices = totalSlices - guests;
        
        System.out.println("Количество гостей: " + guests);
        System.out.println("Кусков в одной пицце: " + slicesPerPizza);
        System.out.println("Минимально нужно пицц: " + pizzasNeeded);
        System.out.println("Всего кусков будет: " + totalSlices);
        System.out.println("Останется кусков: " + leftoverSlices);
    }
}
```

---

## Задача 21 (Остаток от деления): Проверка кратности для расписания смен
**Условие:**
В магазине работают 15 сотрудников. Их нужно распределить по сменам поровну. Смены бывают по 3, 4 или 5 человек. Проверьте, при каком размере смены все сотрудники будут распределены без остатка. Используйте оператор остатка от деления. Программа должна вывести:
```
Количество сотрудников: 15
Смена из 3 человек: делится без остатка
Смена из 4 человек: остаток 3
Смена из 5 человек: делится без остатка
Подходящие размеры смен: 3 и 5 человек
```

**Решение:**
```java
public class ShiftSchedule {
    public static void main(String[] args) {
        int employees = 15;
        
        System.out.println("Количество сотрудников: " + employees);
        
        int shift3 = employees % 3;
        if (shift3 == 0) {
            System.out.println("Смена из 3 человек: делится без остатка");
        } else {
            System.out.println("Смена из 3 человек: остаток " + shift3);
        }
        
        int shift4 = employees % 4;
        if (shift4 == 0) {
            System.out.println("Смена из 4 человек: делится без остатка");
        } else {
            System.out.println("Смена из 4 человек: остаток " + shift4);
        }
        
        int shift5 = employees % 5;
        if (shift5 == 0) {
            System.out.println("Смена из 5 человек: делится без остатка");
        } else {
            System.out.println("Смена из 5 человек: остаток " + shift5);
        }
        
        System.out.println("Подходящие размеры смен: 3 и 5 человек");
    }
}
```

---

**Всего: 21 задача** (18 основных — по 2 на студента + 3 дополнительные на проблемные темы)
