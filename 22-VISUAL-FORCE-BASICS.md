# Visualforce Basics
- `Visualforce` is a web development framework that enables developers to build sophisticated, custom user interfaces for mobile and desktop apps that can be hosted on the Lightning Platform platform. You can use Visualforce to build apps that align with the styling of Lightning Experience, as well as your own completely custom interface.

## Reference
- [Visualforce Basics](https://trailhead.salesforce.com/trails/force_com_dev_beginner/modules/visualforce_fundamentals)

## Example Visualforce Page
```html
<apex:page standardController="Contact" >
    <apex:form >
        
        <apex:pageBlock title="Edit Contact">
            <apex:pageBlockSection columns="1">
                <apex:inputField value="{!Contact.FirstName}"/>
                <apex:inputField value="{!Contact.LastName}"/>
                <apex:inputField value="{!Contact.Email}"/>
                <apex:inputField value="{!Contact.Birthdate}"/>
            </apex:pageBlockSection>
            <apex:pageBlockButtons >
                <apex:commandButton action="{!save}" value="Save"/>
            </apex:pageBlockButtons>
        </apex:pageBlock>
        
    </apex:form>
</apex:page>
```
---

## Create & Edit Visualforce Pages 
- Visualforce pages are basic building blocks for application developers.
- A Visualforce page is similar to a standard Web page, but includes powerful features to access, display, and update your organization’s data. Pages can be referenced and invoked via a unique URL, just as they would be on a traditional web server.

- Visualforce uses a tag-based markup language that’s similar to HTML. Each Visualforce tag corresponds to a coarse or fine-grained user interface component, such as a section of a page, a list view, or an individual field. 

- Visualforce boasts nearly 150 built-in components, and provides a way for developers to create their own components.

- Visualforce markup can be freely mixed with HTML markup, CSS styles, and JavaScript libraries, giving you considerable flexibility in how you implement your app’s user interface.

### Hello World
```html
<apex:page> 
    <h1>Hello World</h1> 
</apex:page>
```

### sidebar and showHeader options have no effect in lightning experience
```html
<apex:page sidebar="false" showHeader="false">
    <h1>Hello World</h1>
    <p>
        This is my first Visualforce Page!
    </p>
</apex:page>
```

### Removing standard Salesforce Stylesheets (CSS)
```html
<apex:page standardStylesheets="false">
    <h1>Hello World</h1>
    <p>
        This is my first Visualforce Page!
    </p>
</apex:page>
```

### Page Blocks and Page Block Sections
- You cannot use page blocks sections without parent page blocks.
```html
<apex:page> 
    <h1>Hello World</h1> 
    
    <apex:pageBlock title="A Block Title">
        <apex:pageBlockSection title="A Section Title">
            I'm three components deep!
        </apex:pageBlockSection>
        
        <apex:pageBlockSection title="A New Section">
            This is another section.
        </apex:pageBlockSection>
    </apex:pageBlock>
</apex:page>
```


### Two other ways to build Visualforce pages
- The development mode `quick fix` and footer is a fast way to quickly create a new page, or make short edits to an existing page. It’s great for quick changes or when you want to create a short page to test out some new code on a blank slate, before incorporating it into your app pages.

- The `Setup editor`, available in Setup by entering Visualforce Pages in the Quick Find box, then selecting `Visualforce Pages`, is the most basic editor, but it provides access to page settings that aren’t available in the Developer Console or development mode footer.


### Solution to Visualforce page challenge
```html
<apex:page showHeader="false">
    <apex:image id="salesforceLogo" value="https://developer.salesforce.com/files/salesforce-developer-network-logo.png" width="220" height="55" alt="Salesforce Logo"/>
</apex:page>
```


---

## Global Variables and Visualforce Expressions
- Visualforce pages can display data retrieved from the database or web services, data that changes depending on who is logged on and viewing the page, and so on. This dynamic data is accessed in markup through the use of global variables, calculations, and properties made available by the page’s controller.

- Together these are described generally as `Visualforce expressions`. Use expressions for dynamic output or passing values into components by assigning them to attributes.

### Visualforce Expressions
- A Visualforce expression is any set of literal values, variables, sub-expressions, or operators that can be resolved to a single value. Method calls aren’t allowed in expressions.

- The expression syntax in Visualforce is: `{! expression }`

- Anything inside the `{! }` delimiters is evaluated and dynamically replaced when the page is rendered or when the value is used. Whitespace is ignored.

- The resulting value can be a primitive (integer, string, and so on), a Boolean, an sObject, a controller method such as an action method, and other useful results.


### Global Variables
- Use global variables to access and display system values and resources in your Visualforce markup.

- For example, Visualforce provides information about the logged-in user in a global variable called `$User`. You can access fields of the $User global variable (and any others) using an expression of the following form: `{! $GlobalName.fieldName }`.


### Sample visualforce expressions with global variables
```html
<apex:page>
    
    <apex:pageBlock title="User Status">
        <apex:pageBlockSection columns="1">
            
            {! $User.FirstName } {! $User.LastName } 
           ({! $User.Username })
            
        </apex:pageBlockSection>
    </apex:pageBlock>
    
</apex:page>
```

### Formula Expressions
- Visualforce lets you use more than just global variables in the expression language. It also supports formulas that let you manipulate values.

- For example, the `&` character is the formula language operator that concatenates strings.

- Sample snippets
```html
{! $User.FirstName & ' ' & $User.LastName }
```

```html
<p> Today's Date is {! TODAY() } </p>
<p> Next week it will be {! TODAY() + 7 } </p>
```

```html
Today's Date is Thu Sep 18 00:00:00 GMT 2014
Next week it will be Thu Sep 25 00:00:00 GMT 2014
```

```html
<p>The year today is {! YEAR(TODAY()) }</p>
<p>Tomorrow will be day number  {! DAY(TODAY() + 1) }</p>
<p>Let's find a maximum: {! MAX(1,2,3,4,5,6,5,4,3,2,1) } </p>
<p>The square root of 49 is {! SQRT(49) }</p>
<p>Is it true?  {! CONTAINS('salesforce.com', 'force.com') }</p>
```

- UserStatus page so far
```html
<apex:page>
    
    <apex:pageBlock title="User Status">
        <apex:pageBlockSection columns="1">
            
            {! $User.FirstName & ' ' & $User.LastName }
           ({! $User.Username })
            
            <p>The year today is {! YEAR(TODAY()) }</p>
            <p>Tomorrow will be day number  {! DAY(TODAY() + 1) }</p>
            <p>Let's find a maximum: {! MAX(1,2,3,4,5,6,5,4,3,2,1) } </p>
            <p>The square root of 49 is {! SQRT(49) }</p>
            <p>Is it true?  {! CONTAINS('salesforce.com', 'force.com') }</p>
        </apex:pageBlockSection>
    </apex:pageBlock>
    
</apex:page>
```

### Conditional Expressions
- Use a `conditional expression` to display different information based on the value of the expression.

- You can do this in Visualforce by using a conditional formula expression, such as `IF()`. The `IF()` expression takes three arguments:

- Sample snippets
```html
<p>{! IF( CONTAINS('salesforce.com','force.com'), 
     'Yep', 'Nope') }</p>
<p>{! IF( DAY(TODAY()) < 15, 
     'Before the 15th', 'The 15th or after') }</p>
```

```html
({! IF($User.isActive, $User.Username, 'inactive') })
```

- UserStatus page so far
```html
<apex:page>
    
    <apex:pageBlock title="User Status">
        <apex:pageBlockSection columns="1">
            
            {! $User.FirstName & ' ' & $User.LastName }
           ({! IF($User.isActive, $User.Username, 'inactive') })
            
            <p>The year today is {! YEAR(TODAY()) }</p>
            <p>Tomorrow will be day number  {! DAY(TODAY() + 1) }</p>
            <p>Let's find a maximum: {! MAX(1,2,3,4,5,6,5,4,3,2,1) } </p>
            <p>The square root of 49 is {! SQRT(49) }</p>
            <p>Is it true?  {! CONTAINS('salesforce.com', 'force.com') }</p>
            <p>{! IF( CONTAINS('salesforce.com','force.com'), 
                'Yep', 'Nope') }</p>
            <p>{! IF( DAY(TODAY()) < 15, 
                'Before the 15th', 'The 15th or after') }</p>
        </apex:pageBlockSection>
    </apex:pageBlock>
    
</apex:page>
```