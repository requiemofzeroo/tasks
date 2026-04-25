--create table Teachers
--(
--Id int primary key not null identity(1,1),
--EmploymentDate date not null check (EmploymentDate >= '1990-01-01'),
--FirstName nvarchar(100) not null check(len(FirstName) > 0),
--Salary money not null check (Salary > 0),
--LastName nvarchar(100) not null check(len(LastName) > 0)
--)

--create table Actions
--(
--Id int primary key not null identity (1,1),
--ActionName nvarchar(100) not null unique check (len(ActionName) >0),
--)

--create table TeacherManipulations
--(
--Id int primary key not null identity(1,1),
--EmployDate date not null check(EmployDate <=getdate()),
--ActionId int not null,
--TeacherId int not null,
--foreign key (ActionId) references Actions (Id),
--foreign key (TeacherId) references Teachers (Id)
--)

--create table TeacherAddedInfos
--(
--Id int primary key not null identity (1,1),
--EmploymentDate date not null check (EmploymentDate >= '1990-01-01'),
--FirstName nvarchar(100) not null check(len(FirstName) > 0),
--Salary money not null check (Salary > 0),
--LastName nvarchar(100) not null check(len(LastName) >0),
--ManipulationId int not null,
--foreign key (ManipulationId) references TeacherManipulations (Id)
--)

--create table TeacherDeletedInfos
--(
--Id int primary key identity (1,1),
--EmploymentDate date not null check (EmploymentDate >= '1990-01-01'),
--FirstName nvarchar(100) not null check(len(FirstName) > 0),
--Salary money not null check (Salary > 0),
--LastName nvarchar(100) not null check(len(LastName) > 0),
--ManipulationId int not null
--)
--;

--insert into Teachers (FirstName, LastName, EmploymentDate, Salary)
--values
--('Алексей', 'Иванов', '2010-02-15', 55000),
--('Марина', 'Кузнецова', '2018-09-01', 48000),
--('Виктор', 'Петров', '1995-03-10', 90000),
--('Елена', 'Смирнова', '2020-11-20', 42000),
--('Олег', 'Соколов', '2012-06-05', 60000);

--insert into Actions (ActionName)
--values 
--(N'Прием на работу'), (N'Повышение зарплаты'), (N'Смена фамилии'), (N'Перевод'), (N'Премия');

--insert into TeacherManipulations (EmployDate, ActionId, TeacherId)
--values 
--('20230110', 1, 1),
--('20230515', 2, 2),
--('20230901', 3, 3),
--('20240210', 4, 4),
--(CAST(GETDATE() AS DATE), 5, 5);

--insert into TeacherAddedInfos (FirstName, LastName, EmploymentDate, Salary, ManipulationId)
--values 
--(N'Алексей', N'Иванов', '20100215', 55000, 1),
--(N'Марина', N'Кузнецова', '20180901', 53000, 2),
--(N'Виктор', N'Петров-Сидоров', '19950310', 90000, 3),
--(N'Елена', N'Смирнова', '20201120', 42000, 4),
--(N'Олег', N'Соколов', '20120605', 65000, 5);

--insert into TeacherDeletedInfos (FirstName, LastName, EmploymentDate, Salary, ManipulationId)
--values 
--(N'Алексей', N'Иванов', '20100215', 1, 1), -- Ставка 1 вместо 0, т.к. у вас CHECK (Salary > 0)
--(N'Марина', N'Кузнецова', '20180901', 48000, 2),
--(N'Виктор', N'Петров', '19950310', 90000, 3),
--(N'Елена', N'Смирнова', '20201120', 42000, 4),
--(N'Олег', N'Соколов', '20120605', 60000, 5);

--create trigger trg_TeachersAudit
--on Teachers
--after insert, update, delete
--as
--begin
--	set nocount on;
--	declare @ActionId int;
--	declare @ManipulationId int;
--	declare @CurrentDate date = cast(getdate() as date);

--	if exists (select * from inserted) and not exists (select * from deleted)
--    begin
--        select @ActionId = Id from Actions where ActionName = N'Прием на работу';
        
        
--        insert into TeacherManipulations (EmployDate, ActionId, TeacherId)
--        select @CurrentDate, @ActionId, Id from inserted;
        
--        set @ManipulationId = SCOPE_IDENTITY();

--        insert into TeacherAddedInfos (FirstName, LastName, EmploymentDate, Salary, ManipulationId)
--        select FirstName, LastName, EmploymentDate, Salary, @ManipulationId from inserted;
--    end

    
--    else if exists (select * from inserted) and exists (select * from deleted)
--    begin
--        select @ActionId = Id from Actions where ActionName = N'Повышение зарплаты'; 

--        insert into TeacherManipulations (EmployDate, ActionId, TeacherId)
--        select @CurrentDate, @ActionId, Id from inserted;

--        set @ManipulationId = SCOPE_IDENTITY();
--        insert into TeacherDeletedInfos (FirstName, LastName, EmploymentDate, Salary, ManipulationId)
--        select FirstName, LastName, EmploymentDate, Salary, @ManipulationId FROM deleted;

        
--        insert into TeacherAddedInfos (FirstName, LastName, EmploymentDate, Salary, ManipulationId)
--        select FirstName, LastName, EmploymentDate, Salary, @ManipulationId FROM inserted;
--    end

    
--    else if not exists (select * from inserted) AND EXISTS (select * from deleted)
--    begin
--        select @ActionId = Id from Actions where ActionName = N'Увольнение';

        
--        insert into TeacherManipulations (EmployDate, ActionId, TeacherId)
--        select @CurrentDate, @ActionId, Id from deleted;

--        set @ManipulationId = SCOPE_IDENTITY();

--        insert into TeacherDeletedInfos (FirstName, LastName, EmploymentDate, Salary, ManipulationId)
--        select FirstName, LastName, EmploymentDate, Salary, @ManipulationId from deleted;
--    end
--end;
