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
Once in API Gateway, select HTTP API and click "Build":  <br/>
<img src="https://i.imgur.com/CiRa3dn.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Name the API and click to add integration: <br/>
<img src="https://i.imgur.com/8FAVCiX.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
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
5.1 Navigate back to AWS Console and chose Lambda to create the Lambda function that will be integrated with API Gateway:  <br/>
<img src="https://i.imgur.com/nNXjfEL.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
6 Click Create Function:  <br/>
<img src="https://i.imgur.com/0Ev5MFu.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
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
