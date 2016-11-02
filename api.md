---
layout: page
title: API
permalink: /api/
---

The API allows communication between 3rd party applications and the Ambassador website. The API is built on top of the WordPress REST API v2. For detailed documentation on the WordPress REST API v2, please view [this link](http://v2.wp-api.org/). For specific information on available endpoints and their schema, visit [this link](http://v2.wp-api.org/reference/users/).

*NOTE: I will still be covering from A-Z how to interact with the API for purposes of this system, but the above documentation links may still prove helpful where I may fall short.* 

**Please visit the [Required Plugins](/required-plugins/) page in order to make sure all required plugins are activated before proceeding.**

### Preliminary note

In the following every URL refers to the final domain (ambassadorsystems.com). Where needed the staging URL (hometohome.staging.wpengine.com) will have to be considered.

This page will cover:

- [Authentication](#authentication)
- [User Schema](#user-schema)
- [Query All Users](#query-all-users)
- [Query User Information](#query-user-info)
- [Update User Metadata](#update-user-metadata)
    - [Ping the Correct Endpoint](#update-user-metadata-endpoint)
    - [Send the Correct Data](#update-user-metadata-data)
    - [The Full curl](#update-user-metadata-curl)
- [Zipcode Query](#zipcode-query)

&nbsp;

## <a name="authentication"></a>Authentication
***

&nbsp;

In efforts of security, basic authorization is required to update and read user metadata. The curl request must contain a header with the format:
 
`Authorization: Basic AUTH_KEY` where `AUTH_KEY` will be replaced with the authorization key. To obtain an authorization key, follow the steps outlined below:

1. Log into the website (ambassadorsystems.com) with your admin account.
2. Go to your edit profile screen `/wp-admin/profile.php`
3. Scroll down to the section titled "Application Passwords" `/wp-admin/profile.php#application-passwords`
4. Enter the "New Application Password Name" field and click "Add New". *Note: the name is arbitrary. Set it to something you will recognize in the future, but note that the name you enter is **not** the password itself, but just a label.*
5. A window will be presented containing the password. **Take note of it for later as you will not be able to see it again.**
6. Note that the password now exists in the table below and can be revoked if it is compromised or if you would like to create a new one on occassion.
7. Go into any terminal on your machine and enter the following command: `echo -n "USERNAME:KEY" | base64` where `USERNAME` is your WordPress username that you used to generate the password with (not your email, your username) and `KEY` is the generated password you just created. Take note of the echoed value in the terminal, this is your authorization key.

Now you have the `AUTH_KEY` to use in the headers mentioned above.

&nbsp;

## <a name="user-schema"></a>User Schema
***

&nbsp;

In all queries below, the user schema will be as follows:

```json
{
    id: (int),
    name: (string),
    url: (string),
    description: (string),
    link: (string),
    slug: (string),
    avatar_urls: (object),
    meta: (object),
    wp_user_avatar: (string),
    agent_ambassadors: (string),
    lender_ambassadors: (string),
    farm_zip_1: (string),
    farm_zip_2: (string),
    farm_zip_3: (string),
    farm_zip_4: (string),
    farm_zip_5: (string),
    Phone: (string),
    business_name: (string),
    Real_Estate_License: (string),
    Biography: (string),
    Brokerage: (string),
    Website: (string),
    Youtube_URL: (string),
    agent_user_id: (string),
    membership_level: (string),
    score: (string),
    cumulative_count: (string),
    weekly_count: (string),
    quarterly_count: (string),
    next_lender_id: (string),
    timestamp_of_last_update: (string),
    custom_preferred_zip: (string),
    agent_ambassador_default: (bool|string),
    lender_ambassador_default: (bool|string),
    wpm_useraddress: {
        company: (string),
        address1: (string),
        address2: (string),
        city: (string),
        state: (string),
        zip: (string),
        country: (string)
    },
    wpm_levels: {
        {level_ID}: {
            Level_ID: (string),
            Name: (string),
            Cancelled: (null|bool),
            CancelDate: (null|string),
            Pending: (null|bool),
            UnConfirmed: (null|bool),
            Expired: (bool),
            ExpiryDate: (bool),
            SequentialCancelled: (null|bool),
            Scheduled: (bool),
            ScheduleInfo: (mixed),
            ParentLevel: (mixed),
            Active: true,
            Status: [
                (string)
            ],
            Timestamp: (int),
            TxnID: (string)
        }
    },
    _links: {
        self: [
            {
                href: (string)
            }
        ],
        collection: [
            {
                href: (string)
            }
        ]
    }
}
```

&nbsp;

## <a name="query-all-users"></a>Query All Users
***

&nbsp;

In order to query information on all of the users, you will use the endpoint `/users/`.

`https://www.ambassadorsystems.com/wp-json/wp/v2/users/`

This endpoint will provide information on all users in segments. By default, this number is 10. You can view up to 100 users at a time by using the query parameter `per_page`. In order to get information on all users, you will need to paginate through the users with the query parameter `page`. The steps will most likely work as below:

1. Query `/users/?per_page=100`
2. Count returned users. If returned users are under 100, stop. If over 100, continue to step 3.
3. Query `/users/?per_page=100&page=n` where `n` represents the current page.
4. Repeat step 2 and 3 as needed until either user count is under 100 or is 0.

## <a name="query-user-info"></a>Query User Information

In order to query information on one user, you will use the endpoint `/users/USER_ID/`, where `USER_ID` is the ID of the user.

`https://www.ambassadorsystems.com/wp-json/wp/v2/users/USER_ID/`

`USER_ID` will need to be replaced with the user ID you want to query.

&nbsp;

## <a name="update-user-metadata"></a>Update User Metadata
***

&nbsp;

User metadata is distinct from the standard set of user data supported directly by WP REST API. Metadata are custom fields defined as user extensions to support the Ambassador Systems website’s unique functions such as linking between users and scoring user’s usage for the LAL (Lead Allocation Logic) system.

In order to update user metadata, you will need to use a curl request. Whether you use a library to do this or a direct bash command does not matter. What matters is that you send the proper headers, ping the correct endpoint, and send the correct data. Let's break this down into those 3 sections.

### <a name="update-user-metadata-endpoint"></a>Ping the Correct Endpoint

The endpoint used for updating user metadata is the same endpoint used for viewing a user's metadata: `/users/USER_ID/`. The difference is that you need to send this curl with the POST method, rather than with the GET method.

### <a name="update-user-metadata-data"></a>Send the Correct Data

The data you will need to send will be in the form of query parameters `KEY=VALUE` where `KEY` is the identifier of the metadata and `VALUE` is the data you wish to write. You may send multiple pieces at once by separating them with an ampersand "&". Like so `KEY1=VALUE1&KEY2=VALUE2&KEY3=VALUE3` etc. Each of these pieces of metadata will be written to the user **only if** that metadata has been pre-approved as writeable. Below is a list of writeable metadata, all other write attemps will be rejected.

- agent_user_id
- membership_level
- score
- cumulative_count
- weekly_count
- quarterly_count
- next_lender_id
- timestamp_of_last_update

As it currently stands, the only way to modify what metadata can be written is to either contact the system administrator to have edits made to the system codebase, or create a plugin on the website that modifies this information via the filter hook `ambassadorprofile_custom_user_rest_fields`. For more information on how to do this, please visit the [developer resources page](https://developer.wordpress.org/reference/functions/add_filter/).

### <a name="update-user-metadata-curl"></a>The Full curl

Below is what the full curl request would look like from terminal:

`curl --header "Authorization: Basic AUTH_KEY" -X POST https://www.ambassadorsystems.com/wp-json/wp/v2/users/USER_ID -d "KEY=VALUE"`

Where you make the following replacements:

| **AUTH_KEY:** | Your custom authorization key        |
| **USER_ID:**  | The user ID to write the metadata to |
| **KEY:**      | The metadata identifier key          |
| **VALUE:**    | The metadata write value             |

&nbsp;

## <a name="zipcode-query"></a>Zipcode Query
***

&nbsp;

In order to query users based on a zipcode, you will use a different endpoint. All endpoints referenced above exist appended to `/wp-json/wp/v2/`. The zipcode query uses endpoints appended to `/wp-json/ambassador/v1/`.

The endpoint for the zipcode query will look like: `/wp-json/ambassador/v1/network/TYPE/ZIP/` where `TYPE` is either `agent` or `lender` and `ZIP` is the zipcode you want to query. If you set "type" to "agents", then only agents will be returned in the result. If you set the "type" to "lenders", then only lenders will be returned in the result.

This will try to match users' zipcodes (farm zipcodes for agents, business zip code for lenders) to what you supply. It first searches for perfect matches. If it cannot find any, it will try to find matches of the first 4 digits. If it still cannot find any, it will simply send over the default agent/lender user ID. So the schema is:

```json
{
  'status'  => (string) {perfect_match, partial_match},
  'data'    => (array) {
    'platinum' => (array),
    'gold'     => (array),
    'silver'   => (array),
    'no_level' => (array),
  }
}
```

Note that "data" will contain a sorted list of user IDs when status is perfect_match or partial_match. If any users don't seem to belong to a level (should not be the case), they will be placed into the no_level key

"data" will simply be the default agent/lender user ID when the status is no_match:

```json
{
  'status' => (string) no_match,
  'data'   => (array) one user ID,
}
```