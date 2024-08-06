---
date: 2023-12-01 (Fri)
---

# MySQL

## Start Server

In Arch Linux, start the server through `systemctl start mariadb.service`. You
can also enable it to start on boot automatically.

## CLI

- `mysql` (or `mariadb` for MariaDB).
  - `mysql -u root -p` Login as root user and prompt for password
  - `-D <database>` selects a database

## Commands

- `show databases;`
- `source <sql_file>` Execute SQL file
- `DESCRIBE <table>;` show table schema

## Settings

- `SET SQL_SAFE_UPDATES = 0;` Allow updates without a `WHERE` clause, also allow
  updates with a subquery
- `SET FOREIGN_KEY_CHECKS = 0;` Disable foreign key checks

## 🧭 Navigation

- [🔖 Parent index](../../index.md)
- [📑 Notes Index](../../index.md)
