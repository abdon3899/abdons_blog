---
title: "Linux Forensics"
description: "My personal SOC notes and research on Linux Forensics."
publishDate: "2026-04-19T00:00:00Z"
tags: ["SOC"]
coverImage: "./Gifs/linux.gif"
---

# Linux Forensics

## OS and account information:

in windows every thing is saved in requesters here in Linux ever thing is save on file .

- **OS info:** can be found at ****`/etc/os-release`
- **User accounts:**  can be found at ****`/etc/passwd`  password is saved at `/etc/shadow`
- **Group Information:** can be found at ****`/etc/group`
- **Sudoers List:**  can be found at ****`/etc/sudoers`
- **Login information:** can be found at ****`/var/log/btmp or wtmp` btmp for failed logins, wtmp keeps historical data of logins. use `last` command
- **Authentication logs:** can be found at ****`/var/log/auth.log` you can use `tail ,head ,more`

## System Configuration:

- **Hostname:**  found at ****`/etc/hostname`
- **Timezone:** found at ****`/etc/timezone`
- **Network Configuration:**  found at ****`/etc/network/interfaces` might need to us `ip`
- **Active network connections:** use the `netstat` command
- **Running processes:** use the `ps` command
- **DNS information:** found at ****`/etc/hosts`  and `/etc/resolv.conf`  for the Linux host

## Persistence mechanisms:

 ways a program can survive after a system reboot. This helps malware authors retain their access to a system even if the system is rebooted. 

- **Cron jobs:** found at ****`/etc/crontab`
- **Service startup:** os startup services found at `/etc/init.d`
- **.Bashrc:** When a bash shell is spawned, it runs the commands stored in the `.bashrc` file.
- **System-wide settings:** stored in `/etc/bash.bashrc` and `/etc/profile`

## Evidence of Execution:

is to know if a program did run on this computer or not 

- **Sudo execution history:** All the commands that ran using `sudo` are stored in `/var/log/auth.log`
- **Bash history:** any non sudo commands is stored away form the user home folder , in**`.bash_history`**
- **Files accessed using vim:** found in ****`.viminfo`

## Log files:

maintain a history of activity performed on the host and the amount of logging depends on the logging level defined on the system. found at`/var/log` 

- **Syslog:**  messages that are recorded by the host about system activity. found at `/var/log/syslog`
- **Auth logs:** information about users and authentication-related logs. found at `/var/log/auth.log`
- **Third-party logs:**  logs for apps like webserver, database, or file share server logs.found at `/var/log`
