# Data Management

## Reference
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