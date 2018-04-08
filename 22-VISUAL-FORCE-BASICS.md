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

