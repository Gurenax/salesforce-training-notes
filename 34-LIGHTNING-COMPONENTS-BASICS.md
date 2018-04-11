# Lightning Components Basics

## Reference
- [Lightning Components Basics](https://trailhead.salesforce.com/modules/lex_dev_lc_basics)

## Lightning Components Framework
- Lightning Components is a UI framework for developing web apps for mobile and desktop devices. It’s a modern framework for building single-page applications with dynamic, responsive user interfaces for Lightning Platform apps. It uses JavaScript on the client side and Apex on the server side.

- Built-in components can come from a variety of different namespaces, such as `aura:`, or `force:`, `lightning:`, or `ui:`.

- CSS classes uses class names that start with `slds`.

### Some exmaples:
- `<lightning:card>` creates a container around a group of information.
- `<lightning:formattedDateTime>` displays formatted date and time.
- `<lightning:relativeDateTime>` displays the relative time difference between the current time and the provided time.

### Example lightning component
```html
<aura:component>
    <aura:attribute name="expense" type="Expense__c"/>
    <aura:registerEvent name="updateExpense" type="c:expensesItemUpdate"/>
    <!-- Color the item green if the expense is reimbursed -->
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

### Example javascript code
- Called by `onchange="{!c.clickReimbursed}"`
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

### Visualforce
- Other frameworks like AngularJS or React can be embedded inside Visualforce using a [container page](container page) and converting the libraries into static resources.
- Reference to embedding React inside a simple html: [How to add React to a Simple HTML File](https://medium.com/@to_pe/how-to-add-react-to-a-simple-html-file-a11511c0235f)


### Run Lightning Components Apps on Other Platforms with Lightning Out
- `Lightning Out` is a feature that extends Lightning apps. It acts as a bridge to surface Lightning components in any remote web container. This means you can use your Lightning components inside of an external site (for example, Sharepoint or SAP), or even elsewhere on the platform such as on Heroku.
