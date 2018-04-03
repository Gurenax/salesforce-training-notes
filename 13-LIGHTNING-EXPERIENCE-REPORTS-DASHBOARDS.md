# Lightning Experience Reports & Dashboards

## Reference
- [Lightning Experience Reports & Dashboards](https://trailhead.salesforce.com/trails/force_com_admin_beginner/modules/lex_implementation_reports_dashboards)

## Report
- In its simplest form, a report is a list of records (like opportunities or accounts) that meet the criteria you define. But reports are much more than simple lists.
- To get the data you need, you can filter, group, and do math on records. You can even display them graphically in a chart!

## Dashboard
- A dashboard is a visual display of key metrics and trends for records in your org.
- The relationship between a dashboard component and report is 1:1; for each dashboard component, there is a single source report. However, you can use the same report in multiple dashboard components on a single dashboard (for example, use the same report in both a bar chart and pie chart).
- You can display multiple dashboard components on a single dashboard page, creating a powerful visual display and a way to consume multiple reports that often have a common theme, like sales performance or customer support.
- `Dynamic dashboards` are dashboards for which the running user is always the logged-in user. This way, each user sees the dashboard according to his or her own access level. If you’re concerned about too much access, dynamic dashboards might be the way to go.

## Report Type
- A report type is like a template that makes reporting easier.
- The report type determines which fields and records are available for use when creating a report. This is based on the relationships between a primary object and its related objects.
- For example, with the ‘Contacts & Accounts’ report type, ‘Contacts’ is the primary object and ‘Accounts’ is the related object.

    - `Primary object with related object` — Records returned are only those where the primary object has at least one related object record. In our example of Opportunities with Products, the only records that would be displayed on the report would be opportunities that have at least one related product record.

    - `Primary object with or without related object` — Records returned are those where the primary object may or may not have a related object record. If we were to create a custom report type, Opportunities with or without Products, then opportunities would be displayed whether or not they have a related product record.

---

## Report Builder
- Salesforce comes with a built-in translator, allowing you to ask your database all the questions you want through a point-and-click interface.

## Tabular reports
- Tabular reports are the simplest and fastest way to look at your data. Similar to a spreadsheet, they consist simply of an ordered set of fields in columns, with each matching record listed in a row. While easy to set up, they can't be used to create groups of data and there are limits to how you can use them in dashboards.

## Summary reports
- Summary reports are similar to tabular reports, but also allow you to group rows of data, view subtotals, and create charts. Summary reports give us many more options for organizing the data, and are great for use in dashboards. Yes!

## Matrix Reports
- Matrix reports allow you to group records both by row and by column. These reports are the most time-consuming to set up, but they also provide the most detailed view of our data.
