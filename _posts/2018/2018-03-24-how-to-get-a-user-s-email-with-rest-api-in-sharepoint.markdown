---
layout: "post"
title: "How to get a user's email address with REST API in SharePoint"
date: "2018-03-24 11:45"
author: "Ray Lee"
pageType: details_page
excerpt: How to know if there is a workflow instance just got suspended? This is something very important for a SharePoint Workflow Admin. I am going to give a simple code to query the number by calling SharePoint Client API with javaScript.
---
When I was working SharePoint REST API recently, I just found one interesting thing about user column regarding a query in REST API.
The thing is when you use a REST to query items which contains user columns, actually you can get a bunch of properties other than just user's ID and Title.
Here is a list of these properties I copied from Stack Overflow,
```
Title

Name

EMail

MobilePhone

SipAddress

Department

JobTitle

FirstName

LastName

WorkPhone

UserName

Office

ID

Modified

Created

ImnName

NameWithPicture

NameWithPictureAndDetails

ContentTypeDisp
```
So here is an example,
```
http://your-sharepoint-web/_api/web/lists/getbytitle('your-list-title')/items?$select=User/ID,User/EMail&$expand=User
```
With this call, you are going to get the user's eamil in the JSON response.
