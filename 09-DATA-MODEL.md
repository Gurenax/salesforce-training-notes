# Data Model
- Collection of objects and fields in an app

## Reference
- [Data Modeling](https://trailhead.salesforce.com/trails/force_com_admin_beginner/modules/data_modeling)

## Standard Objects
- Objects that are included with Salesforce. Common business objects like Account, Contact, Lead, and Opportunity are all standard objects

## Custom Objects
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
