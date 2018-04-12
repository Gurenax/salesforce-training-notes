# Lightning Components: Connect Components with Events

## Events
- Components broadcast events of a particular type. If there’s a component that responds to that type of event, and if that component “hears” your event, then it will act on it.

### Sending an Event from a Component
- `expenseItem.cmp`
```html
<lightning:input type="toggle" 
            label="Reimbursed?"
            name="reimbursed"
            class="slds-p-around--small"
            checked="{!v.expense.Reimbursed__c}"
            messageToggleActive="Yes"
            messageToggleInactive="No"
            onchange="{!c.clickReimbursed}"/>
```
- `type="toggle"` is really a checkbox with a toggle switch design
- the `onchange` attribute of `<lightning:input>` gives us an easy way to wire the toggle switch to an action handler that updates the record when you slide right (checked) or slide left (unchecked).
- Handling the event in the controller `expenseItemController.js`
```javascript
({
    clickReimbursed: function(component, event, helper) {
        var expense = component.get("v.expense");
        var updateEvent = component.getEvent("updateExpense");
        updateEvent.setParams({ "expense": expense });
        updateEvent.fire();
    }
})
```
- `updateExpense` is a a custom event that we define ourselves.

### Define an event
- `File | New | Lightning Event`
- `expensesItemUpdate.evt`
```html
<aura:event type="COMPONENT">
    <aura:attribute name="expense" type="Expense__c"/>
</aura:event>
```
- There are two types of events, `component` and `application`. Here we’re using a component event.
- An ancestor is a component “above” this one in the component hierarchy. If we wanted a “general broadcast” kind of event, where any component could receive it, we’d use an application event instead.
- We named the event when it was created, `expensesItemUpdate`, and its markup is a beginning and ending `<aura:event>` tag, and one `<aura:attribute>` tag. An event’s attributes describe the payload it can carry. In the `clickReimbursed` action handler, we set the payload with a call to `setParams()`.

### Sending an Event
- Add the following to `expenseItem.cmp` below attribute definitions.
```html
<aura:registerEvent name="updateExpense" type="c:expensesItemUpdate"/>
```
- This markup says that our component fires an event, named `updateExpense`, of type `c:expensesItemUpdate`.

### Handling an Event
- Add the following to `expenses.cmp` below the init handler definition.
```html
     <aura:handler name="updateExpense" event="c:expensesItemUpdate"
        action="{!c.handleUpdateExpense}"/>
```
- Add the following to `expensesController.js` below the `clickReimbursed` definition.
```javascript
    handleUpdateExpense: function(component, event, helper) {
        var updatedExp = event.getParam("expense");
        helper.updateExpense(component, updatedExp);
    }
```
- Add the following to `expensesHelper.js` below the `createExpense` definition.
```javascript
    updateExpense: function(component, expense) {
        var action = component.get("c.saveExpense");
        action.setParams({
            "expense": expense
        });
        action.setCallback(this, function(response){
            var state = response.getState();
            if (state === "SUCCESS") {
                // do nothing!
            }
        });
        $A.enqueueAction(action);
    },
```


## Composition and Decomposition
- The right way to build a Lightning Components app is to create independent components, and then compose them together to build new, higher level features.


### Refactor Helper Functions
- `saveExpense` function can be used by both `createExpense` and `updateExpense` with an optional callback
```javascript
    saveExpense: function(component, expense, callback) {
        var action = component.get("c.saveExpense");
        action.setParams({
            "expense": expense
        });
        if (callback) {
            action.setCallback(this, callback);
        }
        $A.enqueueAction(action);
    },
```

- `createExpense` and `updateExpense`
```javascript
    createExpense: function(component, expense) {
        this.saveExpense(component, expense, function(response){
            var state = response.getState();
            if (state === "SUCCESS") {
                var expenses = component.get("v.expenses");
                expenses.push(response.getReturnValue());
                component.set("v.expenses", expenses);
            }
        });
    },
    updateExpense: function(component, expense) {
        this.saveExpense(component, expense);
    },
```

### Refactor the Add Expense Form
- All apex controller actions will be delegated to `expense.cmp` using events.
- Form actions will be handled by their local controllers which calls helpers that fire the event.

- `expenses.cmp`
```javascript
<aura:component controller="ExpensesController">
    <aura:handler name="init" action="{!c.doInit}" value="{!this}"/>
    <aura:handler name="updateExpense" event="c:expensesItemUpdate"
                  action="{!c.handleUpdateExpense}"/>
    <aura:handler name="createExpense" event="c:expensesItemUpdate"
                  action="{!c.handleCreateExpense}"/>
    <aura:attribute name="expenses" type="Expense__c[]"/>

    <!-- PAGE HEADER -->
    <lightning:layout class="slds-page-header slds-page-header--object-home">
        <lightning:layoutItem >
            <lightning:icon iconName="standard:scan_card" alternativeText="My Expenses"/>
        </lightning:layoutItem>
        <lightning:layoutItem padding="horizontal-small">
            <div class="page-section page-header">
                <h1 class="slds-text-heading--label">Expenses</h1>
                <h2 class="slds-text-heading--medium">My Expenses</h2>
            </div>
        </lightning:layoutItem>
    </lightning:layout>
    <!-- / PAGE HEADER -->
    <!-- NEW EXPENSE FORM -->
    <lightning:layout >
        <lightning:layoutItem padding="around-small" size="6">
            <c:expenseForm />
        </lightning:layoutItem>
    </lightning:layout>
    <!-- / NEW EXPENSE FORM -->
    
    <!-- EXPENSE LIST -->
    <c:expensesList expenses="{!v.expenses}"/>
</aura:component>
```
- `expensesController.cmp`
```javascript
({
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
    handleCreateExpense: function(component, event, helper) {
        var newExpense = event.getParam("expense");
        helper.createExpense(component, newExpense);
    },
    handleUpdateExpense: function(component, event, helper) {
        var updatedExp = event.getParam("expense");
        helper.updateExpense(component, updatedExp);
    }
})
```

- `expenseForm.cmp`
```html
<aura:component >
    <aura:attribute name="newExpense" type="Expense__c"
                    default="{ 'sobjectType': 'Expense__c',
                             'Name': '',
                             'Amount__c': 0,
                             'Client__c': '',
                             'Date__c': '',
                             'Reimbursed__c': false }"/>
    <aura:registerEvent name="createExpense" type="c:expensesItemUpdate"/>

	<!-- CREATE NEW EXPENSE -->
    <div aria-labelledby="newexpenseform">
        <!-- BOXED AREA -->
        <fieldset class="slds-box slds-theme--default slds-container--small">
            <legend id="newexpenseform" class="slds-text-heading--small 
                                               slds-p-vertical--medium">
                Add Expense
            </legend>
            
            <!-- CREATE NEW EXPENSE FORM -->
            <form class="slds-form--stacked">          
                <lightning:input aura:id="expenseform" label="Expense Name"
                                 name="expensename"
                                 value="{!v.newExpense.Name}"
                                 required="true"/> 
                <lightning:input type="number" aura:id="expenseform" label="Amount"
                                 name="expenseamount"
                                 min="0.1"
                                 formatter="currency"
                                 step="0.01"
                                 value="{!v.newExpense.Amount__c}"
                                 messageWhenRangeUnderflow="Enter an amount that's at least $0.10."/>
                <lightning:input aura:id="expenseform" label="Client"
                                 name="expenseclient"
                                 value="{!v.newExpense.Client__c}"
                                 placeholder="ABC Co."/>
                <lightning:input type="date" aura:id="expenseform" label="Expense Date"
                                 name="expensedate"
                                 value="{!v.newExpense.Date__c}"/>
                <lightning:input type="checkbox" aura:id="expenseform" label="Reimbursed?"  
                                 name="expreimbursed"
                                 checked="{!v.newExpense.Reimbursed__c}"/>
                <lightning:button label="Create Expense" 
                                  class="slds-m-top--medium"
                                  variant="brand"
                                  onclick="{!c.clickCreate}"/>
            </form>
            <!-- / CREATE NEW EXPENSE FORM -->
            
        </fieldset>
        <!-- / BOXED AREA -->
    </div>
    <!-- / CREATE NEW EXPENSE -->
</aura:component>
```

- `expenseFormController.js`
```javascript
({
    clickCreate: function(component, event, helper) {
        var validExpense = component.find('expenseform').reduce(function (validSoFar, inputCmp) {
            // Displays error messages for invalid fields
            inputCmp.showHelpMessageIfInvalid();
            return validSoFar && inputCmp.get('v.validity').valid;
        }, true);
        // If we pass error checking, do some real work
        if(validExpense){
            // Create the new expense
            var newExpense = component.get("v.newExpense");
            console.log("Create expense: " + JSON.stringify(newExpense));
            helper.createExpense(component, newExpense);
        }
    }
})
```

- `expenseFormHelper.js`
```javascript
({
    createExpense: function(component, newExpense) {
        var createEvent = component.getEvent("createExpense");
        createEvent.setParams({ "expense": newExpense });
        createEvent.fire();
    },
})
```

### Bonus: Minor visual improvements
- Fix green background and white font color for `expenseItem.css`
```
.THIS.slds-card.slds-theme--success {
    background-color: rgb(4, 132, 75);
}
.THIS.slds-card.slds-theme--success .slds-form-element__label {
    color: #ffffff;
}
.THIS.slds-card.slds-theme--success .slds-text-title {
    color: #ffffff;
}
```

- Improve `expenseList.cmp`
```html
<aura:component >
    <aura:attribute name="expenses" type="Expense__c[]"/>
    <aura:iteration items="{!v.expenses}" var="expense">
        <c:expenseItem expense="{!expense}"/>
    </aura:iteration>
</aura:component>
```

- Improve expense list markup in `expense.cmp`
```html
<aura:component controller="ExpensesController">
    <aura:handler name="init" action="{!c.doInit}" value="{!this}"/>
    <aura:handler name="updateExpense" event="c:expensesItemUpdate"
                  action="{!c.handleUpdateExpense}"/>
    <aura:handler name="createExpense" event="c:expensesItemUpdate"
                  action="{!c.handleCreateExpense}"/>
    <aura:attribute name="expenses" type="Expense__c[]"/>

    <!-- PAGE HEADER -->
    <lightning:layout class="slds-page-header slds-page-header--object-home">
        <lightning:layoutItem >
            <lightning:icon iconName="standard:scan_card" alternativeText="My Expenses"/>
        </lightning:layoutItem>
        <lightning:layoutItem padding="horizontal-small">
            <div class="page-section page-header">
                <h1 class="slds-text-heading--label">Expenses</h1>
                <h2 class="slds-text-heading--medium">My Expenses</h2>
            </div>
        </lightning:layoutItem>
    </lightning:layout>
    <!-- / PAGE HEADER -->
    <!-- NEW EXPENSE FORM -->
    <lightning:layout >
        <lightning:layoutItem padding="around-small" size="6">
            <c:expenseForm />
        </lightning:layoutItem>
    </lightning:layout>
    <!-- / NEW EXPENSE FORM -->
    
    <!-- EXPENSE LIST -->
    <lightning:layout>
        <lightning:layoutItem padding="around-small" size="6">
            <c:expensesList expenses="{!v.expenses}"/>
        </lightning:layoutItem>
    </lightning:layout>
</aura:component>
```

---

## Solution to Challenge
- `addItemEvent.evt`
```html
<aura:event type="COMPONENT">
    <aura:attribute name="item" type="Camping_Item__c"/>
</aura:event>
```

- `campingListForm.cmp
```html
<aura:component >
	<aura:attribute name="newItem" type="Camping_Item__c"
                    default="{ 'sObjectType' : 'Camping_Item__c',
                             	'Name' : '',
                             	'Quantity__c' : 0,
                             	'Price__c' : 0,
                             	'Packed__c' : false }" />
    <aura:registerEvent name="addItem" type="c:addItemEvent"/>
    
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
</aura:component>
```
- `campingListFormController.js`
```javascript
({
    clickCreateItem : function(component, event, helper) {
		/* Validate the Form */
        var validItem = component.find('itemForm').reduce(function(validSoFar, inputCmp) {
       		inputCmp.showHelpMessageIfInvalid()
            return validSoFar && inputCmp.get('v.validity').valid
        }, true)
        /* If no errors, fire the event to create the item */
        if(validItem) {
			var newItem = component.get('v.newItem')
			helper.createItem(component, newItem)
        }
	},
})
```
- `campingListFormHelper.js`
```javascript
({
	createItem: function(component, newItem) {
        // Fire the create event
        var createEvent = component.getEvent('addItem')
        createEvent.setParams({ 'item': newItem })
        createEvent.fire()
        
        // Reset new item to original blank sObject
        component.set('v.newItem', { 'sObjectType' : 'Camping_Item__c',
                                    'Name' : '',
                                    'Quantity__c' : 0,
                                    'Price__c' : 0,
                                    'Packed__c' : false })
	}
})
```

- `campingListItem.cmp`
```javascript
<aura:component >
    <aura:attribute name="item" type="Camping_Item__c" required="true"/>
    <lightning:card title="{!v.item.Name}">
        <p class="slds-text-heading--small slds-p-horizontal--small">
            Quantity: <lightning:formattedNumber value="{!v.item.Quantity__c}"
	                               				style="decimal" />
        </p>
        <p class="slds-text-heading--small slds-p-horizontal--small">
            Price: <lightning:formattedNumber value="{!v.item.Price__c}"
                                           style="currency"/>
        </p>
        <p class="slds-p-horizontal--small">
    		<lightning:input aura:id="itemForm"
                     label="Packed?"
                     name="itemPacked"
                     checked="{!v.item.Packed__c}"
                     type="checkbox" />
       </p>
    </lightning:card>
</aura:component>
```
- `campingListItemController.js`
```javascript
({
	packItem : function(component, event, helper) {
		// Get the value of item
        var new_item = component.get("v.item");
        // Set the new value Packed__c
        new_item.Packed__c = true;
        component.set("v.item", new_item);
		// Get the button and its disabled attribute
        var button = event.getSource();
        var buttonDisabled = button.get("v.disabled");
        // Set the disabled value to true
		component.set(buttonDisabled, true);
	}
})
```

- `campingList.cmp`
```html
<aura:component controller="CampingListController">
    <aura:handler name="init" action="{!c.doInit}" value="{!this}"/>
    <aura:handler name="addItem" event="c:addItemEvent"
        action="{!c.handleAddItem}"/>
    <aura:attribute name="items" type="Camping_Item__c[]" />

    <!-- HEADER -->
    <c:campingHeader />
    
    <!-- FORM -->
    <lightning:layout >
        <lightning:layoutItem padding="around-small" size="6">
            <c:campingListForm />
        </lightning:layoutItem>
    </lightning:layout>
    <!-- LIST -->
    <lightning:layout>
        <lightning:layoutItem padding="around-small" size="6">
            <c:expensesList expenses="{!v.expenses}"/>
        </lightning:layoutItem>
    </lightning:layout>
    
    <lightning:layout >
        <lightning:layoutItem padding="around-small" size="6">
            <aura:iteration items="{!v.items}" var="item">
                <c:campingListItem item="{!item}" />
            </aura:iteration>
        </lightning:layoutItem>
    </lightning:layout>
</aura:component>
```

- `campingListController.js`
```javascript
({
    doInit: function(component, event, helper) {
      // Create Action
      var action = component.get('c.getItems')
      // Add callback to action
      action.setCallback(this, function(response) {
          var state = response.getState()
          if(state === 'SUCCESS') {
              component.set('v.items', response.getReturnValue())
          }
          else {
              console.log('Failed with state: ' + state)
          }
      })
      // Send action
      $A.enqueueAction(action);
    },
    handleAddItem: function(component, event, helper) {
        var newItem = event.getParam("item")
        //helper.createItem(component, newItem)

        // Create action
        var action = component.get('c.saveItem')
        // Add params to action
        action.setParams({
            'item' : item
        })
        // Add callback to action
        action.setCallback(this, function(response) {
            var state = response.getState()
            if(state === 'SUCCESS') {
                var items = component.get('v.items')
                items.push(response.getReturnValue())
                component.set('v.items', items)
            }
        })
        $A.enqueueAction(action)
    }
})
```

- `campingListHelper.js`
```javascript
({
	createItem : function(component, item) {
		// Create action
        var action = component.get('c.saveItem')
        // Add params to action
        action.setParams({
            'item' : item
        })
        // Add callback to action
        action.setCallback(this, function(response) {
            var state = response.getState()
            if(state === 'SUCCESS') {
                var items = component.get('v.items')
                items.push(response.getReturnValue())
                component.set('v.items', items)
            }
        })
        $A.enqueueAction(action)
	}
})
```