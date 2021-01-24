---
title: Build and host a blog in 30 minutes
date: 2021-01-23T18:30:03.000+00:00
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

First, you're going to want to have a [GitHub](https://github.com/), [GitLab](https://about.gitlab.com/), or [Bitbucket](https://bitbucket.org/) account. I chose GitHub, but any of the three will play nicely with the services we are going to use. To get started, register for a new account on GitHub's site, and [create](https://github.com/new) a new repository for your site. I used my [domain](https://github.com/thehouseplant/seancollins.io) name as the repository name, but it's definitely not a requirement. I'd create it without a README file so that we can let our static site generator actually build the project directory, but we'll get into that in a moment. Once the repository has been created, we're done with GitHub (for now).

Now, we're to going to pick a static site generator to help build our blog. For the uninitiated, a static site generator is a way of quickly generating and building a site that can be deployed to a static content host. There are far too many generators to keep up with, and I recommend folks take some time to [explore](https://jamstack.org/generators/) the various options in this space, but I have chosen one with a _very_ low barrier to entry: [Hugo](https://gohugo.io/). I like Hugo because the installation is as simple as a `brew install hugo` on macOS, or through one of the other methods in their installation [documentation](https://gohugo.io/getting-started/installing). After you've gotten it installed, I recommend following their [quick start](https://gohugo.io/getting-started/quick-start/) guide to get up and running. You'll want to make the project name the same as the name of the repository you configured earlier. For me, this meant `hugo new site seancollins.io`. This creates a blank Hugo project that we can work with.

Next, you'll want to pick out a theme. I went with [PaperMod](https://themes.gohugo.io/hugo-papermod/). It's clean and blog-friendly, lightning fast, provides a dark/light mode toggle, and gives me plenty of other features to tinker with. There are many other [themes](https://themes.gohugo.io/) to choose from, so don't be afraid to explore other options until you find one that suits your needs. Each theme will provide its own installation guidelines, so you'll want to pay close attention to that as you do your exploration. I went with the git-submodule route by doing the following:

    git submodule add https://github.com/adityatelange/hugo-PaperMod.git themes/PaperMod --depth=1
    git submodule update --init --recursive

With the theme installed, we'll want to configure it. Hugo comes with its own `config.toml` file in the root of the project directory. We're going to change that to a YAML file because it's easier to read. PaperMod provides a [sample](https://adityatelange.github.io/hugo-PaperMod/posts/papermod/papermod-installation/#sample-configyml) configuration, but you'll want to customize some of these values along the way. We'll also want to add another configuration file, this time for our static content host configuration. We'll cover this more in the next section, but for now we can create another file in the root of our directory called `netlify.toml` (Netlify only supports TOML) with the following content: 

    [build]
    publish = "public"
    command = "hugo --gc --minify"
    
    [context.production.environment]
    HUGO_VERSION = "0.80.0"
    HUGO_ENV = "production"
    HUGO_ENABLEGITINFO = "true"
    
    [context.split1]
    command = "hugo --gc --minify --enableGitInfo"
    
    [context.split1.environment]
    HUGO_VERSION = "0.80.0"
    HUGO_ENV = "production"
    
    [context.deploy-preview]
    command = "hugo --gc --minify --buildFuture -b $DEPLOY_PRIME_URL"
    
    [context.deploy-preview.environment]
    HUGO_VERSION = "0.80.0"
    
    [context.branch-deploy]
    command = "hugo --gc --minify -b $DEPLOY_PRIME_URL"
    
    [context.branch-deploy.environment]
    HUGO_VERSION = "0.80.0"
    
    [context.next.environment]
    HUGO_ENABLEGITINFO = "true"

Just trust me. It'll save us some steps later. The end result is that it follows the Hugo Netlify deployment [guide](https://gohugo.io/hosting-and-deployment/hosting-on-netlify/). We'll be following similar steps, but feel free to go through that guide as well. 

Once that has been done, it's time to find a place to host this your new site. Today, we'll be taking advantage of [Netlify](https://www.netlify.com/), one of the many static content hosts on the market today. There are others in this space, like [Vercel](https://vercel.com/), [Surge](https://surge.sh/), or services from AWS like [Amplify](https://aws.amazon.com/amplify/hosting/) and static site hosting via [S3](https://docs.aws.amazon.com/AmazonS3/latest/dev/WebsiteHosting.html). Again, I chose Netlify because of its ease of use and low barrier to entry. To get started on Netlify, head to their [signup](https://app.netlify.com/signup) site and connect to your GitHub (or equivalent) account. You'll get prompted to authorize the application:

![](/img/01_netlify_auth.png#center)

We trust Netlify, so go ahead and hit Authorize so that it can view your public repositories. Simply select the repository we setup earlier, then