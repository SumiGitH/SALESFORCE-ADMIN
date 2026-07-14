













Roll-Up Summary:A field on a Master object that performs a COUNT, SUM, MIN, or MAX calculation across all related Detail (child) records. Only available with Master-Detail relationships.

Formula Fields: A read-only field whose value is automatically calculated by Salesforce using a formula expression. The value updates in real-time when referenced fields change.

Page Layout: A configuration that defines which fields, sections, related lists, and buttons appear on a record's detail and edit pages. Different layouts can be assigned to different profiles

Compact Layout: A configuration that defines up to 10 fields shown in the record highlight panel at the top of a record page and in the Salesforce mobile app.

List View:A filtered and sorted view of records for an object. Users can create personal list views or admins can create shared list views visible to all users.

OWD: The baseline record access level for users who do not own the record. Options: Private, Public Read Only, Public Read/Write. OWD is the most restrictive setting — access can only be opened from here.

Profile: A collection of settings that controls what a user can do in Salesforce — object permissions, FLS, app visibility, and system permissions. Every user has exactly one profile.

FLS:Per-profile settings that control whether a field is Visible, Read-Only, or Hidden. Works in conjunction with Page Layouts — both must allow the field for it to display.

Record Type:A configuration on an object that enables different page layouts, picklist values, and business processes for different subsets of records. Assigned to profiles.

Role Hierarchy:A tree structure of roles in the organisation. Users at higher roles can view (and optionally edit) records owned by users in lower roles, subject to OWD settings.


<img width="732" height="256" alt="image" src="https://github.com/user-attachments/assets/1d702889-e8f7-4b9f-8fec-2f80a0b4a872" />




Validation Rules
Validation rules verify that the data a user enters in a record meets the structural standards you specify before
the user can save the record. A validation rule contains a formula or expression that evaluates the data in one or more fields and returns a value of TRUE or FALSE.
When the formula evaluates to TRUE, it indicates an invalid data entry condition. Salesforce will immediately
block the save operation and display a predefined error message either at the top of the page or directly beneath the offending field.

Key Functions & Operators Quick Reference
Function / Operator Functional Behavior & Return Value
ISBLANK(field) Returns TRUE if the field has no value. Highly recommended for text, numeric, and date fields.

ISNULL(field) Returns TRUE if the field is null. (Legacy function; use ISBLANK for all text fields).
ISPICKVAL(field, 'val') Returns TRUE if the single-select picklist field matches the exact string literal 'val'.
INCLUDES(field, 'val') Returns TRUE if the multi-select picklist field includes 'val' as one of its selected options.

ISNEW() Returns TRUE when the record is currently being created as a completely new entry.
ISCHANGED(field) Returns TRUE when an existing record field value is being modified during an edit operation.
Key Functions & Operators Quick Reference
Function / Operator Functional Behavior & Return Value
ISBLANK(field) Returns TRUE if the field has no value. Highly recommended for text, numeric, and date fields.

ISNULL(field) Returns TRUE if the field is null. (Legacy function; use ISBLANK for all text fields).
ISPICKVAL(field, 'val') Returns TRUE if the single-select picklist field matches the exact string literal 'val'.
INCLUDES(field, 'val') Returns TRUE if the multi-select picklist field includes 'val' as one of its selected options.

ISNEW() Returns TRUE when the record is currently being created as a completely new entry.
ISCHANGED(field) Returns TRUE when an existing record field value is being modified during an edit operation.

Scenario 1: PAN Number Required for India Customers
Business Requirement:
When a Customer (Contact or Account) record is created or updated, if the customer's country is set to 'India',
the custom PAN Number field must be mandatory and filled in.
Schema Elements to Provision:
• Picklist Field: Country__c (Values: India, USA, UK, Australia, Canada, Others)
• Text Field: PAN_Number__c (Maximum Length: 10)
Validation Formula:
AND(
ISPICKVAL(Country__c, 'India'),
ISBLANK(PAN_Number__c)
)

Error Location Field: PAN_Number__c
Error Message PAN Number is mandatory when the country is India.


Scenario 2: Enrollment Date Required When Status = Enrolled
Business Requirement:
When creating or updating a Student custom record, if the field status is modified or set to 'Enrolled', the
Enrollment Date field must be actively provided.
Required Fields:
• Picklist Field: Status__c (Values: Enrolled, Pending, Graduated, Dropped)
• Date Field: Enrollment_Date__c
Validation Formula:
AND(
ISPICKVAL(Status__c, 'Enrolled'),
ISBLANK(Enrollment_Date__c)
)

Architectural Hint: ISPICKVAL evaluates single-select picklist entries. If the expression evaluates to true for
status being Enrolled while the date field has no value, the transaction is rejected.
Error Location Field: Enrollment_Date__c
Error Message Enrollment Date is required when student status is Enrolled.
