---
layout: "post"
title: "How to validate Email Address Columns in SharePoint"
date: "2016-11-29 23:23"
pageType: details_page
excerpt: This post can help you to setup one workflow with email approval
---
There are a lot of cases in which you need to setup one field to hold the email addresses. So it is a very common request for a SharePoint administrator to do email address validation on this field.

## The first thought
Well, the column validation feature from SharePoint 2010 must be the first thing came into your mind. And you are absolutely right. But, there is a but as always, what is the syntax to write the validation expression? Is it regular expression?

The answer is no, SharePoint has it's own [syntax][sharepointcolumnvalidation]{:target="_blank"} to do this. If you looked at that post from Microsoft, you would know it is not easy to learn all that rules. Luckily, it is not necessary to learn all of them if you just want to validate email address. Here is an example I googled from internet.

{% highlight ruby%}
=AND(
    ISERROR(FIND(" ", [Email],1)),
    IF(ISERROR(FIND("@", [Email],2)),
        FALSE,
        AND(
            ISERROR(FIND("@",[Email], FIND("@", [Email],2)+1)),
            IF(ISERROR(FIND(".", [Email], FIND("@", [Email],2)+2)),
                FALSE,
                FIND(".", [Email], FIND("@", [Email],2)+2) < LEN([Email])
            )
        )
    )
)
{% endhighlight %}

The next thing you need to do is apply this formula to validate your SharePoint column. You can stop here is that is all you need.

## What if I want to have a white list for the domain names of email addresses

You can did it somehow by changing the formula if you understand all the syntax, it is not easy. That's why we created a [SharePoint Email Column][emailcolumn]{:target="_blank"} for you. After installed this product in your SharePoint server, you can say goodbye to that formula which hard to read or maintain. Create an email column will be just as easy as a single-line column.

![Email column in your column options](/assets/images/emailcolumn/createfield.png){:class="img-thumbnail"}

And you can have a white name list for the domain names of email addresses can have. Then when users try to input a email address without such domain names, SharePoint will stop it from being saved.

![setup white name list for the domain names](/assets/images/emailcolumn/settings.png){:class="img-thumbnail"}

## One more thing
This product has another feature for the SharePoint users. When end users are typing, the SharePoint email field will show a list of emails with pre-defined domain names, users could just select one of them from the suggestion list without inputting @ and the domain name.

![email input with suggestions](/assets/images/emailcolumn/typing.png){:class="img-thumbnail"}

[sharepointcolumnvalidation]: https://support.office.com/en-us/article/Examples-of-common-formulas-in-SharePoint-Lists-D81F5F21-2B4E-45CE-B170-BF7EBF6988B3
[emailcolumn]:/sharepoint-email-field.html
