---
title: "Optimize Pull Requests in Travis CI"
created_at: Fri Jan 28 2022 15:00:00 EDT
author: Montana Mendy
layout: post
permalink: 2022-01-28-pullrequests
category: news
excerpt_separator: <!-- more --> 
tags:
  - news
  - feature
  - infrastructure
  - community
---

![TCI-Graphics for AdsBlogs (4)](https://user-images.githubusercontent.com/20936398/151615023-838f9eba-c68d-40ec-80ae-831ee61da8b5.png)

One thing you might want when using Travis or any CI tool is to prevent broken code from getting merged, this makes for quality pull requests and making sure nothing broken gets merged. Let's take a dive into Pull Requests in Travis.

<!-- more --> 

## Branch Protection 

Branch protection is something you may not know about, you can create a branch protection rule to enforce certain workflows for one or more branches. So for example  we want `status checks` to pass before merging, let's take a look in GitHub: 

![image](https://user-images.githubusercontent.com/20936398/151615618-93853954-2162-4e62-9507-84659ae06151.png)

This will enforce that the status check pass is required before anything gets merged to a branch. 

## 2 Branches or 1 Branch? 

By default in Travis GUI, you'll see both boxes enabled, this is due to the fact that Travis is running a build on the `branch` and a build on the `pull request`:

<img width="918" alt="Screen Shot 2022-01-28 at 12 22 09 PM" src="https://user-images.githubusercontent.com/20936398/151616036-d207c17f-7708-4770-a823-bab2348866ad.png">

Leaving it on will allow you to fully explore all the deploy/branching options we have to offer. What if you want the opposite though? To build on a pull request without redundancy? 

## Build Pull Requests and Merges to the Master Branch 

Assuming you want to build all PRs, something like the following will do the trick, you'll need to enable both the branch and PR builds on the settings page like we showed above, but have all boxes ticked, and put this line as the first line in your `travis.yml`:

```bash
if: (type = push AND branch IN (master, dev)) OR (type = pull_request AND NOT branch =~ /no-ci/)
```

This will attempt a push build on all pushes and a PR build on all pushes to an open PR, but will filter out any that don't meet the conditionals you set it place. Read more about conditional builds [here](https://docs.travis-ci.com/user/conditional-builds-stages-jobs/).
