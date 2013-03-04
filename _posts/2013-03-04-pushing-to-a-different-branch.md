---
layout: post
title: "Push to a different Git branch"
tagline: "It's all about that colon"
description: "It's simple to push a non-master branch to a different remote branch."
category: development
tags: [heroku, deploying, git, github]
---
{% include JB/setup %}

I found a quick and easy way this weekend to push a local branch to a different remote branch.
You Git experts out there should definitely know this, but I'm constantly discovering new things with Git as I rarely use it
for more than pushing and pulling ;)

It's as simple as: `git push heroku my_feature_branch:master`.

Basically this will push changes from your local branch *my_feature_branch* to the remote branch *master*
(which Heroku requires you to deploy with).