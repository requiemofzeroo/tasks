--create table Departments 
--(
--Id int primary key not null identity (1,1),
--DepartmentName nvarchar(100) not null unique check(len(DepartmentName)>0),
--)

--create table Specializations
--(
--Id int primary key identity (1,1),
--SpecializationName nvarchar(100) not null unique check(len(SpecializationName) > 0)
--)

--create table Sponsors
--(
--Id int primary key not null identity (1,1),
--SponsorName nvarchar(100) not null unique check(len(SponsorName) >0)
--)

--create table Doctors
--(
--Id int primary key not null identity (1,1),
--FirstName nvarchar(100) not null check (len(FirstName) >0),
--Premium money not null default 0 check (Premium >=0),
--Salary money not null check (Salary >0),
--LastName nvarchar(100) not null check (len(LastName) >0)
--)

--create table DoctorsSpecializations
--(
--Id int primary key not null identity (1,1),
--DoctorId int not null,
--SpecializationId int not null,
--foreign key (DoctorId) references Doctors(Id),
--foreign key (SpecializationId) references Specializations(Id)
--)

--create table Donations
--(
--Id int primary key not null identity (1,1),
--Amount money not null check (Amount >=0),
--DonationDate date not null default getdate() check (DonationDate <= getdate()),
--DepartmentId int not null,
--SponsorId int not null,
--foreign key (DepartmentId) references Departments(Id),
--foreign key (SponsorId) references Sponsors(Id)
--)

--create table Vacations 
--(
--Id int primary key not null identity (1,1),
--EndDate date not null,
--StartDate date not null,
--DoctorId int not null,
--constraint Ck_Vacations_EndDate check (EndDate > StartDate),
--foreign key (DoctorId) references Doctors(Id)
--)

--create table Wards 
--(
--Id int primary key not null identity (1,1),
--WardName nvarchar(20) not null unique check(len(WardName) >0),
--DepartmentId int not null,
--foreign key (DepartmentId) references Departments(Id)
--)

--;

--insert into Departments
--values
--('Терапия'),
--('Хирургия'),
--('Кардиология'),
--('Неврология'),
--('Педиатрия'),
--('Онкология');

--insert into Specializations
--values
--('Кардиолог'),
--('Хирург'),
--('Терапевт'),
--('Невролог'),
--('Педиатр'),
--('Онколог');

--insert into Sponsors
--values
--('Umbrella Corporation'),
--('PharmaCorp International'),
--('MedTech Solutions'),
--('HealthFirst Foundation'),
--('BioMed Industries'),
--('Global Health Partners');

--insert into Doctors
--values
--('Александр', 5000.00, 85000.00, 'Петров'),
--('Елена', 3500.00, 72000.00, 'Иванова'),
--('Михаил', 6000.00, 95000.00, 'Сидоров'),
--('Анна', 4000.00, 78000.00, 'Козлова'),
--('Дмитрий', 5500.00, 88000.00, 'Новиков'),
--('Ольга', 4500.00, 80000.00, 'Морозова');

--insert into DoctorsSpecializations
--values
--(2, 1), 
--(3, 2), 
--(4, 3), 
--(5, 4), 
--(6, 5), 
--(7, 6);

--insert into Donations
--values
--(150000.00, '2024-01-15', 1, 1), 
--(200000.00, '2024-02-20', 2, 2), 
--(175000.00, '2024-03-10', 3, 3), 
--(125000.00, '2024-04-05', 4, 4), 
--(250000.00, '2024-05-12', 5, 5), 
--(300000.00, '2024-06-18', 6, 6); 


--insert into Vacations 
--values
--('2024-07-28', '2024-07-01', 2),  
--('2024-08-25', '2024-08-05', 3),
--('2024-10-07', '2024-09-10', 4),
--('2024-11-11', '2024-10-15', 5),
--('2024-11-21', '2024-11-01', 6),
--('2024-12-28', '2024-12-01', 7);


--insert into Wards
--values
--('Палата 101', 1),
--('Палата 205', 2),
--('Палата 312', 3),
--('Палата 418', 4),
--('Палата 520', 5),
--('Палата 630', 6);

--select d.FirstName + ' ' + d.LastName AS FullName, s.SpecializationName AS Specialization
--from Doctors d
--join DoctorsSpecializations ds on d.Id = ds.DoctorId
--join Specializations s on ds.SpecializationId = s.Id; 

--select d.LastName, (d.Salary + d.Premium) as TotalSalary
--from Doctors d
--where d.Id not in (select DoctorId from Vacations);

--select w.WardName
--from Wards w
--join Departments d on w.DepartmentId = d.Id
--where d.DepartmentName = 'Intensive Treatment';

--select distinct  d.DepartmentName
--from Departments d
--join Donations don on d.Id = don.DepartmentId
--join Sponsors s on don.SponsorId = s.Id
--where s.SponsorName = 'Umbrella Corporation';

--select distinct d.LastName, dep.DepartmentName as Department
--from Doctors d
--join Wards w on d.Id = w.Id  
--join Departments dep on w.DepartmentId = dep.Id
--where datepart(weekday, getdate()) NOT IN (1, 7);

--SELECT DISTINCT d.DepartmentName AS Department, doc.FirstName + ' ' + doc.LastName as Doctor
--FROM Departments d
--JOIN Donations don ON d.Id = don.DepartmentId
--LEFT JOIN Doctors doc ON doc.Id IN (SELECT DoctorId FROM Vacations WHERE 1=0)
--WHERE don.Amount > 100000;

--select distinct d.DepartmentName
--from Departments d
--join Doctors doc ON d.Id = doc.Id  
--where doc.Premium = 0;

--select s.SpecializationName
--from Specializations s
--join DoctorsSpecializations ds ON s.Id = ds.SpecializationId
