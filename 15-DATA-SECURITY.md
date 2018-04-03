# Data Security

## Reference
- [Data Security](https://trailhead.salesforce.com/trails/force_com_dev_beginner/modules/data_security)

## Levels of Data Access
1. Organization
- For your whole org, you can maintain a list of authorized users, set password policies, and limit logins to certain hours and locations.

2. Objects
- Access to object-level data is the simplest thing to control. By setting permissions on a particular type of object, you can prevent a group of users from creating, viewing, editing, or deleting any records of that object.
- For example, you can use object permissions to ensure that interviewers can view positions and job applications but not edit or delete them.

3. Fields
- You can restrict access to certain fields, even if a user has access to the object.
- For example, you can make the salary field in a position object invisible to interviewers but visible to hiring managers and recruiters.

4. Records
- You can allow particular users to view an object, but then restrict the individual object records they're allowed to see.
- For example, an interviewer can see and edit her own reviews, but not the reviews of other interviewers. You can manage record-level access in these four ways.

---

## Control Access to Orgs
### Manage Users
- Every Salesforce user is identified by a username, a password, and a single profile. Together with other settings, the profile determines what tasks users can perform, what data they see, and what they can do with the data.

  - Create User
  - Deactivate a User
  - Set Password Policy
  - Restrict Login Access by IP Address
  - Restrict Login Access by Time

---

## Control Access to Objects
### Manage Object Permissions
- The simplest way to control data access is to set permissions on a particular type of object. (An object is a collection of records, like leads or contacts.) You can control whether a group of users can create, view, edit, or delete any records of that object.

- You can set object permissions with profiles or permission sets. A user can have one profile and many permission sets.
  - A user’s profile determines the objects they can access and the things they can do with any object record (such as create, read, edit, or delete).
  - Permission sets grant additional permissions and access settings to a user.

- Use profiles to grant the minimum permissions and settings that all users of a particular type need. Then use permission sets to grant more permissions as needed. The combination of profiles and permission sets gives you a great deal of flexibility in specifying object-level access.


### Use Profiles to Restrict Access
- Each user has a single profile that controls which data and features that user has access to. A profile is a collection of settings and permissions. Profile settings determine which data the user can see, and permissions determine what the user can do with that data.
  - The settings in a user’s profile determine whether she can see a particular app, tab, field, or record type.
  - The permissions in a user’s profile determine whether she can create or edit records of a given type, run reports, and customize the app.

- Profiles usually match up with a user's job function (for example, system administrator, recruiter, or hiring manager), but you can have profiles for anything that makes sense for your Salesforce org. A profile can be assigned to many users, but a user can have only one profile at a time.


### Standard Profiles
- Read Only
- Standard User
- Marketing User
- Contract Manager
- System Administrator

### Permission Sets
- A permission set is a collection of settings and permissions that give users access to various tools and functions. The settings and permissions in permission sets are also found in profiles, but permission sets extend users’ functional access without changing their profiles.

- Permission sets make it easy to grant access to the various apps and custom objects in your org, and to take away access when it’s no longer needed.

- Users can have only one profile, but they can have multiple permission sets.

- You'll be using permission sets for two general purposes: to grant access to custom objects or apps, and to grant permissions—temporarily or long term—to specific fields.

---

## Control Access to Fields

### Modify Field-Level Security
- Defining field-level security for sensitive fields is the second piece of the security and sharing puzzle, after controlling object-level access.

### Restrict Field Access with a Profile
- You apply field settings by modifying profiles or permission sets. 

---

## Control Access to Records

### Record-Level Security
- Record access determines which individual records users can view and edit in each object they have access to in their profile. First ask yourself these questions:
  - Should your users have open access to every record, or just a subset?
  - If it’s a subset, what rules should determine whether the user can access them?

- You control record-level access in four ways. They’re listed in order of increasing access. You use org-wide defaults to lock down your data to the most restrictive level, and then use the other record-level security tools to grant access to selected users, as required.

  1. `Org-wide defaults` - specify the default level of access users have to each other’s records.
  2. `Role hierarchies` - ensure managers have access to the same records as their subordinates. Each role in the hierarchy represents a level of data access that a user or group of users needs.
  3. `Sharing rules` - are automatic exceptions to org-wide defaults for particular groups of users, to give them access to records they don’t own or can’t normally see.
  4. `Manual sharing` - lets record owners give read and edit permissions to users who might not have access to the record any other way.

## Role Hierarchy
- A role hierarchy works together with sharing settings to determine the levels of access users have to your Salesforce data. Users can access the data of all the users directly below them in the hierarchy.

### Sharing Rules
- This enables you to make automatic exceptions to your org-wide sharing settings for selected sets of users.
- Sharing rules can be based on who owns the record or on the values of fields in the record. For example, use sharing rules to extend sharing access to users in public groups or roles. As with role hierarchies, sharing rules can never be stricter than your org-wide default settings. They just allow greater access for particular users.

### Public Group
- Using a public group when defining a sharing rule makes the rule easier to create and, more important, easier to understand later, especially if it's one of many sharing rules that you're trying to maintain in a large organization.
- Create a public group if you want to define a sharing rule that encompasses more than one or two groups or roles, or any individual.

