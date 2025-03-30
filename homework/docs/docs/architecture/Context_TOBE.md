```puml
@startuml
!includeurl https://raw.githubusercontent.com/RicardoNiepel/C4-PlantUML/master/C4_Context.puml

top to bottom direction
title 
   <b>'Smart Home' Context TO BE Diagram v2025.03.30</b>
   <i> Контекст TO BE</i>
end title

top to bottom direction
skinparam wrapWidth 300
'LAYOUT_WITH_LEGEND()
LAYOUT_TOP_DOWN()

Person(user, "User", "A user of the 'Smart Home'  Application")
Person(employee, "Employee of company", "An administrator managing the system")
System(SmartHomeApplication, "'Smart Home' System", "System SmartHome")

'System_Ext(AddHeatingSysytemToSmartHome, "Добавить оборудование", "Добавление оборудования")
System_Ext(HeatingSystem, "Топливная система", "Топливная система заказчика")
System_Ext(Sensor, "Температурный датчик", "Датчик температуры в доме")
System_Ext(Bank, "Bank System", "External bank for processing payments")
System_Ext(CloudDatabase, "Cloud Database", "Save sensor's data")

Rel(user, SmartHomeApplication, "Регистрируют и управляют своим оборудованием")
Rel(user, SmartHomeApplication, "Получают и оплачивают счета")
Rel(user, SmartHomeApplication, "Управляют отоплением")
Rel(user, SmartHomeApplication, "Проверяют температуру")
Rel(employee, SmartHomeApplication,"Администрирование и ТП")
Rel(SmartHomeApplication,HeatingSystem,"Включить/выключить отопление")
Rel(SmartHomeApplication,Sensor,"Получить данные температуры из системы")
Rel(SmartHomeApplication,CloudDatabase,"Saving sensor's data")
Rel(SmartHomeApplication,Bank,"Processing payments")

@enduml
```