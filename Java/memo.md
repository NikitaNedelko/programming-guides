## **Присвоение имени методу** 
По соглашению, имена методов должны **начинается с глагола** в нижнем регистре, за которым следуют прилагательные, существительные и т.д.
> run
> runFast
> getBackground
> getFinalData
> compareTo
> setX
> isEmpty

## **Использование символов подчеркивания в числовых литералах**

В Java SE 7 и более поздних версиях любое количество символов подчеркивания (`_`) может отображаться в любом месте между цифрами в числовом литерале. Эта функция позволяет, например. для разделения групп цифр в числовых литералах, что может улучшить читаемость вашего кода.

> long creditCardNumber = 1234_5678_9012_3456L;
> long socialSecurityNumber = 999_99_9999L;
> float pi =  3.14_15F;

## **continue test;**

```
if (a == 10)
	test: for (int i = 0; i < 9; i++) {
		if (i == 3) continue test;
     } 
```


## **Использование `this` с конструктором**

```
public Rectangle() {
        this(0, 0, 1, 1);
    }
    public Rectangle(int width, int height) {
        this(0, 0, width, height);
    }
    public Rectangle(int x, int y, int width, int height) {
        this.x = x;
        this.y = y;
        this.width = width;
        this.height = height;
    }
```

## `implements`, `interface`, `extended`

> `Определение интерфейса DataStructureIterator, который расширяет интерфейс Iterator<Integer>.`
> `Это означает, что DataStructureIterator унаследует все методы из Iterator<Integer>.`
```
interface DataStructureIterator extends java.util.Iterator<Integer> { }
```

> `Внутренний класс EvenIterator реализует интерфейс DataStructureIterator.`
> `Он также реализует все методы из интерфейса Iterator<Integer>, поскольку DataStructureIterator является его расширением.`
```
private class EvenIterator implements DataStructureIterator {
}
```
> `Реализация методов интерфейса DataStructureIterator (и, следовательно, Iterator<Integer>)`


## **лямбда функции и анонимные классы**


> Используем анонимный класс
```
printPersonsWithPredicate(roster, new Predicate<Person>() {
    public boolean test(Person p) {
        return p.getAge() > 21;
    }
});
```

> Используем лямбда-выражение (предпочтительный способ)
```
printPersonsWithPredicate(roster, p -> p.gender == Person.Sex.FEMALE);
```

### Полный код для лямбда 

```
import java.time.LocalDate;
import java.time.Period;
import java.util.ArrayList;
import java.util.List;

public class HelloWorld {

    interface Predicate<Person> {
        boolean test(Person p);
    }

    static void printPersonsWithPredicate(List<Person> roster, Predicate<Person> tester) {
        for (Person p : roster) {
            if (tester.test(p))
                p.printPerson();
        }
    }

    static class Person {

        public enum Sex {
            MALE, FEMALE
        }

        String name;
        LocalDate birthday;
        Sex gender;
        String emailAddress;

        Person(String name, LocalDate birthday, Sex gender, String emailAddress) {
            this.name = name;
            this.birthday = birthday;
            this.gender = gender;
            this.emailAddress = emailAddress;
        }

        public int getAge() {
            return Period.between(birthday, LocalDate.now()).getYears();
        }

        public void printPerson() {
            System.out.println(name + ", " + getAge() + " years old, " + gender);
        }

    }

    public static void main(String[] args) {
        List<Person> roster = new ArrayList<>();
        roster.add(new Person("John", LocalDate.of(2000, 1, 15), Person.Sex.MALE, "john@example.com"));
        roster.add(new Person("Jane", LocalDate.of(1995, 5, 20), Person.Sex.FEMALE, "jane@example.com"));
        roster.add(new Person("Paul", LocalDate.of(2004, 8, 30), Person.Sex.MALE, "paul@example.com"));
        roster.add(new Person("Alice", LocalDate.of(2002, 11, 10), Person.Sex.FEMALE, "alice@example.com"));

        printPersonsWithPredicate(
                roster,
                p -> p.gender == Person.Sex.MALE
                        && p.getAge() >= 18
                        && p.getAge() <= 25);
    }
}

```


## **generics class или обобщенные классы** 

> class
```
public class Box<T> {
    private T name;

    public void set(T name) {
        this.name = name;
    }

    public T get() {
        return name;
    }
}
```

> main 
```
Box<String> stringBox = new Box<>();
stringBox.set("Hello");
System.out.println(stringBox.get());

Box<Integer> integerBox = new Box<>();
integerBox.set(123);
System.out.println(integerBox.get());

```