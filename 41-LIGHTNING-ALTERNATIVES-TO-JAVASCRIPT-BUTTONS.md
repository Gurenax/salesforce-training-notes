# Lightning Alternatives to JavaScript Buttons
- JavaScript buttons and links are types of actions in the Salesforce Classic UI that let you create inline JavaScript code that can be invoked via a button or link embedded on a record or list page. For example, maybe you prepopulate new records with data upon creation and update values in fields based on other logic. Or maybe you’re a Salesforce partner who uses custom buttons to integrate with your platform.
- What makes JavaScript both useful and dangerous is that it has full access to the Document Object Model (DOM) and Browser Object Model (BOM) which is significant vulnerable to Cross Site Scripting (XSS).

## Reference
- [Lightning Alternatives to JavaScript Button](https://trailhead.salesforce.com/trails/lex_admin_migration/modules/lex_javascript_button_migration)

## LockerService
- a solution to make Lightning components more secure and restrict JavaScript’s unfettered access.

## Quick Actions
- Quick actions support many of the common uses of JavaScript buttons. Quick actions can be based on a specific object or can be global, meaning that they’re more generic and accessible from any record or the Chatter feed. There’s a quick action to do just about anything in Salesforce.

### Validate Field Values
- `Setup | Object Manager | Buttons, Links, and Actions | New Action`
- Action Type `Update a Record`
- Define a label
- Change field properties whether they are `Read-Only` or `Required`
- After save, you can predefine field values (default values) or formulas

### Prepopulate Fields with Values
- `Setup | Object Manager | Buttons, Links, and Actions | New Action`
- Action Type `Create a Record`
- Select a Target Object
- Define a label
- After save, you can predefine field values (default values) or formulas

### Redirect to a Visualforce Page Based on Input Values
- `Setup | Object Manager | Buttons, Links, and Actions | New Action`
- Action Type `Custom Visualforce`
- Select the Visualforce page

### Custom URL Buttons and Links
- `Setup | Object Manager | Buttons, Links, and Actions | New Button or Link`
- Define a label
- Sample custom url:
  - External URL `www.google.com` - URL opens in new tab
  - Relative Salesforce URL, View `/{!Account.Id}` - Record home page opens in existing tab
  - Relative Salesforce URL, Edit `/{!Account.Id}/e` - Edit overlay pops up on the existing page
  - Relative Salesforce URL, List `/001/o` - Object home page opens in existing tab
  - $Action URL, View `{!URLFOR($Action.Account.View, Account.Id)}` - Record home page opens in existing tab
  - $Action URL, Edit `{!URLFOR($Action.Account.Edit, Account.Id)}` - Edit overlay pops up on the existing page


## Apex Triggers
- Apex triggers can be configured to execute before or after a user clicks Save on a record.
- When you need presave validation, calculation, and population of fields, consider using Apex triggers.


## Updating Mass Records with Custom Visualforce Buttons
1. Create your Visualforce page.
```html
<apex:page standardController="Opportunity" recordSetVar="opportunities" extensions="tenPageSizeExt">
   <apex:form>
      <apex:pageBlock title="Edit Stage and Close Date" mode="edit">
         <apex:pageMessages />
         <apex:pageBlockButtons location="top">
            <apex:commandButton value="Save" action="{!save}"/>
            <apex:commandButton value="Cancel" action="{!cancel}"/>
         </apex:pageBlockButtons>
         <apex:pageBlockTable value="{!selected}" var="opp">
            <apex:column value="{!opp.name}"/>
            <apex:column headerValue="Stage">
               <apex:inputField value="{!opp.stageName}"/>
            </apex:column>
            <apex:column headerValue="Close Date">
               <apex:inputField value="{!opp.closeDate}"/>
            </apex:column>
         </apex:pageBlockTable>
      </apex:pageBlock>
   </apex:form>
</apex:page>
```

2. Create a custom button that references your Visualforce page.
3. Add the action to your list view. (Note: Mass actions aren’t supported on the Recently Viewed records list. They are only available on list views.)

---

## Lightning Actions
- quick actions that call Lightning components.

- You make a Lightning component invokable as an action by adding one of two interfaces to the component: `force:lightningQuickAction` or `force:lightningQuickActionWithoutHeader`. The first interface adds the standard header with Save and Cancel buttons to the Lightning action overlay, and the other doesn’t.

- Another useful interface for a Lightning action is `force:hasRecordId`, which provides the record context to the component when it’s invoked from a record page. If you set up your Lightning action as a global quick action, you don’t need the record context. However, if you want to access a record’s data or metadata, you must implement force:hasRecordId.


### Populate Fields Based on User Input and Provide Feedback Messages to Users
- `CreateUser.cmp` — The Lightning component that you see when you open the action. It contains the UI implementation, which includes the text fields, buttons, action title, and so on.
```html
<aura:component implements="force:lightningQuickActionWithoutHeader,force:hasRecordId">
    <aura:attribute name="user" type="Test_User__c" default="{}"/>
    <aura:attribute name="hasErrors" type="Boolean" description="Indicate whether there were failures or not" />
    <aura:attribute name="caseStudy" type="String" />
    <aura:attribute name="recordId" type="String"/>
    <aura:attribute name="errorFromCreate" type="String"/>
    <!-- <aura:handler name="init" value="{!this}" action="{!c.init}" />  -->
    <force:recordData aura:id="frd" mode="EDIT" layoutType="FULL"/>
    
    <div class="slds-page-header" role="banner">
    	<p class="slds-text-heading--label">Case Study</p>
        <h1 class="slds-page-header__title slds-m-right--small slds-truncate slds-align-left" title="Case Study Title">{!v.caseStudy}</h1>
    </div>
    <br/>
    
    <aura:if isTrue="{!v.hasErrors}">
        <div class="userCreateError">
            <ui:message title="Error" severity="error" closable="true">
                Please review the error messages.
            </ui:message>
        </div>
    </aura:if>
    <div class="slds-form--stacked">
                
        <div class="slds-form-element">
            <label class="slds-form-element__label" for="firstName">Enter first name: </label>
            <div class="slds-form-element__control">
              <ui:inputText class="slds-input" aura:id="firstName" value="{!v.user.First}" required="true" keydown="{!c.updateNickname}" updateOn="keydown"/>
            </div>
        </div>
        
        <div class="slds-form-element">
            <label class="slds-form-element__label" for="lastName">Enter last name: </label>
            <div class="slds-form-element__control">
              <ui:inputText class="slds-input" aura:id="lastName" value="{!v.user.Last}" required="true" />
            </div>
        </div>
        
        <div class="slds-form-element">
            <label class="slds-form-element__label" for="nickname">Enter nickname: </label>
            <div class="slds-form-element__control">
              <ui:inputText class="slds-input" aura:id="nickname" value="{!v.user.Nickname}" required="false"/>
            </div>
        </div>
        
        <div class="slds-form-element">
        	<label class="slds-form-element__label" for="userEmail">Enter user's email:</label>
            <div class="slds-form-element__control">
        		<ui:inputEmail class="slds-input" aura:id="userEmail" value="{!v.user.Email__c}" required="true"/>
            </div>
        </div>
        
        <div class="slds-form-element">
        	<label class="slds-form-element__label" for="userPassword">Enter user's password:</label>
            <div class="slds-form-element__control">
        		<ui:inputSecret class="slds-input" aura:id="userPassword" value="{!v.user.Password__c}" required="true"/>
            </div>
        </div>
        
        <div class="slds-form-element">
        	<ui:button class="slds-button slds-button--neutral" press="{!c.cancel}" label="Cancel" />
        	<ui:button class="slds-button slds-button--brand" press="{!c.saveUserForm}" label="Save User" />
        </div>
    </div>
</aura:component>
```

- `CreateUserController.js` — The controller that listens to the component and any UI events that take place, such as the initial load, text input, and button clicks. Its function is to delegate these events to the helper class, `CreateUserHelper.js`.
```javascript
({    
    /**
     * Auto generate the username from firstName on tab
     */
    updateNickname: function(component, event, helper) {
        // Update the nickname field when 'tab' is pressed
        if (event.getParams().keyCode == 9) {
        	var nameInput = component.find("firstName");
        	var nameValue = nameInput.get("v.value");
        	var nickName = component.find("nickname");
            var today = new Date();
        	nickName.set("v.value", nameValue + today.valueOf(today));   
        }
    },
 
    /**
     * Capture the Inputs and invoke the helper.save with the input params 
     */
	  saveUserForm : function(component, event, helper) {
        var name = component.get("v.user.First");
        var last = component.get("v.user.Last");
        var password = component.get("v.user.Password__c");
        var email = component.get("v.user.Email__c");
        var nickname = component.get("v.user.Nickname");
        
        var passwordCmp = component.find("userPassword");
        var emailCmp = component.find("userEmail");
        
        helper.validatePassword(component, event, helper);
        helper.validateEmail(component, event, helper);
        if (passwordCmp.get("v.errors") == null && emailCmp.get("v.errors") == null) {
            component.set("v.hasErrors", false);
        	helper.save(component, name + " " + last, password, email, nickname);         
        } else {
            component.set("v.hasErrors", true);
        }
    },
    
    cancel : function(component, event, helper) {
        $A.get("e.force:closeQuickAction").fire();
    }
})
```

- `CreateUserHelper.js`—The code that deals with all the business logic, such as validating the password and email fields.
```javascript
({  
    save: function(component, name, password, email, nickname) {
        // Create a user record, save it, and close the panel
        var userRecord = {apiName: 'Test_User__c', fields: {}};
        userRecord.fields.Name = {value: name};
        userRecord.fields.Password__c = {value: password};
        userRecord.fields.Email__c = {value: email};
        userRecord.fields.Nickname__c = {value: nickname};
        userRecord.fields.Case_Study__c = {value: component.get("v.recordId")};
        // get the force:recordData and set the targetRecord
        component.find("frd").set('v.targetRecord', userRecord); 
        // invoke saveRecord of force:recordData
        component.find("frd").saveRecord($A.getCallback(function(response) {
            if (component.isValid() && response.state == "SUCCESS") {
                $A.get("e.force:closeQuickAction").fire();
                var toastEvent = $A.get("e.force:showToast");
                toastEvent.setParams({
                	"title": "Success!",
                    "message": "The test user has been created."
                });
                toastEvent.fire();		
                $A.get('e.force:refreshView').fire();
            } else if (response.state == "ERROR") {
                console.log('There was a problem and the state is: '+ response.state);
            }
        }));
    },
    
    validatePassword : function(component, event, helper) {
        var inputCmp = component.find("userPassword");
        var value = inputCmp.get("v.value");
        
        if (value == undefined) {
           inputCmp.set("v.errors", [{message: "You must enter a password."}]);
        } else if (value.length < 7 || value.length > 15) {
            inputCmp.set("v.errors", [{message: "The password is the wrong length (must be <= 15): " + value.length}]);
        } else if (value.search(/[0-9]+/) == -1) {
            inputCmp.set("v.errors", [{message: "The password must contain at least one number."}]);
        } else if (value.search(/[a-zA-Z]+/) == -1) {
            inputCmp.set("v.errors", [{message: "The password must contain at least one letter."}]);
        } else {
            inputCmp.set("v.errors", null);
        }
	},
    
    validateEmail : function(component, event, helper) {
        var inputCmp = component.find("userEmail");
        var value = inputCmp.get("v.value");
        
        if (value == undefined) {
           inputCmp.set("v.errors", [{message: "You must enter an email."}]);
           return;
        }
        
        var apos = value.indexOf("@");
        var dotpos = value.lastIndexOf(".");
        if (apos<1||dotpos-apos<2){
            inputCmp.set("v.errors", [{message: "Email is not in the correct format: " + value}]);
        } else if (value.substring(apos+1, dotpos) != "gmail") {
            inputCmp.set("v.errors", [{message: "Email must be a gmail account: " + value.substring(apos+1, dotpos)}]);
        } else {
            inputCmp.set("v.errors", null);
        }
	}
})
```

- `Setup | Object Manager | Buttons, Links, and Actions | New Action`
- Action Type `Lightning Component`
- Lightning Component	 `c:CreateUser`
- Define a label


### Integrate Third-Party APIs
- Using an Apex server-side controller
- The main compnent needs to implement `implements="flexipage:availableForAllPageTypes,force:hasRecordId,force:lightningQuickAction"`

- `SendTwilioSMS.cmp`
```html
<aura:component controller="TwilioSendSMSController" implements="flexipage:availableForAllPageTypes,force:hasRecordId,force:lightningQuickAction" >
   <aura:attribute name="textMessage" type="String" />
   <aura:attribute name="destinationNumber" type="String" />
   <aura:attribute name="messageError" type="Boolean" />
   <aura:attribute name="recordId" type="String" />
      <aura:handler name="init" value="{!this}" action="{!c.init}" />
   <aura:if isTrue="{!v.messageError}">
      <!-- Load error -->
      <div class="userCreateError">
         <ui:message title="Error" severity="error" closable="true">
            Unable to send message. Please review your data and try again.
         </ui:message>
      </div>
   </aura:if>
   <div class="slds-form--stacked">
      <label class="slds-form-element__label" for="instructMsg">Please enter the message (max 160 char) below: </label>
      <br/>
      <div class="slds-form-element__control">
         <ui:inputText class="slds-input" aura:id="message" label="Text Message" value="{!v.textMessage}" required="true" maxlength="160" size="165" />
      </div>
      <div class="centered">
         <ui:button class="slds-button slds-button--brand" press="{!c.sendMessage}" label="Send Message"/>
      </div>
   </div>
</aura:component>
```

- `SendTwilioSmsController.js`
```javascript
({
   init : function(component, event, helper) {
      var action = component.get("c.getPhoneNumber");
      action.setParams({"contactId": component.get("v.recordId")});
      action.setCallback(this, function(response) {
         var state = response.getState();
         if(component.isValid() && state == "SUCCESS"){
            component.set("v.destinationNumber", response.getReturnValue());
         } else {
            component.set("v.messageError", true);
         }
      });
      $A.enqueueAction(action);
   },
      sendMessage : function(component, event, helper) {
      var smsMessage = component.get("v.textMessage");
      var number = component.get("v.destinationNumber");
      var recordId = component.get("v.recordId")
      var action = component.get("c.sendMessages");
      action.setParams({"mobNumber": number, "message": smsMessage, "contactId": component.get("v.recordId")});
      action.setCallback(this, function(response) {
         var state = response.getState();
         if(component.isValid() && state == "SUCCESS"){
            $A.get("e.force:closeQuickAction").fire();
            var toastEvent = $A.get("e.force:showToast");
            toastEvent.setParams({
               "title": "Success!",
               "message": "SMS has been sent woo hoo!"
            });
            toastEvent.fire();
         } else {
            component.set("v.messageError", true);
         }
      });
      $A.enqueueAction(action);
   }
})
```

- `SendTwilioSmsController.apxc`
```java
/*
* Apex controller that currently contains only one method to send sms message
*/
global class TwilioSendSMSController {
   /*
   * This method uses the Twilio for Salesforce library class and method to
   * send the message using the Twilio api.
   */
   @AuraEnabled
      webService static String sendMessages(String mobNumber, String message, Id contactId) {
         System.debug('the mobNumber is: '+ mobNumber + ' and the message is: '+ message + ' and contactId is: ' + contactId);
         if (mobNumber == null) {
            mobNumber = getPhoneNumber(contactId);
         }
         try {
            TwilioRestClient client = TwilioAPI.getDefaultClient();
            Map<String,String> params = new Map<String,String> {
               'To' => mobNumber,
               'From' => '15555551234',
               'Body' => message
               };
            TwilioSMS sms = client.getAccount().getSMSMessages().create(params);
            return sms.getStatus();
         } catch(exception ex) {
            System.debug('oh no, it failed: '+ex);
            return 'failed';
         }
      }
      @AuraEnabled
      public static String getPhoneNumber(Id contactId) {
         Contact currentRecord = [SELECT Phone FROM Contact WHERE Id = :contactId];
         return currentRecord.Phone.replace(' ', '').replace('-', '').replace(')', '').replace('(', '');
   }
}
```

## Solution to challenge
1. Install [QuickContact](https://login.salesforce.com/packaging/installPackage.apexp?p0=04to0000000Ck0L) package 
2. Add `implements="flexipage:availableForAllPageTypes,force:hasRecordId,force:lightningQuickAction"` to `quickcontact.cmp` component.