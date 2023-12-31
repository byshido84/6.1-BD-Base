# Домашнее задание к занятию 1. «Типы и структура СУБД»

### Задача 1

```
Архитектор ПО решил проконсультироваться у вас, какой тип БД 
лучше выбрать для хранения определённых данных.

Он вам предоставил следующие типы сущностей, которые нужно будет хранить в БД:

- электронные чеки в json-виде,
- склады и автомобильные дороги для логистической компании,
- генеалогические деревья,
- кэш идентификаторов клиентов с ограниченным временем жизни для движка аутенфикации,
- отношения клиент-покупка для интернет-магазина.

Выберите подходящие типы СУБД для каждой сущности и объясните свой выбор.
```

 - электронные чеки в json-виде,  
NoSQL(не реляционная база) и документоориентированная например CouchDb, MongoDb.   
- склады и автомобильные дороги для логистической компании,  
Любая реляционная база. Где склады, дороги, товары, транспорт обладают своими свойствами и связями.  
- генеалогические деревья,  
NoSQL(не реляционная база) и графовая например OrientDB. Объекты у нас будут в виде людей(со своими уникальными данными) и ребра в виде их связей. Но не исключаю и использование сетевой, так как все основные подобные сервисы именно на сетевой бд работают.  
- кэш идентификаторов клиентов с ограниченным временем жизни для движка аутенфикации,  
NoSQL(не реляционная база) и база типа Ключ-значение, например Memcached, Redis я бы не использовал так как он однопоточный. Данный тип БД обеспечит наибольшую производительность в данном случае где ключ - идентификатор, а значение - имя клиента.  
- отношения клиент-покупка для интернет-магазина.  
Я бы выбрал любую реляционную базу, клиенты, товары и т.д. могут обладать своим набором свойств и данными.

### Задача 2

```
Вы создали распределённое высоконагруженное приложение и хотите классифицировать его согласно 
CAP-теореме. Какой классификации по CAP-теореме соответствует ваша система, если 
(каждый пункт — это отдельная реализация вашей системы и для каждого пункта нужно привести классификацию):

- данные записываются на все узлы с задержкой до часа (асинхронная запись);
- при сетевых сбоях система может разделиться на 2 раздельных кластера;
- система может не прислать корректный ответ или сбросить соединение.

Согласно PACELC-теореме как бы вы классифицировали эти реализации?
```

- данные записываются на все узлы с задержкой до часа (асинхронная запись);  
    CAP - AP/CP, так как используется асинхронная запись которая может задерживать или удалять сообщения и не известно есть ли кворум.  
    PACELC - PA-EL, но полноценно при наличии асинхронных транзакций консистентность обеспечить не получится.
- при сетевых сбоях система может разделиться на 2 раздельных кластера;  
    CAP - AP в ущерб согласованности?  
    PACELC - PA/EL хотя возможен и PC/EL и PC/EC и PA/EC. Слишком мало вводных данных.
- система может не прислать корректный ответ или сбросить соединение.  
    CAP - согласованность и устойчивость т.е. CP  
    PAEC - PC/EC

### Задача 3

```
Могут ли в одной системе сочетаться принципы BASE и ACID? Почему?
```

BASE - доступность, ACID - целостность. BASE может обеспечить целостность, но не сразу, а ACID требует сразу. Поэтому ответ - Нет.

### Задача 4

```
Вам дали задачу написать системное решение, основой которого бы послужили:

- фиксация некоторых значений с временем жизни,
- реакция на истечение таймаута.

Вы слышали о key-value-хранилище, которое имеет механизм Pub/Sub. 
Что это за система? Какие минусы выбора этой системы?
```

Redis  
Минусы:  
1. Требуются достаточные ресурсы RAM и постоянный контроль за достаточностью.  
2. Отсутствие поддержки языка SQL, т.е. проблема оперативного поиска данных  
3. При сбое или перезапуске сервиса данные теряются до последней синхронизации на HDD.  
4. Однопоточность.
