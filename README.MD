# Задание
1. Используя команду cat в терминале операционной системы Linux, создать
два файла Домашние животные (заполнив файл собаками, кошками,
хомяками) и Вьючные животными заполнив файл Лошадьми, верблюдами и
ослы), а затем объединить их. Просмотреть содержимое созданного файла.
Переименовать файл, дав ему новое имя (Друзья человека).


mkdir test
cd test
ls

cat <<EOT >> pets.txt
cat
dog
hamster
EOT

## Проверяю:
cat pets.txt   
# Вывод:
cat  
dog  
hamster

cat <<EOT >> pack_animals.txt  
horse  
camel  
donkey
EOT

## Проверяю:
cat pack_animals.txt   
## Вывод:  
horse  
camel  
donkey

## Объединяю два файла:  
cat pets.txt pack_animals.txt > animals.txt

## Проверяю:  
cat animals.txt   
# Вывод:  
cat  
dog  
hamster  
horse  
camel  
donkey  

## Переименовываю:  
mv animals.txt human_friends.txt  
## Проверяю:  
ls  
## Вывод:  
human_friends.txt  pack_animals.txt  pets.txt  





2. Создать директорию, переместить файл туда.


mkdir newdir
mv human_friends.txt newdir

## Проверяю:
ls
## Вывод:
newdir  pack_animals.txt  pets.txt

ls newdir
## Вывод:
human_friends.txt


3. Подключить дополнительный репозиторий MySQL. Установить любой пакет
из этого репозитория

wget -c https://dev.mysql.com/get/mysql-apt-config_0.8.30-1_all.deb
sudo dpkg -i mysql-apt-config_0.8.30-1_all.deb

sudo apt update
sudo apt install mysql-server

## Проверяю:
systemctl status mysql
## Вывод:
● mysql.service - MySQL Community Server
     Loaded: loaded (/usr/lib/systemd/system/mysql.service; enabled; preset: enabled)
     Active: active (running) since Mon 2024-06-17 20:36:27 UTC; 1min 32s ago
       Docs: man:mysqld(8)
             http://dev.mysql.com/doc/refman/en/using-systemd.html
   Main PID: 2647 (mysqld)
     Status: "Server is operational"
      Tasks: 34 (limit: 4613)
     Memory: 430.9M (peak: 444.3M)
        CPU: 1.277s
     CGroup: /system.slice/mysql.service
             └─2647 /usr/sbin/mysqld



4. Установить и удалить deb-пакет с помощью dpkg.

## загрузка пакета nginx
wget -c http://ftp.ru.debian.org/debian/pool/main/n/nginx/nginx_1.22.1-9_amd64.deb
## Устанавливаю пакет
sudo dpkg -i nginx_1.22.1-9_amd64.deb
## Исправление зависимостей…
sudo apt-get install -f

## Удаляю пакет
sudo dpkg -r nginx nginx-common



5. Выложить историю команд в терминале ubuntu

anastasia@anastasia-VirtualBox:-/test$ systemctl status mysql 
● mysql.service - MySQL Community Server
     Loaded: loaded (/usr/lib/systemd/system/mysql.service; enabled; preset: enabled)
     Active: active (running) since Mon 2024-06-16 15:10:32 UTC; 47s ago
       Docs: man:mysqld(8)
             http://dev.mysql.com/doc/refman/en/using-systemd.html
   Main PID: 2837 (mysqld)
     Status: "Server is operational"
      Tasks: 36 (limit: 4613)
     Memory: 431.1M (peak: 444.4M)
        CPU: 622ms
     CGroup: /system.slice/mysql.service
             └─2837 /usr/sbin/mysqld


июн 16 15:10:32 anastasia-VirtualBox systemd[1]: Starting mysql.service - MySQL Community Server…
июн 18 15:10:32 anastasia-VirtualBox systemd[1]: Started mysql.service - MySQL Community Server.
anastasia@anastasia-VirtualBox:-/test$ history40
    1 sudo -s
    2 ip a
    3 sudo -s
    4 mkdir test
    5 cd test
    6 ls
    7 cat <<EOT >> pets.txt
cat
dog
hamster
EOT
    8 cat.pets.txt
    9 car <<EOT >> pack_animals.txt
horse
camel
donkey
EOT
    10 cat pack_animals.txt
    ll cat pets.txt pack_animals.txt > animals.txt
    12 cat animals.txt
    13 mv animals.txt human_friends.txt
    14 ls
    15 mkdir newdir
    16 mv human_friends.txt newdir
    17 ls
    18 ls newdir
    19 wget -c https://dev.mysql.com/get/mysql-apt-config_0.8.30-l_all.deb
    20 sudo dpkg -i mysqi-apt-config_0.8.30-1_all.deb
    21 sudo apt update
    22 sudo apt install mysql-server
    23 systemctl status mysql
    24 history 40


6. Нарисовать диаграмму, в которой есть класс родительский класс, домашние
животные и вьючные животные, в составы которых в случае домашних
животных войдут классы: собаки, кошки, хомяки, а в класс вьючные животные
войдут: Лошади, верблюды и ослы).

![](/img/Screenshot.png)



7. В подключенном MySQL репозитории создать базу данных “Друзья
человека”

CREATE DATABASE Human_friends;



8. Создать таблицы с иерархией из диаграммы в БД

USE Human_friends;
CREATE TABLE animal_classes
(
	Id INT AUTO_INCREMENT PRIMARY KEY, 
	Class_name VARCHAR(20)
);

INSERT INTO animal_classes (Class_name)
VALUES ('вьючные'),
('домашние');  


CREATE TABLE packed_animals
(
	  Id INT AUTO_INCREMENT PRIMARY KEY,
    Genus_name VARCHAR (20),
    Class_id INT,
    FOREIGN KEY (Class_id) REFERENCES animal_classes (Id) ON DELETE CASCADE ON UPDATE CASCADE
);

INSERT INTO packed_animals (Genus_name, Class_id)
VALUES ('Лошади', 1),
('Ослы', 1),  
('Верблюды', 1); 
    
CREATE TABLE home_animals
(
	  Id INT AUTO_INCREMENT PRIMARY KEY,
    Genus_name VARCHAR (20),
    Class_id INT,
    FOREIGN KEY (Class_id) REFERENCES animal_classes (Id) ON DELETE CASCADE ON UPDATE CASCADE
);

INSERT INTO home_animals (Genus_name, Class_id)
VALUES ('Кошки', 2),
('Собаки', 2),  
('Хомяки', 2); 

CREATE TABLE cats 
(       
    Id INT AUTO_INCREMENT PRIMARY KEY, 
    Name VARCHAR(20), 
    Birthday DATE,
    Commands VARCHAR(50),
    Genus_id int,
    Foreign KEY (Genus_id) REFERENCES home_animals (Id) ON DELETE CASCADE ON UPDATE CASCADE
);


9. Заполнить низкоуровневые таблицы именами(животных), командами
которые они выполняют и датами рождения


INSERT INTO cats (Name, Birthday, Commands, Genus_id)
VALUES ('Pool', '2011-01-01', 'кс-кс-кс', 1),
('Lake', '2016-01-01', "отставить!", 1),  
('Vue', '2017-01-01', "", 1); 

CREATE TABLE dogs 
(       
    Id INT AUTO_INCREMENT PRIMARY KEY, 
    Name VARCHAR(20), 
    Birthday DATE,
    Commands VARCHAR(50),
    Genus_id int,
    Foreign KEY (Genus_id) REFERENCES home_animals (Id) ON DELETE CASCADE ON UPDATE CASCADE
);
INSERT INTO dogs (Name, Birthday, Commands, Genus_id)
VALUES ('Bull', '2020-01-01', 'ко мне, лежать, лапу, голос', 2),
('Cake', '2021-06-12', "сидеть, лежать, лапу", 2),  
('Blue', '2018-05-01', "сидеть, лежать, лапу, след, фас", 2), 
('Ball', '2021-05-10', "сидеть, лежать, фу, место", 2);

CREATE TABLE hamsters 
(       
    Id INT AUTO_INCREMENT PRIMARY KEY, 
    Name VARCHAR(20), 
    Birthday DATE,
    Commands VARCHAR(50),
    Genus_id int,
    Foreign KEY (Genus_id) REFERENCES home_animals (Id) ON DELETE CASCADE ON UPDATE CASCADE
);
INSERT INTO hamsters (Name, Birthday, Commands, Genus_id)
VALUES ('Nick', '2020-10-12', '', 3),
('Clous', '2021-03-12', "атака сверху", 3),  
('Angry', '2022-07-11', NULL, 3), 
('Brown', '2022-05-10', NULL, 3);

CREATE TABLE horses 
(       
    Id INT AUTO_INCREMENT PRIMARY KEY, 
    Name VARCHAR(20), 
    Birthday DATE,
    Commands VARCHAR(50),
    Genus_id int,
    Foreign KEY (Genus_id) REFERENCES packed_animals (Id) ON DELETE CASCADE ON UPDATE CASCADE
);
INSERT INTO horses (Name, Birthday, Commands, Genus_id)
VALUES ('Love', '2020-01-12', 'бегом, шагом', 1),
('Huge', '2017-03-12', "бегом, шагом, хоп", 1),  
('Shake', '2016-07-12', "бегом, шагом, хоп, брр", 1), 
('Screem', '2020-11-10', "бегом, шагом, хоп", 1);

CREATE TABLE donkeys 
(       
    Id INT AUTO_INCREMENT PRIMARY KEY, 
    Name VARCHAR(20), 
    Birthday DATE,
    Commands VARCHAR(50),
    Genus_id int,
    Foreign KEY (Genus_id) REFERENCES packed_animals (Id) ON DELETE CASCADE ON UPDATE CASCADE
);
INSERT INTO donkeys (Name, Birthday, Commands, Genus_id)
VALUES ('Grey', '2019-04-10', NULL, 2),
('Honey', '2020-03-12', "", 2),  
('Adorable', '2021-07-12', "", 2), 
('Krow', '2022-12-10', NULL, 2);

CREATE TABLE camels 
(       
    Id INT AUTO_INCREMENT PRIMARY KEY, 
    Name VARCHAR(20), 
    Birthday DATE,
    Commands VARCHAR(50),
    Genus_id int,
    Foreign KEY (Genus_id) REFERENCES packed_animals (Id) ON DELETE CASCADE ON UPDATE CASCADE
);
INSERT INTO camels (Name, Birthday, Commands, Genus_id)
VALUES ('Hill', '2022-04-10', 'вернись', 3),
('Big', '2019-03-12', "остановись", 3),  
('Clown', '2015-07-12', "повернись", 3), 
('Berb', '2022-12-10', "улыбнись", 3);



10. Удалив из таблицы верблюдов, т.к. верблюдов решили перевезти в другой
питомник на зимовку. Объединить таблицы лошади, и ослы в одну таблицу.

SET SQL_SAFE_UPDATES = 0;
DELETE FROM camels;

SELECT Name, Birthday, Commands FROM horses
UNION SELECT  Name, Birthday, Commands FROM donkeys;



11. Создать новую таблицу “молодые животные” в которую попадут все
животные старше 1 года, но младше 3 лет и в отдельном столбце с точностью
до месяца подсчитать возраст животных в новой таблице

CREATE TEMPORARY TABLE animals AS 
SELECT *, 'Лошади' as genus FROM horses
UNION SELECT *, 'Ослы' AS genus FROM donkeys
UNION SELECT *, 'Собаки' AS genus FROM dogs
UNION SELECT *, 'Кошки' AS genus FROM cats
UNION SELECT *, 'Хомяки' AS genus FROM hamsters;

CREATE TABLE yang_animal AS
SELECT Name, Birthday, Commands, genus, TIMESTAMPDIFF(MONTH, Birthday, CURDATE()) AS Age_in_month
FROM animals WHERE Birthday BETWEEN ADDDATE(curdate(), INTERVAL -3 YEAR) AND ADDDATE(CURDATE(), INTERVAL -1 YEAR);
 
SELECT * FROM yang_animal;


12. Объединить все таблицы в одну, при этом сохраняя поля, указывающие на
прошлую принадлежность к старым таблицам.


SELECT h.Name, h.Birthday, h.Commands, pa.Genus_name, ya.Age_in_month 
FROM horses h
LEFT JOIN yang_animal ya ON ya.Name = h.Name
LEFT JOIN packed_animals pa ON pa.Id = h.Genus_id
UNION 
SELECT d.Name, d.Birthday, d.Commands, pa.Genus_name, ya.Age_in_month 
FROM donkeys d 
LEFT JOIN yang_animal ya ON ya.Name = d.Name
LEFT JOIN packed_animals pa ON pa.Id = d.Genus_id
UNION
SELECT c.Name, c.Birthday, c.Commands, ha.Genus_name, ya.Age_in_month 
FROM cats c
LEFT JOIN yang_animal ya ON ya.Name = c.Name
LEFT JOIN home_animals ha ON ha.Id = c.Genus_id
UNION
SELECT d.Name, d.Birthday, d.Commands, ha.Genus_name, ya.Age_in_month 
FROM dogs d
LEFT JOIN yang_animal ya ON ya.Name = d.Name
LEFT JOIN home_animals ha ON ha.Id = d.Genus_id
UNION
SELECT hm.Name, hm.Birthday, hm.Commands, ha.Genus_name, ya.Age_in_month 
FROM hamsters hm
LEFT JOIN yang_animal ya ON ya.Name = hm.Name
LEFT JOIN home_animals ha ON ha.Id = hm.Genus_id;



13. Создать класс с Инкапсуляцией методов и наследованием по диаграмме
14. Написать программу, имитирующую работу реестра домашних животных.
В программе должен быть реализован следующий функционал:
14. 1 Завести новое животное
14. 2 определять животное в правильный класс
14. 3 увидеть список команд, которое выполняет животное
14. 4 обучить животное новым командам
14. 5 Реализовать навигацию по меню
15. Создайте класс Счетчик, у которого есть метод add(), увеличивающий̆
значение внутренней̆int переменной̆на 1 при нажатие “Завести новое
животное” Сделайте так, чтобы с объектом такого типа можно было работать в
блоке try-with-resources. Нужно бросить исключение, если работа с объектом
типа счетчик была не в ресурсном try и/или ресурс остался открыт. Значение
считать в ресурсе try, если при заведения животного заполнены все поля.

[Класс методов](/src/Model/)

