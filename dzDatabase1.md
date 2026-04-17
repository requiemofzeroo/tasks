--create table Departments
--(
--Id int primary key identity (1,1),
--Building int not null check (Building between 1 and 5),
--Financing money not null default 0 check (Financing >=0),
--Flooor int not null check (Flooor >=1),
--DepartmentName nvarchar(100) not null unique check (len(DepartmentName) > 0),
--)

--create table Diseases 
--(
--Id int primary key identity (1,1),
--DiseaseName nvarchar(100) not null unique check (len(DiseaseName) >0),
--Severity int not null default 1 check (Severity >=1)
--)

--create table Doctors
--(
--Id int primary key identity (1,1),
--FirstName nvarchar(100) not null check (len(FirstName) >0),
--Phone char(11) null,
--Premium money not null default 0 check (Premium >=0),
--Salary money not null check (Salary > 0),
--LastName nvarchar(100) not null check (len(LastName) >0)
--)

--create table Examinations
--(
--Id int primary key identity (1,1),
--DayOfWeeek int not null check(DayOfWeeek between 1 and 7),
--EndOfTime time not null,
--ExaminationName nvarchar(100) unique check (len(ExaminationName) >0),
--StartTime time not null check (StartTime >= '08:00:00' and StartTime <= '18:00:00'),

--constraint CK_Examinations_Time check (EndOfTime > StartTime)
--)

--create table Wards
--(
--Id int primary key identity (1,1),
--Building int not null check (Building between 1 and 5),
--Flooor int not null check(Flooor >= 1),
--WardName nvarchar(20) not null unique check (len(WardName) > 0)
--)

--;


--insert into Departments
--values
--(1, 500000.00, 1, 'Терапевтическое отделение'),
--(2, 750000.00, 2, 'Хирургическое отделение'),
--(1, 300000.00, 3, 'Кардиологическое отделение'),
--(3, 600000.00, 1, 'Неврологическое отделение'),
--(2, 450000.00, 4, 'Педиатрическое отделение'),
--(4, 800000.00, 2, 'Онкологическое отделение');

--insert into Diseases 
--values
--('Грипп', 2),
--('Сахарный диабет 2 типа', 3),
--('Гипертоническая болезнь', 3),
--('Острый аппендицит', 4),
--('Бронхиальная астма', 3),
--('Пневмония', 4),
--('Мигрень', 2);

--insert into Doctors
--values
--('Александр', '9123456789', 5000.00, 45000.00, 'Иванов'),
--('Елена', '9234567890', 7000.00, 60000.00, 'Петрова'),
--('Михаил', '9345678901', 3000.00, 40000.00, 'Сидоров'),
--('Ольга', NULL, 10000.00, 75000.00, 'Козлова'),
--('Дмитрий', '9567890123', 6000.00, 55000.00, 'Новиков'),
--('Анна', '9678901234', 4000.00, 42000.00, 'Морозова');


--insert into Examinations
--values
--(1, '09:30:00', 'УЗИ брюшной полости', '09:00:00'),
--(2, '11:00:00', 'Рентген грудной клетки', '10:30:00'),
--(3, '14:00:00', 'ЭКГ', '13:30:00'),
--(4, '10:00:00', 'МРТ головного мозга', '09:00:00'),
--(5, '16:30:00', 'КТ органов дыхания', '16:00:00'),
--(1, '12:00:00', 'Общий анализ крови', '08:00:00'),
--(6, '15:00:00', 'Эндоскопия желудка', '14:00:00');

--insert into Wards
--values
--(1, 1, 'Палата 101'),
--(1, 1, 'Палата 102'),
--(2, 2, 'Палата 201'),
--(2, 2, 'Палата 202'),
--(3, 3, 'Палата 301'),
--(1, 3, 'Палата 302'),
--(4, 2, 'Палата 401');

--select * from Wards - Вывести содержимое таблицы палат

--select LastName, Phone from Doctors - Вывести фамилии и телефоны всех врачей

--select Flooor from Wards - Вывести все этажи без повторений, на которых располагаются палаты.
--order by Flooor

--select DiseaseName as NameOfDisease, Severity as Severity_of_Disease - 
--from Diseases - Вывести названия заболеваний под именем “NameofDisease”
--и степень их тяжести под именем “Severity of Disease”

--select d.LastName as Doctor, dep.DepartmentName as Department, w.WardName as Ward
--from Doctors as d, Departments as dep, Wards as w
--where d.Id = 1 and dep.Id = 1 and w.Id = 1; - Использовать выражение FROM для любых трех таблиц базы данных, используя для них псевдонимы.

--select DepartmentName from Departments 
--where Building = 5 and Financing < 30000; - Вывести названия отделений, расположенных в корпусе 5 и имеющих фонд финансирования менее 30000.

--select DepartmentName from Departments
--where Building = 3 and Financing between 12000 and 15000; - Вывестиназвания отделений, расположенных в 3-мкорпусе с фондом финансирования в диапазоне от 12000 до 15000.

--select WardName from Wards
--where (Building = 4 or Building = 5) and Flooor = 1;

--select DepartmentName, Building, Financing from Departments
--where (Building = 3 or Building = 6) 
-- and (Financing < 11000 or Financing > 25000); -  Вывести названия, корпуса и фонды финансирования отделений, расположенных в корпусах 3 или 6 и имеющих фонд финансирования меньше 11000 или больше 25000.

--select LastName from Doctors
--where (Salary + Premium) > 1500; - Вывести фамилии врачей, чья зарплата (сумма ставки и надбавки) превышает 1500.

--select LastName from Doctors
--where (Salary + Premium) / 2 > (Premium * 3); - Вывести фамилии врачей, у которых половина зарплаты превышает троекратную надбавку.

--select distinct ExaminationName from Examinations
--where DayOfWeeek in (1, 2, 3)
--  and StartTime >= '12:00:00' 
--  and EndOfTime <= '15:00:00'; -  Вывести названия обследований без повторений, проводимых в первые три дня недели с 12:00 до 15:00.

