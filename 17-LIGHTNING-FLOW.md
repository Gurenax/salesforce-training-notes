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
- Cloud Flow Designer is like a workflow which requires human input.
- Approval Automation is a process specifically made to request for an approval or sign-off and that requires human interaction.

