## JS#4
Как вы помните, Кеша еще проходит стажировку. На стажировке, как известно, много денег не заработаешь, а Иннокентию надо вкладываться в рекламу своего сайта. Чтобы немного подзаработать, Кеша решил начать торговать на фондовой бирже.

Наш товарищ не сильно разбирается в торговле акциями, знает только то, что ценные бумаги надо покупать подешевле, а продавать подороже.

Помогите Кеше написать программу, которая будет наблюдать за ростом и падением цен на акции компаний и скупать ценные бумаги по выгодному курсу.

Выгодной ценой мы будем считать первую повышенную после падения. Под падением понимаем хотя бы одну итерацию, в которой вторая цена ниже первой. Все расчеты производим в условных единицах без привязки к валютам и курсам.

Допустим, есть компания "Рога и копыта" Цены на акции этой компании начинаются со 100. На первой итерации торгов акции упали в цене до 80, затем до 50, а потом поднялись до 55.

На ``3`` итерации торгов мы понимаем, что цена на акции выросла, а до этого 2 итерации падала. Эту цену мы считаем за выгодную цену.
  
```js
100 -> 80 -> 50 -> 55
0      1     2     3
```

Первым шагом вам предстоит создать класс ``Company``. У ``Company`` должны быть 4 публичных поля и 1 публичный метод:

-   ``name`` – название компании;
    
-   ``sharePrice`` – цена акции за штуку;
    
-   ``shareCount`` – количество акций компании, выставленных на продажу;
    
-   ``exchangeObserver`` – объект биржи, на которой торгует компания;
    
-   ``updatePrice`` – метод, который принимает параметр с новой ценой акции и устанавливает его в ``sharePrice``. В случае, если у компании есть акции на продажу, то оповещает об изменении цены биржу из поля ``exchangeObserver``.
    
❗Обратите внимание:
-   ``name``, ``sharePrice``, ``shareCount`` и ``exchangeObserver`` должны инициализироваться в конструкторе;
-   ``sharePrice`` и ``shareCount`` должны иметь значения по умолчанию ``0``.
    

  

Прежде чем начать работу с биржей нам потребуется её участники. За создание объектов участников отвечает класс ``Member``. У ``Member`` должны быть 4 поля :

-   ``balance`` – баланс участника рынка;
    
-   ``exchangeObserver`` – объект биржи, на которой торгует участник;
    
-   ``interestingCompanies`` – массив объектов класса Company, содержит компании, за акциями которых участнику было бы интересно следить;
    
-   ``purchasedSharesNumber`` – количество приобретаемых за раз акций;

❗Обратите внимание:

-   ``balance``, ``exchangeObserver``, ``interestingCompanies`` и ``purchasedSharesNumber`` должны инициализироваться в конструкторе;
    
-   ``purchasedSharesNumber`` и ``interestingCompanies`` должны иметь значения по умолчанию ``0`` и ``[]``, соответственно;
    
-   ``exchangeObserver`` должно быть приватным, все остальные поля могут быть публичны.
    
В логике работы класса ``Member`` основный задачей является реализация:

-   Подписки на изменения цен акций компаний из поля ``interestingCompanies`` (с помощью метода ``onUpdateCompany`` из ``exchangeObserver``);
    
-   Поиска выгодной цены акций;
    
-   Скупки акций после нахождения выгодной цены (с помощью метода ``sellShares`` из ``exchangeObserver``).
    
Для полного успеха осталось реализовать класс самой биржи – ``Exchange``. Биржа должна иметь 1 приватное пол и 3 публичных метода:

-   ``listeners`` – массив объектов, где ключи – названия компаний, а значения – массивы функций обратного вызова, которые должны принимать объект класса ``Company``. Эти функции будут вызываться, когда происходит изменение цены акции компании.
    
-   ``sellShares`` – метод, который требуется вызывать, когда участники биржи хотят купить акции какой-нибудь компании. Метод должен принимать 2 параметра:
    
	-   Объект класса ``Company``;
    
	-   Объект класса ``Member``.
    
	Метод должен снять с участника средства, которые потребуются для покупки акций в кол-ве, указанном в поле ``purchasedSharesNumber`` переданного объекта класса ``Member`` и вычесть это кол-во из общего кол-ва акций компании.

	❗Обратите внимание: ``sellShares`` должен породить ошибку, если пользователь хочет купить кол-во акций превышающее кол-во акций компании на рынке, либо у участника рынка недостаточно средств для их покупки.

-   ``updateCompany`` – метод, который принимает объект класса Company и вызывает все функции, которые находятся в поле listeners с ключом равным названию переданной компании.
    
-   ``onUpdateCompany`` – метод, принимающий название компании и функцию обратного вызова. Метод должен добавить в массив с ключом равным названию компании новую функцию обратного вызова.
    
### Пример работы биржи
```js
const exchange = new Exchange();
const greenBank = new Company('Green Bank', 100, exchange, 100);
const kesha = new Member(exchange, 10000, [greenBank]);

greenBank.updatePrice(70);
greenBank.updatePrice(73);

// Тут происходит покупка 10 акций по цене 100

console.log(greenBank.shareCount); // 90

kesha.purchasedSharesNumber = 100; // Меняем кол-во приобретаемых акций с 10 до 100
greenBank.updatePrice(40);
greenBank.updatePrice(45);

// Тут происходит попытка скупить 100 акций у компании greenBank
// Error('У компании недостаточно акций для продажи')
```


Если вы дочитали до конца и сумели выполнить эту задачу, то можете себя поздравить!

Вы стали на шаг ближе к совершенству 😏, а еще вы реализовали паттерн проектирования “Наблюдатель” или Observer. Почитать про него подробнее вы можете в ресурсе, закрепленном в разделе с полезными ссылками.

### Полезные ссылки:

 1. [Работа с объектом Map в JavaScript (вдруг пригодится)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map)
 2. [Паттерн "Наблюдатель"](https://refactoring.guru/ru/design-patterns/observer)
