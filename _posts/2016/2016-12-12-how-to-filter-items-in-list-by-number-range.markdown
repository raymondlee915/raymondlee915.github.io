---
layout: "post"
title: "How to Filter Items in List in an Excel-like Way"
date: "2016-12-12 21:15"
author: "Ray Lee"
pageType: details_page
excerpt: In this post, I am talking about how to filter items in a list by specifying a number range based on field values of items. For example, to find the "Balls" whose weight is between 30 and 35 Kg.
---

I am sure you all know that the built-in list view has filters. You can choose items to display on page on the menu after you clicked on header of each column. With this filters, users can find the ones they want very easily.
## What's the default filters looks like
By default, you can filter the items by choosing the values you want one by one, just like the below screenshot shows. When the items are not so many, it works just fine.
![default item filters in SharePoint](/assets/images/listfilter/normalDateFilter.png)

## What if there are hundreds of items?
First of all, you will get a menu like what the screenshot show below.
![A filter menu without the choices](/assets/images/listfilter/normal-filter-with-too-many-items1.png)

It won't show you anything until you clicked on the text "Show Filter Choices". And after that, this will be what you got.
![Filter menu with all the choices](/assets/images/listfilter/normal-filter-with-too-many-items2.png)

The problem is that this filter menu is not that helpful, because
1. There are too many choices in the dropdown list, a small move of scrollbar could cause a big change in the choices list.
2. You can only select one option each time.
3. If you want to switch to other choices, you have too go though these two steps again.

## We can do better with List View Filter
[List View Filter](/sharepoint-list-view-filter.html) can help you with this situation by add more options for users to filter data. It supports the columns in these three types.
1. Single line of text
2. Date and Time
3. Number, including Currency

After installed [List View Filter](/sharepoint-list-view-filter.html) on SharePoint server, the filter menu will be different from the built in one.
### With Single line of text
You can get more methods to filter the items,
1. Starts with. Search the column value starts with your input.
2. Contains. Search the column value contains your input.
3. Equals with. Search the column value equals with your input.

![Advanced filter menu for Signle line of text ](/assets/images/listfilter/text-filter-menu.PNG)

### With Numeric columns
You can get more methods to filter the items,
1. Equals with. Search the column value equals with your input.
2. Not Equals with. Search the column value does not equal with your input.
3. From * to *. Search the column value in the range of your inputs.

![Advanced filter menu for Number and Currency](/assets/images/listfilter/number-filter-menu.PNG)

### With Date and Time columns
You can get more methods to filter the items,
1. Today. Search the column value equals with your today.
2. This Week. Search the column value belongs to the current week.
3. This Month. Search the column value belongs to the current month.
4. Last Week. Search the column value belongs to the last week.
5. Last Month. Search the column value belongs to the last month.
6. From * to *. Search the column value in the range of your inputs.

![Advanced filter menu for Date and Time](/assets/images/listfilter/date-filter-menu.PNG)


You can find more details about this product [here][e6d2de6a]

  [e6d2de6a]: /sharepoint-list-view-filter.html "SharePoint List View Filter by BluePower solutions"
