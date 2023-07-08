<h1>Secure API Gateway with Amazon Cognito</h1>

### Oauth 2.0 Implicit Grant flow demo: Secure API Gateway with Amazon Cognito Authorizer and extract JSON Web Token (JWT) to analyze security risks and potential exposure of personal identifiable information (PII)

<h2>Introduction</h2>
The OAuth 2.0 authorization framework is a protocol that allows a user to grant a third-party web site or application access to the user's protected resources though the use of short term credentials (web tokens). Instead of sharing long-term credentials such as passwords with the third-party app, OAuth 2.0 utilizes JSON web tokens (JWTs) to delegate granular permissions and authorize specific actions.
<br />
OAuth 2.0 supports several grant types, or “flows” which are used to authorize access to protected resources in different ways. In this example, we will examine using implicit grant to obtain web token, and analyze security risks associated with implicit grant, such as cross-site scripting, cross-site request forgery, token stealing and potential disclosure of sensitive information.
<br />
<h2>Description </h2>
When an API Gateway is first created, it is open or public, meaning that it can be accessed from the internet by anyone who knows the invoke url. To ensure that only authorized users can access this API, an authorizer should be attached to the API Gateway. Using Amazon Cognito, it is possible to implement OAuth 2.0 authorization framework to protect access to the newly created API Gateway and its resources.
<br />
In this example, we will create an API gateway, integrate it with a Lambda function, and then configure a Cognito Authorizer to limit access to only authenticated users.  After the Cognito Authorizer is attached to the API Gateway, a valid JWT must be included in the header of all subsequent API calls to prove that the user making the API call has been properly authenticated and has appropriate permissions to access the API. 
<br />
<h2>Languages and Utilities Used</h2>

- <b>PowerShell</b> 
- <b>Diskpart</b>

<h2>Environments Used </h2>

- <b>Windows 10</b> (21H2)

<h2>Program walk-through:</h2>

<p align="left">
Navigate to the AWS console and select the icon for API Gateway: <br/>
<img src="https://i.imgur.com/oi18NcA.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
Once in API Gateway, select HTTP API and click "Build":  <br/>
<img src="https://i.imgur.com/CiRa3dn.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
Name the API and click to add integration: <br/>
<img src="https://i.imgur.com/8FAVCiX.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
4 Select Lambda integration for the API:  <br/>
<img src="https://i.imgur.com/S1U2ewf.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
5 The Lambda function needs to be created before we can integrate it with the API Gateway :  <br/>
<img src="https://i.imgur.com/crWckjz.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
5.1 Navigate back to AWS Console and chose Lambda to create the Lambda function that will be integrated with API Gateway:  <br/>
<img src="https://i.imgur.com/nNXjfEL.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
6 Click Create Function:  <br/>
<img src="https://i.imgur.com/0Ev5MFu.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
7 Name the function and keep the defaults of author from scratch and runtime and architecture:  <br/>
<img src="https://i.imgur.com/PnraodU.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
8 Lambda will create an execution role with permissions to upload logs. Click create function:  <br/>
<img src="https://i.imgur.com/eB0bGgh.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
9 Function is now created. Scroll down to edit code and deploy:  <br/>
<img src="https://i.imgur.com/JERldJk.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
10 Edit the response message and click deploy:  <br/>
<img src="https://i.imgur.com/KlTFfmI.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
11 Continue to create API Gateway and select the newly created Lambda from the drop down menu:  <br/>
<img src="https://i.imgur.com/WnhCqwI.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
12 Click next to continue creating API. my-api-lambda function will now be integrated with my-api gateway:  <br/>
<img src="https://i.imgur.com/nfZv5qV.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
12.1 The next screen is to configure routes, the method is set to Any by default, but we will change it to Get:  <br/>
<img src="https://i.imgur.com/t7ulGfi.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
13 Change route to Get, keep the rest of the defaults:  <br/>
<img src="https://i.imgur.com/HW8QPWS.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
14 keep the rest of the defualts, click next, resource path and integration target will be specified:  <br/>
<img src="https://i.imgur.com/KyhvfQ6.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
15 keep default stage and click next:  <br/>
<img src="https://i.imgur.com/YlZK7G5.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
16 Review and click create to create API Gateway with lambda integration:  <br/>
<img src="https://i.imgur.com/MR4yTI1.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
17 API gateway now exists copy the invoke url and add the name of the lambda function to the path to see open access:  <br/>
<img src="https://i.imgur.com/YTE32Nm.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
18 copy the name of the function to add to the end of the api invoke url path :  <br/>
<img src="https://i.imgur.com/3Qb6UA1.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
19 function is publically accessible with invoke url of api slash the name of the lambda function we created:  <br/>
<img src="https://i.imgur.com/dh9vJsX.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
20 navigate to postman and use the same link to show access is open :  <br/>
<img src="https://i.imgur.com/hFRIupW.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
21 response in postman is 200 ok and secure me message is returned just by entering the link with no credentials or token:  <br/>
<img src="https://i.imgur.com/H9w3Gbp.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
22 to secure api go to api gateway tab and click on routes:  <br/>
<img src="https://i.imgur.com/uk1P2ZM.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
23 select get route and attach authorization currently it has none and is an open route:  <br/>
<img src="https://i.imgur.com/Clrpm39.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
6 Click Create Function:  <br/>
<img src="https://i.imgur.com/0Ev5MFu.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
6 Click Create Function:  <br/>
<img src="https://i.imgur.com/0Ev5MFu.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
6 Click Create Function:  <br/>
<img src="https://i.imgur.com/0Ev5MFu.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
6 Click Create Function:  <br/>
<img src="https://i.imgur.com/0Ev5MFu.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
6 Click Create Function:  <br/>
<img src="https://i.imgur.com/0Ev5MFu.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
6 Click Create Function:  <br/>
<img src="https://i.imgur.com/0Ev5MFu.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
6 Click Create Function:  <br/>
<img src="https://i.imgur.com/0Ev5MFu.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
6 Click Create Function:  <br/>
<img src="https://i.imgur.com/0Ev5MFu.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
6 Click Create Function:  <br/>
<img src="https://i.imgur.com/0Ev5MFu.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
6 Click Create Function:  <br/>
<img src="https://i.imgur.com/0Ev5MFu.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
6 Click Create Function:  <br/>
<img src="https://i.imgur.com/0Ev5MFu.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
6 Click Create Function:  <br/>
<img src="https://i.imgur.com/0Ev5MFu.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
6 Click Create Function:  <br/>
<img src="https://i.imgur.com/0Ev5MFu.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
</p>

<!--
 ```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
--!>
