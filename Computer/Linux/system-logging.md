---
date: 2024-11-01 (Fri)
---

# System Logging

## Auditing User Logins

Both `lastlog` and `last` get information from the binary file `/var/log/wtmp`
which records all login and logout details. The binary file `/var/log/btmp`
records all failed login attempts, which is read by `lastb`.

- `$ lastlog` shows the last login details (e.g. time, port) for each users
  - `-b {DAYS}` (`--before`) print only records older than DAYS
  - `-t {DAYS}` (`--time`) print only records more recent than DAYS
- `$ last` shows booting, shutdown and login history and details
  - Argument `{user}` shows the history of the user
- `# lastb` shows failed login attempts

## 🧭 Navigation

- [🔼 Back to top](#system-logging)
- [◀️ Back](unix-commands.md)
- [🔖 Parent index](../../index.md)
- [📑 Notes Index](../../index.md)
