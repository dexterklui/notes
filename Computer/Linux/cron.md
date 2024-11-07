---
date: 2024-11-05 (Tue)
---

# Cron

## Mechanism

`crond` checks every minute if there are any cron jobs to run.

## Crontab Command

- `crontab -l` list the content of current user crontab, can be used to check if
  crontab file exists for current user
- `crontab -e` edits the user's crontab file
  - Note that for root user, running `crontab -e` edits the root user's crontab
    file, which is different from the system crontab file `/etc/crontab`
- `crontab -r` removes the user's crontab file

## Crontab File

### Crontab File Location

- For most distro: `/var/spool/cron/crontabs/<username>`
- RHEL based distro: `/var/spool/cron/<username>`

### Crontab Format

Each line in the crontab file is a cron job, with the following fields

- Minute (0-59)
- Hour (0-23)
- Day of the month (1-31)
- Month (1-12)
- Day of the week (0-7, 0 and 7 are both Sunday), or (`sun` - `sat`)
- user (optional)
- command

For the first five fields, you can use the following special characters, or
special strings:

- `,` separates multiple values
- `*` means all possible values

| Special String | Meaning                           |
| -------------- | --------------------------------- |
| `@reboot`      | Run once at system start up       |
| `@yearly`      | Run once every year, `0 0 1 1 *`  |
| `@annually`    | Same as `@yearly`                 |
| `@monthly`     | Run once every month, `0 0 1 * *` |
| `@weekly`      | Run once every week, `0 0 * * 0`  |
| `@daily`       | Run once every day, `0 0 * * *`   |
| `@midnight`    | Same as `@daily`                  |
| `@hourly`      | Run once every hour, `0 * * * *`  |

### Examples of crontab

```crontab
5 0 * * * root $HOME/bin/daily.job
15 2 1 * * root $HOME/bin/monthly.job
0 22 * * 0 root /etc/shutdown -r
```

## Limitation and Other Programs

- Not possible to define all possible periods of time, e.g. last week of the
  month.
- Only specific time can be defined, so if the server is down during that time,
  the job won't run. So you cannot specify the job to be run every week, but as
  soon as possible after the server is up.
  - Use `anacron` for this purpose
- To schedule one-off jobs, use `at` command, with `atd` service

### At Command

Use `at <date/time>`, then type each command on separate lines, and `<Ctrl-D>`
to finish

- `at -l` list all at jobs
- `at -r <job-id>` remove an at job

## 🧭 Navigation

- [🔼 Back to top](#cron)
- [◀️ Back](../../index.md)
- [🔖 Parent index](../../index.md)
- [📑 Notes Index](../../index.md)
