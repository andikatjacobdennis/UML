# UML Diagram Repository

This repository contains PlantUML templates (`.puml` files) and their generated PNG images for various UML diagrams.

## Diagram Templates

### 1. Use Case Diagrams

![Use Case Example](usecase/plantuml_usecase.svg)

```plantuml
@startuml
skinparam linetype polyline
skinparam linetype ortho
skinparam Shadowing false
skinparam Padding 10
skinparam NodePadding 10
skinparam ComponentPadding 10

left to right direction
skinparam packageStyle rectangle

actor "Actor 1" as ACT1
actor "Actor 2" as ACT2
actor "Actor 3" as ACT3
actor "External System" as EXT

ACT1 <|-- ACT2

package "SYSTEM NAME" {
  usecase "Use Case 1" as UC1
  usecase "Use Case 2" as UC2
  usecase "Use Case 3" as UC3
  usecase "Use Case 4" as UC4
  usecase "Use Case 5" as UC5
}

ACT1 --> UC1
ACT1 --> UC2
ACT2 --> UC3
ACT2 --> UC4
ACT3 --> UC5
UC4 --> UC5
UC2 --> EXT

note right of UC1
  Notes can describe preconditions, constraints,
  or additional info about the use case.
end note
@enduml
```

---

### 2. Activity Diagrams

![Activity Example](activity/plantuml_activity.svg)

```plantuml
@startuml
skinparam linetype polyline
skinparam Shadowing false
skinparam Padding 10
skinparam NodePadding 10
skinparam ComponentPadding 10

|Actor|
start
-> "Start Process";

:Initial Activity;

if ("Decision Point?") then (yes)
    :Activity for Yes;
    if ("Nested Decision?") then (yes)
        :Nested Yes Activity;
    else (no)
        :Nested No Activity;
    endif
else (no)
    :Activity for No;
endif

repeat
    :Looped Activity;
repeat while ("Loop Condition?")

fork
    |System A|
    :Parallel Activity A;
fork again
    |System B|
    :Parallel Activity B;
end fork

note right
    Notes can be attached to activities,
    decisions, or flows to give extra context.
end note

if ("Error Occurred?") then (yes)
    :Handle Error;
    stop
else (no)
    :Next Activity;
endif

-> "End Process";
stop
@enduml
```

---

### 3. Class Diagrams

![Class Example](class/plantuml_class.svg)

```plantuml
@startuml
skinparam linetype polyline
skinparam Shadowing false
skinparam Padding 10
skinparam NodePadding 10
skinparam ComponentPadding 10

hide empty members

enum ExampleEnum {
    + VALUE1
    + VALUE2
    + VALUE3
}

interface IExampleInterface {
    + {abstract} +exampleMethod(param: Type): ReturnType
}

abstract class AbstractExample {
    # -id: String
    # -createdAt: DateTime
    + +getId(): String
    + +getCreatedAt(): DateTime
}

class ExampleClass << (C,#FF7700) Entity >> {
    + -attribute1: Type
    + -attribute2: Type
    + +method1(): ReturnType
    + +method2(param: Type): ReturnType
}

AbstractExample <|-- ExampleClass
IExampleInterface <|.. ExampleClass
ExampleClass "1" --> "0..*" AnotherClass1
ExampleClass "1" o-- "0..*" AnotherClass2
ExampleClass "1" *-- "1..*" AnotherClass3

note right of ExampleClass
  This is an example note.
  You can describe purpose, constraints, or rules here.
end note
@enduml
```

---

### 4. Sequence Diagrams

![Sequence Example](sequence/plantuml_sequence.svg)

```plantuml
@startuml
title [SYSTEM NAME] — UML 2.5 Sequence Diagram Template

skinparam linetype polyline
skinparam Shadowing false
skinparam Padding 10
skinparam NodePadding 10
skinparam ComponentPadding 10

actor ACT as "ActorName" <<actor>>
participant BND as "Boundary Component" <<boundary>>
participant CTRL as "Control Component" <<control>>
participant DB as "Entity / DB" <<entity>>
participant EXT as "External System" <<external>>

create TEMP as "Temporary Object"
CTRL -> TEMP : initialize(params)
activate TEMP
TEMP --> CTRL : creationAck
deactivate TEMP

ACT -> BND : requestAction(parameters)
activate BND
BND -> CTRL : processRequest(data)
activate CTRL

alt Condition 1 (Yes)
    CTRL -> DB : queryData(params)
    activate DB
    DB --> CTRL : result
    deactivate DB
else Condition 2 (No)
    CTRL --> BND : errorMessage
    deactivate CTRL
    BND --> ACT : returnError
    deactivate BND
end

opt Optional Step
    CTRL -> EXT : callExternalService()
    activate EXT
    EXT --> CTRL : response
    deactivate EXT
end

loop Repeat N times
    CTRL -> DB : updateRecord()
    DB --> CTRL : ack
end

note right of CTRL
  Notes can describe logic, constraints,
  or explain optional/alternate flows.
end note

CTRL --> BND : finalResponse
deactivate CTRL
BND --> ACT : returnResult
deactivate BND
@enduml
```

---

### 5. Deployment Diagrams

![Deployment Example](deployment/plantuml_deployment.svg)

```plantuml
@startuml Deployment_Generic

skinparam linetype ortho
skinparam defaultFontName Arial
skinparam nodesep 50
skinparam ranksep 50
skinparam shadowing false

cloud "Cloud Environment" <<cloud>> as cloud {
  node "Compute Node" <<execution environment>> as app_node {
    component "Web Application" <<service>> as web_app
    database "Session Store" <<database>> as session_store
  }

  node "Database Node" <<execution environment>> as db_node {
    database "Database Engine\n«PostgreSQL»" <<database>> as db_engine
  }
}

rectangle "Clients" <<device>> as clients {
    artifact "Web Browser" <<client>> as browser
    artifact "Mobile App\n«iOS/Android»" <<client>> as mobile
}

web_app --> db_engine : "Data Queries\n«JDBC»"
browser --> web_app : "Dynamic Content\n«HTTPS»"
mobile --> web_app : "REST API\n«JSON»"

note right of clients
  <b>Client Types:</b>
  • Web Browser
  • Mobile Applications
end note
@enduml
```

## How to Use

### Option 1: Quick Preview

1. Copy any diagram code
2. Paste at [PlantText.com](https://www.planttext.com/) for instant rendering

### Option 2: Local Generation

1. [Install PlantUML](https://plantuml.com/download):

   ```bash
   # Requires Java (brew install openjdk / sudo apt install default-jdk)
   curl -L -o plantuml.jar https://github.com/plantuml/plantuml/releases/latest/download/plantuml.jar
   ```

2. Generate diagrams:

   ```bash
   java -jar plantuml.jar -tpng diagram.puml  # PNG output
   java -jar plantuml.jar -tsvg diagram.puml  # SVG output
   ```

### IDE Integration

- **VS Code**: Install "PlantUML" extension by jebbs
- **IntelliJ**: Install "PlantUML Integration" plugin

## Pro Tips

- Use `-checkmetadata` flag for incremental rendering
- Add `-verbose` flag for debugging generation issues
- For large diagrams: `-DPLANTUML_LIMIT_SIZE=8192`

## License

MIT Licensed - See [LICENSE](LICENSE) for details.