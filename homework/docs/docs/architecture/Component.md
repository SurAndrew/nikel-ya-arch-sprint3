```puml
@startuml
!includeurl https://raw.githubusercontent.com/RicardoNiepel/C4-PlantUML/master/C4_Component.puml

title 
   <b>'Smart Home' Components Diagram v2025.03.30</b>
   <i> Схема компонентов</i>
end title

top to bottom direction
'skinparam wrapWidth 300
'LAYOUT_WITH_LEGEND()
'LAYOUT_LANDSCAPE()
LAYOUT_TOP_DOWN()

System_Ext(HeatingSystem, "Топливная система", "Топливная система заказчика")
System_Ext(Sensor, "Температурный датчик", "Датчик температуры в доме")
System_Ext(Bank, "Bank System", "External bank for processing payments")
System_Ext(CloudDatabase, "Database", "Sensors data")

Container_Boundary(SmartHomeApplication, "'Smart Home' System") {
  Container(WebApp, "Web Application", "Java, Spring", "Handles user interactions")
  Container(MobileApp, "Mobile Application", "Kotlin, Swift", "Handles user interactions")
  Container(PaymentService, "Payment Service", "Node.js", "Processes payments")
  'Container(inDatabase, "Database", "PostgreSQL", "Stores user data and schedules")
  Container_Boundary(RDMBS, "'Smart Home' System") {
    Container(DatabaseUsersAndBilling, "Database", "PostgreSQL", "Stores user data and schedules")
    Container(DatabaseEmplrs, "Database", "PostgreSQL", "Stores user data and schedules")
    Container(DatabaseUsers, "Database", "PostgreSQL", "Stores user data and schedules")
  }
  Container(Billing, "BillingSystem", "Java", "Расчет услуг, формирование счетов, учет платежей")
  Container(InterConnect, "InterconnectSystem", "Java", "Работа с системами заказчиков")
  SystemQueue(KafkaStreaming, "Streaming", "Kafka", "Async взаимодействие сервисов")
  Container(api_gateway, "API Gateway,", "API for data integration")
}

' Взаимодействие компонент с внешними системами
Rel(PaymentService,Bank,"Calls business logic")

' WebApp
Container(WebApp, "Web Application", "Java, Spring") {
  Component(AuthControllerW, "AuthController", "Handles authentication and authorization")
  Component(UserControllerW, "UserController", "Manages user profiles")
  Component(ScheduleControllerW, "ScheduleController", "Manages schedules")
  Component(PaymentControllerW, "PaymentController", "Handles payment processing")
  Component(ServiceLayerW, "Service Layer", "Business logic")
  Component(RepositoryLayerW, "Repository Layer", "Data access logic")
  Component(API_controllerW, "APIController", "Работа с API")
  Component(KafkaControllerW, "Kafka Controller", "Асинхрнная работа с системой")
}
Rel(AuthControllerW,ServiceLayerW,"Calls business logic")
Rel(UserControllerW,ServiceLayerW,"Calls business logic")
Rel(ScheduleControllerW,ServiceLayerW,"Calls business logic")
Rel(PaymentControllerW,ServiceLayerW,"Calls business logic")
Rel(ServiceLayerW,RepositoryLayerW,"Reads/Writes data")
Rel(PaymentControllerW,PaymentService,"Calls business logic")
Rel(RepositoryLayerW,RDMBS,"Reads/Writes user data")
Rel(UserControllerW,KafkaControllerW,"Calls business logic")
Rel(ScheduleControllerW,KafkaControllerW,"Calls business logic")
Rel(PaymentControllerW,KafkaControllerW,"Calls business logic")
Rel(API_controllerW,api_gateway,"Calls business logic")
Rel(KafkaControllerW,KafkaStreaming,"Асинхронное взаимодействие")

' MobileApp
Container(MobileApp, "Mobile Application", "Kotlin, Swift") {
  Component(AuthControllerM, "AuthController", "Handles authentication and authorization")
  Component(UserControllerM, "UserController", "Manages user profiles")
  Component(ScheduleControllerM, "ScheduleController", "Manages schedules")
  Component(PaymentControllerM, "PaymentService", "Handles payment processing")
  Component(ServiceLayerM, "Service Layer", "Business logic")
  Component(RepositoryLayerM, "Repository Layer", "Data access logic")
  Component(API_controllerM, "APIController", "Работа с API")
  Component(KafkaControllerM, "Kafka Controller", "Асинхрнная работа с системой")
}

Rel(AuthControllerM,ServiceLayerM,"Calls business logic")
Rel(UserControllerM,ServiceLayerM,"Calls business logic")
Rel(ScheduleControllerM,ServiceLayerM,"Calls business logic")
Rel(PaymentControllerM,ServiceLayerM,"Calls business logic")
Rel(PaymentControllerM,PaymentService,"Calls business logic")
Rel(ServiceLayerM,RepositoryLayerM,"Reads/Writes data")
Rel(RepositoryLayerM,RDMBS,"Reads/Writes user data")
Rel(UserControllerM,KafkaControllerM,"Calls business logic")
Rel(ScheduleControllerM,KafkaControllerM,"Calls business logic")
Rel(PaymentControllerM,KafkaControllerM,"Calls business logic")
Rel(API_controllerM,api_gateway,"Calls business logic")
Rel(KafkaControllerM,KafkaStreaming,"Асинхронное взаимодействие")

' PaymentService
Container(PaymentService, "Payment Service", "Node.js") {
  Component(AuthControllerP, "AuthController", "Handles authentication and authorization")
  Component(PaymentControllerP, "PaymentService", "Handles payment processing")
  Component(RepositoryLayerP, "Repository Layer", "Data access logic")
  Component(API_controllerP, "APIController", "Работа с API")
  Component(KafkaControllerP, "Kafka Controller", "Асинхрнная работа с платежами")
}

Rel(AuthControllerP,PaymentControllerP,"Calls business logic")
Rel(PaymentControllerP,RepositoryLayerP,"Calls business logic")
Rel(PaymentControllerP,Bank,"Calls business logic")
Rel(API_controllerP,api_gateway,"Calls business logic")
Rel(KafkaControllerP,KafkaStreaming,"Асинхронное взаимодействие")

' Biling
Container(Billing, "BillingSystem", "Java") {
  Component(ScheduleControllerB, "ScheduleController", "Manages schedules")
  Component(BillingLayerB, "Service Layer", "Business logic")
  Component(RepositoryLayerB, "Repository Layer", "Data access logic")
  Component(API_controllerB, "APIController", "Работа с API")
  Component(KafkaControllerB, "Kafka Controller", "Data access logic")
}

Rel(ScheduleControllerB,BillingLayerB,"Периодичность расчетов")
Rel(BillingLayerB,PaymentControllerB,"Выставление счетов и учет оплат")
Rel(BillingLayerB,RepositoryLayerB,"Сохранение данных")
Rel(BillingLayerB,KafkaControllerB,"Подписка на инфо о получение оплат")
Rel(RepositoryLayerB,RDMBS,"Работа с СУБД")
Rel(PaymentControllerB,PaymentService,"Банковское обслуживание")
Rel(PaymentControllerB,KafkaControllerB,"Асинхронное взаимодействие")
Rel(KafkaControllerB,KafkaStreaming,"Асинхронное взаимодействие")


' InterConnect
Container(InterConnect, "InterconnectSystem", "Java") {
  Component(ScheduleControllerI, "ScheduleController", "Manages schedules")
  Component(ServiceLayerI, "Service Layer", "Business logic")
  Component(RepositoryLayerI, "Repository Layer", "Data access logic")
  Component(KafkaControllerI, "Kafka Controller", "Асинхронное взаимодействие")
}
Rel(ScheduleControllerI,ServiceLayerI,"Управление и снятие данных по расписанию")
Rel(ServiceLayerI,RepositoryLayerB,"Работа с СУБД")
Rel(RepositoryLayerB,CloudDatabase,"Работа с Облачной СУБД")
Rel(ServiceLayerI,KafkaControllerI,"Асинхронная передача данных")
Rel(ServiceLayerI,HeatingSystem,"Управлеие отоплением")
Rel(ServiceLayerI,Sensor,"считывание данных")
Rel(KafkaControllerI,KafkaStreaming,"Асинхронное взаимодействие")

@enduml
```