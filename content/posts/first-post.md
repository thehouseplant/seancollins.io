---
title: Start a blog on your lunch break
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

#### Getting Started

First, you're going to want to have a [GitHub](https://github.com/), [GitLab](https://about.gitlab.com/), or [Bitbucket](https://bitbucket.org/) account. I chose GitHub, but any of the three will play nicely with the services we are going to use. To get started, register for a new account on GitHub's site, and [create](https://github.com/new) a new repository for your site. I used my [domain](https://github.com/thehouseplant/seancollins.io) name as the repository name, but it's definitely not a requirement. I'd create it without a README file so that we can let our static site generator actually build the project directory, but we'll get into that in a moment. Once the repository has been created, we're done with GitHub (for now).

#### Static Site Generators

Now, we're to going to pick a static site generator to help build our blog. For the uninitiated, a static site generator is a way of quickly generating and building a site that can be deployed to a static content host. There are far too many generators to keep up with, and I recommend folks take some time to [explore](https://jamstack.org/generators/) the various options in this space. I have chosen one with a _very_ low barrier to entry: [Hugo](https://gohugo.io/). I like Hugo because the installation is as simple as a `brew install hugo` on macOS, or through one of the other methods in their installation [documentation](https://gohugo.io/getting-started/installing). After it's installed I recommend following their [quick start](https://gohugo.io/getting-started/quick-start/) guide to get up and running. You'll want to make the project name the same as the name of the repository you configured earlier. For me, this meant `hugo new site seancollins.io`. This creates a blank Hugo project that we can work with.

#### Themes

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

#### Sample Content

One other thing we'll want to take care of before we move onto the next step, is creating a sample post. This will help later when we get to the content management portion of our deployment. For now, simply run a `hugo new posts/first-post.md` and let Hugo generate a sample post for us. For me, I will be updating the content of this post with the [sample](https://adityatelange.github.io/hugo-PaperMod/posts/papermod/papermod-installation/#sample-pagemd) that PaperMod provides, but your chosen theme may have other options: 

    ---
    title: "My 1st post"
    date: 2020-09-15T11:30:03+00:00
    # weight: 1
    # aliases: ["/first"]
    tags: ["first"]
    author: "Me"
    # author: ["Me", "You"] # multiple authors
    showToc: true
    TocOpen: false
    draft: false
    hidemeta: false
    comments: false
    description: "Desc Text."
    disableHLJS: true # to disable highlightjs
    disableShare: false
    disableHLJS: false
    cover:
        image: "<image path/url>" # image path/url
        alt: "<alt text>" # alt text
        caption: "<text>" # display caption under cover
        relative: false # when using page bundles set this to true
        hidden: true # only hide on current single page
    
    ---

This will expose all of the front matter, or post configuration, to us for later customization. 

#### Hosting

Once that has been done, it's time to find a place to host this your new site. Today, we'll be taking advantage of [Netlify](https://www.netlify.com/), one of the many static content hosts on the market today. There are others in this space, like [Vercel](https://vercel.com/), [Surge](https://surge.sh/), or services from AWS like [Amplify](https://aws.amazon.com/amplify/hosting/) and static site hosting via [S3](https://docs.aws.amazon.com/AmazonS3/latest/dev/WebsiteHosting.html). Again, I chose Netlify because of its ease of use and low barrier to entry. To get started on Netlify, head to their [signup](https://app.netlify.com/signup) site and connect to your GitHub (or equivalent) account. You'll get prompted to authorize the application:

![](/img/01_netlify_auth.png#center)

We trust Netlify, so go ahead and hit Authorize so that you can get logged into the platform. Then, we'll [add](https://app.netlify.com/start) a new site with continuous deployment:

![](/img/02_netlify_start.png#center)

Select your source control platform. You'll see a pop-up confirming the authorization step we went through a moment ago. Then, you'll be confronted with a list of repositories to add to Netlify:

![](/img/03_netlify_create.png#center)

Next, it will pull the settings we specified in our `netlify.toml` file. This is the part where we save a few steps, but this is what the configuration should look like:

![](/img/04_netlify_deploy.png#center)

Mash the deploy button and you will be greeted with the dashboard for your new site. Mine has a few more deploys under its belt, but you should end up with something that looks a bit like this:

![](/img/05_netlify_dashboard.png#center)

Here is your site's command center. The first thing you'll want to do is head into Site Settings and change your site's URL. Netlify automatically generates a URL so that you can easily get started, but you'll want to clean that up a bit so your blog doesn't live at frosty-batwing-12345.netlify.app or something in that vein. In Site Settings, take a look at this section:

![](/img/06_netlify_site_settings.png#center)

Then hit **Change site name** to update the URL to something more to your liking:

![](/img/07_netlify_site_name.png#center)

Other areas of note in your dashboard are various build and deploy, settings, domain management (more on this later), and other built-in Netlify services like [Forms](https://www.netlify.com/products/forms/), [Authentication](https://docs.netlify.com/visitor-access/identity/?_ga=2.17008752.832999977.1611436317-1095185594.1611436317), and [Analytics](https://www.netlify.com/products/analytics/). Otherwise, assuming the build went through successfully, you site should be live. Pretty easy, huh?

#### Content Management

Now, let's go a step further and introduce a content management system into the mix. I personally love working with git, but I know that it may not be everyone's cup of tea. Something about editing blog posts in VS Code may not be the best workflow for folks who are more focused on the content. Here is where I will introduce [Forestry](https://forestry.io/) into the mix. Now, as with every layer of the stack we've talked about in this article, Forestry is not alone in this space. There are many other [services]() in this area, but I like Forestry's clean, no-nonsense approach to getting started and publishing content.

First, we'll want to want to [create](https://app.forestry.io/signup) an account and go through the process of granting access to our version control platform. I recommend simply signing in with your GitHub (or equivalent) account to simplify that handshake:

![](/img/08_forestry_create.png#center)

Once Forestry is connected, you'll be brought your dashboard:

![](/img/09_forestry_dashboard.png#center)

From here simply click **Add site** to start the import process:

![](/img/10_forestry_ssg.png#center)

You will be prompted to select a static site generator. For our purposes, select Hugo and leave the version to whatever it defaults to in the prompt. Also note the blurb at the bottom about using one of their starters. Forestry has [many](https://www.forestry.io/starters/) of them to choose from, and it makes it very easy to started with different static site generators should you choose to. Once you're content with our Hugo settings, hit **Next** and we'll step through the authentication process:

![](/img/11_forestry_git.png#center)

Select your git provider of choice, and hit **Next** when you're ready:

![](/img/12_forestry_repository.png#center)

After that, you'll be prompted to select a repository to connect to. This is probably starting to feel pretty familiar by now, but simply pick the repository you setup earlier in the process. Once you're content, hit **Next** one last time. Once it's finished, you'll be brought back to your dashboard, and it will look something like this:

![](/img/09_forestry_dashboard-1.png#center)

From the dashboard, click on your newly imported site and you will be met with a new dashboard to manage your site's content. 

#### Putting it All Together

You have now built a blog