---
layout: post
title:  "Configuring rbenv on fish shell"
date:   2015-11-25 11:54:00
categories: environment
---

This weekend I made a clean install of OS X on my machine, and when I was about
to install zsh and prezto again, I thought about trying out fish shell. So why
not?

It's not POSIX compatible, so there's some gotchas. When installing rbenv, I had
a couple problems when setting the environment variables and initializing the
function.

First of all, fish does not have an EXPORT command. You set variable environments
using SET. Another thing is that fish's PATH is a list, instead of a semicolon-
sepatared string. So, setting the path becomes:

    set -x PATH $HOME/.rbenv/bin $PATH

Now, the trickiest and most difficult part to find on Google: setting up the
`rbenv` function. rbenv's README says you need to add `eval "$(rbenv init -)"`
to your profile, but you first have to convert it to fish's syntax. Add instead:

    rbenv init - | source


TLDR: Add to your `.config/fish/config.fish`:

    set -x PATH $HOME/.rbenv/bin $PATH
    rbenv init - | source
