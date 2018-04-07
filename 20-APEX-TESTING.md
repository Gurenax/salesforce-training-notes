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
