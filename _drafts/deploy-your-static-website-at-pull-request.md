---
title: 'Deploy your static website at every pull request'
date: 2018-04-28 10:34:47
description: See live a pull request change
categories:
  - Jekyll
tags:
  - GitHub
  - Pull request
  - Netlify
  - Continuous deployment
---
*In the article title, it is referring to static site genertor. But, this tutorial can also be applied on your front-end code like React, Angular, etc.*

Pull request is part of GitHub flow. Other team members can review and comment the changes. Today, everybody agree that pull request improves code quality without any doubts. Unfortunately, displaying the source code is limited, you cannot see the change in real context.

The idea about this article is to deploy each pull request. After deploying it, you should be able to see it live in your browser. Other team members can also access it with one click. Of course, we do not want to alter production version. So, the deployment happens without merging the pull request.

## Deploy Previews, Review Apps

What we want to achieve is named *Deploy Previews* on <a href="https://www.netlify.com/" target="_blank">Netlify</a>. But, on <a href="https://www.heroku.com/" target="_blank">Heroku</a> or <a href="https://gitlab.com/" target="_blank">GitLab</a>, it has another name: *Review App*.

Heroku is an application hosting, it is a little bit overkill to host only static files. In the opposite, GitLab can only host static pages for one branch. Currently, we cannot host several branches on GitLab Pages. It is possible to overcome this limitation but you need to link GitLab CI with a static site hosting like <a href="https://surge.sh/" target="_blank">Surge</a>.

### Netlify more than a static hosting solution

Good news for us! Netlify has everything we need. First, we need to <a href="https://app.netlify.com/signup" target="_blank">create an account on Netlify</a>:

![Netlify sign up page]({{ site.baseurl }}assets/images/posts/netlify-static-hosting-sign-up.png){: .aligncenter}

After signing up and logging into your account, you should see this page:

![Netlify create a new site]({{ site.baseurl }}assets/images/posts/netlify-create-new-site.png){: .aligncenter}

There is a *New site from Git* button on the page, click on it and this following page will appear:

![Netlify links with GitHub]({{ site.baseurl }}assets/images/posts/netlify-github-link.png){: .aligncenter}

Select your Git hosting service. In my case, I will choose GitHub. Then, you need to give Netlify access to your GitHub account.

*I will continue this tutorial on GitHub but the same principle can be applied for GitLab or Bitbucket.*

After authorizing Netlify, you need to select a project from your GitHub account:

![Netlify select a GitHub repository]({{ site.baseurl }}assets/images/posts/netlify-select-github-repository.png){: .aligncenter}

Finally, you need to configure the build and deployment option:

![Netlify configuration]({{ site.baseurl }}assets/images/posts/netlify-build-deploy-option.png){: .aligncenter}

The first option, *Branch to deploy*, is your main branch. In my case, the branch is *master*.

The two other options is more related to the static site generator you use. The *build command* is the command to generate your website. For example, on Jekyll, it is *jekyll build*. And, the *publish directory* is the destination folder of your *build command*. Everything inside your *publish directory* will be served by Netlify.
