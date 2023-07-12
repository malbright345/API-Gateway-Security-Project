<h1>Secure API Gateway with Amazon Cognito</h1>

### Oauth 2.0 Implicit Grant flow demo: Secure API Gateway with Amazon Cognito Authorizer and Extract JSON Web Token (JWT) to Analyze Security Risks and Potential Exposure of Personally Identifiable Information (PII)

<h2>Introduction</h2>
The OAuth 2.0 authorization framework is a protocol that allows a user to grant a third-party web site or application access to the user's protected resources though the use of short-term credentials (web tokens). Instead of sharing long-term credentials such as passwords with the third-party app, OAuth 2.0 utilizes JSON web tokens (JWTs) to delegate granular permissions and authorize specific actions.
<br />
OAuth 2.0 supports several grant types, or “flows” which are used to authorize access to protected resources in different ways. In this example, we will examine using implicit grant to obtain web token, and analyze security risks associated with implicit grant that can lead to broken access control, such as token stealing, session hijacking, and potential disclosure of sensitive information.
<br />
<h2>Description </h2>

In this demo, We will create an Amazon API gateway, integrate it with a Lambda function, and then configure a Cognito Authorizer to limit access to only authenticated users. After the Cognito Authorizer is attached to the API Gateway, a valid JWT must be included in the header of all subsequent API calls to prove that the user making the API call has been properly authenticated and has appropriate permissions to access the API.
<br />
<br />
<h2>Web Services Used </h2>

- <b>Amazon API Gateway</b>
- <b>Amazon Lambda Function</b>
- <b>Amazon Cognito</b>
- <b>Postman.com</b>
- <b>jwt.io</b>

We will use Amazon Web Services (AWS) to illustrate OAuth 2.0 implicit grant work flow, jwt.io to decode and display the plain-text JSON web tokens, and Postman.com to test access controls to the API Gateway. 
<h1>Program walk-through:</h1>
<br />
<p align="left">
<h2>Part One: </h2> <br /> 
In order to observe the OAuth 2.0 Implicit Grant and analyze its potential vulnerabilities, we first need something to secure.  Therefore, we will create an Amazon API Gateway and integrate it with a simple Lambda function as the backend service. 
<br />
<br />
- <b>Create API Gateway</b>
<br />
<br /> 
To get started, sign in to your AWS account and navigate to the AWS console to select the icon for API Gateway: <br/>
<br /> 
<img src="https://i.imgur.com/oi18NcA.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
Once in API Gateway, select HTTP API and click "Build":  <br/>
<br/>
<img src="https://i.imgur.com/CiRa3dn.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
Name the API Gateway and click to "add integration": <br/>
<br/>
<img src="https://i.imgur.com/8FAVCiX.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
Select Lambda integration for the API. This is where we add the backend service that the API will communicate with.  <br/>
<br />
<img src="https://i.imgur.com/S1U2ewf.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
For a Lambda integration, API Gateway invokes the Lambda function and responds with the response from the Lambda function. The Lambda function needs to be created before we can integrate it with the API Gateway :  <br/>
<br />
<img src="https://i.imgur.com/crWckjz.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
 - <b>Create Lambda Function to Integrate with API Gateway</b>
<br />
<br />
<br />
 Navigate back to AWS Console and chose Lambda to create the Lambda function that will be integrated with API Gateway:  <br/>
 <br />
<img src="https://i.imgur.com/nNXjfEL.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
Click "Create Function":  <br/>
 <br />
<img src="https://i.imgur.com/0Ev5MFu.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
 Enter a name for the function and keep the defaults for "author from scratch":  <br/>
 <br />
<img src="https://i.imgur.com/PnraodU.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
 By default, Lambda will create an execution role with permissions to upload logs. Click "create function":  <br/>
  <br/>
<img src="https://i.imgur.com/eB0bGgh.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
The function is now created. We need to scroll down to see the code section and edit the code :  <br/>
 <br />
<img src="https://i.imgur.com/JERldJk.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
 Edit the response message in the code of the Lambda function and click "deploy":  <br/>
  <br/>
<img src="https://i.imgur.com/KlTFfmI.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
 - <b>Integrate Lambda Function with API Gateway</b>
 <br />
<br />
 Navigate back to API Gateway tab and select the newly created Lambda from the drop down menu to integrate it:  <br/>
 <br/>
<img src="https://i.imgur.com/WnhCqwI.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
 Click "next" to continue creating the API Gateway. In this example, the Lambda function we just created "my-api-lambda" , will now be integrated with the API Gateway we just created named "my-api":  <br/>
 <br/>
<img src="https://i.imgur.com/nfZv5qV.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
 The next step is to configure routes, the method is set to ANY by default, but we will change it to GET:  <br/>
<img src="https://i.imgur.com/t7ulGfi.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />

<img src="https://i.imgur.com/HW8QPWS.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
Keep the rest of the defualts, click "next":  <br/>
 <br/>
<img src="https://i.imgur.com/KyhvfQ6.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
Keep default stage and click "next":  <br/>
<br />
<img src="https://i.imgur.com/YlZK7G5.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
Review and click "create" to create API Gateway with Lambda integration:  <br/>
 <br/>
<img src="https://i.imgur.com/MR4yTI1.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
 - <b>Demonstrate That The API Gateway Is Currently Open to the Internet</b>
<br />
<br />
API Gateway is now inegrated with the Lambda function.  Knowing the "Invoke URL" of the API Gateway and the name of the Lambda function is sufficient to access the resource  :  <br/>
<img src="https://i.imgur.com/YTE32Nm.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
Under the "Lambda" tab, we can copy the name of the function and add it to the end of the API "invoke url" path :  <br/>
<br/>
<img src="https://i.imgur.com/3Qb6UA1.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
In the URL bar, type https://ti5gc09a66.execute-api.us-east-2.amazonaws.com/my-api-lambda, which is the "my-api" API "invoke URL" / the name of the lambda function "my-api-lambda." Accessing the "Hello from Lambda: Secure me!" message proves that the endpoint is openly accessible without needing any credentials to authenticate:  <br/>
<br/>
<img src="https://i.imgur.com/dh9vJsX.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
We can also demonstrate open access to the resource by using Postman.com, which is an API platform for building, testing, and using APIs.  We can use Postman.com to add authorization headers to our API calls once we secure our API Gateway.  For now, since we havent't configured a Cognito user pool to serve as our identity provider, and haven't attached an authorizer to the API Gateway, we don't have any tokens to append to our message, nor are they required for access.  Therefore, the same URL that we typed in the browser URL bar to get access to our "Hello from Lambda: Secure me!" message in the screen shot above, can also be used in Postman.com to prove that no credentials are necessary to access our Lambda function. Type in the same URL for our resource into Postman.com (or copy-paste it from your browser's URL) and click send:  <br/>
<br/>
<img src="https://i.imgur.com/hFRIupW.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
The response from Postman is "200 OK" and the "Hello from Lambda: Secure me!" message is returned, demonstrating that my-api-lambda resource is currently freely accessible from the public internet:  <br/>
<img src="https://i.imgur.com/H9w3Gbp.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
 - <b>Begin Configuring Authorizer to Secure API Gateway</b>
 <br />
<br />
 
To begin configuring an authorizer for the API Gateway, go to API Gateway tab and click on routes:  <br/>
<img src="https://i.imgur.com/uk1P2ZM.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
 Select "GET" under the name of the Lambda function to see that there are currently no authorizers attached to this path:  <br/>
  <br/>
<img src="https://i.imgur.com/Clrpm39.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
There are currently no authorizers to select from.  <br/>
<br />
<img src="https://i.imgur.com/9rNZE3D.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
- <b>Configure Cognito User Pool and App Client</b> <br/>
<br />
In order to configure a JWT authorizer to protect this resource, we will first create an identity provider in the form of a Cognito User Pool to issue tokens, and an app client that will request these tokens and return them in a callback URL. Then we will create a test user with which to sign in to the app client to request the tokens that we will then use to access the API Gateway Lambda function:
<br />
<br />
<br />
Navigate back to console and open Cognito in new tab:  <br/>
<br />
<img src="https://i.imgur.com/fSZK1xF.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
Start with Cognito by creating the user pool.  The user pool serves as the authorization server and issuer of JWTs:  <br/>
<br />
<img src="https://i.imgur.com/LITx6py.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
Configure sign in experience through Cognito.  Check user name and email for sign in options :  <br/>
<br />
<img src="https://i.imgur.com/mtijisZ.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
Click "next" :  <br/>
<br />
<img src="https://i.imgur.com/yvnf9Q8.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
Keep Cognito defaults for password requirements:  <br/>
<br />
<img src="https://i.imgur.com/f6qT417.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
Select "No MFA", enable "self-service account recovery email" only for messages, and click next:  <br/>
<br />
<img src="https://i.imgur.com/ONiGy4p.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
Keep defaults for self-service sign-up:  <br/>
<br />
<img src="https://i.imgur.com/bVXtk2B.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
Keep defaults, click "next" at the bottom to continue configuring user pool:  <br/>
<br />
<img src="https://i.imgur.com/ouc9so6.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
Select "send email with Cognito," keep the rest of the default selections and click "next":  <br/>
<br />
<img src="https://i.imgur.com/2DBUElo.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
Name the user pool and select "Use Cognito Hosted UI".  The Cognito Hosted UI will provide the interface to log in with our test user.  Once authenticated, the test user will be able to request and receive JSON Web Tokens   <br/>
<br />
<img src="https://i.imgur.com/OXYOYkB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
Use Cognito domain name:  <br/>
<br />
<img src="https://i.imgur.com/CSY1mUy.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
Select "Public Client" name the app client.  Since we are using OAuth 2.0 implicit grant, select "don't generate client secret":  <br/>
<img src="https://i.imgur.com/yaNqcWH.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
Add localhost:3000 as the callback URL.  This URL is where the test user will be redirected after authenticating.  The JSON Web Tokens will also be sent to the URL bar after the test user successfully authenticaes to the Cognito User Pool through the Cognito Hosted UI:  <br/>
  <br/>
<img src="https://i.imgur.com/VV9EiKl.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
Keep Cognito user pool as the identity provider and select OAuth 2.0 grant type to be "implicit grant" so we can collect the JWT that will be sent to the callback url:  <br/>
<img src="https://i.imgur.com/H4b4ygI.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
Select OIDC scopes which will later show up in the JWT tokens and click next:  <br/>
  <br/>
<img src="https://i.imgur.com/KEoQph5.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
Review settings on next page, scroll to the bottom, and click "create user pool":  <br/>
<img src="https://i.imgur.com/yZF4q8P.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
 Cognito-user-pool is now created and we can click on it to create a test user to use to log in and extract JWTs from the URL bar:  <br/>
<img src="https://i.imgur.com/wwZ9i7X.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
- <b>Create Test User in Cognito User Pool</b> <br/>
<br />
<br />
<br />
In cognito-user-pool, under the users tab, click  "create user":  <br/>
<img src="https://i.imgur.com/FfsnPD6.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
Create user and configure sign-in details:  <br/>
<img src="https://i.imgur.com/sqq8Biu.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
  Set the temporary password and click "create user":  <br/>
<img src="https://i.imgur.com/VAhtKGf.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
 "Testuser" is now created, and we can navigate to "app integration" to log in as this user and retrive JWT credentials that will be sent in the url after successful login:  <br/>
  <br/>
<img src="https://i.imgur.com/GVDj2pw.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
  From "users" tab navigate to "app integration" tab to go to app client.  Scroll down and click on "view hosted UI" to be taken to the page where we can log in as testuser:  <br/>
   <br/>
<img src="https://i.imgur.com/bKalepO.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
 Under "app integration" scroll down to find app client name and click on it to see more about the app client:  <br/>
<img src="https://i.imgur.com/YNNalmJ.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
48 Scroll down to find view hosted UI:  <br/>
<img src="https://i.imgur.com/44ynmlY.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
49 Click View Hosted UI to login as testuser and obtain JWT credentails:  <br/>
<img src="https://i.imgur.com/mKdyfW2.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
50 The hosted UI uses the domain we specified as the app client domain and prompts us to login :  <br/>
<img src="https://i.imgur.com/se8jBVD.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
51 first time we log in we need to change the password :  <br/>
<img src="https://i.imgur.com/BLsrko1.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
52 After successfully logging in we will be redirected to localhost and sent JWT credentails in the url which we will copy and parse into relevant tokens:  <br/>
<img src="https://i.imgur.com/ZrWaXtE.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
53 The JWT Credentials returned in URL copy-pasted into pages:  <br/>
<img src="https://i.imgur.com/f0CgKNf.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
53.01 Show Copy JSON token before pasting into jwt decoder:  <br/>
<img src="https://i.imgur.com/Wr7dU7Z.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
53.02 SHow paste into jwt decoder:  <br/>
<img src="https://i.imgur.com/F0dDBJA.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
53.03 Full JWT but small because zoomed out :  <br/>
<img src="https://i.imgur.com/QbyURoX.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
53.04 JWT consistes of three parts the header body and signature:  <br/>
<img src="https://i.imgur.com/IyCWh3W.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
53.05 Show Decoded JWT zoomed in:  <br/>
<img src="https://i.imgur.com/trhDE4R.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
54 When we refresh the page we see that the api is still open and does not require any authorization to access the lambda function:  <br/>
<img src="https://i.imgur.com/AMVcc8L.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
55 Now that we have JWT credentails extracted from Cognito it's time to secure our API so that it cannot be accessed without first logging in to retrieve credentials:  <br/>
<img src="https://i.imgur.com/AT6dKCr.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
56 Attach Cognito Authorizer to secure API Gateway:  <br/>
<img src="https://i.imgur.com/uDTpliP.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
57 User pool id and app client id attach authorizer:  <br/>
<img src="https://i.imgur.com/RItcY1f.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
57.01 Client ID and user pool id that are put in the authorizer are present in the token claims:  <br/>
<img src="https://i.imgur.com/9OEhNAy.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
58 JWT Authorizer now secures API:  <br/>
<img src="https://i.imgur.com/PrOfq07.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
59 Now that JWT authorizer is attached to API Gateway, my-api-lambda is no longer accessible without first logging in and obtaining JWT credentials:  <br/>
<img src="https://i.imgur.com/CYuXfXu.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
60 Resending API invoke url to see if the resposne is now differnet that the authorizer has been attached:  <br/>
<img src="https://i.imgur.com/AEV5NFX.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
61 Postman now returns 401 unauthorized and message unauthorized:  <br/>
<img src="https://i.imgur.com/4LWylVy.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
62 To use JWT as credentails in Postman copy the access token that we got from the callback url:  <br/>
<img src="https://i.imgur.com/QAaYrIW.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
63 Paste Authorization token into authorization header in postman api:  <br/>
<img src="https://i.imgur.com/bvazzoh.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
64 Paste the token into the Authorization header and resend the request to the api:  <br/>
<img src="https://i.imgur.com/BDsQHix.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
65 we have the 200 OK response and the lambda message showing that we have access because the token was included in the header:  <br/>
<img src="https://i.imgur.com/HEiKnYN.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
66 Access Token that was used in authorization header to access API as authorized user:  <br/>
<img src="https://i.imgur.com/171IOfQ.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
67 The signature at the bottom is verified so its a valid token just needed to be verified by Cognito authorizer as well:  <br/>
<img src="https://i.imgur.com/8Cc8AFS.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
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
