# Reports and Dashboards

## Reference
- [Reports and Dashboards Overview](https://trailhead.salesforce.com/trails/getting_started_crm_basics/modules/reports_dashboards/units/reports_dashboards_overview)

## Reports
- A report is a list of records that meet the criteria you define. It’s displayed in Salesforce in rows and columns, and can be filtered, grouped, or displayed in a graphical chart.

## Dashboards
- A dashboard is a visual display of key metrics and trends for records in your org.
- The relationship between a dashboard component and report is 1:1; for each dashboard component, there is a single underlying report.
- However, you can use the same report in multiple dashboard components on a single dashboard (e.g., use the same report in both a bar chart and pie chart). Multiple dashboard components can be shown together on a single dashboard page layout, creating a powerful visual display and a way to consume multiple reports that often have a common theme, like sales performance, customer support, etc.

## Report Type
- A report type is like a template which makes reporting easier.
- The report type determines which fields and records are available for use when creating a report. This is based on the relationships between a primary object and its related objects. For example, with the ‘Contacts and Accounts’ report type, ‘Contacts’ is the primary object and ‘Accounts’ is the related object.

## Enable Report Builder for All Users
1. To enable the report builder for all users, from Setup, enter `Reports and Dashboard Settings` in the Quick Find box, then select `Reports and Dashboard Settings`.
2. Review the Report Builder Upgrade section of the page, and then click `Enable`. If you don’t see the button, report builder has been enabled for your organization.
3. Confirm your choice by clicking `Yes, Enable Report Builder for All Users`.

## Report Formats
* Tabular	- Make a list
* Summary	- Group and summarize
* Matrix	- Group and summarize, by row and column
* Joined	- Show reports side-by-side, in blocks