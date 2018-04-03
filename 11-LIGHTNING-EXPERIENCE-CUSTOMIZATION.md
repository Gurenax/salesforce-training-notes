# Lightning Experience Customization

## Reference
- [Lightning Experience Customization](https://trailhead.salesforce.com/trails/force_com_admin_beginner/modules/lex_customization)

## Setting up an Org
1. Create a Custom Object
2. Create a Custom Object Tab
3. Create Custom Fields
4. Create Records

## What Is a Lightning App?
- An app is a collection of items that work together to serve a particular function. In Lightning Experience, Lightning apps give your users access to sets of objects, tabs, and other items all in one convenient bundle in the navigation bar.

## App Manager
- The App Manager is your go-to place for managing apps for Lightning Experience. It shows all your connected apps and Salesforce apps.
- Use the Lightning Experience App Manager to:
  - View all your Salesforce apps.
  - Create Lightning apps or connected apps.
  - See which apps are visible in Lightning Experience.
  - Easily manage apps.

---

## List Views
- Shows a list of data in tabular form.
- It can be filtered and sorted.
- It can be used as reference to create Charts as long as the user has permission that that list view (Except the "recently viewed" list view).

---

## Compact Layouts
- Compact layouts control which fields your users see in the highlights panel at the top of a record.
- They also control the fields that appear in the expanded lookup card you see when you hover over a link in record details, and in the details section when you expand an activity in the activity timeline.
- Compact layouts also control how records display in the `Salesforce mobile app`. If your company uses the Salesforce mobile app, you can help your users see what they need on mobile screens, where space is limited and quick recognition of records is important.

---

## Page Layouts
- Helps you manage the content of pages in both our Classic UI and in Lightning Experience
- The page layout editor lets you:
  - Control which fields, lists of related records, and custom links users see
  - Customize the order that the fields appear in the page details
  - Determine whether fields are visible, read only, or required
  - Control which standard and custom buttons appear on records and related lists
  - Control which quick actions appear on the page

---

## Custom Buttons and Links
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