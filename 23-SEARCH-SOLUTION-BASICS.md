# Search Solution Basics

- Search is the no.1 most used Salesforce feature.
- The `search index` and `tokens` allow the search engine to apply advanced features like spell correction, nicknames, lemmatization, and synonym groups.
- `Lemmatization` identifies and returns variants of the search term, such as `add`, `adding`, and `added`, in search results.)
- The search index also provides the opportunity to introduce relevance ranking into the mix.
- In general, you need a `custom search solution` when your org uses a custom UI instead of the standard Salesforce UI.

## Connect to Search with APIs
### Two main APIs
- `Salesforce Object Query Language (SOQL)`
- `Salesforce Object Search Language (SOSL)`

### Other APIs
- `Suggested records API` - e.g. auto-suggestion, instant results, type-ahead
- `Salesforce Federated Search` - search records stored outside of Salesforce.

### SOQL vs SOSL
- Use SOQL when you know in which objects or fields the data resides and you want to:
  1. Retrieve data from a single object or from multiple objects that are related to one another.
  2. Count the number of records that meet specified criteria.
  3. Sort results as part of the query.
  4. Retrieve data from number, date, or checkbox fields.

- Use SOSL when you don’t know in which object or field the data resides and you want to:

  1.  Retrieve data for a specific term that you know exists within a field. Because SOSL can tokenize multiple terms within a field and build a search index from this, SOSL searches are faster and can return more relevant results.
  2. Retrieve multiple objects and fields efficiently, and the objects might or might not be related to one another.
  3. Retrieve data for a particular division in an organization using the divisions feature, and you want to find it in the most efficient way possible.


### Send Queries with Protocols
- `Query (REST) and query() (SOAP)` — Executes a SOQL query against the specified object and returns data that matches the specified criteria.
- `Search (REST) and search() (SOAP)` — Executes a SOSL text string search against your org’s data.
- [REST API Developer Guide](https://developer.salesforce.com/docs/atlas.en-us.212.0.api_rest.meta/api_rest/intro_what_is_rest_api.htm)

---

## Build Search for Common Use Cases
### Search Within a Single Object (SOSL)
- Syntax
```java
FIND {term} RETURNING ObjectTypeName
```
- Example
```java
FIND {march 2016 email} RETURNING Campaign
```

### Search Within Multiple Objects
- Syntax
```java
FIND {term} RETURNING ObjectTypeName1, ObjectTypeName2, ObjectTypeNameYouGetTheIdea
```
- Example
```java
FIND {recycled materials} RETURNING Product2, ContentVersion, FeedItem
```

### Search Within Custom Objects
- Example
```java
FIND {pink hi\-top} RETURNING Merchandise__c
```

### SOQL?
- You use SOQL for single object searches, when you know the fields to be searched, when the search term is an exact match for the field (no partial or out-of-order matches), when you need number, date, or checkbox field data, and when you’re looking for just a few results

---

## Optimize Search Results

### Create Efficient Text Searches
- Search queries can be expensive. The more data you’re searching through and the more results you’re returning, the more you can slow down the entire operation.
- Basic strategies:
  - Limit which data you’re searching through
  - Limit which data you’re returning

### Search by SearchGroup (e.g. EMAIL FIELDS)
- Example
```java
FIND {jsmith@cloudkicks.com} IN EMAIL FIELDS RETURNING Contact
```

### Specify the object to return.	
```java
FIND {Cloud Kicks} RETURNING Account
```

### Specify the field to return.	
```java
FIND {Cloud Kicks} RETURNING Account(Name, Industry)
```

### Order the results by field in ascending order, which is the default.	
```java
FIND {Cloud Kicks} RETURNING Account (Name, Industry ORDER BY Name)
```

### Set the max number of records returned	
```java
FIND {Cloud Kicks} RETURNING Account (Name, Industry ORDER BY Name LIMIT 10)
```

### Set the starting row offset into the results.	
```java
FIND {Cloud Kicks} RETURNING Account (Name, Industry ORDER BY Name LIMIT 10 OFFSET 25)
```

### WITH DIVISION
```java
FIND {Cloud Kicks} RETURNING Account (Name, Industry)
    WITH DIVISION = 'Global'
```

### WITH DATA CATEGORY
```java
FIND {race} RETURNING KnowledgeArticleVersion
    (Id, Title WHERE PublishStatus='online' and language='en_US')
    WITH DATA CATEGORY Location__c AT America__c
```

### WITH NETWORK
```java
FIND {first place} RETURNING User (Id, Name),
FeedItem (id, ParentId WHERE CreatedDate = THIS_YEAR Order by CreatedDate DESC)
WITH NETWORK = '00000000000001'
```

### WITH PRICEBOOK
```java
Find {shoe} RETURNING Product2 WITH PricebookId = '01sxx0000002MffAAE'
```

## Display Suggested Results

### Go-to REST resources
1. `Search Suggested Records` — Returns a list of suggested records whose names match the user’s search string. The suggestions resource provides a shortcut for users to navigate directly to likely relevant records, before performing a full search.
2. `Search Suggested Article Title Matches` — Returns a list of Salesforce Knowledge articles whose titles match the user’s search query string. Provides a shortcut to navigate directly to likely relevant articles before the user performs a search.
3. `SObject Suggested Articles for Case` — Returns a list of suggested Salesforce Knowledge articles for a case.

- Syntax for Search Suggested Article Title Matches
```
/vXX.X/search/suggestTitleMatches?q=search string&language=article language&publishStatus=article publication status
```
- Example for Search Suggested Article Title Matches
```
/vXX.X/search/suggestTitleMatches?q=race+tips&language=en_US&publishStatus=Online
```
- Response JSON
```javascript
{
  "autoSuggestResults" : [ {
    "attributes" : {
    "type" : "KnowledgeArticleVersion",
    "url" : "/services/data/v30.0/sobjects/KnowledgeArticleVersion/ka0D00000004CcQ"
    },
  "Id" : "ka0D00000004CcQ",
  "UrlName" : "tips-for-your-first-trail-race",
  "Title" : "race tips",
  "KnowledgeArticleId" : "kA0D00000004Cfz",
  "isMasterLanguage" : "1",
  } ],
  "hasMoreResults" : false
}
```

---

## Synonyms
- A search for one term in a synonym group returns results for all terms in the group. For example, a search for USB returns results for all terms in the synonym group, which contains `USB`, `thumb drive`, `flash stick`, and `memory stick`.
- Setup | Quick Find box | Synonyms
- The maximum number of characters per term is 100.
- Your organization can create a maximum of 2,000 promoted terms.
