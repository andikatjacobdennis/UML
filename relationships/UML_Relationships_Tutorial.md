# UML 2.5.1 Relationships

## Document Overview

This document provides a professional, verified explanation of key UML 2.5.1 relationships, based on the official OMG UML 2.5.1 specification (Document Number: formal/2017-12-05, December 2017). The content has been fact-checked against the provided PDF ("formal-17-12-05.pdf"), ensuring alignment with the standard. Explanations are grouped logically, with textual descriptions, UML 2.5.1 semantics notes, and corresponding PlantUML code for visualization. Separate diagrams are used for clarity, but a combined example is included at the end.

References to specific sections and pages from the UML 2.5.1 specification are included for traceability. This guide focuses on structural relationships (e.g., associations, dependencies) as per the query context.

---

## 1. Association Relationships

Associations in UML 2.5.1 (Section 11.5) represent structural relationships between classifiers, classifying sets of links between instances. They are bidirectional by default but can specify navigability and ownership.

### Association ("Knows-a / Has-a link")

A general structural relationship where instances of one classifier are linked to instances of another. It implies a semantic connection without implying ownership or lifecycle dependency.
**UML 2.5.1 Semantics**: Associations classify links; each end has a type and multiplicity. If derived, marked with "/". (Pages 241-246)

✅ **Example**: A **Professor** teaches one or more **Courses**.

* Courses exist even if a Professor is removed (they can be taught by another Professor).
* Professors exist even if a course is canceled.
* This is not a whole–part relationship — just a structural link.

**PlantUML Code**:

```plantuml
@startuml
class Professor {
  -name: String
}
class Course {
  -courseCode: String
}
Professor --> Course : teaches
@enduml
```

---

### Directed Association ("One-way Has-a")

A unidirectional association where navigability is specified from one end to the other (one class can access the other, but not vice versa).
**UML 2.5.1 Semantics**: Navigability is indicated by an arrow; non-navigable ends may be marked with "x". (Pages 243-244)

**PlantUML Code**:

```plantuml
@startuml
class Customer {
  -name: String
}
class Order {
  -orderId: String
}
Customer --> Order : places
@enduml
```

---

### Multiplicity ("How many")

Specifies the allowable number of instances in the relationship (e.g., 1, 0..\*, *).
**UML 2.5.1 Semantics**: Defined by lower and upper bounds; "*" means unlimited. Multiplicity constrains cardinality. (Section 7.5, Pages 32-35)

**PlantUML Code**:

```plantuml
@startuml
class Student {
  -studentId: String
}
class Course {
  -courseCode: String
}
Student "*" --> "*" Course : enrolls in
@enduml
```

---

## 2. Inheritance & Implementation

These define taxonomic and contractual relationships (Section 9.2 for Generalization, Section 10.5 for Interfaces).

### Generalization ("Is-a")

A relationship where a specific classifier inherits features from a more general one.
**UML 2.5.1 Semantics**: The specific classifier conforms to the general one; features are inherited. (Pages 141-145)

**PlantUML Code**:

```plantuml
@startuml
class Animal {
  +makeSound()
}
class Dog {
  +makeSound()
}
Dog --|> Animal : is a
@enduml
```

---

### Realization ("Implements")

A classifier realizes (implements) the contract defined by an interface.
**UML 2.5.1 Semantics**: The realizing classifier provides implementations for the interface's operations. (Pages 213-219)

**PlantUML Code**:

```plantuml
@startuml
interface Pet {
  +play()
}
class Dog {
  +play()
}
Dog ..|> Pet : implements
@enduml
```

---

## 3. Dependency & Containment

Dependencies indicate reliance; containment specifies whole-part relationships (Section 7.7 for Dependencies, Section 11.5 for Aggregation/Composition).

### Dependency ("Uses-a")

A weak, temporary relationship where one element depends on another for its specification or implementation.
**UML 2.5.1 Semantics**: Changes in the supplier may impact the client; often for method parameters. (Pages 79-82)

**PlantUML Code**:

```plantuml
@startuml
class Car {
  +navigate()
}
class GPSService {
  +getCoordinates()
}
Car ..> GPSService : uses
@enduml
```

---

### Aggregation ("Has-a (whole–part, independent lifecycle)")

A whole-part relationship where the part can exist independently of the whole (shared aggregation).
**UML 2.5.1 Semantics**: AggregationKind::shared; parts have independent lifecycles. (Pages 153-156, 244)

✅ **Example**: A **Library** contains **Books**.

* A book can exist outside the library (sold in a store, owned privately).
* Destroying the library does not destroy the books.

**PlantUML Code**:

```plantuml
@startuml
class Library {
  -name: String
}
class Book {
  -title: String
}
Library o-- Book : contains (independent)
@enduml
```

---

### Composition ("Part-of (dependent lifecycle)")

A strong whole-part relationship where parts are destroyed when the whole is destroyed (composite aggregation).
**UML 2.5.1 Semantics**: AggregationKind::composite; exclusive ownership, dependent lifecycle. (Pages 153-156, 244)

✅ **Example**: A **House** contains **Rooms**.

* A room cannot exist without the house.
* If the house is destroyed, its rooms are destroyed too.

**PlantUML Code**:

```plantuml
@startuml
class House {
  -address: String
}
class Room {
  -area: Double
}
House *-- Room : part of (dependent)
@enduml
```

---

## Grouped Summary

1. **Association Relationships**

   * Association: Structural "knows-a / has-a" (bidirectional by default).
   * Directed Association: Unidirectional navigability.
   * Multiplicity: Constrains instance counts (e.g., 1, 0..\*, \*).

2. **Inheritance & Implementation**

   * Generalization: Taxonomic "is-a" inheritance.
   * Realization: Contractual "implements" for interfaces.

3. **Dependency & Containment**

   * Dependency: Weak "uses-a" reliance.
   * Aggregation: Shared "has-a" (whole–part, but independent lifecycle).
   * Composition: Owned "part-of" (whole–part, dependent lifecycle).

---

## Combined University System Diagram

This example integrates all relationships in a University System (inspired by spec examples in Sections 7-11).

**PlantUML Code**:

```plantuml
@startuml
class University {
  -name: String
}
class Department {
  -deptId: String
}
University "1" --> "*" Department : contains

class Professor {
  -name: String
}
class Student {
  -studentId: String
}
class Course {
  -courseCode: String
}
Student "*" --> "*" Course : enrolls in
' Professors teach Courses (Association)
Professor "*" -- "*" Course : teaches
class Person {
  -name: String
}
Professor --|> Person : is a

interface Teach {
  +deliverLecture()
}
Professor ..|> Teach : implements

class LibraryService {
  +borrowBook()
}
Student ..> LibraryService : uses

Department o-- Professor : has (independent)

Department *-- Course : part of (dependent)
@enduml
```
