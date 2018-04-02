# Salesforce Training Notes

## What is Salesforce?
- Salesforce is a customer success platform designed to help you sell, service, market, analyze, and connect with customers.
- Salesforce has everything you need to run your business from anywhere. Using standard products and features, you can manage relationships with prospects and customers, collaborate and engage with employees and partners, and store your data securely in the cloud.
---

## Salesforce Data
- Salesforce organizes data into `Objects` and `Records`.

### Objects
- Similar to tabs in a spreadsheet (e.g. Account, Contacts, Quotes)

### Records
- Similar to rows in spreadsheet tabs (e.g. Account Name, Phone, Website, Industry)

---
## Salesforce Standard Objects
- Objects that Salesforce already comes set up with and ready for use.
- Accounts, Contacts, Leads, Opportunities

### Accounts
- Companies you’re doing business with. You can also do business with individual people, like solo contractors, using Person Accounts. More on that later.

### Contacts
- People who work at a company you’re doing business with (Accounts).

### Leads
- Potential prospects who are not yet ready to buy or you haven't determined what product they need.
- You don't have to use Leads, but they can be helpful if you have team selling, or if you have different sales processes for prospects and qualified buyers.

### Opportunities
- Qualified leads that you’ve converted. When you convert a lead, you create an Account and Contact along with the Opportunity.

---
## Data Quality

### Reference
- [Data Quality: Getting Started](https://trailhead.salesforce.com/trails/getting_started_crm_basics/modules/data_quality/units/data_quality_getting_started)

### Causes of Bad Data
- Missing Records
- Duplicate Records
- No Data Standards
- Incomplete Records
- Stale Data

### Benefits of Good Data
- Prospect and target new customers
- Identify cross-sell and upsell opportunities
- Gain account insights
- Increase efficiency
- Retrieve the right info fast
- Build trust with customers
- Increase adoption by reps
- Plan and align territories better
- Score and route leads faster

### Install and Run the Data Quality Analysis Dashboards App
1. Go to [Data Quality Analysis Dashboards App](https://appexchange.salesforce.com/listingDetail?listingId=a0N300000016cshEAA)
2. Click `Get It Now`. The wizard guides you through the installation steps.
3. In your Salesforce org, from the app menu, click `Dashboards`. To see all the dashboards in the package, click `All Dashboards`.

---

## Reports and Dashboards

### Reference
- [Reports and Dashboards Overview](https://trailhead.salesforce.com/trails/getting_started_crm_basics/modules/reports_dashboards/units/reports_dashboards_overview)

### Reports
- A report is a list of records that meet the criteria you define. It’s displayed in Salesforce in rows and columns, and can be filtered, grouped, or displayed in a graphical chart.

### Dashboards
- A dashboard is a visual display of key metrics and trends for records in your org.
- The relationship between a dashboard component and report is 1:1; for each dashboard component, there is a single underlying report.
- However, you can use the same report in multiple dashboard components on a single dashboard (e.g., use the same report in both a bar chart and pie chart). Multiple dashboard components can be shown together on a single dashboard page layout, creating a powerful visual display and a way to consume multiple reports that often have a common theme, like sales performance, customer support, etc.

### Report Type
- A report type is like a template which makes reporting easier.
- The report type determines which fields and records are available for use when creating a report. This is based on the relationships between a primary object and its related objects. For example, with the ‘Contacts and Accounts’ report type, ‘Contacts’ is the primary object and ‘Accounts’ is the related object.

### Enable Report Builder for All Users
1. To enable the report builder for all users, from Setup, enter `Reports and Dashboard Settings` in the Quick Find box, then select `Reports and Dashboard Settings`.
2. Review the Report Builder Upgrade section of the page, and then click `Enable`. If you don’t see the button, report builder has been enabled for your organization.
3. Confirm your choice by clicking `Yes, Enable Report Builder for All Users`.

### Report Formats
* Tabular	- Make a list
* Summary	- Group and summarize
* Matrix	- Group and summarize, by row and column
* Joined	- Show reports side-by-side, in blocks

---

## Chatter
- Chatter is a Salesforce collaboration application that lets your users work together, talk to each other, and share information, all in real time.
- Chatter connects, engages, and motivates users to work efficiently across the organization, regardless of role or location.
- Chatter lets users collaborate on sales opportunities, service cases, campaigns, and projects with embedded apps and custom actions.

### Reference
- [Introduction to Chatter](https://trailhead.salesforce.com/trails/getting_started_crm_basics/modules/chatter/units/chatter_intro)

### Profiles
- Everyone in your organization has a Chatter profile that they can customize with a photo, contact information, and a personal overview.

### Groups
- Groups are the main collaboration space in Chatter where people organize around a shared interest, purpose, or goal. People use their groups to share information, post updates, and ask questions.

### Follow Users, Groups, and Records
- Following is a way to identify the people and records you want to keep track of. When users follow people and records, they see updates from those people and records in their What I Follow feed. For example, when users join a group, they see updates that are posted to the group in their What I Follow feed.

### Email Notifications
- When you turn on email notifications for Chatter, users automatically receive emails about new posts, comments, and other changes.

### Feeds
- When your users follow people and records, they see posts, comments, and updates about them in their Chatter feeds.
- Chatter feeds appear on profile and group pages, the Home tab, topic pages, and on record pages.

### Topics (aka Hashtags)
- Topics give users a way to add searchable terms to their feed posts. Topics are great for adding to an ongoing discussion that is independent of a particular group. For example, your users can add the topic #newtech to many different posts in many different feeds. When any user searches for `#newtech`, their search results include all posts marked with that topic from all feeds the user has access to.

### Feed Tracking
- Feed tracking in Salesforce highlights changes to records by automatically announcing them in the record’s feed.

### Actions
- Actions add functionality, like post, poll, and question, to the Chatter publisher. Actions let your users do more in Salesforce and the Salesforce app.

### Approvals
- An approval process is an automated process your organization can use to approve records in Salesforce. It specifies the steps necessary for a record to be approved and who must provide approval at each step. The approval process can send out an approval request as a Chatter post.

---

## Salesforce CPQ
- A native Salesforce app that helps you and your team close deals even faster.
- CPQ stands for configure, price, and quote.
  - What products does the customer want to buy (configure)?
  - How much do those products cost (price)?
  - How can we give the customer details about the sale (quote)?

### Reference
- [Salesforce CPQ](https://trailhead.salesforce.com/trails/getting_started_crm_basics/modules/sf_cpq/units/sf_cpq_get_started)

---

# Salesforce Admin Basics
- Salesforce is more than just a CRM.
- Salesforce comes with a lot of standard functionality, or out-of-the-box products and features that you can use to run your business. Here are some common things businesses want to do with Salesforce and the features we give you that support those activities.

### Reference
- [Salesforce Platform Basics](https://trailhead.salesforce.com/trails/force_com_admin_beginner/modules/starting_force_com)

### App
- An app in Salesforce is a set of objects, fields, and other functionality that supports a business process.
- You can see which app you’re using and switch between apps using the `App Launcher`.

### Objects
- Objects are tables in the Salesforce database that store a particular kind of information.
- There are standard objects like Accounts and Contacts and custom objects like the Property object you see in the graphic.

### Records
- Records are rows in object database tables.
- Records are the actual data associated with an object. Here, the 211 Charles Street property is a record.

### Fields
- Fields are columns in object database tables.
- Both standard and custom objects have fields. On our Property object, we have fields like Address and Price.

---

## Declarative Development
- Developing without code.

## Programmatic development
- Develop using things like Lightning components, Apex code, and Visualforce pages

---

## Multitenancy
- Salesforce provides a core set of services to all our customers in the multitenant cloud. No matter the size of your business, you get access to the same computing power, data storage, and core features.

## Metadata
- Data about data

## APIs
- Allow different pieces of software to connect to each other and exchange information.

---

## Object Manager
- Object Manager is where you can view and customize standard and custom objects in your org.

### Setup Menu
- The menu gives you quick links to a collection of pages that let you do everything from managing your users to modifying security settings.

  - Administration
    - The Administration category is where you manage your users and data. You can do things like add users, change permissions, import and export data, and create email templates.

  - Platform Tools
    - You do most of your customization in Platform Tools. You can view and manage your data model, create apps, modify the user interface, and deploy new features to your users. If you decide to try your hand at programmatic development, Platform Tools is where you manage your code as well.

  - Settings
    - Settings is where you manage your company information and org security. You can do things like add business hours, change your locale, and view your org’s history.

### Main Window
- This is where you can see whatever it is you’re trying to work on.

---

## AppExchange
- Salesforce has a community of partners that use the flexibility of the Salesforce platform to build amazing apps and other solutions that anyone can use. These offerings are available (some for free, some at a cost) for installation on AppExchange.

---

## Data Model
- Collection of objects and fields in an app

### Reference
- [Data Modeling](https://trailhead.salesforce.com/trails/force_com_admin_beginner/modules/data_modeling)

### Standard Objects
- Objects that are included with Salesforce. Common business objects like Account, Contact, Lead, and Opportunity are all standard objects

### Custom Objects
- Objects that you create to store information that’s specific to your company or industry.

---

## Object Relationship
- Object relationships are a special field type that connects two objects together

### Lookup Relationships
- A lookup relationship essentially links two objects together so that you can “look up” one object from the related items on another object.
- Lookup relationships can be one-to-one or one-to-many (e.g. Account to Contacts is one-to-many).
- Objects in lookup relationships usually work as stand-alone objects and have their own tabs in the user interface.

### Master-Detail Relationships
- In this type of relationship, one object is the master and another is the detail. The master object controls certain behaviors of the detail object, like who can view the detail’s data.
- The detail object doesn’t work as a stand-alone. It’s highly dependent on the master.

### Hierarchical relationships 
- Hierarchical relationships are a special type of lookup relationship.
- The main difference between the two is that hierarchical relationships are only available on the User object. 

---

## Schema Builder
- Schema Builder is a tool that lets you visualize and edit your data model. It’s useful for designing and understanding complex data models.
- You can also create objects using Schema Builder. If you prefer, you can create objects in this visual interface if you’re designing your system and want to be able to revise all your customizations on the spot.

---

## Data Management

### Reference
- [Data Management](https://trailhead.salesforce.com/trails/force_com_admin_beginner/modules/lex_implementation_data_management)

## Data Import

### Data Import Wizard
- Lets you import data in common standard objects, such as contacts, leads, accounts, as well as data in custom objects. It can import up to 50,000 records at a time. It provides a simple interface to specify the configuration parameters, data sources, and the field mappings that map the field names in your import file with the field names in Salesforce.

### Use the Data Import Wizard When:
* You need to load less than 50,000 records.
* The objects you need to import are supported by the wizard.
* You don’t need the import process to be automated.

### Data Loader
- This is a client application that can import up to five million records at a time, of any data type, either from files or a database connection. It can be operated either through the user interface or the command line. In the latter case, you need to specify data sources, field mappings, and other parameters via configuration files. This makes it possible to automate the import process, using API calls.

### Use Data Loader When:
* You need to load 50,000 to five million records. If you need to load more than 5 million records, we recommend you work with a Salesforce partner or visit the AppExchange for a suitable partner product.
* You need to load into an object that is not supported by the Data Import Wizard.
* You want to schedule regular data loads, such as nightly imports.

## Data Export

### Data Export Wizard
- This is an in-browser wizard, accessible through the Setup menu. It allows you to export data manually once every six days (for weekly export) or 28 days (for monthly export). You can also export data automatically, at weekly or monthly intervals.

### Data Loader
- Same definition as Data Loader above.

### Export Now
- Exports data immediately.

### Schedule Export
- Schedule the export process for `Weekly` or `Monthly` intervals.

---

## Lightning Experience Customization

### Reference
- [Lightning Experience Customization](https://trailhead.salesforce.com/trails/force_com_admin_beginner/modules/lex_customization)

### Setting up an Org
1. Create a Custom Object
2. Create a Custom Object Tab
3. Create Custom Fields
4. Create Records

### What Is a Lightning App?
- An app is a collection of items that work together to serve a particular function. In Lightning Experience, Lightning apps give your users access to sets of objects, tabs, and other items all in one convenient bundle in the navigation bar.

### App Manager
- The App Manager is your go-to place for managing apps for Lightning Experience. It shows all your connected apps and Salesforce apps.
- Use the Lightning Experience App Manager to:
  - View all your Salesforce apps.
  - Create Lightning apps or connected apps.
  - See which apps are visible in Lightning Experience.
  - Easily manage apps.

---

### List Views
- Shows a list of data in tabular form.
- It can be filtered and sorted.
- It can be used as reference to create Charts as long as the user has permission that that list view (Except the "recently viewed" list view).

---

### Compact Layouts
- Compact layouts control which fields your users see in the highlights panel at the top of a record.
- They also control the fields that appear in the expanded lookup card you see when you hover over a link in record details, and in the details section when you expand an activity in the activity timeline.
- Compact layouts also control how records display in the `Salesforce mobile app`. If your company uses the Salesforce mobile app, you can help your users see what they need on mobile screens, where space is limited and quick recognition of records is important.

---

### Page Layouts
- Helps you manage the content of pages in both our Classic UI and in Lightning Experience
- The page layout editor lets you:
  - Control which fields, lists of related records, and custom links users see
  - Customize the order that the fields appear in the page details
  - Determine whether fields are visible, read only, or required
  - Control which standard and custom buttons appear on records and related lists
  - Control which quick actions appear on the page

---

### Custom Buttons and Links
- Custom links can link to an external URL, such as www.google.com, a Visualforce page, or your company’s intranet.
- Custom buttons can connect users to external applications, such as web pages, and launch custom links.

### 3 types of Custom Buttons and Links
1. `List button` — Appears on a related list on an object record page.
2. `Detail page link` — Appears in the Links section of the record details on an object record page.
3. `Detail page button` — Appears in the action menu in the highlights panel of a record page.

---

## Quick Actions
- Actions let your users quickly do tasks, such as create records, log calls, send emails, and more.
- With custom actions, you can make your users’ navigation and workflow as smooth as possible by giving them quick access to information that’s most important.

### Object-specific actions
- Object-specific actions have automatic relationships to other records and let users quickly create or update records, log calls, send emails, and more, in the context of a particular object.

### Global actions
- You create global actions in a different place in Setup than you create object-specific actions. They’re called global actions because they can be put anywhere actions are supported.
- Use global actions to let users log call details, create or update records, or send email, all without leaving the page they’re on.

---

## Salesforce Mobile App Customization

### Reference
- [Salesforce Mobile App Customization](https://trailhead.salesforce.com/trails/force_com_admin_beginner/modules/salesforce1_mobile_app)

### Quick Actions
- They are shortcuts in a mobile app.
- They offer a fast way for mobile users to launch a specific workflow in the Salesforce mobile app, like creating records, logging calls, or sharing files.

### Why Quick Actions
- You can create custom actions tailored to your own business processes and use cases.
- Each action has its own unique page layout, so you can limit the fields to just the ones mobile users truly need.
- You can prepopulate fields on the page layout to save mobile users some time.


### Types of Quick Actions
- Object-specific actions
  - `Object-specific actions` let users create or update records in the context of a particular object. In the Salesforce app, object-specific actions show up on record detail pages. So for example, an action associated with the opportunity object is only available when a user is looking at an opportunity.
  - For users to see Object-specific actions, they are added in the page layout.


- Global actions
  - `Global actions` let users create records, but the new record has no relationship with other records. And they’re called global actions because they can be put anywhere actions are supported—on record detail pages, but also places like the feed or Chatter groups.
  - Unlike in Desktop, Global actions cannot update records on mobile.
  - For users to see Global actions, they are added in the publisher layout.

### Global Publisher Layout
- Allows to create layout of actions for global pages such as Home, Chatter Home and User Profile.

---

### Compact Layouts
- `Compact layouts` control which fields appear in the header. For each object, you can assign up to four fields to display in that area.
- Compact layouts don’t support text areas, long text areas, rich text areas, or multi-select picklists.

---

### Custom Mobile Navigation
1. Menu Items
  - Any items you place above the Smart Search Items element when you customize the navigation menu.
  - The first item on the list becomes the landing page.
2. Smart Search Items
  - Includes a set of the user’s recently accessed objects. When expanded, it shows all tabs to which the user has access. Although you can control where in the menu these recent items appear, each user’s list is specific to their own usage history.
3. Apps Section
  - Contains any items you place below the Smart Search Items element.

### Notes About Mobile Navigation
1. You can’t set different menu configurations for different types of users.
2. If you want to include Visualforce pages, Lightning Pages, or Lightning components in the mobile navigation menu, you have to create tabs for them.
3. Before adding a Visualforce page to the Salesforce app, make sure the page is enabled for mobile. Thoroughly test your Visualforce pages in the Salesforce app because they don’t always work the same in the mobile app. See the resources section for more information.

---

## Lightning Experience Reports & Dashboards

### Reference
- [Lightning Experience Reports & Dashboards](https://trailhead.salesforce.com/trails/force_com_admin_beginner/modules/lex_implementation_reports_dashboards)

### Report
- In its simplest form, a report is a list of records (like opportunities or accounts) that meet the criteria you define. But reports are much more than simple lists.
- To get the data you need, you can filter, group, and do math on records. You can even display them graphically in a chart!

### Dashboard
- A dashboard is a visual display of key metrics and trends for records in your org.
- The relationship between a dashboard component and report is 1:1; for each dashboard component, there is a single source report. However, you can use the same report in multiple dashboard components on a single dashboard (for example, use the same report in both a bar chart and pie chart).
- You can display multiple dashboard components on a single dashboard page, creating a powerful visual display and a way to consume multiple reports that often have a common theme, like sales performance or customer support.
- `Dynamic dashboards` are dashboards for which the running user is always the logged-in user. This way, each user sees the dashboard according to his or her own access level. If you’re concerned about too much access, dynamic dashboards might be the way to go.

### Report Type
- A report type is like a template that makes reporting easier.
- The report type determines which fields and records are available for use when creating a report. This is based on the relationships between a primary object and its related objects.
- For example, with the ‘Contacts & Accounts’ report type, ‘Contacts’ is the primary object and ‘Accounts’ is the related object.

    - `Primary object with related object` — Records returned are only those where the primary object has at least one related object record. In our example of Opportunities with Products, the only records that would be displayed on the report would be opportunities that have at least one related product record.

    - `Primary object with or without related object` — Records returned are those where the primary object may or may not have a related object record. If we were to create a custom report type, Opportunities with or without Products, then opportunities would be displayed whether or not they have a related product record.

---

### Report Builder
- Salesforce comes with a built-in translator, allowing you to ask your database all the questions you want through a point-and-click interface.

### Tabular reports
- Tabular reports are the simplest and fastest way to look at your data. Similar to a spreadsheet, they consist simply of an ordered set of fields in columns, with each matching record listed in a row. While easy to set up, they can't be used to create groups of data and there are limits to how you can use them in dashboards.

### Summary reports
- Summary reports are similar to tabular reports, but also allow you to group rows of data, view subtotals, and create charts. Summary reports give us many more options for organizing the data, and are great for use in dashboards. Yes!

### Matrix Reports
- Matrix reports allow you to group records both by row and by column. These reports are the most time-consuming to set up, but they also provide the most detailed view of our data.


---

## Salesforce Platform Development Basics

### Reference
- [Platform Development Basics](https://trailhead.salesforce.com/trails/force_com_dev_beginner/modules/platform_dev_basics)

### Salesforce Platform
- Like any platform, the Salesforce platform is a group of technologies that supports the development of other technologies on top of it. What makes it unique is that the platform supports not only all the Salesforce clouds, but it also supports custom functionality built by our customers and partners. This functionality ranges from simple page layouts to full-scale applications.

### Core Platform
- The core platform lets you develop custom data models and applications for desktop and mobile. And with the platform behind your development, you can build robust systems at a rapid pace.

### Heroku Platform
- Heroku gives developers the power to build highly scalable web apps and back-end services using Python, Ruby, Go, and more. It also provides database tools to sync seamlessly with data from Salesforce.

### Salesforce APIs
- These let developers integrate and connect all their enterprise data, networks, and identity information.

### Mobile SDK
- The Mobile SDK is a suite of technologies that lets you build native, HTML5, and hybrid apps that have the same reliability and security as the Salesforce app.

---

### Metadata-driven development model
- the key differences between developing on the platform and developing outside of Salesforce. Because the platform is metadata-aware, it can auto-generate a significant part of your user experience for you.
- Things like dialogs, record lists, detail views, and forms that you’d normally have to develop by yourself come for free.
- You even get all the functionality to create, read, update, and delete (affectionately referred to as CRUD) custom object records in the database.

---

### Lightning Component Framework
- A UI development framework similar to AngularJS or React.

### Apex
- Salesforce’s proprietary programming language with Java-like syntax.

### Visualforce
- A markup language that lets you create custom Salesforce pages with code that looks a lot like HTML, and optionally can use a powerful combination of Apex and JavaScript.

---

## Data Security

### Reference
- [Data Security](https://trailhead.salesforce.com/trails/force_com_dev_beginner/modules/data_security)

### Levels of Data Access
1. Organization
- For your whole org, you can maintain a list of authorized users, set password policies, and limit logins to certain hours and locations.

2. Objects
- Access to object-level data is the simplest thing to control. By setting permissions on a particular type of object, you can prevent a group of users from creating, viewing, editing, or deleting any records of that object.
- For example, you can use object permissions to ensure that interviewers can view positions and job applications but not edit or delete them.

3. Fields
- You can restrict access to certain fields, even if a user has access to the object.
- For example, you can make the salary field in a position object invisible to interviewers but visible to hiring managers and recruiters.

4. Records
- You can allow particular users to view an object, but then restrict the individual object records they're allowed to see.
- For example, an interviewer can see and edit her own reviews, but not the reviews of other interviewers. You can manage record-level access in these four ways.


### Manage Users
- Every Salesforce user is identified by a username, a password, and a single profile. Together with other settings, the profile determines what tasks users can perform, what data they see, and what they can do with the data.
