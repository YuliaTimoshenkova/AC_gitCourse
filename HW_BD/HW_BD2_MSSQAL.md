# **Перевести БД с MS SQL Server на PostgreSQL**

***


## Скрипты на перенос данных: ##


**1.	cardTypes.**


INSERT INTO [POSTGRESQL].[PlasticOrder].[public].[cardtypes]
Select idCardType, idCardSystem,IIF(name NOT LIKE '[А-Я]%', right (name, len(name)-2),name)
From PlasticOrder.dbo.cardTypes

--Для name убираю пробелы слева.


**2.	clients.**


INSERT INTO [POSTGRESQL].[PlasticOrder].[public].[clients]
Select idClient,firstName, middleName, surname, birthday, regAddress, idDirStatus, createDate, PIN_EQ
From PlasticOrder.dbo.clients


**3.	departments.**


INSERT INTO [POSTGRESQL].[PlasticOrder].[public].[departments]
Select IdDepartment,name,idDepartmentsSystem,workTime,phone,addressString,city,street,build,SCNAME,KLADR,KLADR_STREET
From PlasticOrder.dbo.departments


**4.	dirchannels.**


INSERT INTO [POSTGRESQL].[PlasticOrder].[public].[dirchannels]
Select idDirChannel,name
From PlasticOrder.dbo.dirChannels


**5.	dirStatuses.**


INSERT INTO [POSTGRESQL].[PlasticOrder].[public].[dirstatuses]
Select idDirStatus,name
From PlasticOrder.dbo.dirStatuses


**6.	dirStatusesOrders.**


INSERT INTO [POSTGRESQL].[PlasticOrder].[public].[dirstatusesorders]
Select idDirStatusOrder,name
From PlasticOrder.dbo.dirStatusesOrders


**7.	users.**


INSERT INTO [POSTGRESQL].[PlasticOrder].[public].[users]
Select idUser,login
From PlasticOrder.dbo.users


**8.	orders.**


INSERT INTO [POSTGRESQL].[PlasticOrder].[public].[orders]
Select idorder,idClient,iddirchannel,idcardtype,iddepartment,iddirstatusorder,createdate,edituser,updatedate
From PlasticOrder.dbo.orders
