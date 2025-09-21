### Types of Associations in UML 2.5.1

In the Unified Modeling Language (UML) 2.5.1 specification (document formal/2017-12-05, as provided), associations are structural relationships that define semantic links between two or more classifiers (e.g., classes, interfaces, or components). They describe how instances of these classifiers can be connected at runtime, forming "links" (tuples of instances).

Associations are detailed primarily in **Clause 11.5 (Classifications > Associations)** of the UML specification. An association has at least two **memberEnds** (represented by Properties), and it can be binary (two ends) or n-ary (more than two ends). Associations can also be **directed** (navigable in one direction) or **bidirectional**, and they include features like multiplicity, qualifiers, and ownership.

The key differentiation of association types is based on the **AggregationKind** enumeration (defined in Clause 11.5.3.1), which specifies the semantics of the relationship, particularly for whole-part hierarchies. Below, I summarize the different types of associations, drawing directly from the specification:

#### 1. **Regular Association (AggregationKind::none)**
   - **Description**: A basic structural relationship with no implied aggregation semantics. It simply connects classifiers without implying ownership, lifecycle dependency, or whole-part hierarchy. Instances at either end can exist independently.
   - **Semantics**: No special responsibility for existence or storage. Links are purely referential.
   - **Notation**: A solid line connecting classifiers, with optional arrowheads for navigability. Multiplicities (e.g., 1..*, 0..1) are shown at ends.
   - **Example**: A "Person" class associated with a "Company" class (a person works for a company, but neither owns the other).
   - **Use Case**: General relationships like dependencies or references.

#### 2. **Shared Aggregation (AggregationKind::shared)**
   - **Description**: Indicates a "whole-part" relationship where the part can be shared among multiple wholes. The whole does not exclusively own the part, and the part can exist independently if the whole is destroyed.
   - **Semantics**: The precise meaning varies by domain or modeler (e.g., it might imply loose coupling). No strong lifecycle dependency—the part's existence is not tied to the whole.
   - **Notation**: A solid line with a **hollow diamond** at the "whole" end.
   - **Example**: A "University" (whole) aggregates "Department" (part), but a department could theoretically be shared or exist outside one university.
   - **Use Case**: Loose compositions, like catalogs or collections where parts are reusable.

#### 3. **Composite Aggregation (Composition, AggregationKind::composite)**
   - **Description**: A strong "whole-part" relationship where the whole is responsible for the lifecycle of the parts. Parts are created, stored, and destroyed with the whole. Parts cannot be shared with other wholes.
   - **Semantics**: The composite object manages the existence and storage of composed objects (cross-referenced in Clause 9.5.3 for Composites). If the whole is deleted, parts are typically deleted (cascade deletion).
   - **Notation**: A solid line with a **filled (solid) diamond** at the "whole" end.
   - **Example**: A "Car" (whole) composes "Engine" (part)—the engine doesn't exist without the car in this context.
   - **Use Case**: Strong ownership, like object containment in software design (e.g., GUI windows composing buttons).

#### Additional Variations and Special Types
While the above are the core types based on aggregation, UML associations can have other characteristics that create variations:
- **Directed Association**: Navigable in one direction only (indicated by an arrowhead). The opposite end is not navigable.
- **Bidirectional Association**: Navigable in both directions (no arrowheads or arrowheads on both ends).
- **N-ary Association**: Involves more than two classifiers (e.g., a ternary association). Notated with a diamond in the middle connecting multiple lines.
- **Qualified Association**: One end has a qualifier (e.g., a key like an ID) to refine multiplicity. Notated with a small box at the qualified end.
- **AssociationClass**: An association that is also a class, allowing attributes, operations, or further associations on the relationship itself. Notated as a class rectangle connected by a dashed line to the association line.
  - **Semantics**: Combines association and class semantics (Clause 11.5.3.2).
  - **Example**: A "Enrollment" AssociationClass between "Student" and "Course", with attributes like "grade".
- **Derived Association**: Computed from other model elements (notated with a "/" prefix on the name).

#### Key Properties of Associations (from the Metamodel)
- **memberEnd**: The Properties at each end (at least 2).
- **ownedEnd**: Ends owned by the association itself (implies the association "owns" the relationship).
- **navigableOwnedEnd**: Ends that are navigable but owned by the association.
- **isDerived**: If true, the association is computed (Clause 11.5.3).
- Associations inherit from **Relationship** and **Classifier** in the metamodel (Clause 7.7 and 11.4).

#### Examples from the Specification
- **Figure 11.27**: Shows a binary association between "Window" and "Scrollbar".
- **Figure 11.29**: Demonstrates composition (Tree composing Leaves).
- **Figure 11.30**: Aggregation example (Polygon aggregating Points).

### Examples of Association Types in UML 2.5.1

Below, I'll provide practical examples for the main types of associations described in the UML 2.5.1 specification (Clause 11.5). These are illustrated using textual notation (pseudo-UML diagram descriptions) for clarity, as we can't render actual diagrams here. Each example includes:
- **Description**: Context and semantics.
- **Notation**: How it would appear in a class diagram.
- **Metamodel Reference**: Key elements from the spec (e.g., AggregationKind).

Assume we're modeling a simple library system with classes like `Book`, `Library`, `Author`, and `Shelf`.

#### 1. **Regular Association (AggregationKind::none)**
   - **Description**: A basic link between two independent classes. No ownership or lifecycle dependency. For instance, an `Author` writes many `Book`s, but neither owns the other—books can exist without an author in the model, and vice versa. This is the default association.
   - **Semantics**: Instances form links at runtime, but destruction of one doesn't affect the other. Navigability can be one-way (e.g., Book knows its Author, but not vice versa).
   - **Notation**:
     ```
     [Author] ----1..*---- 1..* [Book]
                  writes
     ```
     - Solid line with multiplicities (1..* means "one or more"). Optional arrowhead on the line if directed (e.g., → for navigability from Book to Author).
   - **Example Scenario**: Querying books by author. In code: A `Book` object has a reference to an `Author` object.
   - **Metamodel Reference**: Association::aggregation = none (Clause 11.5.3.1). See Figure 11.27 in the spec for a similar binary association example.

#### 2. **Shared Aggregation (AggregationKind::shared)**
   - **Description**: A weak "whole-part" relationship where parts can be shared across wholes and exist independently. For example, a `Library` aggregates `Book`s, but books can be shared among multiple libraries (e.g., via interlibrary loans) or exist outside any library.
   - **Semantics**: The whole (`Library`) doesn't control the lifecycle of parts (`Book`). Parts can be removed without destroying the whole.
   - **Notation**:
     ```
     [Library] ◇-----1..*----- 1..* [Book]
                 contains
     ```
     - Hollow diamond (◇) at the whole end (`Library`). Multiplicities indicate one library can contain many books, and a book can be in many libraries.
   - **Example Scenario**: Books in a network of libraries. In code: `Library` has a collection of `Book` references, but deleting a library doesn't delete the books.
   - **Metamodel Reference**: Association::aggregation = shared (Clause 11.5.3.1). Compare to Figure 11.28 in the spec for aggregation notation.

#### 3. **Composite Aggregation (Composition, AggregationKind::composite)**
   - **Description**: A strong "whole-part" relationship with exclusive ownership. For example, a `Shelf` composes `Book`s—the shelf owns the books, and if the shelf is destroyed, the books on it are too. Books can't be shared with other shelves.
   - **Semantics**: The whole (`Shelf`) manages creation, storage, and deletion of parts (`Book`). Multiplicity at the whole end is typically 1 (no sharing).
   - **Notation**:
     ```
     [Shelf] ◆-----1----- 1..* [Book]
                holds
     ```
     - Filled diamond (◆) at the whole end (`Shelf`). The 1 at the shelf end ensures exclusivity.
   - **Example Scenario**: Physical shelves in a library where books are fixed to one shelf. In code: `Shelf` owns a list of `Book` objects; destroying the shelf cascades to delete its books.
   - **Metamodel Reference**: Association::aggregation = composite (Clause 11.5.3.1). See Figure 11.29 in the spec for composition (e.g., Tree-Leaves).

#### Additional Variations with Examples

- **Directed Association** (Variant of Regular):
  - **Description**: Navigable only from one end. E.g., `Book` knows its `Publisher`, but `Publisher` doesn't reference books.
  - **Notation**:
    ```
    [Book] →-----1----- 0..* [Publisher]
              publishedBy
    ```
  - **Example**: One-way reference for lookup. Semantics: Only Book instances can traverse to Publisher.

- **AssociationClass** (Enhances any Association Type):
  - **Description**: The association itself is a class with attributes/operations. E.g., the "borrows" relationship between `Library` and `Member` has a `Loan` class with `dueDate` attribute.
  - **Semantics**: Treats the relationship as an entity (Clause 11.5.4).
  - **Notation**:
    ```
    [Library] ----1-----o----- * [Member]
                   borrows
                       |
                       | (dashed line)
                    [Loan]
                    | dueDate: Date
    ```
  - **Example Scenario**: Tracking loan details. In code: `Loan` instances link `Library` and `Member`, storing extra data like fines.

- **N-ary Association** (Multi-End Association):
  - **Description**: Involves more than two classes. E.g., a ternary association between `Book`, `Author`, and `Translator` for collaborative works.
  - **Notation**:
    ```
          [Book]
            |
    diamond---|----- [Author]
            |
           [Translator]
    ```
    - Central diamond connects three lines.
  - **Example**: Collaborative edition. Semantics: Links form tuples of one instance from each class.

These examples align with UML 2.5.1's metamodel (Clause 11.5), where associations are classifiers with memberEnds (Properties). For implementation, tools like Enterprise Architect or PlantUML can render these. If you'd like PlantUML code, more complex examples, or focus on a specific variation, let me know!### Examples of Association Types in UML 2.5.1

Below, I'll provide concrete examples for each type of association discussed in the UML 2.5.1 specification (Clause 11.5: Classifications > Associations). These examples illustrate real-world or software modeling scenarios, including textual descriptions, metamodel references, and simple textual notations (since UML diagrams can't be rendered here directly). For visual notation, refer to the specification's figures (e.g., Figure 11.27 for basic associations, Figure 11.29 for composition).

#### 1. **Regular Association (AggregationKind::none)**
   - **Scenario**: Modeling a relationship between a "Customer" and an "Order" where a customer can place multiple orders, but neither owns the other. Orders exist independently and can be queried via the customer.
   - **Metamodel Details**: Association between two Classes (Customer and Order). Multiplicity: Customer side (1), Order side (0..*). Navigable in both directions (bidirectional). No aggregation.
   - **Semantics**: Pure reference; deleting a customer doesn't affect orders (unless business rules specify otherwise).
   - **Textual Notation**:
     ```
     [Customer] --- (places) --- [Order]
     Multiplicity: 1          0..*
     ```
   - **UML Diagram Equivalent**: Solid line connecting "Customer" rectangle to "Order" rectangle, with labels "places" and multiplicities at ends.

#### 2. **Shared Aggregation (AggregationKind::shared)**
   - **Scenario**: A "Library" aggregates "Books" that can be shared across multiple libraries (e.g., inter-library loans). Books can exist independently and be transferred.
   - **Metamodel Details**: Association with AggregationKind::shared at the Library end. Multiplicity: Library (1), Book (0..*). The "whole" (Library) end has the hollow diamond.
   - **Semantics**: Weak ownership; books aren't destroyed if the library closes, but the relationship implies shared management.
   - **Textual Notation**:
     ```
     [Library] ◇--- (contains) --- [Book]
     Multiplicity: 1             0..*
     (◇ = hollow diamond for shared)
     ```
   - **UML Diagram Equivalent**: Solid line with a hollow diamond at the Library end, connecting to Book rectangle.

#### 3. **Composite Aggregation (Composition, AggregationKind::composite)**
   - **Scenario**: A "House" composes "Rooms" (e.g., kitchen, bedroom). Rooms are created and destroyed with the house; they can't be shared or exist separately.
   - **Metamodel Details**: Association with AggregationKind::composite at the House end. Multiplicity: House (1), Room (1..*). The "whole" (House) owns the lifecycle of rooms.
   - **Semantics**: Strong ownership; destroying the house cascades to rooms. Rooms are stored within the house instance.
   - **Textual Notation**:
     ```
     [House] ◆--- (has) --- [Room]
     Multiplicity: 1        1..*
     (◆ = filled diamond for composite)
     ```
   - **UML Diagram Equivalent**: Solid line with a filled black diamond at the House end, connecting to Room rectangle.

#### Additional Variations with Examples
These build on the core types and can combine with them.

- **Directed Association**:
  - **Scenario**: A "BankAccount" knows its "Owner" (Person), but the Person doesn't need to know about specific accounts.
  - **Textual Notation**:
    ```
    [Person] <-- (ownedBy) --- [BankAccount]
    Multiplicity: *              1
    (Arrow points to Person, indicating navigability from Account only)
    ```
  - **Metamodel**: One navigableOwnedEnd at the Person side.

- **N-ary Association**:
  - **Scenario**: A ternary relationship between "Supplier", "Part", and "Project" where a supplier provides parts to a project.
  - **Textual Notation**:
    ```
         [Supplier]
            | 
    (suppliesTo) ◇--- [Part] --- (forProject) --- [Project]
    Multiplicity: 1    0..*         1..*
    ```
  - **Metamodel**: Association with three memberEnds; central diamond notation.

- **Qualified Association**:
  - **Scenario**: A "Bank" qualifies access to "Accounts" via an "accountNumber" (qualifier reduces multiplicity from 0..* to 0..1).
  - **Textual Notation**:
    ```
    [Bank] --- [Account]
               {accountNumber: String}
    Multiplicity: 1          0..1 (qualified)
    ```
  - **Metamodel**: Qualifier property on the Bank end.

- **AssociationClass**:
  - **Scenario**: "Marriage" as an AssociationClass between "Person" and "Person", with attributes like "weddingDate".
  - **Textual Notation**:
    ```
    [Person] --- [Marriage] --- [Person]
               | weddingDate: Date
               | spouseOf
    Multiplicity: 0..1         1
    ```
  - **Metamodel**: Association that specializes Class; dashed line from Marriage class to the association line.

- **Derived Association**:
  - **Scenario**: "TotalSalary" derived from "Employee" to "Department" based on individual salaries.
  - **Textual Notation**:
    ```
    [Employee] --- /totalSalary --- [Department]
    (Derived: sum of employee salaries)
    ```
  - **Metamodel**: isDerived = true; often with OCL derivation rule.
