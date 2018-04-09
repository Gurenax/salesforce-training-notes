# Use SOAP API
- Salesforce provides two SOAP API WSDLs for two different use cases.
  1.  The enterprise WSDL is optimized for a single Salesforce org. It’s strongly typed, and it reflects your org’s specific configuration, meaning that two enterprise WSDL files generated from two different orgs contain different information.

  2.  The partner WSDL is optimized for use with many Salesforce orgs. It’s loosely typed, and it doesn’t change based on an org’s specific configuration.

- Typically, if you’re writing an integration for a single Salesforce org, use the enterprise WSDL. For several orgs, use the partner WSDL.


## Setup enterprise WSDL
- Go to `Setup | API | Generate Enterprise WSDL`
- Save the file to local
- The enterprise WSDL reflects your org’s specific configuration, so whenever you make a metadata change to your org, regenerate the WSDL file. This way, the WSDL file doesn’t fall out of sync with your org’s configuration.

## Create a SOAP Project with SoapUI
1. Download [SoapUI](https://www.soapui.org/downloads/soapui.html)
2. Launch SoapUI
3. `New Soap Project` > Project name: Exploring Salesforce SOAP API
4. Initial WSDL is the file generated from the last section.
5. Click OK

## Log In to Your DE Org
1. In the generated SoapUI project, Scroll down to `login`.
2. Open `Request 1`
3. Remove package version from end of URL
4. Replace `urn:username` value with org username.
5. Replace `urn:password` value with org password with appended security token.
6. Remove `urn:LoginScopeHeader` headers
7. Click play button
8. Copy the instance and session ID into a text file.
e.g. for instance: brave-badger-XXXXXX-dev-ed
```xml
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:urn="urn:enterprise.soap.sforce.com">
   <soapenv:Header>
   </soapenv:Header>
   <soapenv:Body>
      <urn:login>
         <urn:username>XXXXXXXX</urn:username>
         <urn:password>XXXXXXXX</urn:password>
      </urn:login>
   </soapenv:Body>
</soapenv:Envelope>
```
- Because your org’s instance is likely to change, don’t hard-code references to the instance when you start building integrations! Instead, use the Salesforce feature My Domain to configure a custom domain. A custom domain not only eliminates the headache of changing instance names, you can use it to highlight your brand, make your org more secure, and personalize your login page.

## Create an Account
1. Scroll down to `create`.
2. Open `Request 1`.
3. Remove package version from end of URL
5. Change login.salesforce.com to org name `(e.g. https://brave-badger-XXXXX-dev-ed.my.salesforce.com/services/Soap/c/42.0/)`
4. Remove headers except `urn:SessionHeader`.
5. Change `<urn:sObjects>` to `<urn:sObjects xsi:type="urn1:Account" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">`
6. Add `<Name>Sample SOAP Account</Name>`
7. Replace `urn:sessionId` with sessionId from login().
8. Click play button
```xml
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:urn="urn:enterprise.soap.sforce.com" xmlns:urn1="urn:sobject.enterprise.soap.sforce.com">
   <soapenv:Header>
      <urn:SessionHeader>
         <urn:sessionId>XXXXXX</urn:sessionId>
      </urn:SessionHeader>
   </soapenv:Header>
   <soapenv:Body>
      <urn:create>
         <urn:sObjects xsi:type="urn1:Account" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
            <Name>Sample SOAP Account</Name>
         </urn:sObjects>
      </urn:create>
   </soapenv:Body>
</soapenv:Envelope>
```

### Solution to Challenge
```xml
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:urn="urn:enterprise.soap.sforce.com" xmlns:urn1="urn:sobject.enterprise.soap.sforce.com">
   <soapenv:Header>
      <urn:SessionHeader>
         <urn:sessionId>XXXXX</urn:sessionId>
      </urn:SessionHeader>
   </soapenv:Header>
   <soapenv:Body>
      <urn:create>
         <urn:sObjects xsi:type="urn1:Account" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
            <Name>Bluebeards Grog House</Name>
            <Description>It is better than Blackbeards</Description>
         </urn:sObjects>
      </urn:create>
   </soapenv:Body>
</soapenv:Envelope>
```