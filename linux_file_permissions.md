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

| Octal Value | Example | Binary Representation | Notes                        |
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
- Position two (-x-): Associated group
- Position three (--x): Everyone else

#### Permissions Explanation
- Permissions applied to the owner: Permissions that the file or directory owner is allowed.
- Permissions applied to groups: Permissions for the group associated with the file or directory.
- Permissions for everyone: Permissions for all other users who are not the owner or in the designated groups.

#### Access Control
Owner: The file or directory owner's permissions are determined by the first position (e.g., x--) in the permission set. The owner typically has the highest level of control over the file or directory, including the ability to read, write, and execute files. 
 
Group permissions are represented by the second position (e.g., -x-) in the permission set. The file or directory has one associated group, and all members of this group share the same set of permissions. Administrators manage group permissions using commands like chgrp to change group ownership or chmod to adjust permissions to the named file or directory for all members of its assigned group.

Everyone: Permissions for all users not the owner or members of the designated group are governed by the third position (e.g., --x) in the permission set. This category encompasses any user not part of the owner or any associated groups.

#### Examples
1. **Setting Permissions for a Shared Directory:**
   Suppose you have a directory named "shared" that needs to be accessed by all users in a group called "team." Here's how you can set the permissions and associate the "team" group with the "shared" directory:
   ```
   # Create the "shared" directory
   $ mkdir shared
   
   # Associate the "team" group with the "shared" directory
   $ chgrp team shared
   
   # Set the permissions to allow the owner full access, the "team" group full access, and deny all access to others
   $ chmod 770 shared
   ```
   This series of commands first creates the "shared" directory and then associates the "team" group with it using the `chgrp` command. Finally, it sets the permissions using the `chmod` command. The owner is given full permissions (7, AKA rwx), and the "team" group is also given full permissions (7, AKA rwx).

2. **Restricting Access to Sensitive Files:**
   Let's say you have a file named "confidential.txt" containing sensitive information. You want to ensure that only the owner can read and modify the file. You can set the permissions as follows:
   ```
   $ chmod 600 confidential.txt
   ```
   This command gives the owner read and write permissions (rw-), while denying all permissions to groups and others.
