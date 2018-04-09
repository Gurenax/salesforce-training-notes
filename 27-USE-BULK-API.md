# Use Bulk API
- Bulk API is based on REST principles and is optimized for working with large sets of data.
- You can use it to insert, update, upsert, or delete many records asynchronously, meaning that you submit a request and come back for the results later. Salesforce processes the request in the background.
- The easiest way to use Bulk API is to enable it for processing records in Data Loader using CSV files. With Data Loader, you don’t have to write your own client app. 

## Setup
1. Log in to your Trailhead DE org and navigate to [Workbench](https://workbench.developerforce.com/).
2. For Environment, select Production. For API Version, select the highest available number. Make sure that you select I agree to the terms of service.
3. Click Login with Salesforce.
4. In the top menu, select utilities | REST Explorer.

## Create a Bulk Job
1. `POST` request
2. Endpoint url syntax `/services/data/vXX.0/jobs/ingest`
- Example: `/services/data/v41.0/jobs/ingest`
3. Request body
```json
{
  "operation" : "insert",
  "object" : "Account",
  "contentType" : "CSV",
  "lineEnding" : "CRLF"
}
```
4. Execute
5. Copy the Job ID (e.g. 7507F000003zdR5QAI)

## Add Data to the Job
1. `PUT` request
2. Endpoint url syntax `/services/data/vXX.0/jobs/ingest/jobID/batches`
- Example: `/services/data/v41.0/jobs/ingest/7507F000003zdR5QAI/batches`
3. Request body
```csv
"Name"
"Sample Bulk API Account 1"
"Sample Bulk API Account 2"
"Sample Bulk API Account 3"
"Sample Bulk API Account 4"
```
4. Request header
```json
Content-Type: text/csv
Accept: application/json
```
5. Execute

## Close the Job
- This will process the data.
1. `PATCH` request
2. Endpoint url syntax `/services/data/vXX.0/jobs/ingest/jobID`
- Example: `/services/data/v41.0/jobs/ingest/7507F000003zdR5QAI`
3. Request body
```json
{
   "state" : "UploadComplete"
}
```
4. Request header
```json
Content-Type: application/json
Accept: application/json
```
5. Execute
6. Check results
- If your state is still `UploadComplete` instead of `JobComplete`, Salesforce is still processing the job. Don’t worry, it’ll be processed in a few minutes.


## Get the Job Results
1. `GET` request
2. Endpoint url syntax `/services/data/vXX.0/jobs/ingest/jobID/successfulResults`
- Example: `/services/data/v41.0/jobs/ingest/7507F000003zdR5QAI/successfulResults`
3. Execute

## Check failed results
1. `GET` request
2. Endpoint url syntax `/services/data/vXX.0/jobs/ingest/jobID/failedResults`
- Example: `/services/data/v41.0/jobs/ingest/7507F000003zdR5QAI/failedResults`
3. Execute