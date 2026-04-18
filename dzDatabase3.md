--create table Teachers
--(
--Id int primary key not null identity (1,1),
--FirstName nvarchar(100) not null,
--LastName nvarchar(100) not null
--)

--create table Assistans
--(
--Id int primary key not null identity (1,1),
--TeacherId int not null
--)

--create table Curators
--(
--Id int primary key not null identity (1,1),
--TeacherId int not null
--)

--create table Decans
--(
--Id int primary key not null identity (1,1),
--TeacherId int not null
--)

--create table Faculties
--(
--Id int primary key not null identity (1,1),
--Building int not null check (Building between 1 and 5),
--FacultyName nvarchar(100) not null unique check(len(FacultyName) > 0),
--DecanId int not null,
--foreign key (DecanId) references Decans(Id)
--)

--create table Heads
--(
--Id int primary key not null identity (1,1),
--TeacherId int not null,
--foreign key (TeacherId) references Teachers(Id)
--)

--create table Departments
--(
--Id int primary key not null identity (1,1),
--Building int not null check (Building between 1 and 5),
--DepartmentName nvarchar(100) not null unique check(len(DepartmentName) > 0),
--FacultyId int not null,
--HeadId int not null,
--foreign key (FacultyId) references Faculties(Id),
--foreign key (HeadId) references Heads(Id)
--)

--create table Groups
--(
--Id int primary key not null identity (1,1),
--GroupName nvarchar(10) not null unique check(len(GroupName) > 0),
--Years int not null check (Years between 1 and 5),
--DepartmentId int not null,
--foreign key (DepartmentId) references Departments(Id)
--)

--create table GroupsCurators
--(
--Id int primary key not null identity (1,1),
--CuratorId int not null,
--GroupId int not null,
--foreign key (CuratorId) references Curators(Id),
--foreign key (GroupId) references Groups(Id)
--)

--create table GroupLectures
--(
--Id int primary key not null identity (1,1),
--GroupId int not null,
--LectureId int not null
--)

--create table LectureRooms
--(
--Id int primary key not null identity (1,1),
--Building int not null check (Building between 1 and 5),
--LectureName nvarchar(10) not null unique check (len(LectureName) > 0)
--)

--create table Subjects
--(
--Id int primary key not null identity (1,1),
--SubjectName nvarchar(100) not null unique check(len(SubjectName) > 0)
--)

--create table Lectures
--(
--Id int primary key not null identity (1,1),
--SubjectId int not null,
--TeacherId int not null,
--foreign key (SubjectId) references Subjects(Id),
--foreign key (TeacherId) references Teachers(Id)
--)

--create table Schedules
--(
--Id int primary key not null identity (1,1),
--Class int not null check (Class between 1 and 5),
--Dayofweeek int not null check (Dayofweeek between 1 and 7),
--Weeek int not null check (Weeek between 1 and 52),
--LectureId int not null,
--LectureRoomId int not null,
--foreign key (LectureId) references Lectures(Id),
--foreign key (LectureRoomId) references LectureRooms(Id) 
--)
--;

--insert into Teachers
--values
--('Иван', 'Иванов'), ('Петр', 'Петров'), ('Мария', 'Сидорова'), ('Анна', 'Кузнецова'), ('Сергей', 'Волков');

--insert into Subjects
--values
--('Математика'), ('Физика'), ('Информатика'), ('Философия'), ('Иностранный язык');

--insert into LectureRooms
--values
--(1, '101А'), (2, '205Б'), (3, '301В'), (4, '402Г'), (5, '503Д');

--insert into Assistans
--values
--(1), (2), (3), (4), (5);
--insert into Curators 
--values
--(1), (2), (3), (4), (5);
--insert into Decans  
--values 
--(1), (2), (3), (4), (5);
--insert into Heads 
--values 
--(1), (2), (3), (4), (5);

--insert into Faculties
--values
--(1, 'Факультет Математики', 1), 
--(2, 'Факультет Физики', 2), 
--(3, 'Факультет Информатики', 3), 
--(4, 'Факультет Гуманитарных наук', 4), 
--(5, 'Факультет Иностранных языков', 5);

--insert into Departments
--values
--(1, 'Кафедра Алгебры', 1, 1), 
--(2, 'Кафедра Теоретической физики', 2, 2), 
--(3, 'Кафедра Программирования', 3, 3), 
--(4, 'Кафедра Философии', 4, 4), 
--(5, 'Кафедра Английского языка', 5, 5);

--insert into Groups
--values
--('ИВТ-101', 1, 1), ('ФИЗ-202', 2, 2), ('ПИ-303', 3, 3), ('ФЛ-404', 4, 4), ('АНГ-505', 5, 5);

--insert into Lectures
--values
--(1, 1), (2, 2), (3, 3), (4, 4), (5, 5);

--insert into GroupLectures
--values
--(1, 1), (2, 2), (3, 3), (4, 4), (5, 5);

--insert into GroupsCurators
--values
--(1, 1), (2, 2), (3, 3), (4, 4), (5, 5);

--insert into Schedules
--values
--(1, 1, 1, 1, 1), (2, 2, 2, 2, 2), (3, 3, 3, 3, 3), (4, 4, 4, 4, 4), (5, 5, 5, 5, 5);

--select distinct lr.LectureName
--from LectureRooms lr
--join Schedules s on lr.Id = s.LectureRoomId
--join Lectures l on s.LectureId = l.Id
--join Teachers t on l.TeacherId = t.Id
--where t.FirstName = 'Иван' and t.LastName = 'Иванов';

--select distinct t.LastName
--from Teachers t
--join Assistans a on t.Id = a.TeacherId
--join Lectures l on t.Id = l.TeacherId
--join GroupLectures gl on l.Id = gl.LectureId
--join Groups g on gl.GroupId = g.Id
--where g.GroupName = 'АНГ-505';

--select distinct s.SubjectName
--from Subjects s
--join Lectures l on s.Id = l.SubjectId
--join Teachers t on l.TeacherId = t.Id
--join GroupLectures gl on l.Id = gl.LectureId
--join Groups g on gl.GroupId = g.Id
--where t.FirstName = 'Сергей' and t.LastName = 'Волков' 
--  and g.Years = 5;

--select t.LastName
--from Teachers t
--where not exists (
--    select 1
--    from Lectures l
--    join Schedules sch on l.Id = sch.LectureId
--    where l.TeacherId = t.Id and sch.Dayofweeek = 1
--);

--select LectureName, Building
--from LectureRooms
--where Id not in (
--    select LectureRoomId
--    from Schedules
--    where Dayofweeek = 3 and Weeek = 2 and Class = 3
--);

--select t.FirstName, t.LastName
--from Teachers t
--join Lectures l on t.Id = l.TeacherId
--join GroupLectures gl on l.Id = gl.LectureId
--join Groups g on gl.GroupId = g.Id
--join Departments d on g.DepartmentId = d.Id
--join Faculties f on d.FacultyId = f.Id
--where f.FacultyName = 'Факультет Математики'
--  and t.Id not in (
--      select t2.Id
--      from Teachers t2
--      join Curators c on t2.Id = c.TeacherId
--      join GroupsCurators gc on c.Id = gc.CuratorId
--      join Groups g2 on gc.GroupId = g2.Id
--      join Departments d2 on g2.DepartmentId = d2.Id
--      where d2.DepartmentName = 'Кафедра Программирования'
--  );

--select Building from Faculties
--union
--select Building from Departments
--union
--select Building from LectureRooms
--order by Building;

--select distinct s.Dayofweeek
--from Schedules s
--join LectureRooms lr on s.LectureRoomId = lr.Id
--where lr.Building in (1, 2) 
--  and lr.LectureName in ('101А', '205Б');
