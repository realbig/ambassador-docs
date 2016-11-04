---
layout: page
title: Edit Profile
permalink: /edit-profile/
---

In efforts to have more customization of and a better presentation for user edit/registration forms, this has been integrated into Gravity Forms.

*This stage assumes you already have a working knowledge of how to create/manage/use forms with Gravity Forms and have at least one form created. If you are unsure of how to do this, please read the [Gravity Forms Documentation](https://www.gravityhelp.com/documentation/article/getting-started/).*

*This stage assumes you already have a working knowledge the Gravity Forms User Registrations Add-On. If you are unsure of how to do this, please read the [Gravity Forms User Registration Add-On Documentation](https://www.gravityhelp.com/documentation/article/creating-feed-user-registration-add/).*

**Please visit the [Required Plugins](/required-plugins/) page in order to make sure all required plugins are activated before proceeding.**

This page will cover:

* TOC
{:toc}

## Creating The Feed

Create a user registration feed, as described in the documentation linked above. Take note that there many custom fields added to the "User Meta" area of the feed. These fields have all been manually added by the Ambassador Profile plugin and they mostly represent WLM specific fields that are not typically useable with the User Registration Add-On.

If you want to edit what fields are available, either contact the system administrator to have edits made to the system codebase, or create a plugin on the website that modifies this information via the filter hook `ambassador_gform_userregistration_wlm_meta_keys`. For more information on how to do this, please visit the [developer resources page](https://developer.wordpress.org/reference/functions/add_filter/).

Example:

```php
add_filter( 'ambassador_gform_userregistration_wlm_meta_keys', function ( $fields ) {

    // value is the key identifier written to the database
    $fields[] = array(
        'value' => 'new_custom_field',
        'label' => 'New Custom Field',
    );
    
    return $fields;
});
```

## Lender/Agent Network Field

There is a need for Lenders and Agents to be able to choose their network Lenders and Agents (Lenders choose Agents and Agents choose Lenders). Different levels (Platinum, Gold, Silver) may choose different numbers of users in their network. In order to facilitate this, there has been an available customization added to drop down fields.

### User Add Network Users

A user will need the ability to add Lenders or Agents to their user network from their profile page.

1. Add a regular "Drop Down" field to your edit profile form
2. Click on the "Appearance" tab
3. Add `populate-lenders` or `populate-agents` to the "Custom CSS Class", respectively.

The `populate-lenders` class will populate the drop down choices with a list of users who are lenders from the site. The `populate-agents` class will do the same, but with agents. This means that the drop down's choices will be automatically populated, so you do not need to worry about editing the field choices manually. Now when the user is editing their profile with this form, that drop down will contain the agents or lenders respectively.

### User Use Network Users

When editing a Submit Form for a dashboard, ollow the above steps, but in step 3, use the classes `populate-user-lenders` or `populate-user-agents`.

The `populate-user-lenders` class will populate the drop down choices with the list of users that were previously selected for the user, in other words their network agents/lenders.