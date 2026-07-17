













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



| Feature                              | What it Controls                                                                                                                                                    | When to Use                                                                             | Example                                                                                                                |
| ------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------- |
| **Organization-Wide Defaults (OWD)** | Defines the **baseline record access** for all users. Determines whether records are Private, Public Read Only, or Public Read/Write.                               | Use when deciding the default visibility of records across the organization.            | Sales reps should only see their own Opportunities → Set Opportunity OWD to **Private**.                               |
| **Profile**                          | Controls **what a user can do** in Salesforce, including object permissions (Create, Read, Edit, Delete), app access, tabs, login hours, and system permissions.    | Use when users with the same job function require the same permissions.                 | Sales Representatives can create and edit Leads, while Support Agents cannot.                                          |
| **Field-Level Security (FLS)**       | Controls **which fields a user can view or edit** on an object.                                                                                                     | Use when users should access a record but not see or modify certain sensitive fields.   | HR can view the "Salary" field, but managers can only view it and employees cannot see it at all.                      |
| **Role Hierarchy**                   | Controls **record visibility based on organizational hierarchy**. Users higher in the hierarchy can access records owned by users below them (when OWD is Private). | Use when managers need visibility into their team's records without changing ownership. | A Sales Manager can see Opportunities owned by all Sales Representatives reporting to them.                            |
| **Record Type**                      | Controls **different business processes, page layouts, and picklist values** for the same object. It does **not** control security.                                 | Use when different teams use the same object differently.                               | On the Case object, Support uses one Record Type while Finance uses another with different fields and picklist values. |


How they work together
Think of Salesforce security in layers:

Profile – Determines what actions a user can perform (Create, Read, Edit, Delete).
Field-Level Security – Determines which fields the user can see or edit.
OWD – Determines the default access to records.
Role Hierarchy – Expands record access based on reporting structure.
Sharing Rules/Manual Sharing (if needed) – Provides additional record access beyond OWD and Role Hierarchy.
Record Type – Customizes the user experience with different page layouts and business processes; it does not affect security.
Interview Answer (2-minute version)
OWD sets the baseline record access across the organization. If records should be private by default, we configure OWD accordingly.

Profiles define what users can do, such as object permissions, app access, tabs, and system permissions.

Field-Level Security controls access to individual fields, allowing sensitive information to be hidden or made read-only for certain users.

Role Hierarchy provides record visibility based on the organization's reporting structure, allowing managers to access records owned by their subordinates.

Record Types support different business processes by providing different page layouts, picklist values, and workflows for the same object. They improve the user experience but do not grant or restrict access.

Easy way to remember
OWD → "Who can see the record by default?"
Profile → "What can the user do?"
FLS → "Which fields can the user see or edit?"
Role Hierarchy → "Who gets additional record access based on reporting structure?"
Record Type → "Which business process and page layout should the user see?"



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

Quick Reference Index Matrix Summary
# Lab Scenario Context Description Key Programmatic Functions Core Conceptual Focus
1 PAN for India Requirements ISPICKVAL, ISBLANK Cross-field conditional mandate
2 Enrollment Date Conditional Block ISPICKVAL, ISBLANK

Dependent transaction state
processing

3
Revenue Amount Security Modification
Lock ISPICKVAL, ISCHANGED

Locking down attributes post-
transaction

4 Agriculture Tiering Compliance Automation ISPICKVAL, NOT Enforcing strict attribute alignment
5 Gmail Domain Pattern Restriction Check CONTAINS, NOT, ISBLANK Data cleansing via domain matching
6 Product Code Structural Factory Match LEFT, ISPICKVAL, OR, AND

Complex programmatic string
manipulation

7 Mobile Length Country-Specific Matrix LEN, ISPICKVAL, OR, AND Character constraint verification
8
Manager Approval Shipment Compliance
Validation ISPICKVAL, NOT (Boolean)

Checkbox-driven security state
authorization

9
Parent Contact Channel Cross-Object
Evaluation ISBLANK, __r Relationship Relational schema traversal logic
10
Chemical Classification Group Matrix
Exclusion ISPICKVAL × 2

Restricting conflicting tracking
matrices

11 Loan Limit Financial Boundary Validation ISPICKVAL, OR, AND, <=, >= Multi-tier numeric value evaluations
12 Enrollment Date Future Horizon Block TODAY() Chronological logic constraints
13
Lead Meeting Timestamp Integrity
Preservation NOW(), ISBLANK DateTime context synchronization
14
Loan Lifespan Hard Operational Ceiling
Limit DATE(), ISBLANK Validating static chronological dates
15
Opportunity Fiscal Closing Year
Verification YEAR() Calendar-year equivalence testing
16
Ford Service Activation Matching
Alignment DATEVALUE()

Casting DateTime structures to
clean Dates

17 Pipeline Stage Initial Creation Block ISNEW(), ISPICKVAL Lifecycle enforcement parameters
18
Case Installation Permission Profile
Restriction $Profile.Name, ISNEW()

User privilege contextual data
access

19
Curriculum Selection Exclusion Rules
Matrix INCLUDES() × 2

Validating multi-select data array
elements
