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

### Solution to global variables challenge
```html
<apex:page >
    <apex:pageBlock title="User Status">
        <apex:pageBlockSection columns="1">
            <apex:pageBlockSectionItem>
            	<p>
                    {! $User.FirstName & ' ' & $User.LastName }
                    ({! IF($User.isActive, $User.Username, 'inactive') })
                </p>
                <p>
                    First Name: {! $User.FirstName }
                </p>
                <p>
                    Last Name: {! $User.LastName }
                </p>
				<p>
                    Alias: {! $User.Alias }
                </p>
                <p>
                    Company: {!$User.CompanyName}
                </p>
            </apex:pageBlockSectionItem>
        </apex:pageBlockSection>
	</apex:pageBlock>
</apex:page>
```

---

## Visualforce Standard Controller
- Visualforce uses the traditional `model–view–controller (MVC) paradigm`, and includes sophisticated built-in controllers to handle standard actions and data access, providing simple and tight integration with the Lightning Platform database. These built-in controllers are referred to generally as `standard controllers`, or even `the` standard controller.

### JavaScript to open a Visualforce page from a standard page
- using browser console
```javascript
$A.get("e.force:navigateToURL").setParams(
    {"url": "/apex/pageName?&id=<ID of Object e.g. Account>"}).fire();
```

### Sample Account Summary
```html
<apex:page standardController="Account">
    
    <apex:pageBlock title="Account Summary">
        <apex:pageBlockSection>
        	
            Name: {! Account.Name } <br/>
            Phone: {! Account.Phone } <br/>
            Industry: {! Account.Industry } <br/>
            Revenue: {! Account.AnnualRevenue } <br/>
            
        </apex:pageBlockSection>
    </apex:pageBlock>
    
</apex:page>
```

- Append `id` in URL as such:
`https://<salesforce-instance>/apex/AccountSummary?core.apexpages.request.devconsole=1&id=<id of an account>`


### Solution to Contact View Challenge
```html
<apex:page standardController="Contact">
    
    <apex:pageBlock title="Contact Summary">
        <apex:pageBlockSection>
        	
            First Name: {! Contact.FirstName } <br/>
            Last Name: {! Contact.LastName } <br/>
            Owner Email: {! Contact.Owner.Email } <br/>

        </apex:pageBlockSection>
    </apex:pageBlock>
    
</apex:page>
```

---

## Display Records, Fields, and Tables 

### Output Components
- Components that output data from a record and enable you to design a view-only user interface.

- Visualforce includes nearly 150 built-in components that you can use on your pages. Components are rendered into HTML, CSS, and JavaScript when a page is requested.

  - `Coarse-grained components` provide a significant amount of functionality in a single component, and might add a lot of information and user interface to the page it’s used on.

  -  `Fine-grained components` provide more focused functionality, and enable you to design the page to look and behave the way you want.


### Display Record Details
```html
<apex:page standardController="Account">
    
    <apex:detail />
    
</apex:page>
```

- `<apex:detail>` is a coarse-grained output component that adds many fields, sections, buttons, and other user interface elements to the page in just one line of markup.


### Display Related Lists
```html
<apex:page standardController="Account">
    
    <apex:detail relatedList="false" />
    <apex:relatedList list="Contacts"/>
	  <apex:relatedList list="Opportunities" pageSize="5"/>

</apex:page>
```

- `relatedList="false"` removes the default related lists sections
- `<apex:relatedList>` adds specific related lists sections
- Use higher level components when they offer the functionality you need, and use lower level components when you need more control over what’s displayed on the page.


### Display Individual Fields
- Replacing `<apex:detail>` with specific fields wrapped in page block and page block section.
```html
<apex:page standardController="Account">
    
	<apex:pageBlock title="Account Details">
      <apex:pageBlockSection>
          <apex:outputField value="{! Account.Name }"/>
          <apex:outputField value="{! Account.Phone }"/>
          <apex:outputField value="{! Account.Industry }"/>
          <apex:outputField value="{! Account.AnnualRevenue }"/>
      </apex:pageBlockSection>
  </apex:pageBlock>

  <apex:relatedList list="Contacts"/>
	<apex:relatedList list="Opportunities" pageSize="5"/>

</apex:page>
```

### Display a Table
- `<apex:pageBlockTable>` is an iteration component that generates a table of data, complete with platform styling. Here’s what’s going on in your markup.
- An `iteration component` works with a collection of similar items, instead of on a single value.

```html
<apex:page standardController="Account">
    
	<apex:pageBlock title="Account Details">
        <apex:pageBlockSection>
            <apex:outputField value="{! Account.Name }"/>
            <apex:outputField value="{! Account.Phone }"/>
            <apex:outputField value="{! Account.Industry }"/>
            <apex:outputField value="{! Account.AnnualRevenue }"/>
        </apex:pageBlockSection>
    </apex:pageBlock>

	<apex:pageBlock title="Contacts">
       <apex:pageBlockTable value="{!Account.contacts}" var="contact">
          <apex:column value="{!contact.Name}"/>
          <apex:column value="{!contact.Title}"/>
          <apex:column value="{!contact.Phone}"/>
       </apex:pageBlockTable>
    </apex:pageBlock>

    <apex:pageBlock title="Opportunities">
       <apex:pageBlockTable value="{!Account.opportunities}" var="opportunity">
          <apex:column value="{!opportunity.Name}"/>
          <apex:column value="{!opportunity.CloseDate}"/>
          <apex:column value="{!opportunity.StageName}"/>
       </apex:pageBlockTable>
    </apex:pageBlock>
</apex:page>
```

- Replaced `<apex:relatedList>` with specific fields using page block table and columns wrapped in page blocks.


### Coarse-grained vs Fine-grained
- `Coarse-grained components` let you quickly add lots of functionality to a page, while `Fine-grained components` give you more control over the specific details of a page.

### Solution to Opportunity View challenge
```html
<apex:page standardController="Opportunity">
    
    <apex:pageBlock title="Opportunity Summary">
        <apex:pageBlockSection>
			<apex:outputField value="{! Opportunity.Name }"/>
            <apex:outputField value="{! Opportunity.Amount }"/>
            <apex:outputField value="{! Opportunity.CloseDate }"/>
            <apex:outputField value="{! Opportunity.Account.Name }"/>
        </apex:pageBlockSection>
    </apex:pageBlock>
    
</apex:page>
```

---

## Input Data Using Forms

### Create a Basic Form
- Use `<apex:form>` and `<apex:inputField>` to create a page to edit data. Combine `<apex:commandButton>` with the save action built into the standard controller to create a new record, or save changes to an existing one.
```html
<apex:page standardController="Account">
    
    <h1>Edit Account</h1>
    
    <apex:form>
    
        <apex:inputField value="{! Account.Name }"/>
        
        <apex:commandButton action="{! save }" value="Save" />
    
    </apex:form>
    
</apex:page>
```

### Add Field Labels and Platform Styling
- When you use input components within `<apex:pageBlock>` and `<apex:pageBlockSection>` tags they adopt the platform visual styling.
- The `<apex:inputField>` component renders the appropriate input widget based on a standard or custom object field’s type. For example, if you use an `<apex:inputField>` tag to display a date field, a calendar widget displays on the form. If you use an `<apex:inputField>` tag to display a picklist field, as we did here for the industry field, a drop-down list displays instead.

```html
<apex:page standardController="Account">
    <apex:form>
    
    <apex:pageBlock title="Edit Account">
		<apex:pageBlockSection columns="1">
            <apex:inputField value="{! Account.Name }"/>
            <apex:inputField value="{! Account.Phone }"/>        
            <apex:inputField value="{! Account.Industry }"/>        
            <apex:inputField value="{! Account.AnnualRevenue }"/>
        </apex:pageBlockSection>
        <apex:pageBlockButtons>
            <apex:commandButton action="{! save }" value="Save" />        
        </apex:pageBlockButtons>
    </apex:pageBlock>
    
    </apex:form>
</apex:page>
```

### Display Form Errors and Messages
- Use `<apex:pageMessages>` to display any form handling errors or messages.

```html
<apex:page standardController="Account">
    <apex:form>
    
    <apex:pageBlock title="Edit Account">
        <apex:pageMessages/>
		<apex:pageBlockSection columns="1">
            <apex:inputField value="{! Account.Name }"/>
            <apex:inputField value="{! Account.Phone }"/>        
            <apex:inputField value="{! Account.Industry }"/>        
            <apex:inputField value="{! Account.AnnualRevenue }"/>
        </apex:pageBlockSection>
        <apex:pageBlockButtons>
            <apex:commandButton action="{! save }" value="Save" />        
        </apex:pageBlockButtons>
    </apex:pageBlock>
    
    </apex:form>
</apex:page>
```

### Edit Related Records
- While you can’t save changes to multiple object types in one request with the standard controller, you can make it easier to navigate to related records by offering links that perform actions such as edit or delete on those records.
```html
<apex:page standardController="Account">
    <apex:form>
    
    <apex:pageBlock title="Edit Account">
        <apex:pageMessages/>
		<apex:pageBlockSection columns="1">
            <apex:inputField value="{! Account.Name }"/>
            <apex:inputField value="{! Account.Phone }"/>        
            <apex:inputField value="{! Account.Industry }"/>        
            <apex:inputField value="{! Account.AnnualRevenue }"/>
        </apex:pageBlockSection>
        <apex:pageBlockButtons>
            <apex:commandButton action="{! save }" value="Save" />       
        </apex:pageBlockButtons>
    </apex:pageBlock>
    <apex:pageBlock title="Contacts">
        <apex:pageBlockTable value="{!Account.contacts}" var="contact">
            <apex:column>
                <apex:outputLink
                    value="{! URLFOR($Action.Contact.Edit, contact.Id) }">
                    Edit
                </apex:outputLink>
                &nbsp;
                <apex:outputLink
                    value="{! URLFOR($Action.Contact.Delete, contact.Id) }">
                    Del
                </apex:outputLink>
            </apex:column>
            <apex:column value="{!contact.Name}"/>
            <apex:column value="{!contact.Title}"/>
            <apex:column value="{!contact.Phone}"/>
        </apex:pageBlockTable>
    </apex:pageBlock>
    </apex:form>
</apex:page>
```

### Solution to Contact Create challenge
```html
<apex:page standardController="Contact">
    <apex:form>
    	<apex:pageBlock title="Create Contact">
            <apex:pageMessages/>
            <apex:pageBlockSection columns="1">
                <apex:inputField value="{! Contact.FirstName }" />
                <apex:inputField value="{! Contact.LastName }" />
                <apex:inputField value="{! Contact.Email }" />
            </apex:pageBlockSection>
            <apex:pageBlockButtons>
                <apex:commandButton action="{! save }" value="Save" />
            </apex:pageBlockButtons>
        </apex:pageBlock>
    </apex:form>
</apex:page>
```

---

## Standard List Controller
- The standard list controller allows you to create Visualforce pages that can display or act on a set of records.

### Display a List of Records
- Use the standard list controller and an iteration component, such as `<apex:pageBlockTable>`, to display a list of records.

```html
<apex:page standardController="Contact" recordSetVar="contacts">
    <apex:pageBlock title="Contacts List">
        
        <!-- Contacts List -->
        <apex:pageBlockTable value="{! contacts }" var="ct">
            <apex:column value="{! ct.FirstName }"/>
            <apex:column value="{! ct.LastName }"/>
            <apex:column value="{! ct.Email }"/>
            <apex:column value="{! ct.Account.Name }"/>
        </apex:pageBlockTable>
        
    </apex:pageBlock>
</apex:page>
```
- `recordSetVar` by convention is usually named the plural of the object name.
- `<apex:pageBlockTable>` is an iteration component that generates a table of data, complete with platform styling. Here’s what’s going on in the table markup.
- There are other iteration components such as `<apex:dataList>` and `<apex:repeat>`.

### Add List View Filtering to the List
- Use `{! listViewOptions }` to get a list of list view filters available for an object.
- Use `{! filterId }` to set the list view filter to use for a standard list controller’s results.

```html
<apex:page standardController="Contact" recordSetVar="contacts">
    <apex:form>
        <apex:pageBlock title="Contacts List" id="contacts_list">
        
            Filter: 
            <apex:selectList value="{! filterId }" size="1">
                <apex:selectOptions value="{! listViewOptions }"/>
                <apex:actionSupport event="onchange" reRender="contacts_list"/>
            </apex:selectList>
            <!-- Contacts List -->
            <apex:pageBlockTable value="{! contacts }" var="ct">
                <apex:column value="{! ct.FirstName }"/>
                <apex:column value="{! ct.LastName }"/>
                <apex:column value="{! ct.Email }"/>
                <apex:column value="{! ct.Account.Name }"/>
            </apex:pageBlockTable>
            
        </apex:pageBlock>
    </apex:form>
</apex:page>
```
- `<apex:actionSupport event="onchange" reRender="contacts_list"/>` reloads the `contacts_list` page block without reloading the entire page.


### Add Pagination to the List
- Use the pagination features of the standard list controller to allow users to look at long lists of records one “page” at a time.

```html
<apex:page standardController="Contact" recordSetVar="contacts">
    <apex:form>
        <apex:pageBlock title="Contacts List" id="contacts_list">
        
            Filter: 
            <apex:selectList value="{! filterId }" size="1">
                <apex:selectOptions value="{! listViewOptions }"/>
                <apex:actionSupport event="onchange" reRender="contacts_list"/>
            </apex:selectList>
            <!-- Contacts List -->
            <apex:pageBlockTable value="{! contacts }" var="ct">
                <apex:column value="{! ct.FirstName }"/>
                <apex:column value="{! ct.LastName }"/>
                <apex:column value="{! ct.Email }"/>
                <apex:column value="{! ct.Account.Name }"/>
            </apex:pageBlockTable>
            
            <!-- Pagination -->
            <table style="width: 100%"><tr>
                <td>
                    <!-- Page X of Y -->
                    Page: <apex:outputText 
                        value=" {!PageNumber} of {! CEILING(ResultSize / PageSize) }"/>
                </td>            
                <td align="center">
                    <!-- Previous page -->
                    <!-- active -->
                    <apex:commandLink action="{! Previous }" value="« Previous"
                         rendered="{! HasPrevious }"/>
                    <!-- inactive (no earlier pages) -->
                    <apex:outputText style="color: #ccc;" value="« Previous"
                         rendered="{! NOT(HasPrevious) }"/>

                    &nbsp;&nbsp;  
                    <!-- Next page -->
                    <!-- active -->
                    <apex:commandLink action="{! Next }" value="Next »"
                         rendered="{! HasNext }"/>
                    <!-- inactive (no more pages) -->
                    <apex:outputText style="color: #ccc;" value="Next »"
                         rendered="{! NOT(HasNext) }"/>
                </td>
                
                <td align="right">
                    <!-- Records per page -->
                    Records per page:
                    <apex:selectList value="{! PageSize }" size="1">
                        <apex:selectOption itemValue="5" itemLabel="5"/>
                        <apex:selectOption itemValue="20" itemLabel="20"/>
                        <apex:actionSupport event="onchange" reRender="contacts_list"/>
                    </apex:selectList>
                </td>
            </tr></table>
        </apex:pageBlock>
    </apex:form>
</apex:page>
```
- In the progress indicator, three properties are used to indicate how many pages there are: `PageNumber`, `ResultSize`, and `PageSize`.
- The page markup is referencing Boolean properties provided by the standard list controller, `HasPrevious` and `HasNext`, which let you know if there are more records in a given direction or not. 
- The records per page selection menu uses an `<apex:selectList>`.
- Aside from `Previous` and `Next`, there are also `First` and `Last` actions for pagination.


### Solution to Account Record Detail challenge
```html
<apex:page standardController="Account" recordSetVar="accounts">
    <apex:form>
        <apex:pageBlock title="Accounts List" id="accounts_list">
            Filter: 
            <apex:selectList value="{! filterId }" size="1">
                <apex:selectOptions value="{! listViewOptions }"/>
                <apex:actionSupport event="onchange" reRender="accounts_list"/>
            </apex:selectList>
            <apex:repeat value="{! accounts }" var="a">
                <li>
                    <apex:outputLink value="/{!a.id}">
                        {! a.name }
                    </apex:outputLink>
                </li>
            </apex:repeat>
        </apex:pageBlock>
    </apex:form>
</apex:page>
```

---

## Static Resources
- Static resources allow you to upload content that you can reference in a Visualforce page. Resources can be archives (such as .zip and .jar files), images, stylesheets, JavaScript, and other files.

- Static resources are managed and distributed by Lightning Platform, which acts as a content distribution network (CDN) for the files. Caching and distribution are handled automatically.

- Static resources are referenced using the `$Resource` global variable, which can be used directly by Visualforce, or used as a parameter to functions such as `URLFOR()`.

### Create and Upload a Simple Static Resource
- Setup | Static Resources | New
- e.g. Name as `JQuery`
- Cache Control should be Public


### Add a Static Resource to a Visualforce Page
```html
<apex:page>
    
    <!-- Add the static resource to page's <head> -->
    <apex:includeScript value="{! $Resource.jQuery }"/>
    
    <!-- A short bit of jQuery to test it's there -->
    <script type="text/javascript">
        jQuery.noConflict();
        jQuery(document).ready(function() {
            jQuery("#message").html("Hello from jQuery!");
        });
    </script>
    
    <!-- Where the jQuery message will appear -->
    <h1 id="message"></h1>
    
</apex:page>
```

### Create and Upload a Zipped Static Resource
- Create zipped static resources to group together related files that are usually updated together.
- Setup | Static Resources | New
- e.g. Name as `jQueryMobile`
- Cache Control should be Public
- Zip file limit is 5MB (i.e. for jQueryMobile, unzip and remove the demos folder then zip it again)

### Add Zipped Static Resources to a Visualforce Page
```html
<apex:page showHeader="false" sidebar="false" standardStylesheets="false">
    
    <!-- Add static resources to page's <head> -->
    <apex:stylesheet value="{!
          URLFOR($Resource.jQueryMobile,'jquery.mobile-1.4.5/jquery.mobile-1.4.5.css')}"/>
    <apex:includeScript value="{! $Resource.jQuery }"/>
    <apex:includeScript value="{!
         URLFOR($Resource.jQueryMobile,'jquery.mobile-1.4.5/jquery.mobile-1.4.5.js')}"/>
    <div style="margin-left: auto; margin-right: auto; width: 50%">
        <!-- Display images directly referenced in a static resource -->
        <h3>Images</h3>
        <p>A hidden message:
            <apex:image alt="eye" title="eye"
                 url="{!URLFOR($Resource.jQueryMobile, 'jquery.mobile-1.4.5/images/icons-png/eye-black.png')}"/>
            <apex:image alt="heart" title="heart"
                 url="{!URLFOR($Resource.jQueryMobile, 'jquery.mobile-1.4.5/images/icons-png/heart-black.png')}"/>
            <apex:image alt="cloud" title="cloud"
                 url="{!URLFOR($Resource.jQueryMobile, 'jquery.mobile-1.4.5/images/icons-png/cloud-black.png')}"/>
        </p>
    <!-- Display images referenced by CSS styles,
         all from a static resource. -->
    <h3>Background Images on Buttons</h3>
    <button class="ui-btn ui-shadow ui-corner-all
         ui-btn-icon-left ui-icon-action">action</button>
    <button class="ui-btn ui-shadow ui-corner-all
         ui-btn-icon-left ui-icon-star">star</button>
    </div>
</apex:page>
```
- Use the `$Resource` global variable and the `URLFOR()` function to reference items within a zipped static resource.
- The `URLFOR()` function can combine a reference to a zipped static resource and a relative path to an item within it to create a URL that can be used with Visualforce components that reference static assets. For example, `URLFOR($Resource.jQueryMobile, 'images/icons-png/cloud-black.png')` returns a URL to a specific graphic asset within the zipped static resource, which can be used by the `<apex:image>` component. You can construct similar URLs for JavaScript and stylesheet files for the `<apex:includeScript>` and `<apex:stylesheet>` components.

### Solution to static resource challenge
```html
<apex:page >
    <apex:image url="{!URLFOR($Resource.vfimagetest, 'cats/kitten1.jpg')}" />
</apex:page>
```

---

## Custom Controllers
- Custom controllers contain custom logic and data manipulation that can be used by a Visualforce page. For example, a custom controller can retrieve a list of items to be displayed, make a callout to an external web service, validate and insert data, and more—and all of these operations will be available to the Visualforce page that uses it as a controller.

- Used when you want to override existing functionality, customize the navigation through an application, use callouts or Web services, or if you need finer control for how information is accessed for your page.


### Create a Custom Controller Apex Class
```java
public class ContactsListController {
    // Controller code goes here
}
```

### Create a Visualforce Page that Uses a Custom Controller
```html
<apex:page controller="ContactsListController">
    <apex:form>
        <apex:pageBlock title="Contacts List" id="contacts_list">
            
            <!-- Contacts List goes here -->
        </apex:pageBlock>
    </apex:form>
</apex:page>
```


### Add a Method to Retrieve Records
- Apex Class
```java
public class ContactsListController {
	private String sortOrder = 'LastName';
        
    public List<Contact> getContacts() {
        
        List<Contact> results = Database.query(
            'SELECT Id, FirstName, LastName, Title, Email ' +
            'FROM Contact ' +
            'ORDER BY ' + sortOrder + ' ASC ' +
            'LIMIT 10'
        );
        return results;
    }
}
```
- Visualforce Page
```html
<apex:page controller="ContactsListController">
    <apex:form>
        <apex:pageBlock title="Contacts List" id="contacts_list">
            
            <!-- Contacts List -->
            <apex:pageBlockTable value="{! contacts }" var="ct">
                <apex:column value="{! ct.FirstName }"/>
                <apex:column value="{! ct.LastName }"/>
                <apex:column value="{! ct.Title }"/>
                <apex:column value="{! ct.Email }"/>
                
            </apex:pageBlockTable>
        </apex:pageBlock>
    </apex:form>
</apex:page>
```

- Any getter method (i.e. `getSomething()` ) is translated to a `{! something }` variable.
- As such, when happens when the `{! contacts }` expression is evaluated, that expression calls the `getContacts()` method. That method returns a List of contact records, which is exactly what the `<apex:pageBlockTable>` is expecting.


### Add a New Action Method
- Create action methods in your custom controller to respond to user input on the page.

- Apex Class
```java
public class ContactsListController {
	private String sortOrder = 'LastName';
        
    public List<Contact> getContacts() {
        
        List<Contact> results = Database.query(
            'SELECT Id, FirstName, LastName, Title, Email ' +
            'FROM Contact ' +
            'ORDER BY ' + sortOrder + ' ASC ' +
            'LIMIT 10'
        );
        return results;
    }
    
    public void sortByLastName() {
        this.sortOrder = 'LastName';
    }
        
    public void sortByFirstName() {
        this.sortOrder = 'FirstName';
    }
}
```

- Visualforce Page
```html
<apex:page controller="ContactsListController">
    <apex:form>
        <apex:pageBlock title="Contacts List" id="contacts_list">
            
            <!-- Contacts List -->
            <apex:pageBlockTable value="{! contacts }" var="ct">
                <apex:column value="{! ct.FirstName }">
                    <apex:facet name="header">
                        <apex:commandLink action="{! sortByFirstName }" 
                            reRender="contacts_list">First Name
                        </apex:commandLink>
                    </apex:facet>
                </apex:column>
                <apex:column value="{! ct.LastName }">
                    <apex:facet name="header">
                        <apex:commandLink action="{! sortByLastName }" 
                            reRender="contacts_list">Last Name
                        </apex:commandLink>
                    </apex:facet>
                </apex:column>
                <apex:column value="{! ct.Title }"/>
                <apex:column value="{! ct.Email }"/>
                
            </apex:pageBlockTable>
        </apex:pageBlock>
    </apex:form>
</apex:page>
```
- `<apex:facet>` lets us set the contents of the column header to whatever we want.
-  The link is created using the `<apex:commandLink>` component, with the `action` attribute set to an expression that references the action method in our controller.
- The attribute `reRender="contacts_list"` will reload the `contacts_list` page block.


### Field Labels which support Internationalization
- Instead of hard coding the header text for 'First Name' or 'Last Name', use the following syntax:
```html
<apex:outputText value="{! $ObjectType.Contact.Fields.FirstName.Label }"/>
```
- This syntax will automatically translate the header text into whichever language the org is using.


### Alternative way to declare Apex properties as getters/setters
```java
public MyObject__c myVariable { get; set; }
```

### Solution to New Case List Controller Challenge
- Apex Class
```java
public class NewCaseListController {
    public List<Case> getNewCases() {
        String newStatus = 'New';
        List<Case> results = Database.query(
            'SELECT Id, CaseNumber ' +
            'FROM Case ' +
            'WHERE Status=:newStatus'
        );
        return results;
    }
}
```

- Visualforce Page
```html
<apex:page controller="NewCaseListController">
    <apex:repeat value="{! newCases }" var="case">
        <li>
            <apex:outputLink value="/{! case.id }">
                {! case.ID }
            </apex:outputLink>
        </li>
    </apex:repeat>
</apex:page>
```