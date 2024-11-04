                                                                                                      Домашнее задание к занятию «Базы данных»
Инструкция по выполнению домашнего задания
Сделайте fork репозитория c шаблоном решения к себе в Github и переименуйте его по названию или номеру занятия, например, https://github.com/имя-вашего-репозитория/gitlab-hw или https://github.com/имя-вашего-репозитория/8-03-hw).
Выполните клонирование этого репозитория к себе на ПК с помощью команды git clone.
Выполните домашнее задание и заполните у себя локально этот файл README.md:
впишите вверху название занятия и ваши фамилию и имя;
в каждом задании добавьте решение в требуемом виде: текст/код/скриншоты/ссылка;
для корректного добавления скриншотов воспользуйтесь инструкцией «Как вставить скриншот в шаблон с решением»;
при оформлении используйте возможности языка разметки md. Коротко об этом можно посмотреть в инструкции по MarkDown.
После завершения работы над домашним заданием сделайте коммит (git commit -m "comment") и отправьте его на Github (git push origin).
Для проверки домашнего задания преподавателем в личном кабинете прикрепите и отправьте ссылку на решение в виде md-файла в вашем Github.
Любые вопросы задавайте в чате учебной группы и/или в разделе «Вопросы по заданию» в личном кабинете.
Желаем успехов в выполнении домашнего задания.

Легенда
Заказчик передал вам файл в формате Excel, в котором сформирован отчёт.

На основе этого отчёта нужно выполнить следующие задания.

Задание 1
Опишите не менее семи таблиц, из которых состоит база данных:

какие данные хранятся в этих таблицах;
какой тип данных у столбцов в этих таблицах, если данные хранятся в PostgreSQL.
Приведите решение к следующему виду:

Сотрудники (

идентификатор, первичный ключ, serial,
фамилия varchar(50),
...
идентификатор структурного подразделения, внешний ключ, integer).
Решение 1

Для примера рассмотрим базу данных для управления сотрудниками компании, их проектами, отделами и задачами. Ниже приведены семь таблиц с описанием хранимых данных и типов столбцов в базе данных PostgreSQL.

### 1. Сотрудники (Employees)
SQL

CREATE TABLE Employees (
    employee_id SERIAL PRIMARY KEY,
    first_name VARCHAR(50),
    last_name VARCHAR(50),
    email VARCHAR(100) UNIQUE NOT NULL,
    phone_number VARCHAR(20),
    hire_date DATE,
    salary NUMERIC(10, 2),
    department_id INTEGER REFERENCES Departments(department_id)
);
Описание:  
Таблица содержит информацию о сотрудниках компании. В ней хранится имя, фамилия, контактная информация, дата найма, зарплата и идентификатор отдела, к которому принадлежит сотрудник.

### 2. Отделы (Departments)
SQL

CREATE TABLE Departments (
    department_id SERIAL PRIMARY KEY,
    department_name VARCHAR(255) NOT NULL,
    manager_id INTEGER REFERENCES Employees(employee_id),
    location_id INTEGER REFERENCES Locations(location_id)
);
Описание:  
В этой таблице содержатся сведения об отделах компании. Каждый отдел имеет уникальное название, руководителя (сотрудника), а также привязку к местоположению (например, офису).

### 3. Местоположение (Locations)
SQL

CREATE TABLE Locations (
    location_id SERIAL PRIMARY KEY,
    street_address VARCHAR(200),
    postal_code VARCHAR(12),
    city VARCHAR(30),
    state_province VARCHAR(25)
);
Описание:  
Эта таблица хранит информацию о физических адресах офисов компании. Здесь указаны улица, почтовый индекс, город и регион/область.

### 4. Проекты (Projects)
SQL

CREATE TABLE Projects (
    project_id SERIAL PRIMARY KEY,
    project_name VARCHAR(150) NOT NULL,
    start_date DATE,
    end_date DATE CHECK (end_date >= start_date),
    budget NUMERIC(15, 2),
    department_id INTEGER REFERENCES Departments(department_id)
);
Описание:   
Таблица проектов включает в себя данные о проектах, над которыми работают сотрудники компании. Для каждого проекта указывается его название, даты начала и окончания, бюджет и отдел, ответственный за проект.

### 5. Назначения сотрудников на проекты (EmployeeAssignments)
SQL

CREATE TABLE EmployeeAssignments (
    assignment_id SERIAL PRIMARY KEY,
    employee_id INTEGER REFERENCES Employees(employee_id),
    project_id INTEGER REFERENCES Projects(project_id),
    role VARCHAR(100),
    hours_per_week SMALLINT CHECK (hours_per_week > 0 AND hours_per_week <= 40)
);
Описание:  
Здесь содержится информация о том, какие сотрудники назначены на конкретные проекты и какую роль они выполняют. Также указывается количество часов в неделю, которое сотрудник тратит на этот проект.

### 6. Задачи (Tasks)
SQL

CREATE TABLE Tasks (
    task_id SERIAL PRIMARY KEY,
    task_description TEXT,
    status VARCHAR(20) CHECK (status IN ('New', 'In Progress', 'Completed')),
    due_date DATE,
    assigned_to INTEGER REFERENCES Employees(employee_id)
);
Описание:    
Таблица задач предназначена для хранения информации о конкретных задачах, поставленных перед сотрудниками. Каждая задача может иметь статус («Новая», «В процессе», «Завершено»), срок выполнения и сотрудника, ответственного за выполнение задачи.

### 7. Типы документов (DocumentTypes)
SQL

CREATE TABLE DocumentTypes (
    document_type_id SERIAL PRIMARY KEY,
    type_name VARCHAR(100) NOT NULL
);
Описание:  
Эта таблица описывает типы документов, связанных с деятельностью компании (например, отчеты, договоры, презентации и т.п.). Она используется для классификации различных видов документации.

---

Таким образом, мы рассмотрели структуру базы данных, состоящей из семи таблиц, каждая из которых отвечает за хранение определенной категории данных, необходимых для управления компанией.