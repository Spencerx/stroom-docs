---
title: "Document Permissions"
linkTitle: "Document Permissions"
weight: 50
date: 2024-11-01
tags:
  - permission
  - authorisation
  - content
  - document
description: >
  Assigning document level permssions (such as 'View') to users or groups.
---

{{% todo %}}
The Users, Groups and Permissions screens are undergoing significant change in Stroom v7.6.
Therefore this section will be updated with more detail in v7.6.
{{% /todo %}}

Document Permissions are permissions that are granted to [Users or Groups]({{< relref "users-and-groups" >}}) for a specific {{< glossary "Document" >}}.
They control what documents a user/group can see and what they can do to those documents.
They allow very fine grained control over what a user/group can see or do in Stroom.

For example, User _jbloggs_ may be granted `Use` permission on the Index {{< stroom-icon "document/Index.svg">}} named `Alert Index` in order for him to be able to query that index in a dashboard, but not be able to see it in the explorer tree or change it in any way.

By default a new user with no [Application Permissions]({{< relref "app-permissions" >}}), Document Permissions or Group memberships **cannot** view/use/modify **any** documents.
They do not even have permission to create any documents.
When logging into Stroom, they will simply see an empty explorer tree.

A user can gain varying levels of access to documents in a number of ways:

* Being added to a Group that has direct or inherited permissions on one or more existing documents.
* Being added to a Group that has direct or inherited permissions to create one or more document types.
* Being directly granted permissions one or more existing documents.
* Being directly granted the permission to create one or more document types.
* Being granted the `Administrator` [Application Permission]({{< relref "app-permissions" >}}) which gives them access to **ALL** documents.

In order to modify the permissions on a document, you must either hold `Owner` permission on the document or have the `Administrator` Application Permission.
The Document Permissions screen for a document/folder can be accessed by right clicking on it in the explorer tree and selecting:

{{< stroom-menu "Permissions" >}}


## Permission Types

The following is the list of different permissions that can be granted to users/groups on a document.

| Permission | Description                                                                                                                                                                |
| ---------- | -----------                                                                                                                                                                |
| **Owner**  | Same as delete plus ability to change the document's permissions (i.e. grant permissions on this document to other users/groups.                                           |
| **Delete** | Same as edit plus permission to delete the document.                                                                                                                       |
| **Edit**   | Same as view plus permission to edit, move, rename or add tags to the document.                                                                                            |
| **View**   | Permission to see the document in the explorer tree, open it as read-only, copy it or export it (subject to also having the `Export Configuration` application permission. |
| **Use**    | Only allow use of a document, e.g. allow use of an index as part of a search process but do not allow viewing of the document itself.                                      |

The following is the list of different permissions that can be granted to users/groups on a folder {{< stroom-icon "document/Folder.svg" >}}.

| Permission | Description                                                                                                                                                                |
| ---------- | -----------                                                                                                                                                                |
| **Owner**  | Same as delete plus ability to change the folder's permissions (i.e. grant permissions on this folder to other users/groups.                                           |
| **Delete** | Same as edit plus permission to delete the folder.                                                                                                                       |
| **Edit**   | Same as view plus permission to edit, move, rename or add tags to the folder.                                                                                            |
| **View**   | Permission to see the folder in the explorer tree (and it's child items that you also have _View_ permission on), open it as read-only, copy it or export it (subject to also having the `Export Configuration` application permission. |
| **Use**    | Only allow use of a folder, e.g. allow use of an index as part of a search process but do not allow viewing of the folder itself.                                      |




### Implied Permissions

Note that each permission in the two tables above also includes all the permissions below it in the table, e.g. a user with `Edit` permission on a document will also have the implied permissions `View` and `Use`.
There is no need to grant these lower permissions to the user, though doing so will have no impact as Stroom will user the highest value permission when checking permissions.


### Inherited Permissions

If a User _jbloggs_ is a member of Group _Team A_ and that group is a member of group _Division 123_, then _jbloggs_ will inherit all permissions from both _Team A_ and _Division 123_.
A User/Group will inherit all permissions of the groups that they are a member of and also from any ancestor groups of those groups.

| User/Group | Permissions | Direct/Inherited |
| --- | --- | --- |
| _Division 123_ | **View** on Dictionary _IP Allow List_ | Direct
| _Team A_ | **View** on Dictionary _IP Allow List_ | Inherited
| _Team A_ | **Owner** on Dashboard _Team Dashboard_ | Direct
| _jbloggs_ | **View** on Dictionary _IP Allow List_ | Inherited
| _jbloggs_ | **Owner** on Dashboard _Team Dashboard_ | Inherited
| _jbloggs_ | **View** on Dashboard _Frank's Dashboard_ | Direct


### `Owner` Permission

A document can have multiple owners.
An owner can be a user or a group.
When a document is created by a user they are automatically made an owner of it.
Any user with the `Administrator` role has implied ownership of **ALL** documents.

Having `Owner` permission on a document means the user can grant permissions on that document to other users, or revoke permissions from other users.


### `Use` Permission

This permission allows users to access a document but not actually see it in the explorer tree or open the document in Stroom.
They can however make use of the document, e.g. selecting and querying an Index {{< stroom-icon "document/Index.svg" >}} in a Dashboard {{< stroom-icon "document/Dashboard.svg" >}}.

The `Use` permission is not relevant to all document types.


## Permissions on Folders

Folders {{< stroom-icon "document/Folder.svg" >}} in the explorer tree work mostly in the same way as documents when it comes to permissions.
There are a couple of exceptions to this.


### Permission on Folder Contents

The permissions on a folder apply **only** to the folder itself and has no bearing on what you can/can't do to its child items.
The permissions on each child item in the folder control what you can/can't do to those items.

For example, if you only have _View_ permission on a folder, but have _Delete_ on a document in that folder, then you are able to delete that document and thus change the contents of the folder.

Similarly, if you have _View_ permission on a folder but have no permission on any of its child items, then you will just see an empty folder.


### Ancestor Folder Visibility

A folder {{< stroom-icon "document/Folder.svg" >}} will be visible to a user in the explorer tree if the user has _View_ permission on it **OR** if the user has _View_ permission on any single document/folder that is a descendant of it.

For example, if a user has _View_ permission on a Dictionary {{< stroom-icon "document/Dictionary.svg" >}} _Dictionary_XYZ_ with path

**System / Folder_A / Folder_B / Dictionary_XYZ**

but **no** permissions on _Folder A_ or _Folder B_, they will be able to see both Folders in the explorer tree in addition to the Dictionary.
They will however not be able to open those Folders as they do not have the permission.

Therefore, when granting permissions on a document/folder to a user/group, you are also implicitly granting visibility (but not _View_ permission) on all ancestor folders.


### Create Permissions

Folders can have one or more _Create Permissions_ granted on them to users/groups.

There is a _Create Permission_ for each document type, e.g. Index, Dictionary, Feed, etc.
A _Create Permission_ is the ability to create a new document of that type in that folder.

For example, user `jbloggs` is an analyst and is granted `Create Dashboard` and `Create Query` permissions on the Folder {{< stroom-icon "document/Folder.svg" >}} named `Joe's Folder`.
This means Joe can only create Dashboard {{< stroom-icon "document/Dashboard.svg" >}} or Query {{< stroom-icon "document/Query.svg" >}} documents in that folder and nothing else.


### Applying Changes to Descendants

When making changes to the permissions on Folder {{< stroom-icon "document/Folder.svg" >}} you have the option of making the changes to just that folder or to all descendants of that folder.
Selecting to apply to all descendants will make all permission changes apply to every descendant, i.e. including any sub-folders and their contents or own sub-folders.


## Moving and Copying Documents

When you move {{< stroom-icon "move.svg" "Move">}} or copy {{< stroom-icon "copy.svg" >}} a document/folder you have the choice of how the destination document/folder's permissions should be derived.
The move/copy dialog offers the following choices:

* **None** - Removes all current permissions.
    Ignores permissions of the destination folder.
    You will be the owner of the moved document if not already.
* **Source** - Keep the current permissions and ownership as they are.
* **Destination** - Removes all current permissions.
    Adds the permissions of the destination folder.
    You will be the owner of the moved document if not already.
* **Combined** - Keep the current permissions and add the permissions
    of the destination folder. You will be the owner of the moved document if not already.

{{% note %}}
You must have `Owner` permission on the source document/folder (or `Admistrator` {{< glossary "Application Permission" >}}) if you wish to use _None_, _Destination_ or _Combined_ as these all involve a change of permissions.
{{% /note %}}

