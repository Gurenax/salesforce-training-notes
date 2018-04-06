# Apex Triggers
- Apex triggers enable you to perform custom actions before or after events to records in Salesforce, such as insertions, updates, or deletions. Just like database systems support triggers, Apex provides trigger support for managing records.

## Reference
- [Apex Trigger](https://trailhead.salesforce.com/trails/force_com_dev_beginner/modules/apex_triggers)
- [Triggers](https://developer.salesforce.com/docs/atlas.en-us.212.0.apexcode.meta/apexcode/apex_triggers.htm)
- [Invoking Callouts Using Apex](https://developer.salesforce.com/docs/atlas.en-us.212.0.apexcode.meta/apexcode/apex_callouts.htm)
- [Apex Callouts](https://developer.salesforce.com/page/Apex_Web_Services_and_Callouts#Apex_Callouts)

## Trigger Syntax
```java
trigger TriggerName on ObjectName (trigger_events) {
   code_block
}
```

- Trigger Events
  - before insert
  - before update
  - before delete
  - after insert
  - after update
  - after delete
  - after undelete

### Sample Trigger on Account Creation
```java
trigger HelloWorldTrigger on Account (before insert) {
	System.debug('Hello World!');
}
```
- Test the trigger using `Execute Anonymous Window`
```java
Account a = new Account(Name='Test Trigger');
insert a;
```

## Types of Triggers

### `Before triggers`
- are used to update or validate record values before they’re saved to the database.

### After triggers
- are used to access field values that are set by the system (such as a record's Id or LastModifiedDate field), and to affect changes in other records. The records that fire the after trigger are read-only.

## Context Variables
- `Trigger.New` contains all the records that were inserted in insert or update triggers.
- `Trigger.Old` provides the old version of sObjects before they were updated in update triggers, or a list of deleted sObjects in delete triggers.

### Iterate over each account in a for loop and update the Description field for each.
```java
trigger HelloWorldTrigger on Account (before insert) {
    for(Account a : Trigger.New) {
        a.Description = 'New description';
    }   
}
```
- The system saves the records that fired the before trigger after the trigger finishes execution. You can modify the records in the trigger without explicitly calling a DML insert or update operation. If you perform DML statements on those records, you get an error.

### Boolean Context Variables
```java
trigger ContextExampleTrigger on Account (before insert, after insert, after delete) {
    if (Trigger.isInsert) {
        if (Trigger.isBefore) {
            // Process before insert
        } else if (Trigger.isAfter) {
            // Process after insert
        }        
    }
    else if (Trigger.isDelete) {
        // Process after delete
    }
}
```

### List of Context Variables
- `isExecuting`	Returns true if the current context for the Apex code is a trigger, not a Visualforce page, a Web service, or an executeanonymous() API call.
- `isInsert`	Returns true if this trigger was fired due to an insert operation, from the Salesforce user interface, Apex, or the API.
- `isUpdate`	Returns true if this trigger was fired due to an update operation, from the Salesforce user interface, Apex, or the API.
- `isDelete`	Returns true if this trigger was fired due to a delete operation, from the Salesforce user interface, Apex, or the API.
- `isBefore`	Returns true if this trigger was fired before any record was saved.
- `isAfter`	Returns true if this trigger was fired after all records were saved.
- `isUndelete`	Returns true if this trigger was fired after a record is recovered from the Recycle Bin (that is, after an undelete operation from the Salesforce user interface, Apex, or the API.)
- `new`	Returns a list of the new versions of the sObject records.
  - This sObject list is only available in insert, update, and undelete triggers, and the records can only be modified in before triggers.

`newMap`	A map of IDs to the new versions of the sObject records.
  - This map is only available in before update, after insert, after update, and after undelete triggers.

`old`	Returns a list of the old versions of the sObject records.
  - This sObject list is only available in update and delete triggers.

`oldMap`	A map of IDs to the old versions of the sObject records.
  - This map is only available in update and delete triggers.

`size`	The total number of records in a trigger invocation, both old and new.


### Calling a Class Method from a Trigger
```java
trigger ExampleTrigger on Contact (after insert, after delete) {
    if (Trigger.isInsert) {
        Integer recordCount = Trigger.New.size();
        // Call a utility method from another class
        EmailManager.sendMail('Your email address', 'Trailhead Trigger Tutorial', 
                    recordCount + ' contact(s) were inserted.');
    }
    else if (Trigger.isDelete) {
        // Process after delete
    }
}
```

### Adding Related Records from a Trigger
- This version will be improved later (down below)
```java
trigger AddRelatedRecord on Account(after insert, after update) {
    List<Opportunity> oppList = new List<Opportunity>();
    
    // Get the related opportunities for the accounts in this trigger
    Map<Id,Account> acctsWithOpps = new Map<Id,Account>(
        [SELECT Id,(SELECT Id FROM Opportunities) FROM Account WHERE Id IN :Trigger.New]);
    
    // Add an opportunity for each account if it doesn't already have one.
    // Iterate through each account.
    for(Account a : Trigger.New) {
        System.debug('acctsWithOpps.get(a.Id).Opportunities.size()=' + acctsWithOpps.get(a.Id).Opportunities.size());
        // Check if the account already has a related opportunity.
        if (acctsWithOpps.get(a.Id).Opportunities.size() == 0) {
            // If it doesn't, add a default opportunity
            oppList.add(new Opportunity(Name=a.Name + ' Opportunity',
                                       StageName='Prospecting',
                                       CloseDate=System.today().addMonths(1),
                                       AccountId=a.Id));
        }           
    }
    if (oppList.size() > 0) {
        insert oppList;
    }
}
```


### Trigger Exception
```java
trigger AccountDeletion on Account (before delete) {
   
    // Prevent the deletion of accounts if they have related opportunities.
    for (Account a : [SELECT Id FROM Account
                     WHERE Id IN (SELECT AccountId FROM Opportunity) AND
                     Id IN :Trigger.old]) {
        Trigger.oldMap.get(a.Id).addError(
            'Cannot delete account with related opportunities.');
    }
    
}
```


### Triggers and Callouts
- Apex allows you to make calls to and integrate your Apex code with external Web services. Apex calls to external Web services are referred to as callouts. 
- For example, you can make a callout to a stock quote service to get the latest quotes.
- When making a callout from a trigger, the callout must be done asynchronously so that the trigger process doesn’t block you from working while waiting for the external service's response.The asynchronous callout is made in a background process, and the response is received when the external service returns it.

### Sample Callout
```java
public class CalloutClass {
    @future(callout=true)
    public static void makeCallout() {
        HttpRequest request = new HttpRequest();
        // Set the endpoint URL.
        String endpoint = 'http://yourHost/yourService';
        request.setEndPoint(endpoint);
        // Set the HTTP verb to GET.
        request.setMethod('GET');
        // Send the HTTP request and get the response.
        HttpResponse response = new HTTP().send(request);
    }
}
```

### Sample Trigger for the Callout
```java
trigger CalloutTrigger on Account (before insert, before update) {
    CalloutClass.makeCallout();
}
```

---

### Match Billing Postal Code challenge
```java
trigger AccountAddressTrigger on Account (before insert, before update) {
    for(Account acct : Trigger.New) {
    	// If match billing address is checked
    	if(acct.Match_Billing_Address__c) {
    		// Set the shipping postal code to be same as billing postal code
    		acct.ShippingPostalCode = acct.BillingPostalCode;
    	}
    }
}
```

---

## Bulk Apex Triggers
- When you use bulk design patterns, your triggers have better performance, consume less server resources, and are less likely to exceed platform limits.

- The benefit of bulkifying your code is that bulkified code can process large numbers of records efficiently and run within governor limits on the Lightning Platform. These governor limits are in place to ensure that runaway code doesn’t monopolize resources on the multitenant platform.

### Not Bulk
```java
trigger MyTriggerNotBulk on Account(before insert) {
    Account a = Trigger.New[0];
    a.Description = 'New description';
}
```

### Bulk (which is better)
```java
trigger MyTriggerBulk on Account(before insert) {
    for(Account a : Trigger.New) {
        a.Description = 'New description';
    }
}
```

### Bulk SOQL Queries
- Use this to avoid Query limits which are 100 SOQL queries for synchronous Apex or 200 for asynchronous Apex.

### Bad Practice
```java
trigger SoqlTriggerNotBulk on Account(after update) {   
    for(Account a : Trigger.New) {
        // Get child records for each account
        // Inefficient SOQL query as it runs once for each account!
        Opportunity[] opps = [SELECT Id,Name,CloseDate 
                             FROM Opportunity WHERE AccountId=:a.Id];
        
        // Do some other processing
    }
}
```

### Good Practice
- Store to a list first before iteration
```java
trigger SoqlTriggerBulk on Account(after update) {  
    // Perform SOQL query once.    
    // Get the accounts and their related opportunities.
    List<Account> acctsWithOpps = 
        [SELECT Id,(SELECT Id,Name,CloseDate FROM Opportunities) 
         FROM Account WHERE Id IN :Trigger.New];
  
    // Iterate over the returned accounts    
    for(Account a : acctsWithOpps) { 
        Opportunity[] relatedOpps = a.Opportunities;  
        // Do some other processing
    }
}
```

### Alternative, when Account parent records are not needed
```java
trigger SoqlTriggerBulk on Account(after update) {  
    // Perform SOQL query once.    
    // Get the related opportunities for the accounts in this trigger.
    List<Opportunity> relatedOpps = [SELECT Id,Name,CloseDate FROM Opportunity
        WHERE AccountId IN :Trigger.New];
  
    // Iterate over the related opportunities    
    for(Opportunity opp : relatedOpps) { 
        // Do some other processing
    }
}
```

### Improve the Alternative with a For-loop in one statement
```java
trigger SoqlTriggerBulk on Account(after update) {  
    // Perform SOQL query once.    
    // Get the related opportunities for the accounts in this trigger,
    // and iterate over those records.
    for(Opportunity opp : [SELECT Id,Name,CloseDate FROM Opportunity
        WHERE AccountId IN :Trigger.New]) {
  
        // Do some other processing
    }
}
```
- Triggers execute on batches of 200 records at a time. So if 400 records cause a trigger to fire, the trigger fires twice, once for each 200 records. For this reason, you don’t get the benefit of SOQL for loop record batching in triggers, because triggers batch up records as well. The SOQL for loop is called twice in this example, but a standalone SOQL query would also be called twice. However, the SOQL for loop still looks more elegant than iterating over a collection variable!


### Bulk DML
- Use this because the Apex runtime allows up to 150 DML calls in one transaction.

### Bad Practice
```java
trigger DmlTriggerNotBulk on Account(after update) {   
    // Get the related opportunities for the accounts in this trigger.        
    List<Opportunity> relatedOpps = [SELECT Id,Name,Probability FROM Opportunity
        WHERE AccountId IN :Trigger.New];          
    // Iterate over the related opportunities
    for(Opportunity opp : relatedOpps) {      
        // Update the description when probability is greater 
        // than 50% but less than 100% 
        if ((opp.Probability >= 50) && (opp.Probability < 100)) {
            opp.Description = 'New description for opportunity.';
            // Update once for each opportunity -- not efficient!
            update opp;
        }
    }    
}
```

### Good Practice
- Store records to a list first before updating. Don't update per record.
```java
trigger DmlTriggerBulk on Account(after update) {   
    // Get the related opportunities for the accounts in this trigger.        
    List<Opportunity> relatedOpps = [SELECT Id,Name,Probability FROM Opportunity
        WHERE AccountId IN :Trigger.New];
          
    List<Opportunity> oppsToUpdate = new List<Opportunity>();
    // Iterate over the related opportunities
    for(Opportunity opp : relatedOpps) {      
        // Update the description when probability is greater 
        // than 50% but less than 100% 
        if ((opp.Probability >= 50) && (opp.Probability < 100)) {
            opp.Description = 'New description for opportunity.';
            oppsToUpdate.add(opp);
        }
    }
    
    // Perform DML on a collection
    update oppsToUpdate;
}
```

### Adding Related Records from a Trigger (IMPROVED)
- Instead of checking an opportunities list size for each record in the loop, add the condition in the main query.
```java
trigger AddRelatedRecord on Account(after insert, after update) {
    List<Opportunity> oppList = new List<Opportunity>();
    
    // Add an opportunity for each account if it doesn't already have one.
    // Iterate over accounts that are in this trigger but that don't have opportunities.
    for (Account a : [SELECT Id,Name FROM Account
                     WHERE Id IN :Trigger.New AND
                     Id NOT IN (SELECT AccountId FROM Opportunity)]) {
        // Add a default opportunity for this account
        oppList.add(new Opportunity(Name=a.Name + ' Opportunity',
                                   StageName='Prospecting',
                                   CloseDate=System.today().addMonths(1),
                                   AccountId=a.Id)); 
    }
    
    if (oppList.size() > 0) {
        insert oppList;
    }
}
```

---

## Solution to Closed Opportunity Trigger challenge
```java
trigger ClosedOpportunityTrigger on Opportunity (after insert, after update) {
	List<Task> taskList = new List<Task>();
	
	// Check Opportunity.StageName if equal to 'Closed Won'
	for(Opportunity o : [SELECT Id FROM Opportunity
												WHERE Id IN :Trigger.New
													AND StageName = 'Closed Won']) {
			
		// Create a task with subject 'Follow Up Test Task'
		// Associate task with Opportunity - Fill the 'WhatId' field with the opportunity ID
		taskList.add(new Task(Subject = 'Follow Up Test Task',
													WhatId = o.Id));
	}
	
	if(taskList.size() > 0) {
		insert taskList;
	}
}
```