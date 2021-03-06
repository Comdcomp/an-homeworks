Домашнее задание к занятию 2.4. Основы ООП - абстракции и интерфейсы
==

## Задача 1. Библиотека
### Описание

Иерархия работников библиотеки. Реализовать совмещение нескольких ролей в библиотеке в одном исполнителе через интерфейсы. Каждый объект в программе умеет определенный набор действий. Часто программист, создающий объект, не представляет все ситуации, в которых тот будет использоваться. Также программисту, использующему объект, часто неизвестны все его детали. Для передачи информации, что должен уметь объект используются интерфейсы.

Примером интерфейсов в нашей библиотеке может служить понятие роли на проекте. Каждая роль подразумевает набор определенных операций, которые должен “уметь” объект User в программе.

Создайте Иерархию “Пользователи библиотеки” со следующими интерфейсами:
* Читатель – берет и возвращает книги.
* Библиотекарь – заказывает книги.
* Поставщик книг – приносит книги в библиотеку.
* Администратор – находит и выдает книги и уведомляет о просрочках времени возврата.

Учтите, что интерфейсы могут “совмещаться” на User-ах. Юзеры могут взаимодействовать другс другом (например библиотекарь заказывает у поставщика). Сделайте 2-3 примера классов, реализующих эти интерфейсы (реализовывать сами методы не обязательно).

### Реализация:
1. Создайте 4 интерфейса: `Reader`, `Librarian`, `Supplier`, `Administrator`. Каждый из них должен содержать описанные выше методы, например у администратора должен быть метод `overdueNotification(Reader reader)`. Методы могут принимать в качестве параметра других user-ов, например читатель берет книги у администратора, библиотекарь заказывает у поставщика и т.д.

2. Создайте несколько классов, демонстрирующих использование интерфесов. В часности, продемонстрируйте совмещение, например поставщик, который может быть также и читателем, библиотекарь – администратором и т.д. Реализация методов, описанных в интерфейсах не обязательна, но желательно продемонстрировать, как методы должны вызываться на объектах.

## Задача 2. Банковские счета
### Описание

Часто в проектировании программ нам удобно опираться на понятия, которые не представлены в реальном мире, но служат удобной “опорой” для объединения нескольких классов.

Так например в банковском деле нет абстрактного понятия “Счет”. Каждый счет в банке имеет четкое назначение -  Сберегательный, Кредитный, Расчетный. Но банковская программа работает с общими для счетов операциями как с одинаковыми объектами - и выполняет их, обращаясь к общему типу “Счет”, хотя его и невозможно явно инстанцировать в программе.
Реализуйте этот сценарий, опираясь на механизмы полиморфизма.

### Реализация
1. Создайте абстрактный класс `Account` с тремя методами: заплатить, перевести, пополнить (`pay(int amount)`, `transfer(Account account, int amount)`, `addMoney(int amount)`). Платеж в нашем случае будет выглядеть просто как списание средств.

2. Реализуйте классы Сберегательный, Кредитный, Расчетный (`SavingsAccount`, `CreditAccount`, `CheckingAccount` соответственно) как потомков класса счет, в них нужно переопределить методы базового класса. Каждый из них должен хранить баланс. Со сберегательного счета нельзя платить, только переводить и пополнять. Также сберегательный не может уходить в минус. Кредитный не может иметь положительный баланс – если платить с него, то уходит в минус, чтобы вернуть в 0 надо пополнить его. Расчетный может выполнять все три операции, но не может уходить в минус.

3. Продемонстрируйте работу счетов. Создайте три переменные типа `Account` и присвойте им три разних типа счетов.

## Задача 3. Резюме инженеров
### Описание

Вы реализуете Hr-систему с базой резюме. У вас есть база кандидатов. У всех их разные специализации и навыки. Однако как специализации, так и навыки могут быть объединены в единую иерархию. На сайте нетологии, в разделе "Программирование" сейчас доступны 4 профессии: frontend-разработчик, python-разработчик, веб-разработчик и fullstack-дизайнер. Многие навыки пересекаются, например, верстка и работа с базами данных есть в нескольких профессиях. Создайте такую иерархию классов и интерфейсов, которая бы представляла навыки людей этих профессий.

### Реализация
1. Для этой иерархии вам нужен базовый класс, например `Engineer`.

2. Наследники, представляющие собой отдельные професии: `FrontEndDeveloper`, `FullStackDesigner`, `PythonDeveloper`, `WebDeveloper`.

3. Интерфейсы, предствляющие собой навыки. Они могут быть общими у разных профессий. Например, `Css`, `Html`, `Databases` и другие.

Это более творческое задание, вы можете отклоняться от предложенных выше сущностей, если у вас есть другие идеи. Требуется только иерархия классов и интерфейсов, с названиями методов.

## Инструкция по выполнению домашнего задания

1. Зарегистрируйтесь на сайте [Repl.IT](http://repl.it/).
2. Перейдите в раздел **my repls**.
3. Нажмите кнопку **Start coding now!**, если приступаете впервые, или **New Repl**, если у вас уже есть работы.
4. В списке языков выберите `Java`.
5. Код пишите в левой части окна, вместо строки `System.out.println("Hello world!");`.
6. Опирайтесь на принятый [стиль оформления кода](https://github.com/netology-code/codestyle/blob/master/java/README.md).
7. Посмотреть результат выполнения файла можно, нажав на кнопку **Run**. Результат появится в правой части окна.
8. После окончания работы нажмите кнопку **Share** и скопируйте ссылку из поля Share link.
9. В личном кабинете на сайте [netology.ru](http://netology.ru/) в поле комментария к домашней работе вставьте скопированную ссылку и отправьте работу на проверку.

*Никаких файлов прикреплять не нужно.*

Все задачи обязательны к выполнению для получения зачета. Присылать на проверку можно каждую задачу по отдельности или все задачи вместе. Во время проверки по частям ваша домашняя работа будет со статусом "На доработке".

Любые вопросы по решению задач задавайте в группе на Facebook.
