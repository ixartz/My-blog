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
