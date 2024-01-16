**UseCase1**
#домашнее задание UML - UC изменение уcловий доставки карты
@startuml
left to right direction
skinparam dpi 200
actor Клиент as K
actor Поддержка as S
rectangle Приложение {
    usecase "изменение условий доставки карты" as UC1
usecase "в отделение" as UC2
usecase "изменение текущей доставки" as UC4
usecase "курьером" as UC3
usecase "требуется помощь сотрудника" as UC5
}
K --> UC1
S --> UC5
UC1<|--UC2
UC1<|--UC3
UC4.>UC3 : //<<extend>>//
UC5.>UC1 : //<<extend>>//
@enduml

//www.plantuml.com/plantuml/png/VPBDJi9058NtynGtx6lvWOqX3ObBbiG7Q634HYYa7H8JDIbbHIHnxyPNG0AAYFKLxdqZppJLBaXi5pqpvyntvYRjnCQFp6myKYDzPSXCA0g6ruO6GQJx9fY4Ahu9mgaV-MCQJ0EgLwlAxvj9HFpCdxpd7Nz9IdvCtT_z5yvuYtSd2R_nWTVMxIdByyFXI2C1Pu_J6NoSjnIwKuHd5kk-7skgOE-TZzRzIyX2vlcX3POVX8PCvfBo6Xi7tfEjaaHIUQpOnikBcYfn6q8wjRS2eY0MiRhwjpM5FA1xYUzfQy5ebemydCWgdp_ZOv5eb0bSCG7iLHR8fA0z4N_RAD1BMP7C8P21lyrlWS4AIaizg2uvZfUFrojKIs7LldSS35FA1cJpp8EWStBTTblV6XqEFCzruRIiKpjrEX3u7Nu0


**Sequence**
#домашнее задание UML - процесс заказа кредитной карты в мобильном приложении
@startuml
mainframe **sd** Заказ кредитной карты в мобильном приложении
actor Клиент as U
control МП as app
participant Сервер as server
database Eq as dbC
control КК as СС
control Скоринг as SC
database "Продуктовый \n Каталог" as dbP
database "Справочник \n Отделений \n Банка" as dbB
U -> app: авторизация в МП 
activate U
activate app
opt
app --> U : Показать окно загрузки
end
app -> server : передача логина пользователя 
activate server
server -> dbC: проверка наличия логина
activate dbC
dbC ->> dbC: поиск логина
alt Login != NULL
dbC --> server : права и данные по логину 
else Login == NULL
dbC --> server: нет пользователя
End
deactivate dbC
server --> app : данные для отображению пользователя
deactivate server
app  ->> app : отображение
app  --> U: Экранная форма Главного экрана
U -> app: запрос на кредитнуцю карту
opt
app --> U : Показать окно загрузки
end
app -> server : передача запроса пользователя
activate server
par
server  ->> dbP  : запрос доступных продуктов
activate dbP
dbP  ->> dbP  : поиск доступных продуктов
dbP  -->> server  : доступные продукты
deactivate dbP  
server  ->> СС: проверка возможности выдачи КК
activate СС
СС ->> dbC : запрос данных клиента \n для скоринга 
dbC ->> dbC: поиск данных клиента
dbC -->> СС : передача данных клиента
СС->> SC : скоринг клиента
activate SC 
SC ->> SC: расчет скоринга клиента
end
server  --> app  : доступные продукты
deactivate server
app  ->> app : отображение
app  --> U  : показать доступные продукты
alt Выбор кредитной карты
app <- U : Выбрать продукт
else 
group neg
app <- U : Отказ от выпуска КК
end
end
app -> server : передача выбора пользователя
activate server
server -> СС : передача выбора пользователя
SC -->> СС : результаты скоринг клиента
deactivate SC 
СС ->> СС : решение о выдачи КК
СС ->> server : решение о выдачи КК
deactivate СС 
server -> app: решение о выдачи КК
deactivate server
app  -> app : отображение
app-> U : решение о выдачи КК
alt Подтверждение предложения КК
app <- U : Подтвердить выпуск
group neg
app <- U : Отказ от выпуска КК
end
end
app -> server : передача выбора пользователя
activate server
server ->> dbC: сохранение выбора пользователя
server ->> dbB: запрос доступных отделений для выдачи карты по городу пользователя
activate dbB
dbB->> dbB: поиск отделений
dbB->> server : информация о доступных отделенях для выдачи карты
deactivate dbB
server -> app : информация о доступных отделенях для выдачи карты
app  -> app : отображение информации о \n доступных отделенях для выдачи карты
app --> U: информация о доступных отделенях для выдачи карты
alt Выбор кредитной карты
app <- U : Выбрать отделение для получения
else 
loop
app <- U : Выбрать другое отделение
end
end
par
app -> server : передача выбора пользователя
ref over server : Выпуск карты
app --> U: Спасибо. Карта выпускается.
end
deactivate server
deactivate U
deactivate app
@enduml

//www.plantuml.com/plantuml/png/pLZ1Rbf75Ds_hxZDqfBp0LKJHOwoiwf85hjkNioBXOI10idQG9DMIbhKT579gchn0PK8XffY0xzmvuzwZkQIONWOluKjPV7CUpDppxtdtZDtHdxMQgVDzflJQdAQLcglc-bf9djxhVBUdkYVEjGPdYlHcJlJYOvrwhewruN-9vnnPwxhFeYEH6ym-5cdUkq-AXVSY2vXWm5y_0lRETwdINhShZT5pp4yvQ3hIjgIKd9ShxMRzQheNth1aRJHI1e8h79SQQIrjkX09u0RyI_dMrdpRTPCocaxVPMsCddn8qVBhvu7f7CzvnWi1s5mWB0NZ0lnN7Bww7a0UQGNc5hesFL0k8ktaVi0gZ_KY3R4o519v_AHT_PotNH0lbWm0lxFIhOp6khVC1k3whNFWS7zZfLpfj2W3fAIx3ybxM-5833oOLx1xtkalOyi--mmXvMtQJkJKdXblkgDTe9VsGTGIO1peGjVFyRkFWe_MHpXY5wYUZsypL2Mh5RsfayjipHVwiIgFWIZeNZkJ1q_blXWiQ-O9ZeWHuGPOh8Q6I2WKHx2CiU-aenE24Svm0TeHbu25YkB1oW6G_zOtz7PXaMrBOVraqfDldaYtvSE3xrPJCmA1TzJ8JrOpb7fYH1r3S_r9CcghSmGdsn51E0SPBgtfYHvWUoMiuZBAYc-vWWg3cDCEzGB4DXM37UrXznlj_kXZpZtn4REpCbMG9tOAge6I_xXNlUn0BelxYSgKM-OhJ_qced331Eq4FVhbyN3TG53NhxChYESY_i7qkhUayUgW_GUMhfHH3l4czDk8_sYNvFUIv5DWcF-a0PWKJ_t3lYRVIGWistWYV22c8kX6G0GB520v2nr4bkYKySg14H4Zfros_v4HmFX6sKVdtiVEXNsHiilFj1i0qK29VopsgwRAGjY9xrPE0nqoEveycS-GhF6r6qTO1UUjrdHswADdSQqeUqHASJnR2mDxB4smUFDO7K6FnruGel8Kuf1AEPG4kiDnKfx3vlVb1Xll2AksNFr4-O-azlkomBzoNVxtELcWyNcAiBrZJSvQTRVDAIMdKI6E5JjPa9MNejBj9KEHxqQaSg2lM6qYhnuPmYdsWvT5S8zYjL91BroFQwb8fYotS9RBpp0RD_5YEwNLRcHh_pE3JR6gfXLSEnzXPnGSSKnjghsJj5IiSK2zVgyK4ghwniReCR1PcbrMxkgkhwP1j7529GtLHjfx-jNg_LErm72EpkwBG_5e6EeWm9duI9t0RR-7XKgT0YxX5t2qDf10PwyGUC9KON38H-0hGnPviOAzvpLVNjnDntNnz1kEl6PUn1laeVsNMGpROc080kSnFSMX3zW7fhilHn3ERs4-xZ9i0V7rYJiaAhMwurTY6FULwbeOENXLvkVjytxQW3DxBNKYH20FeMkSrjr1i0ROi6K3XwZC_bLS1JtBDvbKAx-OuQTx-7h8wNr3_XAdi44_sxu7m00