---
layout: single
classes: wide
title:  "Understanding Azure Apps"
date:   2021-05-11 18:28:33 +0800
--- 
It is important to understand Azure Applications and Security Principals in order to defend them. This post provides basic understanding  for defenders to understand Azure AD Application. 

## Application Architecture
tl;dr

Application Object created as an Application registration is the global representation of an application that can be used across tenants, the Service Principal or Enterprise App is the local representation in a tenant that refers to the globally unique Application Object. Each Tenant must have a Service principal to use the application. Single tenant application will have only on service principal in its home tenant.  

### Application Registration

Identity Provider (IdP) needs to know about the application, for this developer needs to register the application with IdP. This is achieved using Application Registration.   
One Application will have One Application registration and the application is identified by Application ID/Client ID.   

 Application Registration (App Registration) is the definition/registration of the application.This consists of   
 - Application Display name: Users see this when they use the application  
 - Sign in Audience: Who can use the application single tenant (only accessible in your tenant) or multi-tenant (accessible in other tenants))
 - Redirect URI: URI where IdP(AAD) will redirect a user client and send the security token after authentication
     - Must begin with https (except some localhost exceptions)
     - Is case sensitive
     - Cannot contain wildcards (except only work/school sign in an Orgs AAD tenant)
 - Credentials: For Confidential Client applications accessing web-API. Can use client secrets(aka Applicaion password) or certificates
  
Application Registration is the backend configuration of the application, consider this like a parent class from which all Enterpirse Apps derive.  


Permissions that are required to create App registration are
- Application Administrator
  

### Service Principal aka Enterprise Apps

Service Principal is the local representation of the global application object in a single tenant. Service Principal needs to be created in each tenant where the application will be used and will reference the globally unique application object.  

Service Principal is also a security Principal. 

## Permission Model

### Application permissions
Application permissions are used by applications that do not have signed-in user present. 
Effective Permissions  are based on what the administrator consented to for the application. 
The effective permissions are full level of privileges that have been allowed to the application. 

Very powerfull and should be reviewed and only provided to trusted applications with a requirement to perform enterprise wide changes. 


### Delegated Permissions

Delegated permissions are used by applications that would like to use logged in user permissions. These would be used by applications that have a signed in user present and would access data on behalf of the logged in user. 

Effective Permissions are intersection of the user permissions and the application permissions. The effetive permissions here can never be more than the delegated permissions.
Users may be able to provide consent to this for itself. 

## Dangerous Application permissions

In OAuth 2.0 (that is what AAD uses ), types of permission sets are called scopes aka permissions. A permission is represented as a string value as listed below.
 
Dangerous Application Permissions to review

- Mail.*   
- mail.Send  
- MailboxSettings.*
- Contacts.*
- People.*
- Files.*
- Notes.*
- Directory.AccessAsUser.All
- Directory.ReadWrite.All
- Application.ReadWrite.All
- Domain.ReadWrite.All
- EduRoaster.ReadWrite.All
- Group.ReadWrite.All
- Member.Read.Hidden
- RoleManagement.readWrite.Directory
- User.ReadWrite.All
- User.ManageCreds.All
- user_impersonation

Applications with these permissions should be reviewed, to understand and ensure if an application is having these permissions needs them for legitimate reasons. 

Lower Impact Permissions

- User.Read
- open_id
- email
- profile



Reference
- [Script to pull Application permissions](https://aka.ms/getazureadpermissions) 
- [dotNET - Service principles and app registration](https://www.youtube.com/watch?v=2s0vS3RyaVw) 
- BSides CT 2020 - Mark Morowczynski/Michael Epping - Hiding in the clouds https://www.youtube.com/watch?v=mxOHcqHxpi8
- https://docs.microsoft.com/en-us/azure/active-directory/develop/reply-url
- https://docs.microsoft.com/en-us/azure/active-directory/develop/msal-client-applications 
- https://docs.microsoft.com/en-us/azure/active-directory/develop/v2-permissions-and-consent
