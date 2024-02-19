### File Permissions Documentation

#### Octal Value, File Permissions Set, Permissions Description 

| Octal Value | File Permissions Set | Permissions Description |
|-------------|----------------------|-------------------------|
| 0           | ---                  | No permissions          |
| 1           | --x                  | Execute permission only |
| 2           | -w-                  | Write permission only   |
| 3           | -wx                  | Write and execute permissions |
| 4           | r--                  | Read permission only   |
| 5           | r-x                  | Read and execute permissions |
| 6           | rw-                  | Read and write permissions |
| 7           | rwx                  | Read, write, and execute permissions |

#### File Permission Pattern
One notable pattern is observed in the incremental process of setting file permissions, following a binary representation:
- Imagine a set of three binary place values, filled from right to left.
- When assigning a value to indices with existing values to the left, those values to the left are retained.
- When assigning a value to indices with existing values to the right, those values to the right are dropped.

#### Example:

| Octal Value | Example | Binary (Permissions) | Notes                        |
|-------------|---------|----------------------|------------------------------|
| 0           | ---     | 000                  | Initial state                |
| 1           | --x     | 001                  | First increment              |
| 2           | -w-     | 010                  | Middle index filled, values to the right dropped |
| 3           | -wx     | 011                  | Right-most index filled, values to the left retained |
| 4           | r--     | 100                  | Left-most index filled, values to the right dropped |
| 5           | r-x     | 101                  | Right-most index filled, values to the left retained |
| 6           | rw-     | 110                  | Middle index filled, values to the right dropped |
| 7           | rwx     | 111                  | Right-most index filled, values to the left retained |
This results in a total of 8 possible states, making octal (0-7) a suitable method for representing any state. When setting permissions for a file or directory in Linux, the octal format is used to express permissions for all three (read, write, and execute) via a single value.

#### Usage in Linux File System
Linux file system commands utilize a set of three octal values: 0-7 0-7 0-7.
For instance, 644.
Each position out of the three characters represents a user category to apply the permissions to:
- Position one (x--): File owner
- Position two (-x-): Groups
- Position three (--x): Everyone else

#### Permissions Explanation
- Permissions applied to the owner: Permissions that the file or directory owner is allowed.
- Permissions applied to groups: Permissions for any groups with access to the file or directory.
- Permissions for everyone: Permissions for all other users who are not the owner or in the designated groups.

#### Access Control
Owner: The file or directory owner's permissions are determined by the first position (e.g., x--) in the permission set. The owner typically has the highest level of control over the file or directory, including the ability to read, write, and execute files. 
Groups: Group permissions are represented by the second position (e.g., -x-) in the permission set. All groups associated with the file or directory share the same set of permissions. Administrators manage group permissions using commands like chgrp to change group ownership or chmod to adjust permissions. Multiple groups can be associated with a file or directory, with each group having the same permission level set by the second position. 
Everyone: Permissions for all users not the owner or members of designated groups are governed by the third position (e.g., --x) in the permission set. This category encompasses any user not part of the owner or any associated groups.
#### Examples
1. **Setting Permissions for a Shared Directory:**
   Suppose you have a directory named "shared" that needs to be accessed by multiple users in a group called "team." You can set the permissions as follows:
   ```
   $ chmod 770 shared
   ```
   This command gives the owner full permissions (rwx), allows the group "team" to have full permissions (rwx), and denies all permissions to others.

2. **Restricting Access to Sensitive Files:**
   Let's say you have a file named "confidential.txt" containing sensitive information. You want to ensure that only the owner can read and modify the file. You can set the permissions as follows:
   ```
   $ chmod 600 confidential.txt
   ```
   This command gives the owner read and write permissions (rw-), while denying all permissions to groups and others.
