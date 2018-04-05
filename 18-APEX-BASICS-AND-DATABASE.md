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


### Insert Records
```java
// Create the account sObject 
Account acct = new Account(Name='Acme', Phone='(415)555-1212', NumberOfEmployees=100);
// Insert the account by using DML
insert acct;
```

### ID Field Auto-Assigned to new Records
```java
// Create the account sObject 
Account acct = new Account(Name='Acme', Phone='(415)555-1212', NumberOfEmployees=100);
// Insert the account by using DML
insert acct;
// Get the new ID on the inserted sObject argument
ID acctID = acct.Id;
// Display this ID in the debug log
System.debug('ID = ' + acctID);
// Debug log result (the ID will be different in your case)
// DEBUG|ID = 001D000000JmKkeIAF
```

### Bulk DML
- You can perform DML operations either on a single sObject, or in bulk on a list of sObjects.
- Performing bulk DML operations is the recommended way because it helps avoid hitting governor limits, such as the DML limit of 150 statements per Apex transaction.

### Upsert Records
```java
// Insert the Josh contact
Contact josh = new Contact(FirstName='Josh',LastName='Kaplan',Department='Finance');       
insert josh;
// Josh's record has been inserted
//   so the variable josh has now an ID
//   which will be used to match the records by upsert
josh.Description = 'Josh\'s record has been updated by the upsert operation.';
// Create the Kathy contact, but don't persist it in the database
Contact kathy = new Contact(FirstName='Kathy',LastName='Brown',Department='Technology');
// List to hold the new contacts to upsert
List<Contact> contacts = new List<Contact> { josh, kathy };
// Call upsert
upsert contacts;
// Result: Josh is updated and Kathy is created.
```

### Upsert Records using idLookup field matching
```java
Contact jane = new Contact(FirstName='Jane',
                         LastName='Smith',
                         Email='jane.smith@example.com',
                         Description='Contact of the day');
insert jane;
// 1. Upsert using an idLookup field
// Create a second sObject variable.
// This variable doesn’t have any ID set.
Contact jane2 = new Contact(FirstName='Jane',
                         LastName='Smith',  
                         Email='jane.smith@example.com',
                         Description='Prefers to be contacted by email.');
// Upsert the contact by using the idLookup field for matching.
upsert jane2 Contact.fields.Email;
// Verify that the contact has been updated
System.assertEquals('Prefers to be contacted by email.',
                   [SELECT Description FROM Contact WHERE Id=:jane.Id].Description);
```

### Delete Records
```java
Contact[] contactsDel = [SELECT Id FROM Contact WHERE LastName='Smith']; 
delete contactsDel;
```


### DML Exception
```java
try {
    // This causes an exception because 
    //   the required Name field is not provided.
    Account acct = new Account();
    // Insert the account 
    insert acct;
} catch (DmlException e) {
    System.debug('A DML exception has occurred: ' +
                e.getMessage());
}
```

### Database Methods
- Apex contains the built-in Database class, which provides methods that perform DML operations and mirror the DML statement counterparts.

  - `Database.insert()`
  - `Database.update()`
  - `Database.upsert()`
  - `Database.delete()`
  - `Database.undelete()`
  - `Database.merge()`

- Unlike DML statements, Database methods have an optional `allOrNone` parameter that allows you to specify whether the operation should partially succeed. When this parameter is set to false, if errors occur on a partial set of records, the successful records will be committed and errors will be returned for the failed records. Also, no exceptions are thrown with the partial success option.

### Database Insert with allOrNone set to false
```java
Database.insert(recordList, false);
```
- By default, the `allOrNone` parameter is true, which means that the Database method behaves like its DML statement counterpart and will throw an exception if a failure is encountered.


### Database.SaveResult
```java
Database.SaveResult[] results = Database.insert(recordList, false);
```

- Upsert returns `Database.UpsertResult` objects, and delete returns `Database.DeleteResult` objects.

### Database Insert with partial success
```java
// Create a list of contacts
List<Contact> conList = new List<Contact> {
        new Contact(FirstName='Joe',LastName='Smith',Department='Finance'),
        new Contact(FirstName='Kathy',LastName='Smith',Department='Technology'),
        new Contact(FirstName='Caroline',LastName='Roth',Department='Finance'),
        new Contact()};
            
// Bulk insert all contacts with one DML call
Database.SaveResult[] srList = Database.insert(conList, false);
// Iterate through each returned result
for (Database.SaveResult sr : srList) {
    if (sr.isSuccess()) {
        // Operation was successful, so get the ID of the record that was processed
        System.debug('Successfully inserted contact. Contact ID: ' + sr.getId());
    } else {
        // Operation failed, so get all errors
        for(Database.Error err : sr.getErrors()) {
            System.debug('The following error has occurred.');
            System.debug(err.getStatusCode() + ': ' + err.getMessage());
            System.debug('Contact fields that affected this error: ' + err.getFields());
	 }
    }
}
```

### DML Statements vs Database Methods
- Use DML statements if you want any error that occurs during bulk DML processing to be thrown as an Apex exception that immediately interrupts control flow (by using try. . .catch blocks). This behavior is similar to the way exceptions are handled in most database procedural languages.

- Use Database class methods if you want to allow partial success of a bulk DML operation — if a record fails, the remainder of the DML operation can still succeed. Your application can then inspect the rejected records and possibly retry the operation. When using this form, you can write code that never throws DML exception errors. Instead, your code can use the appropriate results array to judge success or failure. Note that Database methods also include a syntax that supports thrown exceptions, similar to DML statements.


### Insert Related Records
```java
Account acct = new Account(Name='SFDC Account');
insert acct;
// Once the account is inserted, the sObject will be 
// populated with an ID.
// Get this ID.
ID acctID = acct.ID;
// Add a contact to this account.
Contact mario = new Contact(
    FirstName='Mario',
    LastName='Ruiz',
    Phone='415.555.1212',
    AccountId=acctID);
insert mario;
```

### Update Related Records
```java
// Query for the contact, which has been associated with an account.
Contact queriedContact = [SELECT Account.Name 
                          FROM Contact 
                          WHERE FirstName = 'Mario' AND LastName='Ruiz'
                          LIMIT 1];
// Update the contact's phone number
queriedContact.Phone = '(415)555-1213';
// Update the related account industry
queriedContact.Account.Industry = 'Technology';
// Make two separate calls 
// 1. This call is to update the contact's phone.
update queriedContact;
// 2. This call is to update the related account's Industry field.
update queriedContact.Account;
```

### Delete Related Records
```java
Account[] queriedAccounts = [SELECT Id FROM Account WHERE Name='SFDC Account'];
delete queriedAccounts;
```

### Account Handler Challenge
```java
public class AccountHandler {
	public static Account insertNewAccount(String accountName) {
		try {
			// Create account using accountName
			Account acct = new Account(Name=accountName);
			insert acct;
			// Return account object
			return acct;
		}
		catch(DmlException e) {
			System.debug('A DML exception has occurred: ' + e.getMessage());
			return null;
		}
	}
}
```

---

## Salesforce Object Query Language (SOQL)
- used to read saved records. SOQL is similar to the standard SQL language but is customized for the Lightning Platform.

- Because Apex has direct access to Salesforce records that are stored in the database, you can embed SOQL queries in your Apex code and get results in a straightforward fashion. When SOQL is embedded in Apex, it is referred to as inline SOQL.

### Inline SOQL
```java
Account[] accts = [SELECT Name,Phone FROM Account];
```

### Query Editor
- The Developer Console provides the Query Editor console, which enables you to run your SOQL queries and view results. The Query Editor provides a quick way to inspect the database. It is a good way to test your SOQL queries before adding them to your Apex code. When you use the Query Editor, you need to supply only the SOQL statement without the Apex code that surrounds it.


### Basic SOQL Syntax
```java
SELECT Name,Phone FROM Account
```

### SOQL Syntax with WHERE condition
```java
SELECT Name,Phone FROM Account WHERE Name='SFDC Computing'

SELECT Name,Phone FROM Account WHERE (Name='SFDC Computing' AND NumberOfEmployees>25)

SELECT Name,Phone FROM Account WHERE (Name='SFDC Computing' OR (NumberOfEmployees>25 AND BillingCity='Los Angeles'))
```

### SOQL Syntax with ORDER BY clause
```java
SELECT Name,Phone FROM Account ORDER BY Name

SELECT Name,Phone FROM Account ORDER BY Name ASC

SELECT Name,Phone FROM Account ORDER BY Name DESC
```

### SOQL Syntax with LIMIT clause
```java
SELECT Name,Phone FROM Account LIMIT 1
```

### SOQL Syntax combined
```java
SELECT Name,Phone FROM Account 
      WHERE (Name = 'SFDC Computing' AND NumberOfEmployees>25)
      ORDER BY Name
      LIMIT 10
```

### SOQL Inline combined
```java
Account[] accts = [SELECT Name,Phone FROM Account 
      WHERE (Name='SFDC Computing' AND NumberOfEmployees>25)
      ORDER BY Name
      LIMIT 10];
System.debug(accts.size() + ' account(s) returned.');
// Write all account array info
System.debug(accts);
```

### Accessing Variables in SOQL Queries
```java
String targetDepartment = 'Wingo';
Contact[] techContacts = [SELECT FirstName,LastName 
                          FROM Contact WHERE Department=:targetDepartment];
```

### Querying Related Records
```java
SELECT Name, (SELECT LastName FROM Contacts) FROM Account WHERE Name = 'SFDC Computing'
```

### Querying Related Records (inline)
```java
Account[] acctsWithContacts = [SELECT Name, (SELECT FirstName,LastName FROM Contacts)
      FROM Account 
      WHERE Name = 'SFDC Computing'];
// Get child records
Contact[] cts = acctsWithContacts[0].Contacts;
System.debug('Name of first associated contact: ' + cts[0].FirstName + ', ' + cts[0].LastName);
```

### Traversing a relationship from child object
```java
Contact[] cts = [SELECT Account.Name FROM Contact 
                 WHERE FirstName = 'Carol' AND LastName='Ruiz'];
Contact carol = cts[0];
String acctName = carol.Account.Name;
System.debug('Carol\'s account name is ' + acctName);
```

### Querying Record in Batches By Using SOQL For Loops
```java
insert new Account[]{new Account(Name = 'for loop 1'), 
                     new Account(Name = 'for loop 2'), 
                     new Account(Name = 'for loop 3')};
// The sObject list format executes the for loop once per returned batch
// of records
Integer i=0;
Integer j=0;
for (Account[] tmp : [SELECT Id FROM Account WHERE Name LIKE 'for loop _']) {
    j = tmp.size();
    i++;
}
System.assertEquals(3, j); // The list should have contained the three accounts
                       // named 'yyy'
System.assertEquals(1, i); // Since a single batch can hold up to 200 records and,
                       // only three records should have been returned, the 
                       // loop should have executed only once
```

---

## Salesforce Object Search Language (SOSL)
- a Salesforce search language that is used to perform text searches in records. Use SOSL to search fields across multiple standard and custom object records in Salesforce. SOSL is similar to Apache Lucene.

### Inline SOSL
```java
List<List<SObject>> searchList = [FIND 'SFDC' IN ALL FIELDS 
                                      RETURNING Account(Name), Contact(FirstName,LastName)];
```

### SOQL vs SOSL
- Like SOQL, SOSL allows you to search your organization’s records for specific information.
- Unlike SOQL, which can only query one standard or custom object at a time, a single SOSL query can search all objects.
- Another difference is that SOSL matches fields based on a word match while SOQL performs an exact match by default (when not using wildcards). For example, searching for 'Digital' in SOSL returns records whose field values are 'Digital' or 'The Digital Company', but SOQL returns only records with field values of 'Digital'.
- Use SOQL to retrieve records for a single object.
- Use SOSL to search fields across multiple objects. SOSL queries can search most text fields on an object.

### Query Editor
- The Developer Console provides the Query Editor console, which enables you to run SOSL queries and view results. The Query Editor provides a quick way to inspect the database. It is a good way to test your SOSL queries before adding them to your Apex code. When you use the Query Editor, you need to supply only the SOSL statement without the Apex code that surrounds it.


### Basic SOSL Syntax
```java
FIND 'SearchQuery' [IN SearchGroup] [RETURNING ObjectsAndFields]
```

- `SearchQuery` is the text to search for (a single word or a phrase). Search terms can be grouped with logical operators (AND, OR) and parentheses. Also, search terms can include wildcard characters (*, ?). The `* wildcard` matches zero or more characters at the middle or end of the search term. The `? wildcard` matches only one character at the middle or end of the search term.

- Text searches are case-insensitive. For example, searching for Customer, customer, or CUSTOMER all return the same results.

- `SearchGroup` is optional. It is the scope of the fields to search. If not specified, the default search scope is all fields. SearchGroup can take one of the following values.

  - ALL FIELDS
  - NAME FIELDS
  - EMAIL FIELDS
  - PHONE FIELDS
  - SIDEBAR FIELDS

- `ObjectsAndFields` is optional. It is the information to return in the search result — a list of one or more sObjects and, within each sObject, list of one or more fields, with optional values to filter against. If not specified, the search results contain the IDs of all objects found.



### SOSL Apex Example
```java
List<List<sObject>> searchList = [FIND 'Wingo OR SFDC' IN ALL FIELDS 
                   RETURNING Account(Name),Contact(FirstName,LastName,Department)];
Account[] searchAccounts = (Account[])searchList[0];
Contact[] searchContacts = (Contact[])searchList[1];
System.debug('Found the following accounts.');
for (Account a : searchAccounts) {
    System.debug(a.Name);
}
System.debug('Found the following contacts.');
for (Contact c : searchContacts) {
    System.debug(c.LastName + ', ' + c.FirstName);
}
```


### SOSL Query with WHERE condition
`RETURNING Account(Name, Industry WHERE Industry='Apparel')`

### SOSL Query with ORDER BY clause
`RETURNING Account(Name, Industry ORDER BY Name)`

### SOSL Query with LIMIT clause
`RETURNING Account(Name, Industry LIMIT 10)`

### Contact and Lead Search Challenge
```java
public class ContactAndLeadSearch {
	public static List<List<sObject>> searchContactsAndLeads(String nameParam) {
		List<List<sObject>> nameList = [FIND :nameParam IN NAME FIELDS
																		RETURNING Contact(FirstName,LastName),Lead(Name)];
		System.debug(nameList);
		return nameList;
	}
}
```