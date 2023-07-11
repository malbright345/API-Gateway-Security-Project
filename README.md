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
<h2>Program walk-through:</h2>
<br />
<br /> 

<p align="left">
To get started, sign in to your AWS account and navigate to the AWS console to select the icon for API Gateway: <br/>
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
24 Instead of creating authorizer here we will first create an authorizer and app client through Cognito:  <br/>
<img src="https://i.imgur.com/9rNZE3D.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
25 navigate back to console and open Cognito in new tab:  <br/>
<img src="https://i.imgur.com/fSZK1xF.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
26 start with Cognito by vreating user pool:  <br/>
<img src="https://i.imgur.com/LITx6py.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
27 Configure sign in experience through Cognito check user name and email for sign in options :  <br/>
<img src="https://i.imgur.com/mtijisZ.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
28 click next :  <br/>
<img src="https://i.imgur.com/yvnf9Q8.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
29 Keep cognito defaults for password requirements:  <br/>
<img src="https://i.imgur.com/f6qT417.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
30 Select No MFA Enable self-service account recovery email only for messages and click next:  <br/>
<img src="https://i.imgur.com/ONiGy4p.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
31 Keep defaults for self-service sign-up:  <br/>
<img src="https://i.imgur.com/bVXtk2B.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
32 Keep defaults click next at the bottom to continue configuring user pool:  <br/>
<img src="https://i.imgur.com/ouc9so6.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
33 Select send email with Cognito keep the rest of the default selections and click next:  <br/>
<img src="https://i.imgur.com/2DBUElo.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
34 Name user pool and select use Cognito Hosted UI:  <br/>
<img src="https://i.imgur.com/OXYOYkB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
35 Use Cognito domain name the domain and select public client for app client:  <br/>
<img src="https://i.imgur.com/CSY1mUy.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
36 Select Public client name the app client and select don't generate client secret:  <br/>
<img src="https://i.imgur.com/yaNqcWH.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
37 Add Postman callback url to be able to use Postman to extract the JWT credentials when they are sent as well as using localhost to collect the JWT credentials:  <br/>
<img src="https://i.imgur.com/VV9EiKl.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
38 Keep Cognito user pool as the identity provider and select oauth grant type to be implicit grant so we can collect the JWT that will be sent to the callback url:  <br/>
<img src="https://i.imgur.com/H4b4ygI.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
39 Select OIDC scopes which will later show up in the JWT tokens and click next:  <br/>
<img src="https://i.imgur.com/KEoQph5.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
40 Review settings on next page scroll to the bottom and click create user pool:  <br/>
<img src="https://i.imgur.com/yZF4q8P.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
41 Cognito-user-pool is now created and we can click on it to create a dummy user to use to log in and extract JWT credentials:  <br/>
<img src="https://i.imgur.com/wwZ9i7X.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
42 In cognito-user-pool under the users tab click to create user:  <br/>
<img src="https://i.imgur.com/FfsnPD6.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
43 Create user and configure sign-in details:  <br/>
<img src="https://i.imgur.com/sqq8Biu.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
44 Set the temporary password and click create user:  <br/>
<img src="https://i.imgur.com/VAhtKGf.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
45 Testuser is now created and we can navigate to app integration to log in as this user and retrive JWT credentials that will be sent in the url after successful login:  <br/>
<img src="https://i.imgur.com/GVDj2pw.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
46 from users tab navigate to app integration tab to go to app client and open the hosted UI to log in as testuser:  <br/>
<img src="https://i.imgur.com/bKalepO.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
47 Under app integration scroll down to find app client name and click on it to see more about the app client:  <br/>
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
