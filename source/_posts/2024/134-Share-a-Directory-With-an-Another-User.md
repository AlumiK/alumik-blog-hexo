---
title: Share a Directory With an Another User
standard: 1
date: 2024-07-01 01:03:03
categories: Linux 系统和软件
tags:
abbrlink: 134
references:
  - https://askubuntu.com/questions/313089/how-can-i-share-a-directory-with-an-another-user
  - https://help.ubuntu.com/community/Bindfs-SharedDirectoryLocalUsers
---
The classic Linux way of doing this sort of thing goes something like this:

Create the shared folder:

```bash
sudo mkdir /home/shared
```

Create the new user's group:

```bash
sudo addgroup sharespace
```

Change ownership of the shared folder to the new group:

```bash
sudo chown :sharespace /home/shared
```

Add your desired users to that group:

```bash
sudo adduser user1 sharespace
```

Repeat for all users.

Now you have some decisions to make about what you want those users to be able to do:

1. All group users can add to and delete from the folder and can read and but not write to each others files:

    ```bash
    sudo chmod 0770 /home/shared
    ```

2. Same as above but only the owner of the file can delete it:

    ```bash
    sudo chmod 1770 /home/shared
    ```

3. All group users can add to and delete from the folder and can read and write to each other's files:

    ```bash
    sudo chmod 2770 /home/shared
    ```

4. Same as choice 3 except only the owner of the file can delete it:

    ```bash
    sudo chmod 3770 /home/shared
    ```

A `1` in the first position of the chmod command is the sticky bit which prevents deletion of a file to anyone other than the owner.

A `2` in the first position of the chmod command is the setgid bit which forces all new or copied files to have the group of that folder.

A `3` in the first position of the chmod command is the combination of the sticky (`1`) & setgid (`+2`) bits.

{% note warning %}
There is one caveat to all this as far as the setgid bit is concerned.
All new files created in and any files copied to that folder will in fact inherit the group of the folder.
But not files moved to that folder.
Moved files retain the ownership from wherever they were moved from.
One way to get past this problem is to use bindfs.
{% endnote %}

Finally if you want others outside the group to be able to see the files but not change them change the final `0` in the chmod command to a `5`:

```bash
sudo chmod 0775 /home/shared
```
