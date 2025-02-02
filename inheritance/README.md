# Домашнее задание к занятию «Наследование и расширяемость систем. Проблемы наследования»

В качестве результата пришлите ссылки на ваши GitHub-проекты в личном кабинете студента на сайте [netology.ru](https://netology.ru).

Все задачи этого занятия можно делать в одном репозитории.

**Важно**: если у вас что-то не получилось, то оформляйте Issue [по установленным правилам](../report-requirements.md).

Вы можете делать все задачи этого занятия в одном репозитории (если делаете их в разных ветках).

Напоминалку по некоторым теоретическим моментам в джаве вы можете найти [здесь](../tips/tips.md).

## Как сдавать задачи

1. Ознакомьтесь с [особенностями Lombok при использовании наследования](../extra/lombok/inheritance.md)
1. Ознакомьтесь с доп.материалами о [`static`](../extra/static.md) и [`reflection`](../extra/reflection.md)
1. Инициализируйте на своём компьютере пустой Git-репозиторий
1. Добавьте в него готовый файл [.gitignore](../.gitignore)
1. Добавьте в этот же каталог необходимые файлы (`pom.xml`)
1. Сделайте ветки для первой задачи (после того, как сделаете первую задачу, на её основе создайте ветку для второй задачи)
1. Создайте публичный репозиторий на GitHub и свяжите свой локальный репозиторий с удалённым
1. Сделайте пуш (удостоверьтесь, что ваш код и обе ветки появились на GitHub)
1. Ссылку на ваш проект отправьте в личном кабинете на сайте [netology.ru](https://netology.ru)
1. Задачи, отмеченные как необязательные, можно не сдавать, это не повлияет на получение зачета

## Задача №1 - "Менеджер Товаров"

На основании [проекта из лекции](https://github.com/netology-code/javaqa-code/tree/master/3.5_inheritance/products) необходимо реализовать менеджер товаров, который умеет:

1. Добавлять товары в репозиторий
1. Искать товары

Что нужно сделать:
1. Разработайте базовый класс `Product`, содержащий `id`, название, стоимость
1. Разработать два унаследованных от `Product` класса: `Book` (с полями название* и автор) и `Smartphone` (с полями название* и производитель)
1. Разработайте репозиторий, позволяющий сохранять `Product`'ы, получать все сохранённые `Product`'ы и удалять по `id`. Для этого репозиторий будет хранить у себя поле с типом `Product[]` (массив товаров).
1. Разработайте менеджера, который умеет добавлять `Product`'ы в репозиторий и осуществлять поиск по ним. Для этого вам нужно создать класс, конструктор которого будет принимать параметром репозиторий, а также с методом `publid void add(Product product)` и методом поиска (см. ниже).

Примечание*: надеемся, вы догадались, что название итак уже есть в классе `Product` и будет наследовано от него в классах `Book` и `Smartphone`

#### Как осуществлять поиск

У менеджера должен быть метод `searchBy(String text)`, который возвращает массив найденных товаров 

```java
public class ProductManager {
  // добавьте необходимые поля, конструкторы и методы

  public Product[] searchBy(String text) {
    // ваш код
  }

  public boolean matches(Product product, String search) {
    if (product instanceof Book) { // если в параметре product лежит объект класса Book
      Book book = (Book) product; // положем его в переменную типа Book чтобы пользоваться методами класса Book
      if (product.getAuthor().contains(search)) { // проверим есть ли поисковое слово в данных об авторе
        return true;
      }
      if (product.getTitle().contains(search)) {
        return true;
      }
      return false;
    }
    ...
    return false;
  }
}
```

Менеджер при переборе всех продуктов, хранящихся в массиве (или в репозитории, если вы в предыдущих ДЗ сделали репозиторий)*, должен для каждого продукта вызывать определённый в классе менеджера же метод `matches`, который проверяет, соответствует ли продукт поисковому запросу.

Примечание*: если вы сделали репозиторий, то пусть менеджер забирает из репозитория все товары и сам уже по ним ищет.

При проверке на соответствие запросу книги мы проверяем по полям названия и автора, для смартфона по полям названия и производителя.

<details>
  <summary>Подсказка</summary>
  
```java
public class ProductManager {
  // добавьте необходимые поля, конструкторы и методы

  public Product[] searcyBy(String text) {
    Product[] result = new Product[0];
    for (Product product: repository.findAll()) {
      if (matches(product, text)) {
        Product[] tmp = new Product[result.length + 1];
        // используйте System.arraycopy, чтобы скопировать всё из result в tmp
        tmp[tmp.length - 1] = product;
        result = tmp;
      }
    }
    return result;
  }

  public boolean matches(Product product, String search) {
    // ваш код
  }
}
```
</details>

Требования к проекту:
1. Создайте ветку (не делайте ДЗ в `master`!)
1. Подключите плагин Surefire так, чтобы сборка падала в случае отсутсвия тестов
1. Подключите плагин JaCoCo в режиме генерации отчётов (обрушать сборку по покрытию не нужно)
1. Реализуйте нужные классы и методы
1. Напишите автотесты на метод поиска (только на метод поиска в менеджере), добившись 100% покрытия по branch'ам* (вспомните, что мы говорили про тестирование методов, возвращающих набор значений)
1. Подключите CI на базе Github Actions и выложите всё на Github

Примечание*: использовать Mockito или нет, мы оставляем на ваше усмотрение.

Итого: у вас должен быть репозиторий на GitHub, в котором расположен ваш Java-код в ветке (в `master` должен быть только `pom.xml`).

## Задача №2 - "Менеджер Товаров" (Rich Model)*

**Важно**: это необязательная задача. Её (не)выполнение не влияет на получение зачёта по ДЗ.

### Легенда

Достаточно часто объекты, моделирующие предметную область, называют моделями. Достаточно часто модели делают "глупыми", т.е. не содержащими никакой логики.

Но есть и другой подход, который позволяет делать "умные" или "богатые" модели (Rich Model), которые могут уже содержать определённую логику.

Что нужно сделать:
1. Создайте новую ветку на базе ветки, в которой вы решали первую задачу
1. Реализуйте в классе `Product` метод `public boolean matches(String search)`, который определяет, подходит ли продукт поисковому запросу исходя из названия
1. Переопределите этот метод в дочерних классах, чтобы они сначала вызывали родительский метод и только если родительский метод вернул `false`, тогда проводили доп.проверки (`Book` - по автору, `Smartphone` - по производителю).
1. Уберите из менеджера все `instanceof` и метод `matches`, т.к. теперь у вас "умные" модели и благодаря переопределению методов вам этот код больше не нужен
1. Зато теперь вам нужны unit-тесты на методы ваших умных моделей (напишите их)
1. Удостоверьтесь, что ранее написанные тесты на менеджера (из решения задачи 1) проходят
 
Итого: у вас должен быть репозиторий на GitHub, в котором расположен ваш Java-код в двух ветках (в `master` должен быть только `pom.xml`).

<details>
  <summary>Подсказка №1</summary>
  
```java
public class ProductManager {
  // добавьте необходимые поля, конструкторы и методы

  public Product[] searcyBy(String text) {
    Product[] result = new Product[0];
    for (Product product: repository.findAll()) {
      if (product.matches(text)) {
        Product[] tmp = new Product[result.length + 1];
        // используйте System.arraycopy, чтобы скопировать всё из result в tmp
        tmp[tmp.length - 1] = product;
        result = tmp;
      }
    }
    return result;
  }
}
```
</details>

<details>
  <summary>Подсказка №2 (про оператор ||)</summary>
  
  У нас есть замечательный логический оператор `||`, который работает следующим образом: вычисляет правую часть выражения только в случае, если левая равна `false`
  
```java
public class Book {
  // ваши поля, конструкторы, методы
  public boolean matches(String search) {
    return super.matches(search) || ... ваше выражение ...;
  }
}
```

Никто не говорит, что этот вариант лучше вот этого (наоборт, этот легче тестировать и отлаживать):
```java
public class Book {
  // ваши поля, конструкторы, методы
  public boolean matches(String search) {
    if (super.matches(search)) {
      return true;
    }
    return ... ваше выражение ...;
  }
}
```

Но вы должны знать оба варианта.
</details>
