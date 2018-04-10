# Lightning Experience Chatter Basics for Admins

## Reference
- [Lightning Experience Chatter Basics for Admins](https://trailhead.salesforce.com/trails/lex_admin_implementation/modules/lex_implementation_chatter)

## Chatter
- Chatter is a Salesforce collaboration application that lets your users talk to each other and share information in real time

- Depending on how you set up Chatter for your org, everyone with access to a post can comment on it, like it, and share it. They can also edit, bookmark, and mute it.

- Mute a post from an email by replying with `mute` in the body of the message.

---

## Feed Tracking
- For users to be notified when a field value changes on a record.
- Feed tracking lets you select not only the record types, but also the specific record fields to follow.
- Enable feed tracking so your users get Chatter posts and email notifications about changes to the records they follow.
- Feed tracking makes it easy to see changes to critical records anytime, anywhere.
- You can track fields on `User`, `Group`, `Custom`, and `External objects`, and the following standard records: `Account, Article Type, Asset, Campaign, Case, Contact, Contract, Dashboard, Event, Lead, Opportunity, Product, Report, Solution, and Task`.
- Sharing rules and field-level security affect the visibility of record changes in Chatter feeds.
- Tracked feed updates that are older than 45 days and have no likes or comments are deleted automatically.
- You can track up to `20 fields per object`.

---

## Approvals in Chatter
- An approval process outlines the steps for approving a record, like an account, and identifies the person who approves each step.
- The approval process can send an approval request as a Chatter post.
- You can create a template for that post to ensure that the same type of data is posted with every request.

### Steps
1. Enable Approvals in Chatter.
2. Create an approval post template.
3. Create an approval process.
4. Ensure that Chatter feed tracking is enabled for the record type you’re creating the approval process for.
5. Take steps 2–4 for each record type you want to send through an approval process.

### Create a Post Template
- Setup | Post Templates | New Template

### Create an Approval Process
- Setup | Approval Processes

