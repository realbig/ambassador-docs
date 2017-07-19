---
layout: page
title: Shortcodes
permalink: /shortcodes/
---

There are some custom shortcodes within the Ambassador system. They are detailed below.

Note: All parameter names are case-insensitive.

This page will cover:

* TOC
{:toc}

## Ambassador Tool

This shortcode is used to display "tools" in the form of "embed codes".

`[ambassador_tool]`

**Parameter List**
- tool_name: Name of tool to lookup in Tool Catalogue
- keys: "true" or "false". If "true", `key_0` and `key_1` will be replaced in the embed code with the current keys
- tool_lookup_{n}: {n} is any digit, arbitrary. "tool_name:replacement_variable". "tool_name" is tool name to lookup in tool catalogue and "replacement_variable" is the name of the variable to replace in the embed code.
- user_id_{type}: {type} is client, agent, or lender. `user_id` variable in embed code is replaced with user ID from active entry based on type set. If {type} is omitted, and parameter is just `user_id`, the currently logged in user's ID is used
- {anything_else}: Every other parameter will simply replace in the embed code the parameter name as the variable name and the parameter value as the replacement value

## Ambassador User Field

This shortcode will output a user meta field based on some parameters.

`[ambassador_userfield]`

**Parameter List**
- type: "client", "lender", or "agent". The user to use from the active entry. Omit to use currently logged in user
- field: User meta field to output.
- avatar_size: If "field" is set to "avatar", this can be used to set the size to output. Options: "full", "medium", "thumbnail" (defaults to "thumbnail")
- error_message_active_record: If used, the parameter value will be output as an error message if no active entry is selected
