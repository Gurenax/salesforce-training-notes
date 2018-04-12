# Lightning Components: Server-side Controllers

## Create a new Apex class `ExpensesController`
```java
public with sharing class ExpensesController {
    @AuraEnabled
    public static List<Expense__c> getExpenses() {
        return [SELECT Id, Name, Amount__c, Client__c, Date__c,
                       Reimbursed__c, CreatedDate
                FROM Expense__c];
    }
}
```

* The `with sharing` keywords automatically applies your org’s sharing rules to the records that are available via these methods. Using the with sharing keywords is one of the essential security measures you need to take when writing server-side controller code.
* The `@AuraEnabled` annotation before the method declaration. “Aura” is the name of the open source framework at the core of Lightning Components.
* The `static` keyword. All `@AuraEnabled` controller methods `must be static methods`, and either public or global scope.

## Loading Data from Salesforce

### Connect the `ExpensesController` to `expenses.cmp` component
```html
<aura:component controller="ExpensesController">
```

### Add an init handler to `expenses.cmp`
```html
<aura:handler name="init" action="{!c.doInit}" value="{!this}"/>
```

### Add the doInit method to `expensesController.js` client-side controller
```javascript
    // Load expenses from Salesforce
    doInit: function(component, event, helper) {
        // Create the action
        var action = component.get("c.getExpenses");
        // Add callback behavior for when response is received
        action.setCallback(this, function(response) {
            var state = response.getState();
            if (state === "SUCCESS") {
                component.set("v.expenses", response.getReturnValue());
            }
            else {
                console.log("Failed with state: " + state);
            }
        });
        // Send action off to be executed
        $A.enqueueAction(action);
    },
```

* The `c` markup in a view (.cmp file) pertains to a javascript client-side controller. But the `c` markup in a javascript controller pertains to an `Apex controller` (e.g. `var action = component.get("c.getExpenses");`)
* `$A.enqueueAction(action)` queues up the server request.
* `action.setCallback(this, function(response))` is an asynchronous call. `this` is a scope which pertains to action itself and `function(response)` is the callback function when the async call has succeeded/failed.

## Further securing the method

* Aside from the `with sharing` keyword, there are also `isAccessible()` and `isUpdatable()` checks.
* Below is an example `getExpenses()` method using the `isAccessible()` check:
```java
    @AuraEnabled
    public static List<Expense__c> getExpenses() {

        // Check to make sure all fields are accessible to this user
        String[] fieldsToCheck = new String[] {
            'Id', 'Name', 'Amount__c', 'Client__c', 'Date__c',
            'Reimbursed__c', 'CreatedDate'
        };

        Map<String,Schema.SObjectField> fieldDescribeTokens =
            Schema.SObjectType.Expense__c.fields.getMap();

        for(String field : fieldsToCheck) {
            if( ! fieldDescribeTokens.get(field).getDescribe().isAccessible()) {
                throw new System.NoAccessException();
                return null;
            }
        }

        // OK, they're cool, let 'em through
        return [SELECT Id, Name, Amount__c, Client__c, Date__c,
                       Reimbursed__c, CreatedDate
                FROM Expense__c];
    }
```

## Save Data to Salesforce

* Update apex controller `ExpensesController.apxc`
```java
public with sharing class ExpensesController {
    @AuraEnabled
    public static List<Expense__c> getExpenses() {
        // Perform isAccessible() checking first, then
        return [SELECT Id, Name, Amount__c, Client__c, Date__c,
                       Reimbursed__c, CreatedDate
                FROM Expense__c];
    }

    @AuraEnabled
    public static Expense__c saveExpense(Expense__c expense) {
        // Perform isUpdatable() checking first, then
        upsert expense;
        return expense;
    }
}
```

* Update the createExpense function in the helper `expenseHelper.js`
```javascript
    createExpense: function(component, expense) {
        var action = component.get("c.saveExpense");
        action.setParams({
            "expense": expense
        });
        action.setCallback(this, function(response){
            var state = response.getState();
            if (state === "SUCCESS") {
                var expenses = component.get("v.expenses");
                expenses.push(response.getReturnValue());
                component.set("v.expenses", expenses);
            }
        });
        $A.enqueueAction(action);
    },
```

* Parameter name in `action.setParams` must match the parameter name used in the Apex declaration.

## Solution to Challenge

* `CampingListController.apxc`
```java
public with sharing class CampingListController {
	// getItems
	@AuraEnabled
    public static List<Camping_Item__c> getItems() {
        return [SELECT Id, Name, Quantity__c, Price__c, Packed__c, CreatedDate
               	FROM Camping_Item__c];
    }
	// saveItem
	@AuraEnabled
    public static Camping_Item__c saveItem(Camping_Item__c item) {
        upsert item;
        return item;
    }
}
```

* `campingList.cmp`
```html
<aura:component controller="CampingListController">
    <aura:handler name="init" action="{!c.doInit}" value="{!this}"/>
    <aura:attribute name="items" type="Camping_Item__c[]" />
	<aura:attribute name="newItem" type="Camping_Item__c"
                    default="{ 'sObjectType' : 'Camping_Item__c',
                             	'Name' : '',
                             	'Quantity__c' : 0,
                             	'Price__c' : 0,
                             	'Packed__c' : false }" />
    <!-- HEADER -->
    <c:campingHeader />

    <!-- FORM -->
    <lightning:layout >
        <lightning:layoutItem padding="around-small" size="6">
            <form class="slds-form--stacked">
                <lightning:input aura:id="itemForm"
                                 label="Name"
                                 name="itemName"
                                 value="{!v.newItem.Name}"
                                 required="true" />
                <lightning:input aura:id="itemForm"
                                 label="Quantity"
                                 name="itemQuantity"
                                 value="{!v.newItem.Quantity__c}"
                                 type="number"
                                 min="1"
                                 messageWhenRangeUnderflow="Quantity should be at least 1" />
                <lightning:input aura:id="itemForm"
								 label="Price"
                                 name="itemPrice"
                                 value="{!v.newItem.Price__c}"
                                 type="number"
                                 formatter="currency"
                                 step="0.01" />
                <lightning:input aura:id="itemForm"
                                 label="Packed?"
                                 name="itemPacked"
                                 checked="{!v.newItem.Packed__c}"
                                 type="checkbox" />
                <lightning:button label="Create Item"
                                  onclick="{!c.clickCreateItem}"
                                  variant="brand"
                                  class="slds-m-top--medium" />
            </form>
        </lightning:layoutItem>
    </lightning:layout>
    <!-- LIST -->
    <lightning:layout >
        <lightning:layoutItem padding="around-small" size="6">
            <ol>
            <aura:iteration items="{!v.items}" var="item">
                <li>
                    <c:campingListItem item="{!item}" />
                </li>
            </aura:iteration>
            </ol>
        </lightning:layoutItem>
    </lightning:layout>
</aura:component>
```

* `campingListController.js`
```javascript
({
  doInit: function(component, event, helper) {
    // Create Action
    var action = component.get('c.getItems')
    // Add callback to action
    action.setCallback(this, function(response) {
      var state = response.getState()
      if (state === 'SUCCESS') {
        component.set('v.items', response.getReturnValue())
      } else {
        console.log('Failed with state: ' + state)
      }
    })
    // Send action
    $A.enqueueAction(action)
  },
  clickCreateItem: function(component, event, helper) {
    /* Validate the Form */
    var validItem = component
      .find('itemForm')
      .reduce(function(validSoFar, inputCmp) {
        inputCmp.showHelpMessageIfInvalid()
        return validSoFar && inputCmp.get('v.validity').valid
      }, true)
    /* If no errors, create the item */
    if (validItem) {
      var newItem = component.get('v.newItem')
      helper.createItem(component, newItem)
    }
  }
})
```

* `campingListHelper.js`
```javascript
({
  createItem: function(component, item) {
    // Create action
    var action = component.get('c.saveItem')
    // Add params to action
    action.setParams({
      item: item
    })
    // Add callback to action
    action.setCallback(this, function(response) {
      var state = response.getState()
      if (state === 'SUCCESS') {
        var items = component.get('v.items')
        items.push(response.getReturnValue())
        component.set('v.items', items)

        // Reset new item to original blank sObject
        component.set('v.newItem', {
          sObjectType: 'Camping_Item__c',
          Name: '',
          Quantity__c: 0,
          Price__c: 0,
          Packed__c: false
        })
      }
    })
    $A.enqueueAction(action)
  }
})
```
