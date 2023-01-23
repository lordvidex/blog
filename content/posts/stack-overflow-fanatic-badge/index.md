---
title: Getting the Fanatic badge on Stack Overflow the PRO way
date: 2022-03-16T13:48:38+03:00
draft: false
cover:
  image: images/fanatic-badge.png
  alt: Fanatic Badge Image
  relative: true
weight: 1
summary: Automating your browser with “cron” and shell scripts ( Linux | MacOS )
author: Evans Owamoyo
description: '"Automating your browser with “cron” and shell scripts ( Linux | MacOS )"'
categories: [Scripting]
tags:
  - sh
  - stack-overflow
---

Have you ever striven to get the Fanatic gold badge on stack overflow? **OR** Has your progress reset when you were on the 99th streak? **OR** Have you ever wanted to automate your browsers to open a website periodically?

THEN, this article might help you !!

## Introduction

The Fanatic badge is earned by visiting the [stack overflow](https://stackoverflow.com/) website consecutively for 100 days.

## Problem

Visiting the stack overflow website routinely can prove difficult if you have no need to visit the website for just one day, or when you are on a code break / vacation.

## Solution

I solved this issue by creating a [cron](https://en.wikipedia.org/wiki/Cron) job that opens my stack overflow profile while my system is on.

I will provide two solutions for Mac OS Monterey ([here]({{< ref "#-mac-os" >}})) and Ubuntu 20.04 Linux OS ([here](#61fc)) respectively because I have tested the scripts on these platforms.

## Let’s Code

![](https://cdn-images-1.medium.com/max/2000/1*ruWA6ZuAZoxdcgIS0qmrHw.gif)

## **# Mac OS**

1. Create an alias for your browser in terminal (I use Google Chrome) and save this in **~/.zshrc** terminal configuration file
```bash
$ echo alias chrome="open -a 'Google Chrome'" >> ~/.zshrc 
```
2. Create a sh script that opens your stack overflow profile (or opens any website you want) and save as `cronjob.sh`.

{{< gist lordvidex b491085f187f7a0505990ef6b1230b38 >}}

2.5. Run `zsh cronjob.sh` and make sure it opens up your stack overflow page on Google Chrome.

3. Define and set the cron job
> a. The below command opens a list of cron jobs in [vim](https://en.wikipedia.org/wiki/Vim_(text_editor)) where each line represents a cron job
```bash
$ crontab -e
```

> b. Press `i` to enter insert mode and on a new line enter:

    0 01,11,23 * * * zsh /path/to/script/cronjob.sh

* ** Note: Change *`/path/to/script/cronjob.sh`* to the absolute path of the script file created in [(step 2)](#-mac-os) .

* The above command can be interpreted in the following way:
```
0 - 0 minutes
01,11,23 - 1, 11 and 23 hours respectively # my active times
* - all days of the month (1-31)
* - all months (1-12)
* - all days of the week (0-6)

# this cron time config is followed by the command to be run
```

> c. Press `Esc` to exit from insert mode and press `:wq` to save and quit the vim environment. \

> d. You should get a message like this:
```
crontab: installing new crontab
```
> e. You can check your cron jobs by entering the following command
```bash
$ crontab -l # displays list of cron jobs
```
<!-- add an image using hugo function -->
{{< figure align=center src=images/well-done.gif >}}

## # **Ubuntu**
The default browser of Ubuntu 20.04 is Firefox **BUT** I will be using **Google Chrome** because of [errors i encountered](https://discourse.mozilla.org/t/run-firefox-from-cron/87478) while running Firefox from `cron`, also, the default shell on Linux is `bash`. The commands to enter are basically similar with those entered above for Mac OS. But I’ll reiterate for clarity and reference purposes.

1. Test google-chrome, cron and bash
```bash
$ google-chrome https://stackoverflow.com

# this should open your page BUT if it does not install chrome
# with this guide: https://itsfoss.com/install-chrome-ubuntu/

$ crontab -l # this should list your cron jobs

$ bash --version # prints out your bash version
```

2. Create a bash script to open Google Chrome and save as `cronjob.sh`

{{<gist lordvidex b491085f187f7a0505990ef6b1230b38>}}

2.5. Run bash cronjob.sh and make sure it opens up your stack overflow page on Google Chrome.

3. Define and set the cron job
```
$ crontab -e 

# you might be prompted to select an editor, pick you favorite
```
* Enter on a new line the following command
```
0 01,11,23 * * * bash /path/to/cronjob.sh
```
* The above command can be interpreted in the following way:
```
0 - 0 minutes
01,11,23 - 1, 11 and 23 hours respectively # my active times
* - all days of the month (1-31)
* - all months (1-12)
* - all days of the week (0-6)

# this cron time config is followed by the command to be run
```

* You should get a message like this:
```plain
crontab: installing new crontab
```
* You can check your cron jobs by entering the following command
```
$ crontab -l # displays list of cron jobs
```

{{< figure align=center src=images/proud-of-you.gif >}}

## Conclusion

Cron jobs are really cool and powerful when properly used. Also, they are easy to setup and manage. You can run your .py, .swift, .java scripts *(not just shell scripts)* from cron.

There is a catch anyways.

{{< figure src="images/gotcha.gif" caption="gotcha" align=center >}}

> Cron jobs would not wake your system from sleep mode. So you have to use a cron time configuration that best suits you i.e. when your system is going to be switched on. Read more on creating cron jobs at [crontab.guru](https://crontab.guru/).

## UPDATE
{{< figure align=center width=80% src=https://cdn-images-1.medium.com/max/2000/1*6UdKqwcSxYDGid9Ycoj-Lw.png >}}

*Today is the 4th of July and I’ve gotten the badge, use CRON job today!😌*

Cheers🥂🎉
