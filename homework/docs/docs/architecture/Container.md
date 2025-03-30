```puml
@startuml
!includeurl https://raw.githubusercontent.com/RicardoNiepel/C4-PlantUML/master/C4_Container.puml

title 
   <b>'Smart Home' Container Diagram v2025.03.30</b>
   <i> Схема контейнеров</i>
end title

top to bottom direction
skinparam wrapWidth 300
LAYOUT_WITH_LEGEND()
'LAYOUT_LANDSCAPE()
LAYOUT_TOP_DOWN()


Person(user, "User", "A user of the 'Smart Home'  Application")
Person(employee, "Employee of company", "An administrator managing the system")
System(SmartHomeApplication, "'Smart Home' System", "System SmartHome")

Container_Boundary(SmartHomeApplication, "'Smart Home' System") {
  Container(WebApp, "Web Application", "Java, Spring", "Handles user interactions")
  Container(MobileApp, "Mobile Application", "Kotlin, Swift", "Handles user interactions")
  Container(PaymentService, "Payment Service", "Node.js", "Processes payments")
  'Container(inDatabase, "Database", "PostgreSQL", "Stores user data and schedules")
  Container_Boundary(RDMBS, "'Smart Home' System") {
    Container(DatabaseUsersAndBilling, "Database", "PostgreSQL", "Stores user data and schedules")
    Container(DatabaseEpl, "Database", "PostgreSQL", "Stores user data and schedules")
    Container(DatabaseUsers, "Database", "PostgreSQL", "Stores user data and schedules")
  }
  Container(Billing, "BillingSystem", "Java", "Расчет услуг, формирование счетов, учет платежей")
  Container(InterConnect, "InterconnectSystem", "Java", "Работа с системами заказчиков")
  SystemQueue(KafkaStreaming, "Streaming", "Kafka", "Async взаимодействие сервисов")
  Container(api_gateway, "API Gateway,", "External API for data integration")
}

'Container_Boundary(RDMBS, "'Smart Home' System") {
'  Container(DatabaseUsersAndBilling, "Database", "PostgreSQL", "Stores user data and schedules")
'  Container(DatabaseEpl, "Database", "PostgreSQL", "Stores user data and schedules")
'  Container(DatabaseUsers, "Database", "PostgreSQL", "Stores user data and schedules")
'}


System_Ext(HeatingSystem, "Топливная система", "Топливная система заказчика")
System_Ext(Sensor, "Температурный датчик", "Датчик температуры в доме")
System_Ext(Bank, "Bank System", "External Bank for processing payments")
System_Ext(CloudDatabase, "Database", "Sensors data")


' Юзеры в системе
Rel(user, WebApp, "Uses the system")
Rel(employee, WebApp,"Manages the system")
Rel(user, MobileApp, "Uses the system")
Rel(employee, MobileApp,"Manages the system")

' Система внутри
Rel(WebApp,api_gateway,"Взаимодействие с системой")
Rel(MobileApp,api_gateway,"Взаимодействие с системой")
Rel(WebApp,KafkaStreaming,"Асинхронное взаимодействие")
Rel(MobileApp,KafkaStreaming,"Асинхронное взаимодействие")

' Кафка работает с интерконнектом
Rel(KafkaStreaming,InterConnect,"Упревление оборудованием Клиента")
Rel(InterConnect,KafkaStreaming,"Упревление оборудованием Клиента")


Rel(api_gateway,InterConnect,"Упревление оборудованием Клиента")
Rel_L(InterConnect,HeatingSystem,"Включить/выключить отопление")
Rel_L(InterConnect,Sensor,"Получить данные температуры из системы")

' Работаем с СУБД
Rel(api_gateway,RDMBS,"Упревление оборудованием Клиента")
Rel(api_gateway,CloudDatabase,"Управление оборудованием Клиента")

' Работаем с банками
Rel(Billing,api_gateway,"Выставление счетов, учет оплат")
Rel(api_gateway,PaymentService,"Взаимодействие с платежной системой")
Rel(PaymentService,Bank,"Processes payment requests")

'Rel(user, SmartHomeApplication, "Управляют отоплением")
'Rel(user, SmartHomeApplication, "Проверяют температуру")
'Rel(employee, SmartHomeApplication,"Добавляют топливную систтему Заказчиков к SmartHome")
''Rel(SmartHomeApplication,AddHeatingSysytemToSmartHome,"Добавить оборудование в систему")

'Rel(SmartHomeApplication,RDBMS,"Сохранить, получить данные")

@enduml
```