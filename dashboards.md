---
layout: page
title: Dashboards
permalink: /dashboards/
---

Dashboards are what the end-user will see when managing their home to homes.

![dashboard](/ambassador-docs/images/dashboard-example.png "Example Dashboard")

You can create as many dashboards as you like and customize each one individually.

**Please visit the [Required Plugins](/required-plugins/) page in order to make sure all required plugins are activated before proceeding.**

This page will cover:

* TOC
{:toc}

## Create A Dashboard

You can now dynamically create as many dashboards as you need. Even though the typical is only the 3 (client, agent, lender), this gives you full control over each one and the ability to create more if needed, like for testing. To do so go to Dashboards in the admin menu. It's managed like any other post type. Add New or edit an existing one.

There are a lot of settings to take in here. I recommend setting up a test dashboard to get a feel for how it works. I will cover some of the less self-explanatory details here. You can view the dashboard itself, without use of a shortcode, by simply clicking "View Dashboard" or "Preview Changes", like any other page.

### Connect the dashboard to a Google Sheet

*Note: You will need to connect the site to Google first. Instructions on how to do so can be found [here](/ambassador-docs/google-sheets#integrating-website).*

The first thing that you need to do is connect the dashboard to a Google Sheet. Use the metabox labelled "Dashboard Display Google Sheet". Once you select/change the sheet, all column selectboxes on the page will populate with the available sheet column headers. It is important that your sheet has column headers. Simply name the first row cells for each column you will be using.

### Connect the dashboard to a Gravity Form

*Note: You will need to create a form and setup a feed. Instructions on how to do so can be found [here](/ambassador-docs/google-sheets#feeds).*

Each dashboard can submit a request to the Google Sheet to add a new row. This is why you have to connect this dashboard to a sheet. Next, you have to connect the dashboard to a form for making the submission. You will need to create a form, setup the Google Sheet feed, and then select that form when creating the dashboard. You can select the form in the metabox labelled "Submit Form". For more information on setting up the form, see <a href="#dashboard-form">Dashboard Form</a>.

### Setup the entry filtering

The user viewing the dashboard only needs to see specific entries, typically in some way associated with their account email. The metabox "Filter Entries" allows you to set this up. Simply select a column from the selectbox that will contain the user's email address in each entry. Only entries that match the logged in user's email address to the value in that entry's column will be shown.

You can also enter a "Custom Email" to match against instead of the logged in user's email address. Technically speaking, you could enter any value here and it would match against whichver column you choose.

### Everything else

Now go through and setup everything else. All other settings are visual and mostly self-explanatory. Simply edit the settings, update the dashboard, and then preview your changes.

Note that the Edit Profile link is generated from the setting you setup on the [settings page](/ambassador-docs/settings/#edit-profile-page).

## Dashboard Form

You will need to create a Gravity Form to link the Dashboard to. This form should be setup to send the form to a Google Sheet. For information on setting up the Google Sheet feed, visit the <a href="/ambassador-docs/google-sheets/#setting-up-feeds">Google Sheets page</a>.

Setup the form like you would any form, but there are a few unique fields you will need to add. Add the following **hidden** fields. When adding each field, click on the "Advanced" tab in the field's settings, check "Allow field to be populated dynamically", and for "Parameter Name", type in according to the table below:

| **Member Level:** | member_level |
| **Member Type:**  | member_type  |
| **User ID:**      | user_id      |

When the form is submitted from the dashboard, the above fields will automatically be filled in and sent to the Google Sheet, but hidden from the user submitting the form.

## Embed A Dashboard

The dashboard can be displayed by using the shortcode `[ambassador_dashboard id="N"]`, where "N" is the dashboard ID. You can copy and paste the provided shortcode from the metabox "How to Use" and place it any page or post. I recommending doing so now on a test page so that you can continually preview your dashboard and ensure it is setup right. You can also preview the changes like any other page by clicking "Preview Changes" or "View Dashboard".

With the Ambassador System, you will want to place dashboards within WLM (WishList Member) visibility shortcodes. They look like `[wlm_private 'Level 1|Level 2']CONTENT[/wlm_private]`, where `Level 1` and `Level 2` represent various levels, separated by pipes `|`, that should be able to view the content. For more information on how to implement this, view the [WLM documentation](http://member.wishlistproducts.com/11-private-tag-protection/).