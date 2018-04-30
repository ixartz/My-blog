---
title: 'Deploy your static website at every pull request'
date: 2018-05-01 10:34:47
description: See live a pull request change
categories:
  - Jekyll
tags:
  - GitHub
  - Pull request
  - Netlify
  - Continuous deployment
---
*In the article title, it is referring to static site genertor. Nevertheless, this tutorial can also be applied to your front-end code like React, Angular, etc.*

Pull request is part of GitHub flow. Other team members can review and comment the changes. Today, everybody agrees that pull request improves code quality without any doubts. Unfortunately, displaying the source code is limited, you cannot see the change in real environment.

The idea about this article is to deploy each pull request. After deploying it, you should be able to see it live in your browser. Other team members can also have access to it with one click. Of course, we do not want to alter production version. So, the deployment happens without merging the pull request.

## Deploy Previews, Review Apps

What we want to use is named *Deploy Previews* on <a href="https://www.netlify.com/" target="_blank">Netlify</a>. But, on <a href="https://www.heroku.com/" target="_blank">Heroku</a> or <a href="https://gitlab.com/" target="_blank">GitLab</a>, it has another name: *Review App*.

Heroku is an application hosting, it is a little bit overkill to host only static files. In the opposite, GitLab can only host static pages for one branch. Currently, we cannot host several branches on GitLab Pages. It is possible to overcome this limitation but you need to link GitLab CI with a static hosting like <a href="https://surge.sh/" target="_blank">Surge</a>.

### Netlify more than a static hosting solution

Good news for us! Netlify has everything we need. First, we need to <a href="https://app.netlify.com/signup" target="_blank">create an account on Netlify</a>:

![Netlify sign up page]({{ site.baseurl }}assets/images/posts/netlify-static-hosting-sign-up.png){: .aligncenter}

After signing up and logging into your account, you should see this page:

![Netlify create a new site]({{ site.baseurl }}assets/images/posts/netlify-create-new-site.png){: .aligncenter}

There is a *New site from Git* button on the page, click on it and this following page will appear:

![Netlify links with GitHub]({{ site.baseurl }}assets/images/posts/netlify-github-link.png){: .aligncenter}

Select your Git hosting service. In my case, I will choose GitHub. Then, you need to give Netlify access to your GitHub account.

*I will continue this tutorial on GitHub but the same principle can be applied to GitLab or Bitbucket.*

After authorizing Netlify, you need to select a project from your GitHub account:

![Netlify select a GitHub repository]({{ site.baseurl }}assets/images/posts/netlify-select-github-repository.png){: .aligncenter}

Finally, you need to configure the building and deployment option:

![Netlify configuration]({{ site.baseurl }}assets/images/posts/netlify-build-deploy-option.png){: .aligncenter}

The first option, *Branch to deploy*, is your main branch. In my case, the branch is *master*.

The two other options are more related to the static site generator you use. The *build command* is the command to generate your website. For example, on Jekyll, it is *jekyll build*. And, the *publish directory* is the destination folder of your *build command*. Everything inside your *publish directory* will be served by Netlify.

After submitting the form, Netlify will build your project and deploy it. When it finishes, you will see this screen:

![Netlify deployed site]({{ site.baseurl }}assets/images/posts/netlify-deployed-website.png){: .aligncenter}

Click on the green link, you should see your site deployed.

### Behind the scene

You do not need to do anything to make it work with a pull request. Netlify uses GitHub webhooks in order to be notified when someone creates a pull request.

![Netlify links Github webhooks]({{ site.baseurl }}assets/images/posts/netlify-github-webhooks.png){: .aligncenter}

When a change is commited and pushed, Netlify can start a build and make deployment.

### Configuring netlify.toml file

Configuring with UI, it is great but you do not have versioning. And, when someone forks your project, he needs to configure it again. The good news is that Netlify offers the configuration through *netlify.toml* file. Here is an example:

{% highlight yaml %}
[build]
    publish = '_site/'
    command = 'sh command for production environment'

[context.deploy-preview]
    command = 'sh command for pull request environment'
{% endhighlight %}

*build* context is the default build configuration. There is another *deploy-preview* context related to the deployment from a pull request.

In the above example, I want to lanch two different build configurations whether it is a pull request or it is from any branches.

### Testing, create a pull request

Now, you can test by making a pull request and you will see a new line in the status checks of your pull request:

![Netlify status checks]({{ site.baseurl }}assets/images/posts/netlify-status-checks.png){: .aligncenter}

Click on the *Details* link and you will see your pull request deployed and your changes live in your browser. You can continue to work on your change by making new commits and pushing to the pull request, it will be automatically deployed again.
