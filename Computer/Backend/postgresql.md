---
date: 2023-11-27 (Mon)
---

# PostgreSQL

## Architecture

- Server-client model
- Every operation is made with a role

## Commands

From shell, run `psql` as a postgres user (e.g. `sudo -u postgres psql` to run
as the default postgres root user) to enter the database shell. If no `sudo` is
available, you can run `psql -U postgres`.

Or try `psql DBNAME USERNAME`.

### Privileges

Some of the command line tools required you to have different privileges.
Usually the "root" user with superuser privileges is `postgres` user. You can
switch to that with `sudo su`.

### Users

- `createuser` creates a new user
  - You can use `--interactive` flag
- When running PostgreSQL commands, you can use `-U myuser` to specify a
  PostgreSQL user name to run the command as.
- You can also set the `PGUSER` environment variable to specify the user name.

### Databases

- Many tools assumes the default database name to be the same as the user name.
- `createdb mydb` creates a new database.
- `dropdb mydb` drops a database. (This doesn't have a default database name)
  - This deletes all associated files and **cannot be undone**.
- `psql -l` lists all databases.
- `psql mydb` connects to a database and enters the interactive shell.

### Management Commands

- `\c <database>` connects to a database.
- `\conninfo` shows info about current connection
- `\l` lists all databases.
- `\dt` lists tables in the current database.
- `\d [target]` lists tables, views or sequence, or describe the given table,
  view, sequence, or index.
- `\di` list indexes (primary keys?)

## Interactive shell

- Prompt: `mydb=>` or `mydb=#` if you are a superuser.
- You can type SQL queries by ending with a semicolon.

### Internal commands

- `\q` or `CTRL+D` exists the shell.
- `\h` lists all SQL commands.
- `\?` lists all internal commands.

## References and Links

- [Arch Wiki](https://wiki.archlinux.org/index.php/PostgreSQL)
- [PostgreSQL Docs](https://www.postgresql.org/docs/current)

## 🧭 Navigation

- [🔖 Parent index](../../index.md)
- [📑 Notes Index](../../index.md)
