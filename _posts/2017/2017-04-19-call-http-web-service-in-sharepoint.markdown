---
layout: "post"
title: "Call HTTP Web Service in SharePoint workflow"
date: "2017-04-19 20:59"
author: "Ray Lee"
pageType: details_page
excerpt: Working with SharePoint workflow, sometimes we need to read the items from a list in different website. "Call HTTP Web Service" in SharePoint Workflow Designer and SharePoint REST API can be very helpful in this case.
---
Working with SharePoint workflow, sometimes we need to read the items from a list in different website. "Call HTTP Web Service" in SharePoint Workflow Designer can be very helpful in this case. In this post, I am going to show you how to read list items by calling SharePoint REST API in workflow.

### **Conditions**
The solution here only works for SharePoint 2013 and Office 365 (SharePoint On-line). And the workflow action "Call HTTP Web Service" is only exist in SharePoint 2013 Workflow in SharePoint Designer 2013. So if you are using SharePoint 2010 Workflow, you won't even be able to find this action.

### **Workflow in SharePoint Designer**
When you work with SharePoint 2013 Workflow, you can find this action in your SharePoint Designer.
![Call Http Web Service](/images/2017/04/Call Http Web Service Action.png)

Here is a overview picture of what the workflow looks like at end.
![Call Http Web Service in SharePoint Designer](/images/2017/04/callHttpWorkflowOverview.PNG)

Now I am going to explain all the actions in this workflow one by one.
1. Calling a Web Service provided by SharePoint, by default, the response would be in XML format. We need to specify two HTTP headers to make the result in JSON format, that format can be consumed by SharePoint Workflow by transferring it into Dictionary. Here is the string we set as headers,>*{"Accept":"application/json;odata=verbose","Content-type":"application/json;odata=verbose"}*
![Call Http Web Service with Headers](/images/2017/04/callHttpRequestHeaderStr.png)
2. Assign the string value to a Dictionary variable. The reason why I am not using action "Build a Dictionary" is that the content always got lost when copy the Stage between workflows in different SharePoint Designer window.
3. Created a variable for the URL of the Web Service in SharePoint. If you don't know how to get your URL, you can refer to this [post][a47df27c] from Microsoft. The URL we put in this picture is,>*http://sppc/b/_api/web/lists/GetByTitle('Employees')/Items?$select=\*&$filter=Title eq 'Linda'* ![Call Http Web Service URL](/images/2017/04/callHttpUrl.png)
4. Action "Call HTTP Web Service". Set three parameters in this action: URL, RequestHeader, ResponseContent. You can set the RequestHeader like the following picture, which is brought up by right click on the action itself then select "Property".![Call Http Service set request header](/images/2017/04/callHttpSetHeader.png)
5. The following steps are to read the response. Before you know how to read it, you need to know the structure of the response. You can test this URL with request headers in [Postman][93f6ebf0].
6. Action "Get an Item from a Dictionary". The first parameter is string represents the path of the value you need, it is "d/results" in this case. The second parameter is used to store the result. If you need to get an item form an Array, use "(*index*)" to get it like I did in the step "Get One Item".
7. Action "Count Items in a Dictionary". With this one can get how many items are included in this response.

### **Cross Website Issue**
If the target website is the same one where this workflow is running, this workflow would work just fine. But the HTTP call won't be succeed if you are querying data from a different website. If you log the responseCode out, you would find the its value is "Unauthorized". By default, SharePoint does not allow cross-website query. The following are the step how to fix this problem.

**First you need to enable the "Workflows can use app permissions" feature at the site you are running the HTTP web service call workflow on.**

To allow workflow to use app permissions:

1) Click the **Settings** icon at the top of the page (Gear cog icon).

2) Go to **Site Settings**.

3) Under the **Site Actions** section, select **Manage site features**.

4) Locate the feature called 'Workflows can use app permissions', as shown in the screenshot below, and then click **Activate**.  

**Next we need to grant full control to the workflow app on the sub site you are running the web service call against.**

To grant full control permission to a workflow:

1) Navigate again to the **Site Settings** page on the site you are running the workflow from.

2) Under the **Users and Permissions** section, select **Site app permissions**.

3) On this page the app permissions will be displayed for all apps on your site. Copy the client section of the App Identifier for Workflow. This is the identifier between the last "|" and the "@"

![Get App ID](/images/2017/04/callHttpAppId.PNG)

4) Then navigate to the **Grant permission to an app** page for the site you are trying to run the web service call against. This must be manually navigated to by typing the following URL:
>http://YourSite/_layouts/15/appinv.aspx

5) Paste the **App Id** that you copied in step three and click **Lookup**. This will fill the Title, App Domain, and Redirect URL fields automatically.
![Call HTTP Web Service, Grant Permission](/images/2017/04/callHttpGrantPermission.png)

6) In the Permissions Request field, paste the following XML:
{% highlight XML %}
<AppPermissionRequests>  
  <AppPermissionRequest Scope="http://sharepoint/content/sitecollection/web" Right="FullControl" />  
</AppPermissionRequests>  
{% endhighlight %}

After you finished all these steps, your workflow should work just as expected.

  [a47df27c]: https://msdn.microsoft.com/en-us/library/office/dn292552.aspx "SharePoint REST API"
  [93f6ebf0]: https://www.getpostman.com/ "Postman"
