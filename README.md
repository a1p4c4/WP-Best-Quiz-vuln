## Description

The WP Best Quiz WordPress plugin through 1.0 did not properly sanitise the text fields (category name) when creating or editing a quiz form, leading to an authenticated (author+) stored cross-site scripting issue. This could allow medium privilege accounts (such as author and editor) to perform XSS attacks against high privilege ones like administrator.

## Code Analysis

Line 80 of file add_categories.php,the insert_category() function use echo function to print out variable $_POST['qcategory'] which is inputed by user, and store it in the database.There are no filtering and verification measures in the whole process,leading to a stored Cross-Site Scripting (XSS) issue.

![image](https://user-images.githubusercontent.com/59782799/192555913-e1570355-a9a8-459c-b1a1-4a2b32a6097a.png)

## PoC

step1.Download the plugin from WordPress SVN and activate the plugin.

step2.Go to Quiz menu

![image](https://user-images.githubusercontent.com/59782799/192553789-a9f64861-e990-4919-9e09-c5dad33f3585.png)

step3.Add a category named like 

```
<script>alert(1)</script>
```

![image](https://user-images.githubusercontent.com/59782799/192554247-7dea9e38-6633-4ce0-83d5-4d8c0d31a33c.png)

step4.Then threre will be a pop-up,proving that the vulnerability exists.

![image](https://user-images.githubusercontent.com/59782799/192555063-68ca7281-f045-4a70-9852-d6aea45e24d1.png)

step5.Go to database,and the javascript code has been stored in database:

![image](https://user-images.githubusercontent.com/59782799/192557979-407b6cf1-f029-4525-bc37-79503a702809.png)

step6.Since the plugin can be accessed by a editor or author,it's possible to gain higher privilege for a lower privilege account.

## MISC

Source Code Link:https://downloads.wordpress.org/plugin/wp-best-quiz.zip

