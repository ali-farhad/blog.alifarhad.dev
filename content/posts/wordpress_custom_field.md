---
author: ["alifarhad"]
title: "How to Add Custom Fields to Custom Post Types with ACF in Wordpress?"
date: "2024-09-17"
description: "Learn how to enhance your WordPress site by adding custom fields to custom post types using the Advanced Custom Fields (ACF) plugin."
summary: "Learn how to enhance your WordPress site by adding custom fields to custom post types using the Advanced Custom Fields (ACF) plugin."
tags: ["WordPress", "Custom Fields", "Wordpress Theme Development"]
categories: ["WordPress"]
series: ["Wordpress Theme Development"]
ShowToc: true
TocOpen: false
---

## Introduction

Custom fields in WordPress allow you to add additional metadata to your posts, pages, or custom post types.This can be incredibly useful for adding extra information to your content. In this tutorial, we'll walk you through how to add custom fields to a custom post type using the Advanced Custom Fields (ACF) plugin and how to display that data on the frontend.

## Prerequisites

- A WordPress site
- The Advanced Custom Fields (ACF) plugin installed and activated
- A custom post type already created

## Step 1: Create Custom Fields with ACF

1. **Install and Activate ACF Plugin**

   - Go to `Plugins > Add New` in your WordPress dashboard.
   - Search for "Advanced Custom Fields".
   - Install and activate the plugin.

2. **Create a Field Group**

   - Navigate to `Custom Fields > Add New`.
   - Enter a title for your field group.
   - Click on `Add Field` to start adding custom fields.

3. **Configure Your Fields**

   - For each field, you can set the field label, name, type, and other settings.
   - Add as many fields as you need.

4. **Attach Field Group to Custom Post Type**
   - In the `Location` box, set the rules to show this field group if `Post Type` is equal to your custom post type.
   - Publish the field group.

## Step 2: Add Custom Fields to Custom Post Type

1. **Edit Your Custom Post Type**

   - Go to `Custom Post Type > Add New` or edit an existing post.
   - You should see the custom fields you created with ACF.

2. **Enter Data in Custom Fields**
   - Fill in the custom fields with the relevant data.
   - Save or update the post.

## Step 3: Display Custom Field Data on the Frontend

1. **Edit Your Theme Files**

   - Open the template file where you want to display the custom field data. This could be `single-{post_type}.php` or another relevant template file.

2. **Fetch and Display Custom Field Data**

   - Use the `get_field()` function provided by ACF to fetch the custom field data.
   - Example code to display a custom field:

   ```php
   <?php
   // Fetch custom field data
   $custom_field_value = get_field('custom_field_name');

   // Check if the custom field has a value
   if ($custom_field_value) {
       echo '<p>' . esc_html($custom_field_value) . '</p>';
   }
   ?>
   ```

## FootNotes

Having the ability to add custom fields into the wordpress meta data is incredibly powerful. Happy Learning!
