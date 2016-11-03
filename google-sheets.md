---
layout: page
title: Google Sheets
permalink: /google-sheets/
---

One very important piece of this system is the integration into Google Sheets. This is achieved via a custom Gravity Forms integration for Google Sheets. The plugin name is "Gravity Forms Google Sheets Add-On".

**Please visit the [Required Plugins](/required-plugins/) page in order to make sure all required plugins are activated before proceeding.**

This page will cover:

* TOC
{:toc}

&nbsp;

## Creating The Google APP
***

&nbsp;

In order to provide an integration between the website and Google, an app must be created. This will provide proper authorization to the Google Sheets.

The steps are:

1. [Log Into Google Developer Console](#google-app-login)
2. [Create The APP](#google-app-create)
3. [Setup The Integrations](#google-app-integrations)
4. [Setup The Credentials](#google-app-credentials)

### Log Into Google Developer Console

You must first log into Google. **Log into Google with whichever account has proper access to the Google Sheets that you will need to push to.** Once logged in, head over to the [Google Developer Console](https://console.developers.google.com).

### Create The APP 

1. Click on the dropdown labelled "Project" to the right of the "Google APIs" logo.
2. Click "Create Project" from the dropdown.
3. Enter your Project Name and click "Create". This may take a moment.
4. Once created you will be automatically re-directed to the APP dashboard. If you are not, refresh the page and select your APP from the same dropdown as in step 1.

### Setup The Integrations

1. With your APP selected, click on "Library" in the left-hand toolbar.
2. Navigate or search for the "Drive API".
3. Click the "Enable" button near the top of the window.
4. Repeat steps 1-3 but with the "Sheets API".

### Setup The Credentials

1. With your APP selected, click on "Credentials" in the left-hand toolbar.
2. Click on the blue, "Create credentials" button in the middle of the screen.
3. Select the "OAuth client ID" option.
4. Click blue, "Configure consent screen" button near the right of the window.
5. Fill out the "Product name shown to users" field with your APP name. This is arbitrary and can be whatever you want to be seen when you authorize the application from the website later.
6. Click "Save" at the bottom.
7. Select "Web application" from the "Application type" radio options.
8. Fill in the "Name" field. *Note: this will most likely be the same name from step 5, but again, it can be whatever you like.*
9. Fill in the "Authorized redirect URIs" field with the following appended to your website URL: `/wp-admin/admin.php?page=gf_settings&subview=gravityformsgooglesheets`. So, if your website was `http://www.mywebsite.com`, the full Redirect URI would be `http://www.mywebsite.com/wp-admin/admin.php?page=gf_settings&subview=gravityformsgooglesheets`.
10. Click Enter on your keyboard to submit the Redirect URI.
11. Click "Create" at the bottom of the window.
12. A window will appear showing you your "Client ID" and your "Client Secret". Take note of these somewhere private. *Note: If you ever need to obtain these again, visit this same page and click the pencil button to the right of the credential you just created. They will be at the top of the page.*

Your Google APP is now setup and ready to go!

&nbsp;

## Integrating The Website
***

&nbsp;

You now need to tell the website that the Google APP exists and give the website authorization.

1. Visit the "Google Sheets" settings page `/wp-admin/admin.php?page=gf_settings&subview=gravityformsgooglesheets`.
    1. To get to this page, click on "Forms" on the left side of the admin menu when logged into the backend of your WordPress website.
    2. Click on "Settings" in the "Forms" sub-menu.
    3. Click on "Google Sheets" in the Gravity Forms sub-menu in the page.
2. Enter your Client ID and Client Secret into the fields and click the "Click here to authenticate with Google Sheets" button.
3. You will be taken to a new page where you can authenticate.
4. You will be taken back to the "Google Sheets" settings page and you will see a message that says "Successfully authenticated Google Sheets".

Your website now is authorized to manipulate Google Sheets with your Google account.

&nbsp;

## Setting Up Feeds
***

&nbsp;

In order to submit data to Google Sheets, you will need to setup feeds from your forms.

*This stage assumes you already have a working knowledge of how to create/manage/use forms with Gravity Forms and have at least one form created. If you are unsure of how to do this, please read the [Gravity Forms Documentation](https://www.gravityhelp.com/documentation/article/getting-started/).*

Go to one of your forms and navigate to its "Settings" -> "Google Sheets". You are now on the "Edit Feeds" screen. Click on "Add New". Give the Feed a name. It is arbitrary and only so you can easily identify it. Select a Sheet from the dropdown. This will be populated by all Google Sheets the authorized account as access to.

Once loaded, you will see a 2 column table. This table allows you to map various Sheet columns to your form fields. The left column contains all of your Sheet column headers and the right column contains dropdowns for selecting your form fields. Whenever a form is submitted, the form fields will send their values to the Sheet, using the mapping that you setup here.

Once done, you may add a condition using the "Enable Condition" checkbox. This allows you to only process this feed if the conditions are met.

Click "Update Settings" when done. Your feed is now created and live! If you want to temporarily disable any feeds, go back to the main "Edit Feeds" screen and click on the green/grey button to the left of each feed name. You can create as many feeds as you like and all enabled feeds will be processed on every form submission.