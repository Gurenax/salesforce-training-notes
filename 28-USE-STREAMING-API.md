# Use Streaming API
- `Streaming API` lets you push a stream of notifications from Salesforce to client apps based on criteria that you define.
- Streaming API lets you define events and push notifications to your client app when the events occur.
- You can use Streaming API to keep your external source in sync with your Salesforce data.
- Streaming API lets you process business logic in an external system in response to data changes in Salesforce.
- You can use Streaming API to broadcast notifications for events defined outside of Salesforce.
- Streaming API uses the `Bayeux protocol` and the `CometD messaging library`.

## PushTopics
- We interface with Streaming API through `PushTopics`.
- A PushTopic is an sObject that contains the criteria of events you want to listen to, such as data changes for a particular object.
- You define the criteria as a SOQL query in the PushTopic and specify the record operations to notify on (create, update, delete, and undelete). In addition to event criteria, a PushTopic represents the channel that client apps subscribe to.

### Supported Objects in PushTopic Queries
- PushTopic queries support all custom objects.
- PushTopic queries support the following standard objects.
  - Account
  - Campaign
  - Case
  - Contact
  - Lead
  - Opportunity
  - Task
- The following standard objects are supported in PushTopic queries through a pilot program.
  - ContractLineItem
  - Entitlement
  - LiveChatTranscript
  - Quote
  - QuoteLineItem
  - ServiceContract

### PushTopics and Notifications
- A PushTopic enables you to define the object, fields, and criteria you’re interested in receiving event notifications for.
```java
PushTopic pushTopic = new PushTopic();
pushTopic.Name = 'AccountUpdates';
pushTopic.Query = 'SELECT Id, Name, Phone FROM Account WHERE BillingCity=\'San Francisco\'';
pushTopic.ApiVersion = 37.0;
insert pushTopic;
```
- By default all fields in SELECT and WHERE are the ones that trigger notifications.
-  To change which fields trigger notifications, set `pushTopic.NotifyForFields` to one of these values: `All`, `Referenced (default)`, `Select`, `Where`.

- To set notification preferences explicitly, set the following properties to either true or false. By default, all values are set to true.
```java
pushTopic.NotifyForOperationCreate = true;
pushTopic.NotifyForOperationUpdate = true;
pushTopic.NotifyForOperationUndelete = true;
pushTopic.NotifyForOperationDelete = true;
```

- If you create an account, an event notification is generated. The notification is in JSON and contains the fields that we specified in the PushTopic query: Id, Name, and Phone. The event notification looks similar to the following.
```json
{
  "clientId": "lxdl9o32njygi1gj47kgfaga4k", 
  "data": {
    "event": {
      "createdDate": "2016-09-16T19:45:27.454Z", 
      "replayId": 1, 
      "type": "created"
    }, 
    "sobject": {
      "Phone": "(415) 555-1212", 
      "Id": "001D000000KneakIAB", 
      "Name": "Blackbeard"
    }
  }, 
  "channel": "/topic/AccountUpdates"
}
```
- The notification message includes the channel for the PushTopic, whose name format is `/topic/PushTopicName`. When you create a PushTopic, the channel is created automatically.

### PushTopic Queries
- PushTopic queries are regular SOQL queries
- The SELECT statement’s field list must include `Id`.
- Syntax:
```java
SELECT <comma-separated list of fields> FROM <Salesforce object> WHERE <filter criteria>
```
- Example:
```java
SELECT Id, Name, Phone FROM Account WHERE BillingCity='San Francisco' OR BillingCity='New York'
```

### Retrieve Past Notifications Using Durable Streaming
- As of API version 37.0, Salesforce stores events that match PushTopic queries, even if no one is subscribed to the PushTopic. The events are stored for 24 hours, and you can retrieve them at any time during that window.
- Starting with API version 37.0, each event notification contains a field called `replayId`.
- Similar to replaying a video, Streaming API replays the event notifications that were sent by using the replayId field. The value of the replayId field is a number that identifies the event in the stream. The replay ID is unique for the org and channel.
- When you replay an event, you’re retrieving a stored event from a location in the stored stream. You can either retrieve a stream of events starting after the event specified by a replay ID, or you can retrieve all stored events.
- Replay options:
  - `replayId` - Subscriber receives notifications for events that occur after the event specified by the replay ID value.
  - `-1` - Subscriber receives notifications for all new events that occur after subscription.
  - `-2` - Subscriber receives notifications for all new events that occur after subscription and previous events that fall within the 24-hour retention window.

### Sample Durable Streaming Apps
- To learn how to subscribe to events and use replay options, check out the durable streaming code examples in the [Streaming API Developer Guide](https://developer.salesforce.com/docs/atlas.en-us.api_streaming.meta/api_streaming/).


### Generic Streaming
- Streaming API supports sending notifications with a generic payload that aren’t tied to Salesforce data changes.
- You can use generic streaming for any situation where you want to send custom notifications, such as:
  - Broadcasting messages to specific teams or to your entire organization
  - Sending notifications for events that are external to Salesforce
- To use generic streaming, you need:
  - A streaming channel that defines the channel
  - One or more clients subscribed to the channel
  - The Streaming Channel Push resource to monitor and invoke events on the channel
- You can create a streaming channel for generic streaming either through the Streaming Channels app in the user interface, or through the API.
- A streaming channel is represented by the StreamingChannel sObject, so you can create it through Apex, REST API, or SOAP API.
- The format of the channel name for generic streaming is `/u/ChannelName`. For example, this Apex snippet creates a channel named Broadcast.
```java
StreamingChannel ch = new StreamingChannel();
ch.Name = '/u/Broadcast';
insert ch;
```
- Alternatively, you can opt to have Salesforce create the streaming channel dynamically for you if it doesn’t exist. To enable dynamic streaming channels in your org, from `Setup`, enter `User Interface` in the `Quick Find box`, then select `User Interface`. On the User Interface page, select the `Enable Dynamic Streaming Channel Creation` option.
- You can subscribe to the channel by using a CometD client
- To generate events, make a POST request to the following REST resource. Replace `XX.0` with the API version and `Streaming Channel ID` with the ID of your channel.
```
/services/data/vXX.0/sobjects/StreamingChannel/Streaming Channel ID/push
```
- To obtain your channel ID, run a SOQL query on StreamingChannel, such as: `SELECT Id, Name FROM StreamingChannel`


### Example REST request body
```json
{ 
  "pushEvents": [
      { 
          "payload": "Broadcast message to all subscribers", 
          "userIds": [] 
      } 
   ] 
}
```

### Event notification sample from subscribed client
```json
{
  "clientId": "1p145y6g3x3nmnlodd7v9nhi4k", 
  "data": {
    "payload": "Broadcast message to all subscribers", 
    "event": {
      "createdDate": "2016-09-16T20:43:39.392Z", 
      "replayId": 1
    }
  }, 
  "channel": "/u/Broadcast"
}
```