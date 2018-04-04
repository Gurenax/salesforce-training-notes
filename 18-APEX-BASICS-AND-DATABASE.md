# Apex Basics & Database

## References
- [Apex Basics & Database](https://trailhead.salesforce.com/trails/force_com_dev_beginner/modules/apex_database)
- [Apex Developer Guide](https://developer.salesforce.com/docs/atlas.en-us.212.0.apexcode.meta/apexcode/apex_dev_guide.htm)

## What is Apex?
- Apex is a programming language that uses Java-like syntax and acts like database stored procedures.

- Apex enables developers to add business logic to system events, such as button clicks, updates of related records, and Visualforce pages.

- As a language, Apex is:
  - `Hosted` — Apex is saved, compiled, and executed on the server—the Lightning Platform.
  - `Object oriented` — Apex supports classes, interfaces, and inheritance.
  - `Strongly typed` — Apex validates references to objects at compile time.
  - `Multitenant aware` — Because Apex runs in a multitenant platform, it guards closely against runaway code by enforcing limits, which prevent code from monopolizing shared resources.
  - `Integrated with the database` — It is straightforward to access and manipulate records. Apex provides direct access to records and their fields, and provides statements and query languages to manipulate those records.
  - `Data focused` — Apex provides transactional access to the database, allowing you to roll back operations.
  - `Easy to use` — Apex is based on familiar Java idioms.
  - `Easy to test` — Apex provides built-in support for unit test creation, execution, and code coverage. Salesforce ensures that all custom Apex code works as expected by executing all unit tests prior to any platform upgrades.
  - `Versioned` — Custom Apex code can be saved against different versions of the API.

## Apex Data Types
### Primitives
- Integer
- Double
- Long
- Date
- Datetime
- String
- ID
- Boolean

### sObject
- e.g. Account, Contact, or MyCustomObject__c
- can also be a generic sObject

### Collections
- List
- Set
- Map

### Enum
### User-defined Apex classes
### System-supplied Apex classes

---

## Data Manipulation Language (DML)
- Create and modify records in Salesforce by using the Data Manipulation Language, abbreviated as DML.
- DML provides a straightforward way to manage records by providing simple statements to insert, update, merge, delete, and restore records.

### DML Statements
- insert
- update
- upsert
  - DML operation creates new records and updates sObject records within a single statement, using a specified field to determine the presence of existing objects, or the ID field if no field is specified.
- delete
- undelete
- merge
  - merges up to three records of the same sObject type into one of the records, deleting the others, and re-parenting any related records.

### Bulk DML
- You can perform DML operations either on a single sObject, or in bulk on a list of sObjects.
- Performing bulk DML operations is the recommended way because it helps avoid hitting governor limits, such as the DML limit of 150 statements per Apex transaction.

