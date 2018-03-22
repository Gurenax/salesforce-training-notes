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