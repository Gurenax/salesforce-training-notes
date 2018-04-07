# Developer Console
- The Developer Console is an integrated development environment (more typically called an IDE) where you can create, debug, and test apps in your org.
- The Developer Console is connected to one org and is browser-based.

## Reference
- [Developer Console Basics](https://trailhead.salesforce.com/trails/force_com_dev_beginner/modules/developer_console)

## Workspaces
- Workspaces in the Developer Console help you organize information to show just what you need as you work on each of your development tasks. Although it sounds like a fancy term, a workspace is simply a collection of resources, represented by tabs, in the main panel of the Developer Console. You can create a workspace for any group of resources that you use together.
- Workspaces reduce clutter and make it easier to navigate between different resources.

## Navigate/Edit Source Codes
### Create and Execute Apex Class
- Apex is a Salesforce programming language that you can use to customize business processes in your org.

- File | New | Apex Class
```java
public class EmailMissionSpecialist {
   // Public method
   public void sendMail(String address, String subject, String body) {
      // Create an email message object
      Messaging.SingleEmailMessage mail = new Messaging.SingleEmailMessage();
      String[] toAddresses = new String[] {address};
      mail.setToAddresses(toAddresses);
      mail.setSubject(subject);
      mail.setPlainTextBody(body);
      // Pass this email message to the built-in sendEmail method 
      // of the Messaging class
      Messaging.SendEmailResult[] results = Messaging.sendEmail(
                                new Messaging.SingleEmailMessage[] { mail });
      
      // Call a helper method to inspect the returned results
      inspectResults(results);
   }
   // Helper method
   private static Boolean inspectResults(Messaging.SendEmailResult[] results) {
      Boolean sendResult = true;
      // sendEmail returns an array of result objects.
      // Iterate through the list to inspect results. 
      // In this class, the methods send only one email, 
      // so we should have only one result.
      for (Messaging.SendEmailResult res : results) {
         if (res.isSuccess()) {
            System.debug('Email sent successfully');
         }
         else {
            sendResult = false;
            System.debug('The following errors occurred: ' + res.getErrors());                 
         }
      }
      return sendResult;
   }
}
```

- Debug | Open Execute Anonymous Window
```java
EmailMissionSpecialist em = new EmailMissionSpecialist();
em.sendMail('Enter your email address', 'Flight Path Change', 
   'Mission Control 123: Your flight path has been changed to avoid collision '
   + 'with asteroid 2014 QO441.');
```


### Create Lightning Components
- Lightning Components is a framework for developing mobile and desktop apps. You can use it to create responsive user interfaces for Lightning Platform apps. Lightning components also make it easier to build apps that work well across mobile and desktop devices.

- Using the Developer Console, you can create a `Lightning component bundle`. A component bundle acts like a folder in that it contains components and all other related resources, such as style sheets, controllers, and design.

- To create lightning components, File | New | Lightning Component

- To open lightning components, File | Open Lightning Resources


### Create Visualforce Pages and Components
- Visualforce is a web development framework for building sophisticated user interfaces for mobile and desktop apps. These interfaces are hosted on the Lightning Platform platform. Your user interface can look like the standard Salesforce interface, or you can customize it.

- To create a visual force page, File | New | Visualforce Page

- To open a visual force page, File | Open | Entity Type > Pages



---

## Generate/Analyze Logs
- Logs are one of the best places to identify problems with a system or program. Using the Developer Console, you can look at various debug logs to understand how your code works and to identify any performance issues.

### Two Ways to open debug logs
- `Before execution` - enable Open Log in the Enter Apex Code window. The log opens after your code has been executed.
- `After execution` - double-click the log that appears in the Logs tab.

### Debug Log columns
1. `Timestamp`
— The time when the event occurred. The timestamp is always in the user’s time zone and in HH:mm:ss:SSS format.

2. `Event`
— The event that triggered the debug log entry. For instance, in the execution log that you generated, the FATAL_ERROR event is logged when the email address is determined to be invalid.

3. `Details`
— Details about the line of code and the method name where the code was executed.

### Log Inspector
- The handy Log Inspector exists to make it easier to view large logs! The Log Inspector uses log panel views to provide different perspectives of your code. - To open, Debug | View Log Panels

### Log Inspector Panels
1. `Stack Tree` — Displays log entries within the hierarchy of their objects and their execution using a top-down tree view. For instance, if one class calls a second class, the second class is shown as the child of the first.

2. `Execution Stack` — Displays a bottom-up view of the selected item. It displays the log entry, followed by the operation that called it.

3. `Execution Log` — Displays every action that occurred during the execution of your code.

4. `Source` — Displays the contents of the source file, indicating the line of code being run when the selected log entry was generated.

5. `Source List` — Displays the context of the code being executed when the event was logged. For example, if you select the log entry generated when the faulty email address value was entered, the Source List shows execute_anonymous_apex.

6. `Variables` — Displays the variables and their assigned values that were in scope when the code that generated the selected log entry was run.

7. `Execution Overview` — Displays statistics for the code being executed, including the execution time and heap size.


### Perspective Manager
- A perspective is a layout of grouped panels. For instance, the predefined Debug perspective displays the Execution Log, Source, and Variables, while the Analysis perspective displays the Stack Tree, Execution Log, Execution Stack, and Execution Overview.
- To choose a perspective, Debug | Switch Perspectives or Debug | Perspective Manager


### Log Categories
- `ApexCode`, which logs events related to Apex code and includes information about the start and end of an Apex method.
- `Database`, which includes logs related to database events, including Database Manipulation Language (DML), SOSL, and SOQL queries.

### Log Levels
- Log levels control how much detail is logged for each log category.
- From the least amount of data logged (level = NONE) to the most (level = FINEST).
  * NONE
  * ERROR
  * WARN
  * INFO
  * DEBUG
  * FINE
  * FINER
  * FINEST

- To change, Debug | Change Log Levels

---

## Inspect Objects at Checkpoints
- Checkpoints show you snapshots of what’s happening in your Apex code at particular points during execution.

- You can set checkpoints only when the Apex log level is set to `FINEST`.

### Checkpoint Inspector
- The Checkpoint Inspector has two tabs: Heap and Symbols.
  1. `Heap` — Displays all objects present in memory at the line of code where your checkpoint was executed.
  2. `Symbols` — Displays all symbols in memory in tree view.

---

## Execute SOQL and SOSL Queries 
- SOQL and SOSL Queries can also be executed using the Anonymous Execute Window, Debug | Open Execute Anonymous Window