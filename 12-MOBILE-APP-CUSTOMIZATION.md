# Salesforce Mobile App Customization

## Reference
- [Salesforce Mobile App Customization](https://trailhead.salesforce.com/trails/force_com_admin_beginner/modules/salesforce1_mobile_app)

## Quick Actions
- They are shortcuts in a mobile app.
- They offer a fast way for mobile users to launch a specific workflow in the Salesforce mobile app, like creating records, logging calls, or sharing files.

## Why Quick Actions
- You can create custom actions tailored to your own business processes and use cases.
- Each action has its own unique page layout, so you can limit the fields to just the ones mobile users truly need.
- You can prepopulate fields on the page layout to save mobile users some time.


## Types of Quick Actions
### Object-specific actions
- `Object-specific actions` let users create or update records in the context of a particular object. In the Salesforce app, object-specific actions show up on record detail pages. So for example, an action associated with the opportunity object is only available when a user is looking at an opportunity.
- For users to see Object-specific actions, they are added in the page layout.

### Global actions
- `Global actions` let users create records, but the new record has no relationship with other records. And they’re called global actions because they can be put anywhere actions are supported—on record detail pages, but also places like the feed or Chatter groups.
- Unlike in Desktop, Global actions cannot update records on mobile.
- For users to see Global actions, they are added in the publisher layout.

## Global Publisher Layout
- Allows to create layout of actions for global pages such as Home, Chatter Home and User Profile.

---

## Compact Layouts
- `Compact layouts` control which fields appear in the header. For each object, you can assign up to four fields to display in that area.
- Compact layouts don’t support text areas, long text areas, rich text areas, or multi-select picklists.

---

## Custom Mobile Navigation
1. Menu Items
    - Any items you place above the Smart Search Items element when you customize the navigation menu.
    - The first item on the list becomes the landing page.
2. Smart Search Items
    - Includes a set of the user’s recently accessed objects. When expanded, it shows all tabs to which the user has access. Although you can control where in the menu these recent items appear, each user’s list is specific to their own usage history.
3. Apps Section
    - Contains any items you place below the Smart Search Items element.

## Notes About Mobile Navigation
1. You can’t set different menu configurations for different types of users.
2. If you want to include Visualforce pages, Lightning Pages, or Lightning components in the mobile navigation menu, you have to create tabs for them.
3. Before adding a Visualforce page to the Salesforce app, make sure the page is enabled for mobile. Thoroughly test your Visualforce pages in the Salesforce app because they don’t always work the same in the mobile app. See the resources section for more information.
