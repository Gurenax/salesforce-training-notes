# Lightning Flow
- Lightning Flow provides declarative process automation for every Salesforce app, experience, and portal. Included in Lightning Flow are two point-and-click automation tools: Process Builder and Cloud Flow Designer. 

## Reference
- [Lightning Flow](https://trailhead.salesforce.com/trails/force_com_dev_beginner/modules/business_process_automation)

## With Process Builder, you build processes. With Cloud Flow Designer, you build flows.

### Process Builder
- A simple but versatile tool for automating simple business processes. In Process Builder, you build `processes`.
- Process Builder is the simplest Lightning Flow tool for the job.
- For a given object, Process Builder lets you control the order of your business processes without any code.


### Cloud Flow Designer
- A drag-and-drop tool for building more complex business processes. In the Cloud Flow Designer, you build `flows`.
- In Cloud Flow Designer, you can’t define a trigger. For a flow to start automatically, you have to call it from something else, like a process or Apex trigger.

---

### Guided visual experiences
- Business processes that need input from users, whether they’re employees or customers.
- `Use Cloud Flow Designer` - If the business process you’re automating requires user input, use the Cloud Flow Designer to build a flow. That way, you can provide a rich, guided experience for your users, whether in your Salesforce org or in a Lightning community.

### Behind-the-scenes automation
- Business processes that get all the necessary data from your Salesforce org or a connected system. In other words, user input isn’t needed.
- `Use Process Builder` Most behind-the-scenes business processes are designed to start automatically when a particular thing occurs. Generally, we refer to that “thing” as a trigger. Business processes can be automatically triggered when:
  - A record changes
  - A platform event message is received
  - A certain amount of time passes or specified time is reached

### Approval automation
- Business processes that determine how a record, like a time-off request, gets approved by the right stakeholders.
- To automate your company’s business processes for approving records, use Approvals to build an approval process.

---

### In a nutshell: Process Builder vs Cloud Flow Designer vs Approval Automation
- Process Builder and Cloud Flow Designer are Lightning Flow tools. Approval Automation is just an additional tool.

- Process Builder is like a background job which doesn't need human interaction.
- Cloud Flow Designer is like a workflow which may or may not requires human input. (e.g. Screen flows requires human input, Autolaunches flows do not )
- Approval Automation is a process specifically made to request for an approval or sign-off and that requires human interaction.

---

## Process Builder
- Process Builder is a point-and-click tool that lets you easily automate if/then business processes and see a graphical representation of your process as you build.

- Every process consists of a trigger, at least one criteria node, and at least one action. You can configure `immediate actions` or `schedule actions` to be executed at a specific time.
  - Each `immediate action` is executed as soon as the criteria evaluates to true.
  - Each `scheduled action` is executed at the specified time, such as 10 days before the record’s close date or 2 days from now.

### Process Types
1. Record Change	- Process starts when `a record is created or edited`
2. Invocable	- Process starts when `it’s called by another process`
3. Platform Event - Process starts when `a platform event message is received`


---

## Cloud Flow Designer
- Cloud Flow Designer is a point-and-click tool for building flows.

### Flow Building Blocks
1. `Elements` - appear on the canvas. To add an element to the canvas, drag it there from the palette.
2. `Connectors` - define the path that the flow takes at runtime. They tell the flow which element to execute next.
3. `Resources` - are containers that represent a given value, such as field values or formulas. You can reference resources throughout your flow. For example, look up an account’s ID, store that ID in a variable, and later reference that ID to update the account.

### Flow Elements
1. `Screen` - Display data to your users or collect information from them with Screen elements. You can add simple fields to your screens, like input fields and radio buttons and out-of-the-box Lightning components like File Upload

2. `Logic` - Control the flow of... well, your flow. Create branches, update data, loop over sets of data, or wait for a particular time.

3. `Actions` - Do something in Salesforce when you have the necessary information (perhaps collected from the user via a screen). Flows can look up, create, update, and delete Salesforce records. They can also create Chatter posts, submit records for approval, and send emails. If your action isn’t possible out of the box, call Apex code from the flow.

4. `Integrations` - In addition to connecting with external systems by calling Apex code, the Cloud Flow Designer has a couple of tie-ins to platform events. Publish platform event messages with a Record Create element. Subscribe to platform events with a Wait element.


### Flow Variables
1. `Variable` - A single value.
2. `sObject Variable` - A set of field values for a single record.
3. `Collection Variable` - Multiple values of the same data type.
4. `sObject Collection Variable`- A set of field values for multiple records that have the same object.


---

## Approvals
- An approval process automates how Salesforce records are approved in your org. In an approval process, you specify:
  - The steps necessary for a record to be approved and who approves it at each step.
  - The actions to take based on what happens during the approval process.

