---
layout: page
title: Client Creation
permalink: /client-creation/
---

"Client Creation" refers to the process of having specific Gravity Forms attempt to create a new user account for the "client", as well as send any other email notifications to the "agent" and "lender".

This page will cover:

* TOC
{:toc}

## WordPress Site Settings

First thing to do is visit the "Ambassador Settings" page under "Settings" in the main WordPress administrative section of the website (/wp-admin/).

Once there, you will see an area labelled "Client Creation" and a few settings:

### Creation Forms

This setting tells Gravity Forms which specific forms should begin the client creation process on submit. By default, no forms will do this. Select 1 or more forms from the dropdown.

### Client Creation Wishlist Member Level

This specifies what wishlist member level the new client user account will be added to upon creation.

### Mandrill API Key

This system uses Mandrill to facilitate all email notifications. Thusly, an API key must be supplied to provide integration. Details on how to obtain this are below. Once obtained, enter here.

### Mandrill Template - New User Notification

Select the mandrill email template that should be used for emailing the new user of his account.

### Mandrill Template - Home to Home

Select the mandrill email template that should be used for emailing the new use and his agent/lender a notification about a new Home to Home.

## Mandrill Settings

Next, we should setup Mandrill to receive input.

### API Key

An API key must be obtained and entered into the WordPress Settings, as mentioned above. To do so, in Mandrill, visit "Settings". At the bottom is an area to manage API keys. Copy an existing one or create a new one.

### Templates

Create the templates to be used for the email notifications the site will send. There are two: New User Notification and Home to Home. Once you create and publish these, they will become available for selection within the aforementioned WordPress Settings.

#### Merge Tags

There are a few available merge tags to use in the templates. Merge tags are in the format `*|MERGETAG|*`. Note how there are no spaces or punctuation and it is fully uppercase. Follow this format strictly. Below are the available merge tags:

| Merge Tag   | Content Replaced             |
| ----------- | ---------------------------- |
| FNAME       | Client first name            |
| LNAME       | Client last name             |
| USERNAME    | Client login username        |
| AGENTEMAIL  | Agent email address          |
| AGENTFNAME  | Agent first name             |
| AGENTLNAME  | Agent last name              |
| AGENTPHONE  | Agent phone number           |
| LENDEREMAIL | Lender email address         |
| LENDERFNAME | Lender first name            |
| LENDERLNAME | Lender last name             |
| LENDERPHONE | Lender phone number          |
| PWRESETLINK | Link for password reset page |

*Note: Merge tags will be replaced when content is made available. Sometimes, merge tags may be replaced with an empty string because no content was supplied. EG: The client has no lender, so all LENDER merge tags will become empty in the final email. If you want to build a template with conditional content based on the presence of merge tag content, see this article on how to do so: http://kb.mailchimp.com/merge-tags/use-conditional-merge-tag-blocks*

## Setup Gravity Form

Each form that uses this system must have some additional setup. You need to tell each field in the form how it should be used for the client creation. To do so, edit the form, expand a field settings, click the "Appearance" tab, and enter the identifier into the "CSS Class" field.

Identifiers look like this: `client_creation_field`, where "field" is the field it will be used for. EG: A field being used for the client's first name would be `client_creation_first_name`. Below are the following available identifiers:

| Identifier                    | Content Replaced                  |
| ----------------------------- | --------------------------------- |
| client_creation_first_name    | Client first name                 |
| client_creation_last_name     | Client last name                  |
| client_creation_email         | Client email address              |
| client_creation_phone         | Client phone number               |
| client_creation_preferred_zip | Client preferred zip code         |
| client_creation_agent         | Client preferred agent (user ID)  |
| client_creation_lender        | Client preferred lender (user ID) |

Once you have the form setup with all field identifiers, it is ready for submission.

## What happens on form submit?

The following:

1. New WordPress user account is possibly created for the client. Possibly because if the email supplied with the identifier `client_creation_email` already exists, no new user account is created. If the email does not exist as a WordPress user, a new account is created.
2. The client (new or previously existing) WordPress user ID is passed through back to the form submission. This is done via the dynamic field population parameter `client_user_id`. For more information on dynamic field population, see here: https://www.gravityhelp.com/documentation/article/using-dynamic-population/
3. Emails are sent:
- New User Notification. This is sent only if a new user was created. It is sent to the client.
- Home to Home. This is always sent. The agent and lender email addresses are cc'd onto the email and it is sent to the client email.
