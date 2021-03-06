# Домашнее задание к занятию 4.4. InMemory-хранение данных - коллекция HashMap

Необходимо выполнить и предоставить на проверку следующие задачи:

## Задача № 1

А почему бы не написать нам программу для учета товаров на складах?
Основная цель: по серийному номеру товара узнать, на каком складе он хранится.

И так у нас есть:
* Товар: номер (уникальный), название, цена;
* Склад: имя (уникальное), адрес;
* Сопоставление идентификатора товара с самим товаром;
* Сопоставление идентификатора склада со складом;
* Сопоставление на каком складу лежит товар.

Из функциональных требований к программе реализуем:
* Добавление товара;
* Добавление склада;
* Нахождение товара по номеру (вывод информации о товаре и складе, на котором он находится);
* Перевозка товара с одного склада на другой.

### Реализация

1. Создадим классы Product и Storage.

1. Для корректной работы коллекций HashMap и HashSet необходимо переопределить функции `hashCode()` и `equals()` в используемых классах. Так как у товара и у склада есть поля с уникальными значениями (номер у товара и имя у склада), сделать это будет легко! Просто воспользуемся реализацией hashCode и equals этих полей.

Например:
```java
@Override
public boolean equals(Object obj) {
  return name.equals(obj);
}

@Override
public int hashCode() {
  return name.hashCode();
}
```

2. Следующим шагом создадим функции для считывания сущности с клавиатуры, например для `Product`:

```java
private String readProductId(Scanner scanner) {
  System.out.println("Введите индентификатор продукта:");
  String id = scanner.nextLine();
  return id;
}
private Product readProduct(String id, Scanner scanner) {
  System.out.println("Введите имя продукта:");
  String name = scanner.nextLine();
  // цена с проверкой корректности - неотрицательное число
  return new Product(id, name, price);
}
```

Аналогичным образом создаются функции для ввода имени склада и ввода всей сущности с клавиатуры.

Для вывода объекта на экран переопределим метод `toString()`.

3. Далее необходимо реализовать логику заполнения нашей базы данных:
   1) Запрашиваем ввод идентификатора товара:
      * Если введен несуществующий идентификатор, выдать сообщение "товар %s не найден, создать новый с данным идентификатором?", при положительном ответе считать товар с клавиатуры.
      * Если введен существующий идентификатор, вывести информацию о товаре и предложить сменить склад (см. следующий пункт).
   2) После создания / при редактировании товара спросить на каком складе он находится:
      * Если введено имя существующего склада, переложить его туда.
      * Если введено неизвестное имя склада, предложить его создать по аналогии с товаром.
   3) Повторить с первого пункта до тех пор, пока не будет введен `end`.

4. Для демонстрации работы программы создадим в коде при запуске программы несколько складов (~2) и разложим по ним несколько товаров (~3).

## Задача 2

Эта задача про телефонный справочник с поддержкой групп контактов. Один контакт может входить в несколько групп, например, если вы работаете со своим другом, то он будет в группах Друзья и Работа. У пользователя должна быть возможность создания групп и контактов, добавления контакта в несколько групп. Ну и конечно же наш список контактов отсортирован в алфавитном порядке.

1. Вам нужно создать класс `Contact`:
    * Имя
    * Номер телефона

2. Переопределить методы `hashCode` и `equals` для  корректной работы HashSet. Также для вывода контакта на экран переопределить метод `toString`.

3. По аналогии с первой задачей создать методы для ввода `Contact` с клавиатуры. 

4. Для того чтобы наш список контактов был отсортирован по имени, необходимо чтобы класс `Contact` реализовывал интерфейс `Comparable`. C похожей задачей мы уже сталкивались на лекции по одномерным массивам.

5. Создадим `List<Contact>`, который будем всегда поддерживать отсортированным по имени. Он нам пригодиться если пользователь захочет просмотреть все контакты в алфавитном порядке. Для этого перед добавлением очередного элемента в список, будем находить для него правильную позицию, при которой имя предыдущего будет меньше, а имя следующего больше. 
Так как список у нас все время будет отсортированный, то эффективнее всего искать позицию для элемента с помощью [бинарного поиска](https://wikipedia.org/wiki/Двоичный_поиск). 
В  Java этот алгоритм уже реализован в `Collections.binarySearch(list, key)`: 
* если элемент содержится в списке, то метод вернет его позицию, 
* если элемента в коллекции нет, то метод вернет место, куда он должен быть вставлен. 
Можно воспользоваться им или написать свою реализацию.

> На следующих лекциях мы будем рассматривать структуры данных которые "из коробки" позволяют хранить данные отсортированными. Например класс TreeSet.

6. Далее создадим HashMap, где ключом будет имя группы, а значением множество HashSet контактов в этой группе (в группе может быть несколько контактов, но каждый контакт может встречаться в этой группе только один раз).

7. Логика работы приложения. На выбор пользователю выводится несколько действий:
    1. Просмотреть все контакты в алфавитном порядке.
    2. Просмотреть все группы с содержащимися контактами (см. пример).
    3. Добавить новый контакт.
    4. При создании нового контакта приложение предлагает добавить его в группы. Если введенной группы ещё не существует – она создается автоматически.

Пример отображения списка групп:
```
Семья:
  Мама +70000000000
  Папа +70000000001
  Я +70000000002

Работа:
  Саня +70000000003

Друзья:
  Саня +70000000003
```

8. Для демонстрации работы программы в коде создайте несколько контактов и добавьте в несколько групп.

> Так как со временем наши задачи становятся всё объёмнее, пора бы задуматься об архитектуре нашего приложения:
> Как можно больше логики выносите из функции main в небольшие, узкоспециализированные функции! Так будет намного легче писать/читать/поддерживать код.

## Задача 2`*`

В дополнение к возможным действиям задачи № 2 добавьте возможность редактирования контакта (как его полей, так и групп в которые он входит).

## Задача 3

Итак, самая объемная и сложная задача позади. Открылось второе дыхание, и вы готовы ринуться выполнять третью задачу!

Необходимо написать простейшего поискового паука. Это робот, который ходит с некоторой периодичностью по сайтам, внесенным для индексации, и заносит информацию в базу данных поисковика.
В нашем случае сайты будут заданы в виде Map<String, String> – сопоставления адреса сайта и его содержимого. Поисковый запрос будет состоять из одного или нескольких слов, разделенных пробелом.

Процесс реализации заключается в создании структуры данных, которая будет хранить сопоставление адреса сайта с сопоставлением слов на этом сайте с их количеством (да, вложенное сопоставление). При запросе мы будем выдавать сайты с наибольшим числом повторений искомых слов.

1. Создадим и инициализируем наши "сайты" в коде. 
```java 
Map<String, String> sites = new HashMap<String, String>();
sites.add("pikabu.ru", "Котята занялись рукоделием, посмотрите. #пятничное #моё #котята");
sites.add("vk.com", "Бизнес цитаты и поцанский паблик. Заработал МЕЛЛИОН за день в 16 лет.");
sites.add("rutracker.org", "Доступ запрещен. Слава Роскомнадзору!");
// ...
```

2. Создадим новую структуру `Map<String, Map<String, Integer>>`.

3. Проходя по каждому элементу sites, будем создавать элемент в структуре из шага `2` и заполнять его значениями: 
   1. приводить весь текст в нижний регистр `string.toLowerCase()`;
   2. убрать все знаки пунктуации `string.replaceAll("\\p{P}", "")`
   3. текст сайта разобьем на массив слов с помощью функции `string.split(" ")`;
   4. для каждого слова, если его нет в map будем добавлять его со значением 1. Если слово там уже находится увеличивать значение на 1.

4. После того как сайты проиндексированы. Будем считывать поисковые запросы от пользователя до тех пор, пока он не введет `\exit`.

5. Строку запроса также переведём в нижний регистр и разобьём на слова.

6. Затем создадим `Map<String, Integer>`, в котором будем хранить в качестве ключа адрес сайта, а в качестве значения — сколько раз встречались искомые слова на данном сайте.

7. Проходим по `map` из пункта 6 и выводим пользователю сайты в порядке релевантности.

Пример запроса: `Бизнес котята`. Для сайтов которые мы ввели в пункте 1 должен получиться примерно такой результат:
```
pikabu.ru котята 2
vk.com бизнес 1
```

> Также как и в предыдущей задаче советуем как можно больше логики вынести из функции `main` в другие небольшие функции.

## Задача 3`**` _с двумя звездочками B-)_

Реализовать сортировку с помощью PriorityQueue и Comparator.

> Подсказка: для каждого запроса будет создаваться новая queue (в неё передастся наша структура данных из шага 2) и новый comparator (в конструкторе принимает слова из запроса).  

=======

Все задачи обязательны к выполнению для получения зачета, кроме задач со `*`. Присылать на проверку можно каждую задачу по отдельности или все задачи вместе. Во время проверки по частям ваша домашняя работа будет со статусом "На доработке".

Любые вопросы по решению задач задавайте в группе на Facebook.

## Инструкция по выполнению домашнего задания

1. Код пишите в IDE (рекомендуется Intellij Idea), а на сайт только вставьте для отправки!
    1. Почему IDE? Быстрота, подсветка ошибок, отладка по шагам.
    2. Почему Intellij Idea? Родитель Android Studio, бесплатная, умная, лучшая IDE во всей мультивселенной.
3. Опирайтесь на принятый [стиль оформления кода](https://github.com/netology-code/codestyle/blob/master/java/README.md).
4. Зарегистрируйтесь на сайте [Repl.IT](http://repl.it/).
5. Перейдите в раздел **my repls**.
6. Нажмите кнопку **Start coding now!**, если приступаете впервые, или **New Repl**, если у вас уже есть работы.
7. В списке языков выберите `Java`.
8. Убедитесь что на сайте всё работает также как и у вас на компьютере, нажав на кнопку **Run**. Результат появится в правой части окна.
9. После окончания работы нажмите кнопку **Share** и скопируйте ссылку из поля Share link.
10. В личном кабинете на сайте [netology.ru](http://netology.ru/) в поле комментария к домашней работе вставьте скопированную ссылку и отправьте работу на проверку.

*Никаких файлов прикреплять не нужно.*
