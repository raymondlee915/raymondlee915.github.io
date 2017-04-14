---
layout: "post"
title: "How to get the number of suspended workflow instances in a SharePoint list"
date: "2017-04-14 23:56"
author: "Ray Lee"
pageType: details_page
excerpt: How to know if there is a workflow instance just got suspended? This is something very important for a SharePoint Workflow Admin. I am going to give a simple code to query the number by calling SharePoint Client API with javaScript.
---

SharePoint provides a set of client APIs for developers, a lot of things can be done with these APIs. Today I am going to show you how to query if there is any workflow instance got suspended, but this codes can only work for SharePoint 2013 workflows.

Microsoft has a [post][ad38ef38] about how to use client side object model to work with SharePoint 2013 workflow services. But the quality of these documentation from Microsoft is much worse than the ones for server side object model, a lot of details are missed and sometimes they even guide you to the wrong way.

To complete this feature, there are some class defined in SharePoint client object model should be known.

[WorkflowSubscriptionService][4aaed17f]

[WorkflowInstanceService][c516e4f3]

[WorkflowStatus][fc7ca885]



And here comes the codes,
{% highlight javaScript %}
    function execute(clientContext) {
          var defer = jQuery.Deferred()
          clientContext.executeQueryAsync(
              function() {
                  defer.resolve();
              },
              function() {
                  defer.reject(arguments[1].get_message())
              }
          );

          return defer;
      }

      JSRequest.EnsureSetup();
      hostweburl = decodeURIComponent(JSRequest.QueryString["SPHostUrl"]);
      appweburl = decodeURIComponent(JSRequest.QueryString["SPAppWebUrl"]);

      var context = new SP.ClientContext(appweburl);
      var appContextSite = new SP.AppContextSite(context, hostweburl);
      var web = appContextSite.get_web();
      var workflowServicesManager = SP.WorkflowServices.WorkflowServicesManager.newObject(context, web);
      var workflowSubscriptionService = workflowServicesManager.getWorkflowSubscriptionService();
      var workflowInstanceService = workflowServicesManager.getWorkflowInstanceService();

      // find the subscription ID manually on your SharePoint site. List Setting->Workflow Setting,
      // you can find the subscription ID by the URL of the workflow name on this page.
      var subscriptionInstance = workflowSubscriptionService.getSubscription(subscriptionId);
      context.load(subscriptionInstance);
      window.count = 0;
      execute(context).then(function() {
          count = workflowInstanceService.countInstancesWithStatus(subscriptionInstance, 2);

          execute(context).done(function() {
              alert("The count of workflow instances is "+window.count);
          }).fail(function(message) {
              console.error("Error happened when trying to get worklfow subscriptions for list" + listId);
              console.error(message);

          });
      });

{% endhighlight %}

I am using jQuery to help me. The most tricky thing in this code is you have to pass the subscription object to method **countInstancesWithStatus**, you can not just pass the ID. That is why we need to load the subscription object first with the ID we found on the page.

In this code, it is querying the suspended instances. You can query instances in any status by changing the second parameter for method **countInstancesWithStatus**. Here is the options you can have,
{% highlight javaScript %}
var WorkflowStatus =
   {
       NotStarted : 0,
       Started : 1,
       Suspended : 2,
       Canceling : 3,
       Canceled : 4,
       Terminated : 5,
       Completed : 6,
       NotSpecified : 7,
       Invalid : 8
   }
{% endhighlight %}

Hope this post can help! Oh, **DO NOT** forget to load **sp.workflowservices.js** on your page before you run these codes.

  [ad38ef38]: https://msdn.microsoft.com/en-us/library/office/dn481315.aspx "Working with the SharePoint 2013 Workflow Services Client Side Object Model"
  [4aaed17f]: https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.workflowservices.workflowsubscriptionservice_members.aspx "WorkflowSubscrptionService Members"
  [c516e4f3]: https://msdn.microsoft.com/en-us/library/office/microsoft.sharepoint.workflowservices.workflowinstanceservice_members.aspx "WorkflowInstanceService Members"
  [fc7ca885]: https://msdn.microsoft.com/en-US/library/office/microsoft.sharepoint.client.workflowservices.workflowstatus.aspx "WorkflowStatus Enumeration"
