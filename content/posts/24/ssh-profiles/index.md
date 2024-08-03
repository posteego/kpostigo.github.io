---
title: "Multiple SSH Profiles"
summary: How to use multiple SSH profiles with SSH Config
date: 2024-08-03
draft: false
tags: ["SSH", "SSH Config", "Github"]
hidemeta: false
disableShare: false
hideFooter: true
---

#### A quick note on BeatBridge

I decided to keep BeatBridge in TestFlight for a while longer, probably until I get a better idea of what I want to do with it. So far, it's proven to be a convenient utility for my friends and I, but I don't feel the need to publish the app to the App Store at the moment.

## Preface

This summer, I am forunate enough to be enrolled in [Kura Labs](https://www.kuralabs.org/) Infrastructure Engineering Fellowship. The program is teaching me to build and deploy infrastructure for the cloud. I'm excited to learn more about cloud infrastructure, but in the meantime I will be writing about anything I find interesting along my journey.

For this article, I wanted to document my experience in distinguishing between my personal and Kura program Github profiles via SSH on my local machine. I am using a Mac, so the commands may be specific for Linux/Max users.

## SSH Config

Conveniently, having multiple SSH profiles is a simple task that requires at the minimum the main SSH config file. I've been using the `~/.ssh/config` file for a while now, and I have a few profiles that I use for different purposes. Here is an example of my config file:

```bash
Host github.com
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_rsa

Host bitbucket.org
  HostName bitbucket.org
  User git
  IdentityFile ~/.ssh/id_rsa_2
```

I had to create an additional account on Github for the Kura program, so I created a new SSH key and added it to my Github account, but was confused at first with how to tell my mac when to use whcih profile automatically. The first thing you want to do to make this work is to customize the url you want to use for each account:

```bash
Host github.com
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_rsa

Host kura.github.com # notice the url is different than my personal account above
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_rsa_kura

# other profiles
```

This was simple enough: I can replace a github URL with the `kura.github.com` url and it will use the correct SSH key when _cloning_ a repo. What about pushing to the repo as the correct author?

## Git Config

For my personal Github account, I had global git config settings that I wanted to use for all my repos throughout my machine. This was saved in the `~/.gitconfig` file:

```bash
[user]
  name = Ken Postigo
  email = 1234567+kpaccount@users.noreply.github.com

[init]
  defaultBranch = main
```

I realized that for my Kura Github account, I wanted to use a different name and email address, so I created a new file in my `~/.ssh` folder called `.gitconfig-kura` and added the following:

```bash
# .gitconfig-kura
[user]
  name = Kura Ken
  email = 7654321+kpKURAaccount@users.noreply.github.com
```

Now I go back to my original `~/.gitconfig` file and add the following:

```bash
# [user] info from my personal account here

[includeIf "gitdir:~/work/kura/"]
  path = ~/.gitconfig-kura

# other git config settings from above
```

This automates the process of adding the correct git config settings for each repo I work on, depending on the repo's location on my machine! Whenever I create a new repo in the `~/work/kura/` folder, it will automatically use the `~/.gitconfig-kura` file, which will set the correct author name and email address for the repo.

If I need to clone a repo from a specific account with the `git clone` command, I simply need to use the correct URL as specified in my `~/.ssh/config` file (either `github.com` or `kura.github.com`).

## Conclusion

I hope this concise article has been helpful in explaining how to use multiple SSH profiles and how to automate the process of setting the correct git config settings for each repo!
