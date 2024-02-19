# **Перевести БД с MS SQL Server на PostgreSQL**

***


## Скрипты создания структуры БД в postgreSQL: ##


**Для всех типов данных CHAR(n),NCHAR(n), VARCHAR(n), NVARCHAR(n), VARCHAR(max), NVARCHAR(max) использую тип TEXT.**

**1.	cardTypes.**

CREATE TABLE public.cardTypes(
		idCardType smallint NOT NULL,
		idCardSystem bigint NULL,
		name varchar(50) NULL,
		Constraint "PK_CardType" PRIMARY KEY (idCardType))


**2.	clients.**


CREATE TABLE public.clients(
		idclient bigint NOT NULL,
		firstname text  NULL,
		middlename text  NULL,
		surname text  NULL,
		birthday date NULL,
		regaddress text  NULL,
		iddirStatus smallint NULL,
		createdate date NULL,
		pin_eq text  NULL,
	Constraint "PK_client" PRIMARY KEY (idclient))


**3.	departments.**


CREATE TABLE public.departments(
	iddepartment int NOT NULL,
	name text  NULL,
	iddepartmentsSystem bigint NULL,
	worktime text  NULL,
	phone text  NULL,
	addressstring text  NULL,
	city text  NULL,
	street text  NULL,
	build text  NULL,
	scname text  NULL,
	kladr text  NULL,
	kladr_street text  NULL,
	Constraint "PK_department" PRIMARY KEY (iddepartment))


**4.	dirchannels.**


CREATE TABLE public.dirchannels(
	iddirchannel smallint NOT NULL,
	name text  NULL,
	Constraint "PK_dirchannel" PRIMARY KEY (iddirchannel))


**5.	dirstatuses.**


CREATE TABLE public.dirstatuses(
	iddirstatus smallint NOT NULL,
	name text  NULL,
	Constraint "PK_dirstatus" PRIMARY KEY (iddirstatus))


**6.	dirstatusesorders.**


CREATE TABLE public.dirstatusesorders(
	iddirstatusorder smallint NOT NULL,
	name text  NULL,
	Constraint "PK_dirstatusorder" PRIMARY KEY (iddirstatusorder))


**7.	users.**


CREATE TABLE public.users(
	idUser bigint NOT NULL,
	login text  NULL,
	Constraint "PK_user" PRIMARY KEY (idUser))


**8.	orders.**


CREATE TABLE public.orders(
	idOrder bigint NOT NULL,
	idClient bigint NULL REFERENCES public.clients (idclient),
	idDirChannel smallint NULL REFERENCES public.dirchannels (iddirchannel),
	idCardType smallint NULL REFERENCES public.cardtypes (idcardtype),
	idDepartment int NULL REFERENCES public.departments (iddepartment),
	idDirStatusOrder smallint NULL REFERENCES public.dirstatusesorders (iddirstatusorder),
	createDate date NULL,
	editUser  bigint NULL REFERENCES public.users (iduser) ,
	updateDate date NULL,
	Constraint "PK_Order" PRIMARY KEY (idOrder))
