---
title: How to standardize author's name on Zotero
subtitle: Changing multiple author entries at the same time
date: 2022-01-12T10:09:41-05:00
summary: ""
draft: false
featured: true
authors:
  - admin
lastmod: 2022-01-12T10:09:41-05:00
tags:
  - zotero
categories: 
projects: []
image:
  caption: ""
  focal_point: ""
  preview_only: false
---
I often forget how to do this, so this post is primarily for my own memory, and it might be helpful for someone else. 

Zotero is amazing, but there is still space for improvement. One of these instances is when we have multiple entries for the same authors. The author name has to be exactly the same for all these entries. As expected, Zotero will treat them as two different authors if they are slightly different. For instance, if you have two of my publications, one of them as Ruam Pimentel and the other as R. Pimentel. Zotero will treat them as different authors, so when citing these studies on Zotero, they will put the authors' first name (i.e., Ruam or R.) in the in-text citation to differentiate both authors. There is no way to create an alias for the authors, and tell Zotero that all of these authors would be the same Author:

* Ruam Pimentel
* R. Pimentel
* Ruam P F A Pimentel
* R. P. F. A. Pimentel
* Ruam Pedro Francisco de Assis Pimentel

Yeah, I know, I have a big name. And I want Zotero to recognize all of my names as me. Anyway. But there is a workaround. 

After reading different discussion boards from Zotero's amazing community ([Discussion 1](https://forums.zotero.org/discussion/16547/bulk-standardize-author-names), [Discussion 2](https://forums.zotero.org/discussion/64353/first-name-appearing-in-in-text-citation) ), here is the workaround that Zotero offers for now using JavaScript. Hold on. You can do it directly from the Zotero interface. No big deal. 

This code was extracted from Zotero's documentation: [Click here to see Zotero's Original Documentation](https://www.zotero.org/support/dev/client_coding/javascript_api)

To accomplish that you will have to edit and run the following code from Zotero's JavaScript interface. I will explain how to that step by step after the code:

## Code from [Zotero's Documentation](https://www.zotero.org/support/dev/client_coding/javascript_api)

### JavaScript

```jsx
var oldName = "R. P. F. A. Pimentel";
var newFirstName = "Ruam P. F. A.";
var newLastName = "Pimentel";
var newFieldMode = 0; // 0: two-field, 1: one-field (with empty first name)
 
var s = new Zotero.Search();
s.libraryID = ZoteroPane.getSelectedLibraryID();
s.addCondition('creator', 'is', oldName);
var ids = await s.search();
if (!ids.length) {
    return "No items found";
}
await Zotero.DB.executeTransaction(async function () {
    for (let id of ids) {
        let item = await Zotero.Items.getAsync(id);
        let creators = item.getCreators();
        let newCreators = [];
        for (let creator of creators) {
        	if (`${creator.firstName} ${creator.lastName}`.trim() == oldName) {
        		creator.firstName = newFirstName;
        		creator.lastName = newLastName;
        		creator.fieldMode = newFieldMode;
        	}
        	newCreators.push(creator);
        }
        item.setCreators(newCreators);
        await item.save();
    }
});
return ids.length + " item(s) updated";
```

## How to run JavaScript from Zotero and update Author's name

Open Zotero > in the top menu, click on "Tools" > Select "Developers" > Select "Run JavaScript" > Make sure that "Run as async function" is not selected > Copy, edit, end paste the syntax below in the "Code:" area.

Here are two important parts:
You have to edit this syntax.
You have to run this syntax for each change (yes, one at a time). 

Using the same examples of my name, I want to substitute all my different names for just the "correct" name entry that will appear in all my publications. For that, I have to edit lines 1, 2, 3, and 4. I will set lines 2, 3, and 4 for what I want as output, and then I will change line 1 multiples times and run it multiple times. Here is how it looks. 

Edit lines 2, 3, and 4 to match your desirable outcome. I would like that my name appears as Ruam P. F. A. Pimentel in my Zotero files.
Line 2 will have only my "first name" (in this case, it is everything but my last name): Ruam P. F. A.
Line 3 will have only my "last name": Pimentel
Line 4 sets how I would like to see my name in the files, Last name first (0: two-field), or my whole name in sequence (one-field). I like to see the last name first, hence I will select "two-field" (0)
Here is how this part of the syntax looks like:

```javascript
var newFirstName = "Ruam P. F. A."; var new
LastName = "Pimentel"; var new
FieldMode = 0; // 0: two-field, 1: one-field (with empty first name)
```

Finally, in the first line, I will put which names I would like to change to the new format. So I will put all the entries I mentioned above and run the entire syntax. Again, I have to run the syntax again for each entry. So in this case, I will run the syntax five times, and for each time I will change only the first line!
here is the examples of all my first line:

```javascript
var oldName = "Ruam Pimentel";
var oldName = "R. Pimentel";
var oldName = "Ruam P F A Pimentel";
var oldName = "R. P. F. A. Pimentel";
var oldName = "Ruam Pedro Francisco de Assis Pimentel";
```