# Lightning Components: Attributes and Expressions

## Attributes

* Attributes on components are like instance variables in objects.
* Similar to props in React.
* Example attribute `message`

```html
<aura:component>

    <c:helloMessage message="You look nice today."/>

</aura:component>
```

* An attribute is defined using an `<aura:attribute>` tag, which requires values for the `name` and `type` attributes, and accepts these optional attributes: `default`, `description`, `required`.

```html
<aura:component>
    <aura:attribute name="message" type="String"/>
    <p>Hello! [ message goes here, soon ]</p>
</aura:component>
```

## Expressions

* An expression is any set of literal values, variables, sub-expressions, or operators that can be resolved to a single value.
* An expression is basically a formula, or a calculation, which you place within expression delimiters `{!<expression>}`
* Example expression `{!v.message}`

```html
<aura:component>
    <aura:attribute name="message" type="String"/>
    <p>Hello! {!v.message}</p>
</aura:component>
```

* More complex expression

```html
<aura:component>
    <aura:attribute name="message" type="String"/>
    <p>{!'Hello! ' + v.message}</p>
</aura:component>
```

* Passing expressions to a child component

```html
<aura:component>
    <aura:attribute name="customMessage" type="String"/>
    <p> <c:helloMessage message="{!v.customMessage}"/> </p>
</aura:component>
```

## Value Providers

* In the previous example, `v` is something called a value provider.
* Value providers are a way to group, encapsulate, and access related data.

## Attribute Data Types

* Can be any of the following:
  * Primitives data types, such as Boolean, Date, DateTime, Decimal, Double, Integer, Long, or String. The usual suspects in any programming language.
  * Standard and custom Salesforce objects, such as Account or MyCustomObject\_\_c.
  * Collections, such as List, Map, and Set.
  * Custom Apex classes.
  * Framework-specific types, such as Aura.Component, or Aura.Component[].

### Other Aspects of Attribute Definitions

* The `default` attribute defines the default attribute value. It’s used when the attribute is referenced and you haven’t yet set the attribute’s value.
* The `required` attribute defines whether the attribute is required. The default is false.
* The `description` attribute defines a brief summary of the attribute and its usage.

### Hello Playground Example

* helloMessage.cmp

```html
<aura:component>
    <aura:attribute name="message" type="String"/>
    <p>Hello! {!v.message}</p>
</aura:component>
```

* helloPlayground

```html
<aura:component>
    <aura:attribute name="messages" type="List"
        default="['You look nice today.',
            'Great weather we\'re having.',
            'How are you?']"/>

    <h1>Hello Playground</h1>
    <p>Silly fun with attributes and expressions.</p>

    <h2>List Items</h2>
    <p><c:helloMessage message="{!v.messages[0]}"/></p>
    <p><c:helloMessage message="{!v.messages[1]}"/></p>
    <p><c:helloMessage message="{!v.messages[2]}"/></p>

    <h2>List Iteration</h2>
    <aura:iteration items="{!v.messages}" var="msg">
        <p><c:helloMessage message="{!msg}"/></p>
    </aura:iteration>

    <h2>Conditional Expressions and Global Value Providers</h2>
    <aura:if isTrue="{!$Browser.isIPhone}">
        <p><c:helloMessage message="{!v.messages[0]}"/></p>
    <aura:set attribute="else">
        <p><c:helloMessage message="{!v.messages[1]}"/></p>
        </aura:set>
    </aura:if>
</aura:component>
```

* `<aura:iteration>` component repeats its body once per item in its items attribute.
* `<aura:if>` lets you add conditions.

## Solution to challenge

```html
<aura:component>
    <aura:attribute name="item" type="Camping_Item__c" required="true"/>
    <div>
      Name: {!v.item.Name}
    </div>
    <div>
      Price: <lightning:formattedNumber value="{!v.item.Price__c}" style="currency" />
    </div>
    <div>
      Quantity: <lightning:formattedNumber value="{!v.item.Quantity__c }" style="decimal" />
    </div>
    <div>
	    <lightning:input type="toggle" label="Packed" name="Packed" checked="{!v.item.Packed__c}" />
    </div>
</aura:component>
```
