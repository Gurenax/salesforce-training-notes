# Use REST API
- You use a REST resource to interact with your Salesforce org. For example, you can:
  - Retrieve summary information about the API versions available to you.
  - Obtain detailed information about a Salesforce object, such as Account, User, or a custom object.
  - Perform a query or search.
  - Update or delete records.

## Setup REST API
1.  Log in to your Trailhead DE org and navigate to [Workbench](https://workbench.developerforce.com/).
2.  For Environment, select Production.
3.  For API Version, select the highest available number.
4.  Make sure that you select I agree to the terms of service.
5.  Click Login with Salesforce.
6.  In the top menu, select `utilities | REST Explorer`.

## Execute a request
1. `GET` request
```
/services/data/v41.0/sobjects/account/describe
```
2. Click `Execute`
3. Click `Show Raw Response`


## Create an Account
1. `POST` requuest
```
/services/data/v41.0/sobjects/account
```
2. Request body
```json
{
  "Name" : "NewAccount1",
  "ShippingCity" : "San Francisco"
}
```
3. Click `Execute`
4. Click `Show Raw Response`

## Create an Account: Encounter an error
1. `POST` requuest
```
/services/data/v41.0/sobjects/account
```
2. Request body
```json
{
  "ShippingCity" : "San Francisco"
}
```
3. Click `Execute`
4. Click `Show Raw Response`

## Execute a Query
1. `GET` request
```
/services/data/v41.0/query/?q=SELECT+Name+From+Account+WHERE+ShippingCity='San+Francisco'
```
2. Click `Execute`
3. Click `Show Raw Response`

---

### Sample Node.js using Nforce
```javascript
var nforce = require('nforce');
// create the connection with the Salesforce connected app
var org = nforce.createConnection({
  clientId: process.env.CLIENT_ID,
  clientSecret: process.env.CLIENT_SECRET,
  redirectUri: process.env.CALLBACK_URL,
  mode: 'single'
});
// authenticate and return OAuth token
org.authenticate({
  username: process.env.USERNAME,
  password: process.env.PASSWORD+process.env.SECURITY_TOKEN
}, function(err, resp){
  if (!err) {
    console.log('Successfully logged in! Cached Token: ' + org.oauth.access_token);
    // execute the query
    org.query({ query: 'select id, name from account limit 5' }, function(err, resp){
      if(!err && resp.records) {
        // output the account names
        for (i=0; i<resp.records.length;i++) {
          console.log(resp.records[i].get('name'));
        }
      }
    });
  }
  if (err) console.log(err);
});
```

### Sample Ruby using Restforce
```ruby
require 'restforce'
// create the connection with the Salesforce connected app
client = Restforce.new :username => ENV['USERNAME'],
  :password       => ENV['PASSWORD'],
  :security_token => ENV['SECURITY_TOKEN'],
  :client_id      => ENV['CLIENT_ID'],
  :client_secret  => ENV['CLIENT_SECRET']
// execute the query
accounts = client.query("select id, name from account limit 5")
// output the account names
accounts.each do |account|
  p account.Name
end
```