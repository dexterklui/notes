---
date: 2024-10-31 (Thu)
---

# User Administration

- `w` - show who is logged on and what they are doing
- `who` - show who is logged on, where they are logged in from, and when they
  logged in
- `whoami`
- `finger` - user information lookup program
- `users` - list users currently logged in
- `id` - print real and effective user and group IDs
  - `-u` user ID
  - `-g` group ID
  - `-G` all groups
  - `-n` name instead of number

## Users

- `usermod {option} [user]` - modify a user account
  - `-aG mygroup,myothergroup` adds supplementary groups of a user
  - `-G mygroup,myothergroup` sets (i.e. replaces) supplementary group list
  - `-c "description"` sets a comment about a user
- `useradd` create a new user or update default new user information
  - e.g. `useradd -m -d /home/robert -s /bin/bash robert`
  - `-m` create home directory if not exist (default is to respect `CREATE_HOME`
    setting defined `/etc/login.defs`, and if no settings in there, default is
    to create home directory for non-system users)
  - `-d {/path/to/home/dir}`
  - `-s {/path/to/login/shell}`
  - `/etc/default/useradd` defines default account attribute values for
    `useradd`
    - E.g. defines the path to the home directory skeleton
- `userdel {username}` deletes an entry in `/etc/passwd`
  - `-r` also remove home directory and mail spool

### Password

Encrypted passwords are stored in `/etc/shadow`, and `/etc/login.defs` defines
password policies.

When a password stored in `/etc/shadow` is prefixed with `!`, the account is
locked.

- `passwd {user}` change password for user robert
  - By default a password must be at least 6 characters long, with at least one
    letter and at least one number or symbol
  - `-l {user}` locks user account
  - `-u {user}` unlocks user account
  - `-S {user}` show password status
- password policies are stored in `/etc/login.defs` and
  `/etc/security/pwquality.conf`
- `chage [opt]... {user}`
  - `-l` list password expiration and account aging information

## Group

### Commands

- `groups [user]` list groups current or specified user belongs to
- `groupadd` creates new groups
  - `-g {GID}` group id
- `groupmod` modifies existing groups
- `groupdel` deletes groups
- `gpasswd` sets a group password and manages secondary group membership
  - `-a {user} {group}` add user to a group
  - `-M {user1,user2,...}` **set** the members of a group
- [See `usermod`](#users) to add groups to a user

### Types of groups

A user belongs to:

- One and only one primary group (in `/etc/passwd`)
- Up to 15 secondary groups (in `/etc/group`)
- Private group with only one member (the user)

## File Integrity

- `pwck` checks the integrity of the password and shadow files
- `grpck` checks the integrity of the group and gshadow files

## 🧭 Navigation

- [🔼 Back to top](#user-administration)
- [◀️ Back](unix-commands.md)
- [🔖 Parent index](../../index.md)
- [📑 Notes Index](../../index.md)
