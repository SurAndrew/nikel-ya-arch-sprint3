```puml
@startuml
!includeurl https://raw.githubusercontent.com/RicardoNiepel/C4-PlantUML/master/C4_Component.puml

title 
   <b>'Smart Home' Code Diagram v2025.03.30</b>
end title

top to bottom direction
LAYOUT_TOP_DOWN()

class User {
  +String name
  +String email
  +void register()
  +void login()
}

class Device {
  +String name
  +String place
  +String DeviceType
  +void register()
  +void delete()
  +void add_data()
}

class DeviceType {
  +String DeviceType
}

class Schedule {
  +Date date
  +String activity
  +void addActivity()
  +void removeActivity()
}

class TelemetryData {
  +String Device
  +Date date
  +Json data
  +void takeData()
}

class Bill {
  +Date date
  +String activity
  +Int total
  +void addActivity()
  +void removeActivity()
}

User --> Device : generates
User --> Bill : affects
User --> Schedule : set
Device --> DeviceType : have
Device --> TelemetryData : have
Schedule --> TelemetryData : create

@enduml
```