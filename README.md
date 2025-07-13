# Решение заданий тренажёра с [SQL ACADEMY](https://sql-academy.org/ru)

<details>
<summary><h2>Задание 1</h2><br>Вывести имена всех людей, которые есть в базе данных авиакомпаний</summary>

```mysql
select name
from passenger;
```

</details>
<details>
<summary><h2>Задание 2</h2><br>Вывести названия всеx авиакомпаний</summary>

```mysql
select name
from company;
```

</details>
<details>
<summary><h2>Задание 3</h2><br>Вывести все рейсы, совершенные из Москвы</summary>

```mysql
select *
from Trip
where town_from LIKE 'Moscow';
```

</details>
<details>
<summary><h2>Задание 4</h2><br>Вывести имена людей, которые заканчиваются на "man"</summary>

```mysql
SELECT name FROM Passenger
WHERE name LIKE "%man";
```

</details>
<details>
<summary><h2>Задание 5</h2><br>Вывести количество рейсов, совершенных на TU-134</summary>

```mysql
select count(*) as count
from Trip
where plane like 'TU-134'
group by plane;
```

</details>
<details>
<summary><h2>Задание 6</h2><br>Какие компании совершали перелеты на Boeing</summary>

```mysql
select name
from Company
where id in (
		select company
		from Trip
		where plane like 'Boeing'
	);
```

</details>
<details>
<summary><h2>Задание 7</h2><br>Вывести все названия самолётов, на которых можно улететь в Москву (Moscow)</summary>

```mysql
select DISTINCT plane
from Trip
where town_to like 'Moscow';
```

</details>
<details>
<summary><h2>Задание 8</h2><br>В какие города можно улететь из Парижа (Paris) и сколько времени это займёт?</summary>

```mysql
select town_to,
	TIMEDIFF(time_in, time_out) as flight_time
from Trip
where town_from like 'Paris';
```

</details>
<details>
<summary><h2>Задание 9</h2><br>Какие компании организуют перелеты из Владивостока (Vladivostok)?</summary>

```mysql
select name
from Company
where id in (
		select company
		from Trip
		where town_from like 'Vladivostok'
	);
```

</details>
<details>
<summary><h2>Задание 10</h2><br>Вывести вылеты, совершенные с 10 ч. по 14 ч. 1 января 1900 г.</summary>

```mysql
select *
from Trip
where time_out BETWEEN '1900-01-01T10:00:00.000Z' AND '1900-01-01T14:00:00.000Z';
```

</details>
<details>
<summary><h2>Задание 11</h2><br>Выведите пассажиров с самым длинным ФИО. Пробелы, дефисы и точки считаются частью имени.</summary>

```mysql
select name
from Passenger
order by LENGTH(name) DESC
limit 2;
```

</details>
<details>
<summary><h2>Задание 12</h2><br>Выведите идентификаторы всех рейсов и количество пассажиров на них. Обратите внимание, что на каких-то рейсах пассажиров может не быть. В этом случае выведите число "0".</summary>

```mysql
SELECT trip.id,
	COUNT(passenger) as COUNT
FROM Trip
	LEFT OUTER JOIN Pass_in_trip ON Pass_in_trip.trip = trip.id
GROUP BY Trip.id;
```

</details>
<details>
<summary><h2>Задание 13</h2><br>Вывести имена людей, у которых есть полный тёзка среди пассажиров</summary>

```mysql
SELECT name
FROM Passenger
GROUP BY name
HAVING COUNT(*) > 1;;
```

</details>
<details>
<summary><h2>Задание 14</h2><br>В какие города летал Bruce Willis</summary>

```mysql
select DISTINCT town_to
from Trip as t
	join Pass_in_trip as p on t.id = p.trip
	join Passenger as pa on pa.id = p.passenger
where name like 'Bruce Willis';
```

</details>
<details>
<summary><h2>Задание 15</h2><br>Выведите идентификатор пассажира Стив Мартин (Steve Martin) и дату и время его прилёта в Лондон (London)</summary>

```mysql
select p.passenger as id,
	time_in
from Trip as t
	join Pass_in_trip as p on t.id = p.trip
	join Passenger as pa on pa.id = p.passenger
WHERE name like 'Steve Martin'
	and town_to like 'London';
```

</details>
<details>
<summary><h2>Задание 16</h2><br>Вывести отсортированный по количеству перелетов (по убыванию) и имени (по возрастанию) список пассажиров, совершивших хотя бы 1 полет.</summary>

```mysql
select name,
	count(*) as count
from Passenger as passenger
	join Pass_in_trip as Pass on passenger.id = Pass.passenger
group by name
order by count desc,
	name ASC;
```

</details>
<details>
<summary><h2>Задание 17</h2><br>Определить, сколько потратил в 2005 году каждый из членов семьи. В результирующей выборке не выводите тех членов семьи, которые ничего не потратили.
</summary>

```mysql
select member_name,
	status,
	SUM(pay.amount * pay.unit_price) as costs
from FamilyMembers as Family
	join Payments as Pay on member_id = family_member
where YEAR(date) = '2005'
group by member_name,
	status;
```

</details>
<details>
<summary><h2>Задание 18</h2><br>Выведите имя самого старшего человека. Если таких несколько, то выведите их всех.</summary>

```mysql
select member_name
from FamilyMembers
order by birthday
limit 1;
```

</details>
<details>
<summary><h2>Задание 19</h2><br>Определить, кто из членов семьи покупал картошку (potato)</summary>

```mysql
select DISTINCT status
from FamilyMembers
	join Payments as pa on member_id = family_member
	join Goods on good = good_id
where good_name like 'potato';
```

</details>
<details>
<summary><h2>Задание 20</h2><br>Сколько и кто из семьи потратил на развлечения (entertainment). Вывести статус в семье, имя, сумму</summary>

```mysql
select status,
	member_name,
	sum(amount * unit_price) as costs
from FamilyMembers
	join Payments on member_id = family_member
	join Goods on good = good_id
	join GoodTypes on type = good_type_id
where good_type_name like 'entertainment'
group by member_name,
	status;
```

</details>
<details>
<summary><h2>Задание 21</h2><br>Определить товары, которые покупали более 1 раза</summary>

```mysql
select good_name
from Goods
	join Payments on good_id = good
group by good
having count(*) > 1;
```

</details>
<details>
<summary><h2>Задание 22</h2><br>Найти имена всех матерей (mother)</summary>

```mysql
select member_name
from FamilyMembers
where status like 'mother';
```

</details>
<details>
<summary><h2>Задание 23</h2><br>Найдите самый дорогой деликатес (delicacies) и выведите его цену</summary>

```mysql
select good_name,
	unit_price
from Goods
	join Payments on good_id = good
	join GoodTypes on good_type_id = type
where good_type_name like 'delicacies'
order by unit_price desc
limit 1;
```

</details>
<details>
<summary><h2>Задание 24</h2><br>Определить кто и сколько потратил в июне 2005</summary>

```mysql
select member_name,
	sum(amount * unit_price) as costs
from Payments
	join FamilyMembers on member_id = family_member
where MOnthname(date) = 'June'
	and year(date) = '2005'
group by member_name;
```

</details>
<details>
<summary><h2>Задание 25</h2><br>Определить, какие товары не покупались в 2005 году
</summary>

```mysql
select good_name
from Goods
where good_id not in (
		select good
		from Payments
		where year(date) = 2005
	);
```

</details>
<details>
<summary><h2>Задание 26</h2><br>Определить группы товаров, которые не приобретались в 2005 году</summary>

```mysql
select good_type_name
from GoodTypes
where good_type_id not in(
		select type
		from Goods
			join Payments on good_id = good
		where year(date) = 2005
	);
```

</details>
<details>
<summary><h2>Задание 27</h2><br>Узнайте, сколько было потрачено на каждую из групп товаров в 2005 году. Выведите название группы и потраченную на неё сумму. Если потраченная сумма равна нулю, т.е. товары из этой группы не покупались в 2005 году, то не выводите её.</summary>

```mysql
select good_type_name,
	sum(amount * unit_price) as costs
from GoodTypes
	join Goods on good_type_id = type
	join Payments on good_id = good
where year(date) = 2005
group by good_type_name;
```

</details>
<details>
<summary><h2>Задание 28</h2><br>Сколько рейсов совершили авиакомпании из Ростова (Rostov) в Москву (Moscow) ?</summary>

```mysql
select count(id) as count
from Trip
where town_from like 'Rostov'
	and town_to like 'Moscow';
```

</details>
<details>
<summary><h2>Задание 29</h2><br>Выведите имена пассажиров улетевших в Москву (Moscow) на самолете TU-134. В ответе не должно быть дубликатов.</summary>

```mysql
select DISTINCT name
from Passenger as Passenger
	join Pass_in_trip as Pass_in_trip on Passenger.id = Pass_in_trip.passenger
	join Trip as Trip on Trip.id = Pass_in_trip.trip
where plane like 'TU-134'
	and town_to like 'Moscow';
```

</details>
<details>
<summary><h2>Задание 30</h2><br>Выведите нагруженность (число пассажиров) каждого рейса (trip). Результат вывести в отсортированном виде по убыванию нагруженности.</summary>

```mysql
select trip,
	count(*) as count
from Pass_in_trip
group by trip
order by count desc;
```

</details>
<details>
<summary><h2>Задание 31</h2><br>Вывести всех членов семьи с фамилией Quincey.</summary>

```mysql
select *
from FamilyMembers
where member_name like '% Quincey';
```

</details>
<details>
<summary><h2>Задание 32</h2><br>Вывести средний возраст людей (в годах), хранящихся в базе данных. Результат округлите до целого в меньшую сторону.</summary>

```mysql
select floor(avg(2024 - year(birthday))) as age
from FamilyMembers;
```

</details>
<details>
<summary><h2>Задание 33</h2><br>Найдите среднюю цену икры на основе данных, хранящихся в таблице Payments. В базе данных хранятся данные о покупках красной (red caviar) и черной икры (black caviar). В ответе должна быть одна строка со средней ценой всей купленной когда-либо икры.</summary>

```mysql
select avg(unit_price) as cost
from Goods
	join Payments on good = good_id
where good_name like '% caviar';
```
</details>
<details>
<summary><h2>Задание 34</h2><br>Сколько всего 10-ых классов</summary>

```mysql
select count(c.id) as count
from Class as c
where name like '10%';
```

</details>
<details>
<summary><h2>Задание 35</h2><br>Сколько различных кабинетов школы использовались 2 сентября 2019 года для проведения занятий?</summary>

```mysql
select count(DISTINCT classroom) as count
from Schedule
where YEAR(date) = 2019
	and MONTHNAME(date) = 'September'
	and day(date) = 2;
```

</details>
<details>
<summary><h2>Задание 36</h2><br>Выведите информацию об обучающихся живущих на улице Пушкина (ul. Pushkina)?
</summary>

```mysql
select *
from Student
where address like 'ul. Pushkina%';
```

</details>
<details>
<summary><h2>Задание 37</h2><br>Сколько лет самому молодому обучающемуся ?</summary>

```mysql
select (2024 - year(birthday)) as year
from Student
order by year asc
limit 1;
```

</details>
<details>
<summary><h2>Задание 38</h2><br>Сколько Анн (Anna) учится в школе ?</summary>

```mysql
select count(id) as count
from Student
where first_name like 'Anna';
```

</details>
<details>
<summary><h2>Задание 39</h2><br>Сколько обучающихся в 10 B классе ?</summary>

```mysql
select count(class) as count
from Student_in_class as s
	join Class as c on c.id = s.class
WHERE name LIKE '10 B';
```

</details>
<details>
<summary><h2>Задание 40</h2><br>Выведите название предметов, которые преподает Ромашкин П.П. (Romashkin P.P.). Обратите внимание, что в базе данных есть несколько учителей с такой фамилией.</summary>

```mysql
select name as subjects
from Subject as sj
	join Schedule as sc on sc.subject = sj.id
	join Teacher as t on sc.teacher = t.id
where last_name = 'Romashkin'
	and first_name like 'P%'
	and middle_name like 'P%';
```
