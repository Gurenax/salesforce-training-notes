# Lightning App Builder
- The Lightning App Builder is a point-and-click tool that makes it easy to create custom pages for the Salesforce mobile app and Lightning Experience.

## Reference
- [Lightning App Builder](https://trailhead.salesforce.com/en/modules/lightning_app_builder)

## With the Lightning App Builder, you can build:
- Single-page apps that drill down into standard pages
- Dashboard-style apps, such as apps to track top sales prospects or key leads for the quarter
- “Point” apps to solve a particular task, such as an expense app for users to enter expenses and monitor expenses they’ve submitted
- Custom record pages for your objects, tailored to the needs of your users
- Custom Home pages containing the components and features that your users use most

---

## Lightning Page
- A Lightning page is a custom layout that lets you design pages for use in the Salesforce mobile app or Lightning Experience.
- A Lightning page is composed of regions that contain components.
- Lightning pages contain:
  1. Lightning Components
  2. Global Actions
- Lightning pages support these components:
  1. Standard Components - Lightning components built by Salesforce.
  2. Custom components - Lightning built by you or someone else have created.
  3. Third-Party Components on AppExchange - The AppExchange provides packages containing components already configured and ready to use in the Lightning App Builder.

## Lightning Components
- A Lightning component is a compact, configurable, and reusable element that you can drag and drop onto a Lightning page in the Lightning App Builder.


## Lightning Page Types
1. App Page
- A single page app that has access to the user's most importand objects/items (e.g. dashboards, recent items).

2. Home Page
- An app that is similar to the salesforce Home Page.

3. Record Page
- An app that is simiar to salesforce record pages (e.g. Opportunities, Leads).

---

## Create a Custom Home Page
- Go to `Setup | App Builder | Lightning App Builder | New | Home Page`

## Create a Custom Lightning Record Page
- Go to `Setup | App Builder | Lightning App Builder | New | Record Page`

## Create an App Page
- Go to `Setup | App Builder | Lightning App Builder | New | App Page`

---

## Custom Lightning Components
- Custom Lightning components don’t automatically work on Lightning pages or in the Lightning App Builder. To make a custom component usable in both, you need to:
  1. Configure the component and its component bundle so that they’re compatible with the Lightning App Builder and Lightning pages.
  2. Deploy My Domain in your org.

