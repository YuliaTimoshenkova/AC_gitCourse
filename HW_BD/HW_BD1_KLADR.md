# **ПРОЕКТ ПО ОБРАБОТКЕ АДРЕСНЫХ ОБЪЕКТОВ**

***


## Цель проекта: ##

**Реализовать поддержку формата Государственного адресного реестра (ГАР) в Системе заказа банковских карт в удаленных каналах, чтобы соответствовать требованиям Регуляторов и повысить точность адресов доставки.**


## Задачи проекта: ##

1.	Реализовать получение ГАР;
2.	Реализовать возможность регулярного обновления ГАР;
3.	Интегрировать ГАР в Систему.


## SQL скрипты: ##

**1.	Создать вью для адресной строки без города. Чтобы можно было разделять по символам ','.**

CREATE VIEW Ex_city1
AS
SELECT   idDepartment	
		,addressString
		, IIF(LEN(addressString)> 0
						, SUBSTRING(addressString, (CHARINDEX(',', addressString)+2), 
												len(addressString)-CHARINDEX(',', addressString))	
			, NULL) AS 'ex_cityall'
FROM [PlasticOrder].[dbo].[departments]


**2.	Создать вью для адресной строки без города и улицы. Чтобы можно было разделять по символам ','.**

   
CREATE VIEW Ex_city_streetview2
AS
SELECT   idDepartment	
		,addressString
		,ex_cityall
	, IIF(LEN(ex_cityall)> 0
						, left(ex_cityall, (CHARINDEX(' ', ex_cityall)-1))	
			, NULL) AS 'ex_streetall'
			FROM Ex_city1


**3.	Создать вью для распарсенного адреса**


CREATE VIEW splitadr4
AS
SELECT   e.idDepartment	
		,e.addressString
		,e.ex_cityall
		,e.ex_streetall
		,IIF(LEN(addressString)> 0
						, SUBSTRING(addressString, 4, 
												CHARINDEX(',', addressString)
									-4)	
			, NULL) AS 'City'
		, IIF(CHARINDEX('д.', Ex_cityall)- CHARINDEX(' ', Ex_cityall) > 0
		,LTRIM (SUBSTRING (Ex_cityall ,CHARINDEX(' ', Ex_cityall)
						,CHARINDEX('д.', Ex_cityall)-CHARINDEX(' ', Ex_cityall)-2) ), NULL) AS 'Street'
		,RIGHT(addressString, (len (addressString)- CHARINDEX('д. ', addressString)- CHARINDEX('г', addressString))) AS 'Build'
		, replace (ex_streetall, '.','') AS 'Socrname'
FROM  Ex_city_streetview2 as E



**4.	Создать вью для кода города**


CREATE VIEW citycodView3
AS
select p.*, k.CODE
from splitadr4 as p
left join  (SELECT distinct (code), NAME FROM [KLADR].[dbo].[KLADR] where SOCR like 'г') K on K.NAME=p.City


**5.	Объединить адрес с кодами из KLADR**


select p.*,t.SOCR, k.CodeKladr, t.CODE
from splitadr4 as p
left join  (SELECT distinct (code) as CodeKladr, NAME ,IIF(LEN(CODE)> 0
						, SUBSTRING(CODE, 1, 11), NULL) AS 'citycodKladr' FROM [KLADR].[dbo].[KLADR] where SOCR like 'г') K on K.NAME=p.City
left join (SELECT name,SOCR, CODE, COUNT(CODE) as 'Count',IIF(LEN(CODE)> 0
						, SUBSTRING(CODE, 1, 11), NULL) AS 'citycodStreet' FROM [KLADR].[dbo].[STREET]
						where code like '%00'
group by CODE, name, SOCR) t on (t.NAME=p.Street and t.Socr=p.Socrname and k.citycodKladr=t.citycodStreet)


**7.	Создаю вью для наглядности и запуска merge**


CREATE VIEW KLADR3ENDview
AS
select p.*,t.SOCR, k.CodeKladr, t.CODE
from splitadr4 as p
left join  (SELECT distinct (code) as CodeKladr, NAME ,IIF(LEN(CODE)> 0
						, SUBSTRING(CODE, 1, 11), NULL) AS 'citycodKladr' FROM [KLADR].[dbo].[KLADR] where SOCR like 'г') K on K.NAME=p.City
left join (SELECT name,SOCR, CODE, COUNT(CODE) as 'Count',IIF(LEN(CODE)> 0
						, SUBSTRING(CODE, 1, 11), NULL) AS 'citycodStreet' FROM [KLADR].[dbo].[STREET]
						where code like '%00'
group by CODE, name, SOCR) t on (t.NAME=p.Street and t.Socr=p.Socrname and k.citycodKladr=t.citycodStreet)


**8.	Делаю merge**


merge [PlasticOrder].[dbo].[departments] as D 
using (select * from KLADR3ENDview) as V 
	on D.[IdDepartment] = V.[IdDepartment]
	WHEN MATCHED
    THEN
        UPDATE
        SET D.[KLADR] = V.CodeKladr, D.[KLADR_STREET] = V.CODE;


        

## И вот в конце я понял, что можно сделать это все красивее через with и почему речь шла про курсор и цикл((.
## Недостатки своего подхода понял, но очень рад, столько запросов раньше не писал)

