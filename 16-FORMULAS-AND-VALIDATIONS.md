# Formulas & Validations

## Reference
- [Formulas & Validations](https://trailhead.salesforce.com/trails/force_com_dev_beginner/modules/point_click_business_logic)


## Formula Fields
- Fields that have calculated values based on a Formula which is entered in the Formula Editor.
- You can create custom formula fields on any standard or custom object.
- Always use the `Check Syntax` button
  - Check Syntax button in the editor is an important tool for debugging your formulas.
  - The syntax checker tells you what error it encountered and where it’s located in your formula. 


## Roll-Up Summary Fields
- While formula fields calculate values using fields within a single record, roll-up summary fields calculate values from a set of related records, such as those in a related list.
- You can create roll-up summary fields that automatically display a value on a master record based on the values of records in a detail record. These detail records must be directly related to the master through a master-detail relationship

- You can perform different types of calculations with roll-up summary fields. You can count the number of detail records related to a master record, or calculate the sum, minimum value, or maximum value of a field in the detail records. For example, you might want:
  - A custom account field that calculates the total of all related pending opportunities.
  - A custom order field that sums the unit prices of products that contain a description you specify.

- Types of Summaries that can be used
  - `COUNT` -	Totals the number of related records.
  - `SUM` -	Totals the values in the field you select in the Field to Aggregate option. Only number, currency, and percent fields are available.
  - `MIN` -	Displays the lowest value of the field you select in the Field to Aggregate option for all directly related records. Only number, currency, percent, date, and date/time fields are available.
  - `MAX` -	Displays the highest value of the field you select in the Field to Aggregate option for all directly related records. Only number, currency, percent, date, and date/time fields are available.


## Validation Rules
- Validation rules verify that data entered by users in records meet the standards you specify before they can save it.
- A validation rule can contain a formula or expression that evaluates the data in one or more fields and returns a value of “True” or “False.”
- Validation rules can also include error messages to display to users when they enter invalid values based on specified criteria. Using these rules effectively contributes to quality data. 
- For example, you can ensure that all phone number fields contain a specified format or that discounts applied to certain products never exceed a defined percentage.
