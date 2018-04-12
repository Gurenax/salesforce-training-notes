# Lightning Components: Handling Actions with Controllers

## Controller

* A controller is basically a collection of code that defines your app’s behavior which are user inputs, timer and other events, data updates, and so on.

* helloMessageInteractive

```html
<aura:component >

    <aura:attribute name="message" type="String"/>

    <p>Message of the day: {!v.message}</p>

    <div>
        <lightning:button label="You look nice today."
            onclick="{!c.handleClick}"/>

        <lightning:button label="Today is going to be a great day!"
            onclick="{!c.handleClick}"/>
    </div>

</aura:component>
```

* helloMessageInteractiveController.js

```javascript
({
  handleClick: function(component, event, helper) {
    var btnClicked = event.getSource() // the button
    var btnMessage = btnClicked.get('v.label') // the button's label
    component.set('v.message', btnMessage) // update our message
  }
})
```

* In lightning, the controller is a JavaScript object which is a key-value pair of the `action handler` and the value is a `function definition`.
* function definition attributes:
  * `component` — the component. In this case, it’s helloMessageInteractive.
  * `event` — the event that caused the action handler to be called.
  * `helper` — the component’s helper, another JavaScript resource of reusable functions.

### Action Handler

* The combination of name-value pair and specific function signature is an `action handler`.

### Function Chaining, Rewiring, and Simple Debugging

```javascript
({
  handleClick: function(component, event, helper) {
    var btnClicked = event.getSource() // the button
    var btnMessage = btnClicked.get('v.label') // the button's label
    component.set('v.message', btnMessage) // update our message
  },
  handleClick2: function(component, event, helper) {
    var newMessage = event.getSource().get('v.label')
    console.log('handleClick2: Message: ' + newMessage) // Debug message to browser console
    component.set('v.message', newMessage)
  },
  handleClick3: function(component, event, helper) {
    component.set('v.message', event.getSource().get('v.label'))
  }
})
```

## Solution to challenge

```javascript
({
  packItem: function(component, event, helper) {
    // Get the value of item
    var new_item = component.get('v.item')
    // Set the new value Packed__c
    new_item.Packed__c = true
    component.set('v.item', new_item)

    // Get the button and its disabled attribute
    var button = event.getSource()
    var buttonDisabled = button.get('v.disabled')
    // Set the disabled value to true
    component.set(buttonDisabled, true)
  }
})
```
