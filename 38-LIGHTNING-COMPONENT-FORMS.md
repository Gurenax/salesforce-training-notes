# Lightning Component: Forms

## Create a new `expensesApp.app`

```html
<aura:application extends="force:slds">
	<!-- This component is the real "app" -->
  <c:expenses/>
</aura:application>
```

* The `extends="force:slds"` attribute activates SLDS in this app, by including the same Lightning Design System styles provided by Lightning Experience and the Salesforce app

## Expenses App Component `expenses.cmp`

```html
<aura:component>
    <!-- PAGE HEADER -->
    <lightning:layout class="slds-page-header slds-page-header--object-home">
        <lightning:layoutItem>
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
    <lightning:layout>
        <lightning:layoutItem padding="around-small" size="6">
        <!-- [[ expense form goes here ]] -->
        </lightning:layoutItem>
    </lightning:layout>
    <!-- / NEW EXPENSE FORM -->
</aura:component>
```

## Expenses Form

* replace `<!-- [[ expense form goes here ]] -->` above with following code

```html
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
```

* `<lightning:input>` is the Swiss army knife for input fields
* The label text is automatically displayed next to the input field by the `label` attribute.
* `aura:id` attribute is set on each tag. It sets a (locally) unique ID on each tag it’s added to, and that ID is how you’ll read values out of the form fields.

## Attributes for Salesforce Objects (sObjects)

* Include right after the opening `<aura:component>` tag.

```html
  <aura:attribute name="newExpense" type="Expense__c"
        default="{ 'sobjectType': 'Expense__c',
                      'Name': '',
                      'Amount__c': 0,
                      'Client__c': '',
                      'Date__c': '',
                      'Reimbursed__c': false }"/>
```

* Since `Expense__c` is a `sobject`, the default value has to be a JSON representation of an sObject

## Handle Form Submission in an Action Handler

* expensesController.js

```javascript
({
  clickCreate: function(component, event, helper) {
    var validExpense = component
      .find('expenseform')
      .reduce(function(validSoFar, inputCmp) {
        // Displays error messages for invalid fields
        inputCmp.showHelpMessageIfInvalid()
        return validSoFar && inputCmp.get('v.validity').valid
      }, true)
    // If we pass error checking, do some real work
    if (validExpense) {
      // Create the new expense
      var newExpense = component.get('v.newExpense')
      console.log('Create expense: ' + JSON.stringify(newExpense))
      helper.createExpense(component, newExpense)
    }
  }
})
```

* `component.find('expenseform')` gets a reference to the array of `<lightning:input>` fields that require validation. If the ID is unique, the reference returns the component. In this case, the ID is not unique and the reference returns an array of components.
* The JavaScript `reduce()` method reduces the array to a single value that’s captured by `validSoFar`, which remains true until it finds an invalid field, changing `validSoFar` to false. An invalid field can be a required field that’s empty, a field that has a number lower than a specified minimum number, among many others.
* `inputCmp.get('v.validity').valid` returns the validity of the current input field in the array.
* `inputCmp.showHelpMessageIfInvalid()` displays an error message for invalid fields. `<lightning:input>` provides default error messages that can be customized by attributes like messageWhenRangeUnderflow, which you’ve seen in the expense form example.
* `component.find()` only lets you access direct child components.

## Create the New Expense

* include before the `newExpense` attribute

```html
    <aura:attribute name="expenses" type="Expense__c[]"/>
```

## Helper

* A component’s helper is the appropriate place to put code to be shared between several different action handlers.
* A component’s helper is a great place to put complex processing details, so that the logic of your action handlers remains clear and streamlined.
* Helper functions can have any function signature. That is, they’re not constrained the way that action handlers in the controller are.
* Expense helper `expenseHelper.js`

```javascript
({
  createExpense: function(component, expense) {
    var theExpenses = component.get('v.expenses')

    // Copy the expense to a new object
    // THIS IS A DISGUSTING, TEMPORARY HACK
    var newExpense = JSON.parse(JSON.stringify(expense))

    // Puts the new expense into theExpenses attribute array component
    theExpenses.push(newExpense)
    component.set('v.expenses', theExpenses)
  }
})
```

## Displaying the List of Expenses

* `expenseItem.cmp`

```html
<aura:component>
    <aura:handler name="init" value="{!this}" action="{!c.doInit}"/>
    <aura:attribute name="formatdate" type="Date"/>
    <aura:attribute name="expense" type="Expense__c"/>

    <lightning:card title="{!v.expense.Name}" iconName="standard:scan_card"
                    class="{!v.expense.Reimbursed__c ?
                           'slds-theme--success' : ''}">
        <aura:set attribute="footer">
            <p>Date: <lightning:formattedDateTime value="{!v.formatdate}"/></p>
            <p class="slds-text-title"><lightning:relativeDateTime value="{!v.formatdate}"/></p>
        </aura:set>
        <p class="slds-text-heading--medium slds-p-horizontal--small">
           Amount: <lightning:formattedNumber value="{!v.expense.Amount__c}" style="currency"/>
        </p>
        <p class="slds-p-horizontal--small">
            Client: {!v.expense.Client__c}
        </p>
        <p>
            <lightning:input type="toggle"
                             label="Reimbursed?"
                             name="reimbursed"
                             class="slds-p-around--small"
                             checked="{!v.expense.Reimbursed__c}"
                             messageToggleActive="Yes"
                             messageToggleInactive="No"
                             onchange="{!c.clickReimbursed}"/>
        </p>
    </lightning:card>
</aura:component>
```

* `expenseItemController.js`

```javascript
({
  doInit: function(component, event, helper) {
    var mydate = component.get('v.expense.Date__c')
    if (mydate) {
      component.set('v.formatdate', new Date(mydate))
    }
  }
})
```

* This is captured by the `<aura:handler>` tag.

* `expensesList.cmp`

```html
<aura:component>
    <aura:attribute name="expenses" type="Expense__c[]"/>
    <lightning:card title="Expenses">
        <p class="slds-p-horizontal--small">
            <aura:iteration items="{!v.expenses}" var="expense">
                <c:expenseItem expense="{!expense}"/>
            </aura:iteration>
        </p>
    </lightning:card>
</aura:component>
```

* Add the expenseList component at the end of `expenses` component

```html
    <!-- EXPENSE LIST -->
    <c:expensesList expenses="{!v.expenses}"/>
```

## Complete `expenses.cmp`

```html
<aura:component>
    <!-- A LIST OF CREATED EXPENSES -->
    <aura:attribute name="expenses" type="Expense__c[]"/>
    <!-- EXPENSE OBJECT USED BY FORM TO SUBMIT -->
    <aura:attribute name="newExpense" type="Expense__c"
        default="{ 'sobjectType': 'Expense__c',
                      'Name': '',
                      'Amount__c': 0,
                      'Client__c': '',
                      'Date__c': '',
                      'Reimbursed__c': false }"/>
    <!-- PAGE HEADER -->
    <lightning:layout class="slds-page-header slds-page-header--object-home">
        <lightning:layoutItem>
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
    <lightning:layout>
        <lightning:layoutItem padding="around-small" size="6">
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
        </lightning:layoutItem>
    </lightning:layout>
    <!-- / NEW EXPENSE FORM -->

    <!-- EXPENSE LIST -->
    <c:expensesList expenses="{!v.expenses}"/>
</aura:component>
```

## Solution to Challenge

* `campingApp.app`

```html
<aura:application extends="force:slds">
    <c:campingList />
</aura:application>
```

* `campingHeader.cmp`

```html
<aura:component >
    <!-- PAGE HEADER -->
    <lightning:layout class="slds-page-header slds-page-header--object-home">
        <lightning:layoutItem>
            <lightning:icon iconName="action:goal" alternativeText="My Expenses"/>
        </lightning:layoutItem>
        <lightning:layoutItem padding="horizontal-small">
            <div class="page-section page-header">
                <h1 class="slds-text-heading--label">Camping List</h1>
                <h2 class="slds-text-heading--medium">My Camping List</h2>
            </div>
        </lightning:layoutItem>
    </lightning:layout>
</aura:component>
```

* `campingList.cmp`

```html
<aura:component >
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
    <lightning:layout>
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
    <lightning:layout>
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
      //var newItem = component.get('v.newItem')
      //helper.createItem(component, newItem)
      // Instead of using a helper
      var item = component.get('v.newItem')
      // Get the Items List from the View
      var theItemsList = component.get('v.items')
      // Hacky: Get the new item and serialize it to a JSON Object
      var newItem = JSON.parse(JSON.stringify(item))
      // Push the new item to the items list
      theItemsList.push(newItem)
      // Hacky: Refresh the item list component
      component.set('v.items', theItemsList)
      // Reset new item to original blank sObject
      component.set('v.newItem', {
        sObjectType: 'Camping_Item__c',
        Name: '',
        Quantity__c: 0,
        Price__c: 0,
        Packed__c: false
      })
    }
  }
})
```

* `campingListItem.cmp`

```html
<aura:component >
    <aura:attribute name="item" type="Camping_Item__c" required="true"/>
    <p>
	    Name: {!v.item.Name}<br/>
    </p>
    <p>
	    Quantity: <lightning:formattedNumber value="{!v.item.Quantity__c}"
                               				style="decimal" />
    </p>
    <p>
	    Price: <lightning:formattedNumber value="{!v.item.Price__c}"
		                               style="currency"/>
    </p>
    <p>
    <lightning:input aura:id="itemForm"
                     label="Packed?"
                     name="itemPacked"
                     checked="{!v.item.Packed__c}"
                     type="checkbox" />
       </p>
</aura:component>
```

* Not used because challenge prefers code to be in controller: `campingListHelper.js`

```javascript
({
  createItem: function(component, item) {
    // Get the Items List from the View
    var theItemsList = component.get('v.items')
    // Hacky: Get the new item and serialize it to a JSON Object
    var newItem = JSON.parse(JSON.stringify(item))
    // Push the new item to the items list
    theItemsList.push(newItem)
    // Hacky: Refresh the item list component
    component.set('v.items', theItemsList)
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
```
