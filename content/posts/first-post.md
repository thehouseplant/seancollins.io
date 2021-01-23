---
title: Build and host a blog in 30 minutes
date: 2021-01-23T18:30:03+00:00
tags:
- tutorial
author: Me
showToc: false
TocOpen: false
hidemeta: false
comments: false
description: How to build and host a blog using Hugo, Netlify, and Forestry in about
  30 minutes (or less)
disableHLJS: false
disableShare: false
cover:
  image: "<image path/url>"
  alt: "<alt text>"
  caption: "<text>"
  relative: false
  hidden: true

---
Like many people in tech, we are told that we should start a blog. I have tried to do exactly that about a dozen times, each met with failure or a lack of desire to finish the project. This is a guide for folks that want to get started in this space, but do not want to invest hours in trying to find a stack, technology, or platform to enable them to do it. 

Let's build a blog in 30 minutes.

First, you're going to want to have a [GitHub](https://github.com/), [GitLab](https://about.gitlab.com/), or [Bitbucket](https://bitbucket.org/) account. I chose GitHub, but any of the three will play nicely with the services we are going to use. 

Next, we're to going to pick a static site generator to help build our blog. For the uninitiated, a static site generator is a way of quickly generating and building a site that can be deployed to a static content host. There are far too many generators to keep up with, and I recommend folks take some time to [explore](https://jamstack.org/generators/) the various options in this space, but I have chosen one with a _very_ low barrier to entry: [Hugo](https://gohugo.io/). I like Hugo because the installation is as simple as a `brew install hugo` on macOS, or through one of the other methods in their installation [documentation](https://gohugo.io/getting-started/installing). After you've gotten it installed, I recommend following their [quick start](https://gohugo.io/getting-started/quick-start/) guide to get up and running. 

Once that has been done, it's time to find a place to host this your new site. Today, we'll be taking advantage of [Netlify](https://www.netlify.com/), one of the many static content hosts on the market today. There are others in this space, like [Vercel](https://vercel.com/), [Surge](https://surge.sh/), or services from AWS like [Amplify](https://aws.amazon.com/amplify/hosting/) and static site hosting via [S3](https://docs.aws.amazon.com/AmazonS3/latest/dev/WebsiteHosting.html). Again, I chose Netlify because of its ease of use and low barrier to entry. To get started on Netlify, head to their [signup](https://app.netlify.com/signup) site and connect to your GitHub (or equivalent) account. You'll get prompted to authorize the application: 

![](/img/01_netlify_auth.png)

We trust Netlify, so go ahead and hit Authorize so that it can view your public repositories. 