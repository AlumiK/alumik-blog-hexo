---
title: Make a Program Continue to Run After Log Out From SSH
date: 2024-03-22 14:30:43
categories: Linux 系统和软件
tags:
abbrlink: 133
standard: 1
references:
  - https://stackoverflow.com/questions/954302/how-to-make-a-program-continue-to-run-after-log-out-from-ssh
---
Assuming that you have a program running in the foreground, press <kbd>Ctrl</kbd>+<kbd>Z</kbd>, then:

```
[1]+  Stopped                 myprogram
$ disown -h %1
$ bg 1
[1]+ myprogram &
$ logout
```

If there is only one job, then you don't need to specify the job number.
Just use `disown -h` and `bg`.

Explanation of the above steps:

You press <kbd>Ctrl</kbd>+<kbd>Z</kbd>.
The system suspends the running program, displays a job number and a "Stopped" message and returns you to a bash prompt.

You type the `disown -h %1` command (here, I've used a 1, but you'd use the job number that was displayed in the Stopped message) which marks the job so it ignores the `SIGHUP` signal (it will not be stopped by logging out).

Next, type the `bg` command using the same job number; this resumes the running of the program in the background and a message is displayed confirming that.

You can now log out and it will continue running.
