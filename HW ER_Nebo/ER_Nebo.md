#ER диаграмма в нотации Crow’s Foot предметной области для задачи "Заказ карты для физического лица через мобильное приложение с выдачей в отделении"

@startuml
entity "Customers" as e1{
*IdCustomer
--
**Login**
**Password**
Phone
}

entity "Mobile App" as e2{
*IdApp
--
**Version**
**Phone**
}

entity "Operations" as e4{
*IdOperation
--
* **IdCustomer** : <<FK>> Customers.IdCustomer
* **IdOperationType** : <<FK>> OperationTypes.IdOperationType
* **IdCreditСType** : <<FK>> CreditС.IdCreditСType
* **IdProduct** : <<FK>> Products.IdProduct
**Description**
}

entity "Eq" as e5{
*IdEq
--
* **IdCustomer** : <<FK>> Customers.IdCustomer
* **IdBranch** : <<FK>> Branches.IdBranch
**FirstName**
**LastName**
Middle Name
**Birthday**
**Passport**
**Phone**
**CreationDate**
**UpdateDate**
**Salary**
**Job**
**Products**
**City**
}

entity "OperationTypes" as e7{
*IdOperationType
--
**Name**
Description
**CreationDate**
}

entity "CreditС" as e8{
*IdCreditСType
--
**IdCustomer**
Description
**Product**
**Descision**
**CreationDate**
}

entity "Accounts" as e9{
*IdAccount
--
* **IdCustomer** : <<FK>> Customers.IdCustomer
**Number**
}

entity "Scoring" as e10{
*IdScoring
--
* **IdCreditСType** : <<FK>> CreditС.IdCreditСType
**Number**
}
entity "Products" as e11{
*IdProduct
--
**Name**
**Descision**
}
entity "Branches" as e12{
*IdBranch
--
**Name**
**Location**
**Operation_time**
}

e4 }o-- e1 : какие операции доступны
e4 }|-- e7 : какой тип операции выбрал
e1 --{ e5 : какими данными обладает Клиент
e2 -- e1 : где совершает операции
e4 }o-- e8 : какое решение получено \n по кредитному продукту
e9 }|-- e1 : имеет счета
e8 }|-- e10 : скоринг клиента \n по выбранному продукту
e4 --|{ e11 : какие продукты доступны \n для выбраной операции
e5 ||--o{ e12 : какие отдленеия \n есть в городе клиента
@enduml


//www.plantuml.com/plantuml/png/dLNTIjn06BtFKmnU1bRiOb6BY5eLscehsFQgK68oQC3DnCaiHLIeAxG52zxthc_WZu6jF-ihpBnHJsOcqqaiNdXHo9pvpZdVTxxPEb-8Y8j-RoUWcKZ57XbxsIy4wr5UZ96e8FJPVX2-Icemw7I2C5nbMsaMXlXQZuhY2-C93klRBAF1OU24rjXckaF9GfuRfQvMmj68V8H5oeoCvkBBOx_BAOy42cmVavhjKAr1Gg-rC2GloEpiofkvEU9c6FTQHxcly7ulfpwdSG5Yix8supH9XVpT9jVuk5_YMEkS9VrO-0GBmQU-HGnBj8XvcbUUpJ2MTqq8ptK8oxjFddwHHrcyxHSR10FNb-XZ9UM5U1lrg5xEQkGEQscIOANL2HUBAHVRIRJdNaBEkF3tYI-aeJDSYeG1FkG9Fjrn8zg9k55upJODsKPZ-9Y_cKLpJoQNAPrBuyOyCjkt5sohfwPslJkZEgrLQyo9-gctfTscxJPJyyGVDLs8OzRFX1rbHfjQx0bxnipztYOA6nuRCUDfjcL_qXFQmsBmUEhZzgoSKxqzQsN--x2WlvDcG9w0VP2rG5SB6B35NsMnZbGVtF8_YxHN9pn93bcd0nKC8w_b6VuDPKdaIDxBKdsLP-eRW26HLtAaZjH07UFYJfreva75d79CaFuGDK3z_Ny4BjI9FAy0UHF0hjFPnw_LTxtLDZZSLGRw29LpUGC8CDG6HFx24GsYOX3GBd6jNwAa9Ee8Z8lAL_smZ7OZ_qQUzXe75oKbM90s2Tm3lL77whk6HkHJfZ5GgahO3J4gBkIjEiODm96yGZpNLKW1dK4yTNTwk5AtWmmX2EPPW0PioGHgq3hwWCeGefTmSOEYrfbxATuzOZw9O0xseTnUQxFovC5QjT4LvZvjMc6p3vF4-Wum0AkCkkttWxQXexCh4TMfLYwrrqzeLniRcMPGtfmscATPWZ-uVm40