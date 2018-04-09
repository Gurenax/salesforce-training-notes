# API Basics
- Salesforce takes an API-first approach to building features on the Salesforce Platform.
- API first means building a robust API for a feature before focusing on designing its UI.

## Reference
- [API Basics](https://trailhead.salesforce.com/modules/api_basics)

## Salesforce Data APIs
- In the sea of Salesforce APIs, there’s a key archipelago of commonly used APIs that we focus on in this module. They are `REST API`, `SOAP API`, `Bulk API`, and `Streaming API`.
- Their purpose is to let you manipulate your Salesforce data, whereas other APIs let you do things like customize page layouts or build custom development tools.

---

## REST API
- `REST API` is a simple and powerful web service based on RESTful principles. It exposes all sorts of Salesforce functionality via REST resources and HTTP methods.
- For example, you can create, read, update, and delete (CRUD) records, search or query your data, retrieve object metadata, and access information about limits in your org.
- REST API supports both XML and JSON.
- Because REST API has a lightweight request and response framework and is easy to use, it’s great for writing mobile and web apps.

## SOAP API
- `SOAP API` is a robust and powerful web service based on the industry-standard protocol of the same name. It uses a Web Services Description Language (WSDL) file to rigorously define the parameters for accessing data through the API.
- SOAP API supports XML only.
- Most of the SOAP API functionality is also available through REST API. It just depends on which standard better meets your needs.
- Because SOAP API uses the WSDL file as a formal contract between the API and consumer, it’s great for writing server-to-server integrations.

## Bulk API
- `Bulk API` is a specialized RESTful API for loading and querying lots of data at once. By lots, we mean `50,000 records or more`.
- Bulk API is asynchronous, meaning that you can submit a request and come back later for the results. This approach is the preferred one when dealing with large amounts of data.
- There are two versions of Bulk API (1.0 and 2.0). Both versions handle large amounts of data, but `Bulk API 2.0 is easier to use`.
- Bulk API is great for performing tasks that involve lots of records, such as loading data into your org for the first time.

## Streaming API
- `Streaming API` is a specialized API for setting up notifications that trigger when changes are made to your data. 
- It uses a publish-subscribe, or pub/sub, model in which users can subscribe to channels that broadcast certain types of data changes.
- The pub/sub model reduces the number of API requests by eliminating the need for polling. Streaming API is great for writing apps that would otherwise need to frequently poll for changes.

---

## API Access and Authentication
- To access Salesforce APIs. All you need is an org on one of the following editions: `Enterprise Edition`, `Unlimited Edition`, `Developer Edition`, `Performance Edition`, or `Professional Edition (with an add-on)`.
- Make sure that you have the “API Enabled” permission, and you’re ready to start integrating.

- All API calls, except for the SOAP API `login()` call, require authentication. You can either use one of the supported OAuth flows or authenticate with a session ID retrieved from the SOAP API `login()` call. Check the developer guide for your API of choice to get started.

## API Limits
- Salesforce limits the number of API calls per org to ensure the health of the instance. These limits exist to prevent rogue scripts from smashing our servers into driftwood.
- There are two types of API limits:
  - `Concurrent limits` cap the number of long-running calls (20 seconds or longer) that are running at one time. 
  - `Total limits` cap the number of calls made within a rolling 24-hour period.

- Concurrent limits vary by org type. For a Developer Edition org, the limit is five long-running calls at once. For a sandbox org, it’s 25 long-running calls.

- Total limits vary by org edition, license type, and expansion packs that you purchase. For example, an Enterprise Edition org gets 1,000 calls per Salesforce license and 200 calls per Salesforce Light App license. With the Unlimited Apps Pack, that same Enterprise Edition org gets an extra 4,000 calls. Total limits are also subject to minimums and maximums based on the org edition.

- To check remaining API Calls, go to `Setup | System Overview`
- To setup notifications for when your org exceeds a number of API calls, go to `Setup | API Usage Notifications`.

- When using REST or SOAP API, the `LimitInfoHeader` response header gives you information on your remaining calls. You can also access the REST API Limits resource for information about all sorts of limits in your org.


### When to Use REST API
- REST API provides a powerful, convenient, and simple REST-based web services interface for interacting with Salesforce. Its advantages include ease of integration and development, and it’s an excellent choice of technology for use with mobile applications and web projects.
- However, if you have many records to process, consider using Bulk API, which is based on REST principles and optimized for large sets of data.

### When to Use SOAP API
- SOAP API provides a powerful, convenient, and simple SOAP-based web services interface for interacting with Salesforce. You can use SOAP API to create, retrieve, update, or delete records. You can also use SOAP API to perform searches and much more. Use SOAP API in any language that supports web services.
- For example, you can use SOAP API to integrate Salesforce with your org’s ERP and finance systems. You can also deliver real-time sales and support information to company portals and populate critical business systems with customer information.

### When to Use Chatter REST API
- Use Chatter REST API to display Salesforce data, especially in mobile applications.
- In addition to Chatter feeds, users, groups, and followers, Chatter REST API provides programmatic access to files, recommendations, topics, notifications, Data.com purchasing, and more.
- Chatter REST API is similar to APIs offered by other companies with feeds, such as Facebook and Twitter, but it also exposes Salesforce features beyond Chatter.

### When to Use the Analytics REST API
- You can access Analytics assets—such as datasets, lenses, and dashboards—programmatically using the Analytics REST API.
- Send queries directly to the Analytics Platform.
- Access datasets that have been imported into the Analytics Platform.
- Create and retrieve lenses.
- Access XMD information.
- Retrieve a list of dataset versions.
- Create and retrieve Analytics applications.
- Create, update, and retrieve Analytics dashboards.
- Retrieve a list of dependencies for an application.
- Determine what features are available to the user.
- Work with snapshots. Manipulate replicated datasets.

### When to Use Bulk API
- Bulk API is based on REST principles and is optimized for loading or deleting large sets of data.
- You can use it to query, queryAll, insert, update, upsert, or delete many records asynchronously by submitting batches. Salesforce processes batches in the background.
- The easiest way to use Bulk API is to enable it for processing records in Data Loader using CSV files. Using Data Loader avoids the need to write your own client application.

### When to Use Metadata API
- Use Metadata API to retrieve, deploy, create, update, or delete customizations for your org.
- The most common use is to migrate changes from a sandbox or testing org to your production environment.
- Metadata API is intended for managing customizations and for building tools that can manage the metadata model, not the data itself.
- The easiest way to access the functionality in Metadata API is to use the Force.com IDE or Ant Migration Tool.

### When to Use Streaming API
- Use Streaming API to receive notifications for changes to data that match a SOQL query that you define.
- Streaming API is useful when you want notifications to be pushed from the server to the client.
- Consider Streaming API for applications that poll frequently. Applications that have constant polling against the Salesforce infrastructure consume unnecessary API call and processing time.
- Streaming API reduces the number of requests that return no data, and is also ideal for applications that require general notification of data changes.
- Streaming API enables you to reduce the number of API calls and improve performance.

### When to Use Apex REST API
- Use Apex REST API when you want to expose your Apex classes and methods so that external applications can access your code through REST architecture.
- Apex REST API supports both OAuth 2.0 and Session ID for authorization.

### When to Use Apex SOAP API
- Use Apex SOAP API when you want to expose Apex methods as SOAP web service APIs so that external applications can access your code through SOAP.
- Apex SOAP API supports both OAuth 2.0 and Session ID for authorization.

### When to Use Tooling API
- Use Tooling API to integrate Salesforce metadata with other systems. Metadata types are exposed as sObjects, so you can access one component of a complex type. This field-level access speeds up operations on complex metadata types. You can also build custom development tools for Force.com applications.
- For example, use Tooling API to manage and deploy working copies of Apex classes and triggers and Visualforce pages and components. You can also set checkpoints or heap dump markers, execute anonymous Apex, and access logging and code coverage information.
- REST and SOAP are both supported.
