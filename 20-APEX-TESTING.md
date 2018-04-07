# Apex Testing
- The Apex testing framework enables you to write and execute tests for your Apex classes and triggers on the Lightning Platform platform. Apex unit tests ensure high quality for your Apex code and let you meet requirements for deploying Apex.

## Reference
- [Apex Testing](https://trailhead.salesforce.com/trails/force_com_dev_beginner/modules/apex_testing)

## Code Coverage Requirement for Deployment
- Before you can deploy your code or package it for the Lightning Platform​ AppExchange, at least 75% of Apex code must be covered by tests, and all those tests must pass.
- In addition, each trigger must have some coverage. Even though code coverage is a requirement for deployment, don’t write tests only to meet this requirement. Make sure to test the common use cases in your app, including positive and negative test cases, and bulk and single-record processing.

## Test Method Syntax
```java
@isTest static void testName() {
    // code_block
}
```
or
```java
static testMethod void testName() {
    // code_block
}
```
- Using the `isTest annotation` instead of the `testMethod keyword` is more flexible as you can specify parameters in the annotation.
- Visibility of a `test method` doesn’t matter, so declaring a test method as public or private doesn’t make a difference.
- `Test classes` can be either private or public. If you’re using a test class for unit testing only, declare it as private. Public test classes are typically used for test data factory classes.
```java
@isTest
private class MyTestClass {
    @isTest static void myTest() {
        // code_block
    }
}
```

## Temperature Converter
```java
public class TemperatureConverter {
    // Takes a Fahrenheit temperature and returns the Celsius equivalent.
    public static Decimal FahrenheitToCelsius(Decimal fh) {
        Decimal cs = (fh - 32) * 5/9;
        return cs.setScale(2);
    }
}
```

## Temperature Converter Test Class
```java
@isTest
private class TemperatureConverterTest {
		@isTest static void testWarmTemp() {
        Decimal celsius = TemperatureConverter.FahrenheitToCelsius(70);
        System.assertEquals(21.11,celsius);
    }
    
    @isTest static void testFreezingPoint() {
        Decimal celsius = TemperatureConverter.FahrenheitToCelsius(32);
        System.assertEquals(0,celsius);
    }
    
    @isTest static void testBoilingPoint() {
        Decimal celsius = TemperatureConverter.FahrenheitToCelsius(212);        
        System.assertEquals(100,celsius,'Boiling point temperature is not expected.');
    } 
    
    @isTest static void testNotBoilingPoint() {
        Decimal celsius = TemperatureConverter.FahrenheitToCelsius(212);        
        // Simulate failure
        System.assertNotEquals(0,celsius,'Boiling point temperature is not expected.');
    } 
    
    @isTest static void testNegativeTemp() {
        Decimal celsius = TemperatureConverter.FahrenheitToCelsius(-10);
        System.assertEquals(-23.33,celsius);
    }
}
```

## Increase Your Code Coverage
- When writing tests, try to achieve the highest code coverage possible. Don’t just aim for 75% coverage, which is the lowest coverage that the Lightning Platform platform requires for deployments and packages. 

## Sample TaskUtil Class
```java
public class TaskUtil {
    public static String getTaskPriority(String leadState) {
        // Validate input
        if (String.isBlank(leadState) || leadState.length() > 2) {
            return null;
        }
            
        String taskPriority;
        
        if (leadState == 'CA') {
             taskPriority = 'High'; 
        } else {
             taskPriority = 'Normal';
        }
        
        return taskPriority;
    }
}
```
- Note: The equality operator (==) performs case-insensitive string comparisons, so there is no need to convert the string to lower case first. This means that passing in 'ca' or 'Ca' will satisfy the equality condition with the string literal 'CA'.

## Sample test for TaskUtil
### Not 100% Coverage
```java
@isTest
private class TaskUtilTest {
    @isTest static void testTaskPriority() {
        String pri = TaskUtil.getTaskPriority('NY');
        System.assertEquals('Normal', pri);
    }
}
```

### 100% Coverage
```java
@isTest
private class TaskUtilTest {
    @isTest static void testTaskPriority() {
        String pri = TaskUtil.getTaskPriority('NY');
        System.assertEquals('Normal', pri);
    }
    
    @isTest static void testTaskHighPriority() {
        String pri = TaskUtil.getTaskPriority('CA');
        System.assertEquals('High', pri);
    }
    
    @isTest static void testTaskPriorityInvalid() {
        String pri = TaskUtil.getTaskPriority('Montana');
        System.assertEquals(null, pri);
    }
}
```

### Solution to Verify Date Challenge
- VerifyDate class
```java
public class VerifyDate {
	
	//method to handle potential checks against two dates
	public static Date CheckDates(Date date1, Date date2) {
		//if date2 is within the next 30 days of date1, use date2.  Otherwise use the end of the month
		if(DateWithin30Days(date1,date2)) {
			return date2;
		} else {
			return SetEndOfMonthDate(date1);
		}
	}
	
	//method to check if date2 is within the next 30 days of date1
	private static Boolean DateWithin30Days(Date date1, Date date2) {
		//check for date2 being in the past
        	if( date2 < date1) { return false; }
        
        	//check that date2 is within (>=) 30 days of date1
        	Date date30Days = date1.addDays(30); //create a date 30 days away from date1
		if( date2 >= date30Days ) { return false; }
		else { return true; }
	}

	//method to return the end of the month of a given date
	private static Date SetEndOfMonthDate(Date date1) {
		Integer totalDays = Date.daysInMonth(date1.year(), date1.month());
		Date lastDay = Date.newInstance(date1.year(), date1.month(), totalDays);
		return lastDay;
	}

}
```

- TestVerifyDate class
```java
@istest
private class TestVerifyDate {
	
	// When CheckDates() method is a Date1 within Date2
	@istest static void testCheckDatesWhenWithinDate1() {
		Date given1 = Date.parse('01/01/2018');
		Date given2 = Date.parse('20/01/2018');
		Date expected = Date.parse('20/01/2018');
		Date result = VerifyDate.CheckDates(given1, given2);
		System.assertEquals(result, expected);
	}
	
	// When CheckDates() method is a Date1 not within Date2
	@istest static void testCheckDatesWhenNotWithinDate1() {
		Date given1 = Date.parse('01/01/2018');
		Date given2 = Date.parse('15/02/2018');
		Date expected = Date.parse('31/01/2018');
		Date result = VerifyDate.CheckDates(given1, given2);
		System.assertEquals(result, expected);
	}
}
```
---

## Test Apex Triggers
- Before deploying a trigger, write unit tests to perform the actions that fire the trigger and verify expected results.


### Account Deletion trigger
- AccountDelation class
```java
trigger AccountDeletion on Account (before delete) {
   
    // Prevent the deletion of accounts if they have related contacts.
    for (Account a : [SELECT Id FROM Account
                     WHERE Id IN (SELECT AccountId FROM Opportunity) AND
                     Id IN :Trigger.old]) {
        Trigger.oldMap.get(a.Id).addError(
            'Cannot delete account with related opportunities.');
    }
    
}
```

- TestAccountDeletion class
```java
@isTest
private class TestAccountDeletion {
    @isTest static void TestDeleteAccountWithOneOpportunity() {
        // Test data setup
        // Create an account with an opportunity, and then try to delete it
        Account acct = new Account(Name='Test Account');
        insert acct;
        Opportunity opp = new Opportunity(Name=acct.Name + ' Opportunity',
                                       StageName='Prospecting',
                                       CloseDate=System.today().addMonths(1),
                                       AccountId=acct.Id);
        insert opp;
        
        // Perform test
        Test.startTest();
        Database.DeleteResult result = Database.delete(acct, false);
        Test.stopTest();
        // Verify 
        // In this case the deletion should have been stopped by the trigger,
        // so verify that we got back an error.
        System.assert(!result.isSuccess());
        System.assert(result.getErrors().size() > 0);
        System.assertEquals('Cannot delete account with related opportunities.',
                             result.getErrors()[0].getMessage());
    }
    
}
```

### `Test.startTest()` and `Test.stopTest()`
- The test method contains the Test.startTest() and Test.stopTest() method pair, which delimits a block of code that gets a fresh set of governor limits. In this test, test-data setup uses two DML statements before the test is performed. 
- The `startTest` method marks the point in your test code when your test actually begins. Each test method is allowed to call this method `only once`. All of the code before this method should be used to initialize variables, populate data structures, and so on, allowing you to set up everything you need to run your test. Any code that executes after the call to startTest and before stopTest is assigned a new set of governor limits.

### Solution to Restrict Contact By Name Challenge
- RestrictContactByName class
```java
trigger RestrictContactByName on Contact (before insert, before update) {
	//check contacts prior to insert or update for invalid data
	For (Contact c : Trigger.New) {
		if(c.LastName == 'INVALIDNAME') {	//invalidname is invalid
			
			// Note: Commented because this not provide data for e.getDmlFields in actual test
			//c.AddError('The Last Name "'+c.LastName+'" is not allowed for DML');
			
			// Fix: e.getDmlFields[0](0) is now Contact.LastName
			c.LastName.AddError('The Last Name "'+c.LastName+'" is not allowed for DML');
		}

	}    
}
```

- TestRestrictContactByName class
```java
@istest private class TestRestrictContactByName {
	@istest static void CheckInvalidLastName() {
		// Setup
		String givenLastName = 'INVALIDNAME';
		Contact givenContact = new Contact(LastName=givenLastName);
		
		// Perform test
		try {
			insert givenContact;
		}
		catch(DmlException e) {
			//Assert Error Message
			System.assert(
				e.getMessage().contains('The Last Name "'+givenLastName+'" is not allowed for DML'),
				e.getMessage()
			);
						
			//Assert Field
	   	System.assertEquals(Contact.LastName, e.getDmlFields(0)[0]);
			
			//Assert Status Code
		  System.assertEquals('FIELD_CUSTOM_VALIDATION_EXCEPTION', e.getDmlStatusCode(0));
		}
	}
	
	@istest static void CheckValidLastName() {
		// Setup
		String givenLastName = 'Smith';
		Contact givenContact = new Contact(LastName=givenLastName);
		
		// Perform test
		try {
			Test.startTest();
			insert givenContact;
			Contact[] resultContacts = [SELECT LastName FROM Contact WHERE LastName=:givenLastName];
			Test.stopTest();
			
			// Verify
			System.assertEquals(resultContacts[0].LastName, givenLastName);
		}
		catch(DmlException e) {
			//Assert Error Message
			System.assert(
				String.isEmpty(e.getMessage()),
				e.getMessage()
			);
		}
	}
}
```
---

## Create Test Data for Apex Tests 
- Use test utility classes to add reusable methods for test data setup.

### TestDataFactory class
```java
@isTest
public class TestDataFactory {
    public static List<Account> createAccountsWithOpps(Integer numAccts, Integer numOppsPerAcct) {
        List<Account> accts = new List<Account>();
        
        for(Integer i=0;i<numAccts;i++) {
            Account a = new Account(Name='TestAccount' + i);
            accts.add(a);
        }
        insert accts;
        
        List<Opportunity> opps = new List<Opportunity>();
        for (Integer j=0;j<numAccts;j++) {
            Account acct = accts[j];
            // For each account just inserted, add opportunities
            for (Integer k=0;k<numOppsPerAcct;k++) {
                opps.add(new Opportunity(Name=acct.Name + ' Opportunity ' + k,
                                       StageName='Prospecting',
                                       CloseDate=System.today().addMonths(1),
                                       AccountId=acct.Id));
            }
        }
        // Insert all opportunities for all accounts.
        insert opps;
        
        return accts;
    }
}
```

### Modify TestAcccountDeletion class
```java
@isTest
private class TestAccountDeletion {
    @isTest static void TestDeleteAccountWithOneOpportunity() {
        // Test data setup
        // Create one account with one opportunity by calling a utility method	
        Account[] accts = TestDataFactory.createAccountsWithOpps(1,1);
        
        // Perform test
        Test.startTest();
        Database.DeleteResult result = Database.delete(accts[0], false);
        Test.stopTest();
        // Verify 
        // In this case the deletion should have been stopped by the trigger,
        // so verify that we got back an error.
        System.assert(!result.isSuccess());
        System.assert(result.getErrors().size() > 0);
        System.assertEquals('Cannot delete account with related opportunities.',
                             result.getErrors()[0].getMessage());
    }
    
    @isTest static void TestDeleteAccountWithNoOpportunities() {
        // Test data setup
        // Create one account with no opportunities by calling a utility method
        Account[] accts = TestDataFactory.createAccountsWithOpps(1,0);
        
        // Perform test
        Test.startTest();
        Database.DeleteResult result = Database.delete(accts[0], false);
        Test.stopTest();
        // Verify that the deletion was successful
        System.assert(result.isSuccess(), result);
    }
    
    @isTest static void TestDeleteBulkAccountsWithOneOpportunity() {
        // Test data setup
        // Create accounts with one opportunity each by calling a utility method
        Account[] accts = TestDataFactory.createAccountsWithOpps(200,1);
        
        // Perform test
        Test.startTest();
        Database.DeleteResult[] results = Database.delete(accts, false);
        Test.stopTest();
        // Verify for each record.
        // In this case the deletion should have been stopped by the trigger,
        // so check that we got back an error.
        for(Database.DeleteResult dr : results) {
            System.assert(!dr.isSuccess());
            System.assert(dr.getErrors().size() > 0);
            System.assertEquals('Cannot delete account with related opportunities.',
                                 dr.getErrors()[0].getMessage());
        }
    }
    
    @isTest static void TestDeleteBulkAccountsWithNoOpportunities() {
        // Test data setup
        // Create accounts with no opportunities by calling a utility method
        Account[] accts = TestDataFactory.createAccountsWithOpps(200,0);
        
        // Perform test
        Test.startTest();
        Database.DeleteResult[] results = Database.delete(accts, false);
        Test.stopTest();
        // For each record, verify that the deletion was successful
        for(Database.DeleteResult dr : results) {
            System.assert(dr.isSuccess());
        }
    }
    
}
```

### Solution to RandomContactFactory challenge
```java
public class RandomContactFactory {
	public static List<Contact> generateRandomContacts(Integer num, String name) {
		Contact[] contactList = new List<Contact>();
		for(Integer i=0; i<num; i++) {
			contactList.add(new Contact(FirstName=name + ' ' + i));
		}
		return contactList;
	}
}
```