# User Management

## Reference
- [User Management](https://trailhead.salesforce.com/trails/lex_admin_implementation/modules/lex_implementation_user_setup_mgmt)

## Key Terms
### Username
- Each user has both a username and an email address.
- It must be formatted like an email address and must be unique across all Salesforce organizations.

### User Licenses
- Determines which features the user can access in Salesforce.

### Profiles
- Profiles determine what users can do in Salesforce. 
- Each user can have only one profile.
- Profiles come with a `permission sets` which grant access to particular objects, fields, tabs, and records. 

### Roles
- Roles determine what users can see in Salesforce based on where they are located in the role hierarchy.
- Users at the top of the hierarchy can see all the data owned by users below them.
- Users at lower levels can't see data owned by users above them, or in other branches, unless sharing rules grant them access.
- Roles are optional but each user can have only one.

### Alias
- An alias is a short name to identify the user on list pages, reports, or other places where their entire name doesn't fit.
- By default, the alias is the first letter of the user's first name and the first four letters of their last name.

---

## Guidelines for Adding Users

### Username
- Each user must have a username that is unique across all Salesforce organizations (not just yours).

### Username Format
- Users must have a username in the format of an email address (that is, jdoe@domain.com), but they don't have to use a real email address. (They can use their email address if they wish as long as their email address is unique across all Salesforce orgs.)

### Email
- Users can have the same email address across organizations.

### Passwords
- Users must change their password the first time they log in. A new user's temporary password expires in six months.

### Login Link
- Users can only use the login link in the sign–up email once. If a user follows the link and does not set a password, you (the admin) have to reset their password before they can log in.

---

## Add Users
- You can add users one at a time or several at a time.
- The maximum number of users you can add is determined by your Salesforce edition and the number of user licenses you purchase.


---

## Data Security

### Levels of Data Access

- Organization
  - At the highest level, you can secure access to your organization by maintaining a list of authorized users, setting password policies, and limiting login access to certain hours and certain locations.

- Objects
  - Object–level security provides the simplest way to control which users have access to which data. By setting permissions on a particular type of object, you can prevent a group of users from creating, viewing, editing, or deleting any records of that object. For example, you can use object permissions to ensure that interviewers can view positions and job applications but not edit or delete them.

- Fields
  - You can use field–level security to restrict access to certain fields, even for objects a user has access to. For example, you can make the salary field in a position object invisible to interviewers but visible to hiring managers and recruiters.

- Records
  - To control data with greater precision, you can allow particular users to view an object, but then restrict the individual object records they're allowed to see. For example, record–level access allows interviewers to see and edit their own reviews, without exposing the reviews of other interviewers. You can manage record–level access in the following ways:

    - `Organization–wide` defaults specify the default level of access users have to each others' records. You use organization–wide sharing settings to lock down your data to the most restrictive level, and then use the other sharing tools to selectively give access to other users. For example, you can give all employees access to an object called Candidate to allow anyone to add a candidate to the database. But you can restrict access to Positions so that anyone can see the jobs available but only the employees with the proper permissions can edit them.

    - `Role hierarchies` open up access to those higher in the hierarchy so they inherit access to all records owned by users below them in the hierarchy. Role hierarchies don't have to match your organization chart exactly. Instead, each role in the hierarchy represents a level of data access that a user or group of users needs. For example, you can restrict access to Candidates by setting the organization–wide default to Private, but allow recruiters to view and edit the candidate records that they own. Recruiters can't see candidate records they don't own because recruiters are all at the same level in the role hierarchy. However, hiring managers can be given read/write access to all candidate records because they are at a higher level in the role hierarchy than recruiters.

    - `Sharing rules` enable you to make automatic exceptions to organization–wide defaults for particular groups of users, to give them access to records they don't own or can't normally see. Sharing rules, like role hierarchies, are only used to give more users access to records—they can't be stricter than your organization–wide default settings. For example, you can allow all employees to view Positions, but use sharing rules to grant full editing access to employees in a role or group called Hiring Managers.

    - `Manual sharing` allows owners of particular records to share them with other users. Although manual sharing isn't automated like organization–wide sharing settings, role hierarchies, or sharing rules, it can be useful in some situations, for example, if a recruiter going on vacation needs to temporarily assign ownership of a job application to another employee.


### Organization–Wide Sharing Defaults
- These are the defaults that specify the baseline level of access that the most restricted user should have.
- You can use organization–wide defaults to lock down your data to this most restrictive level, and then use other record–level security and sharing tools (role hierarchies, sharing rules, and manual sharing) to open up the data to other users who need to access it.

