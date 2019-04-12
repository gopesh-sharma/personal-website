---
layout: post
title: 'My Blog on Jekyll and GitHub Pages'
categories: blog
tags: ['jekyll', 'gh-pages']
excerpt: 'My blog runs on Jekyll, using so-simple-theme and served over GitHub Pages and have custom domain.'
date: April 12, 2019
author: self
---

If you would have read [my first blog](https://www.gopeshsharma.dev/blog/hello-world/), I said that this blog runs on Jekyll, using so-simple-theme and served over GitHub Pages. Jekyll is a website generator which is easy to use and perfect for static blogs which can be hosted on GitHub Pages. All I need to do is to write my blog content in Markdown and Jeykll takes care of converting it to a static website. 

Why I love Jeykll over others is because of its simplicity. I dont want to have an overhead of maintaining database or worring about security, Jeykll takes care of it for me. I also do not have to worry about hosting, because GitHub Pages can host my website for free of charge. The only thing I have to worry about is to write my learning using markdown :).

If you also want to start your blog using Jeykll, the steps are as follows:

1. Fork My Repo in GitHub (https://github.com/gopesh-sharma/personal-website).
2. Go to Settings of your new forked repository and select Source as master branch under GitHub Pages.
3. If you do not want to use Custom Domain Name, change the repository name to YOURUSERNAME.github.io.
4. I am using so-simple theme, if you want to have any other theme, you can Change the theme under GitHub Pages.
5. Since I am using Custom Domain, I have added www.gopeshsharma.dev under Custom Domain and also added the below Custom resource records in Google Domains.

<figure style="width: 600px" class="align-center">
  <img src="{{ '/images/blog/domain-gh-pages.png' | absolute_url }}" alt="Google Domain Custom Resource Records">
  <figcaption>Domain Custom Resource Records</figcaption>
</figure>   

The Full Configuration of GitHub Pages look like :

<figure style="width: 600px" class="align-center">
  <img src="{{ '/images/blog/github-settings.png' | absolute_url }}" alt="GitHub Pages Configuration">
  <figcaption>GitHub Pages Configuration</figcaption>
</figure>   

6. If you want to change the navigation, go to [_data/navigation.yml](https://github.com/gopesh-sharma/personal-website/blob/master/_data/navigation.yml) file and change it according to your blog navigation.
7. For writing posts, you need to go to [_posts](https://github.com/gopesh-sharma/personal-website/tree/master/_posts) folder and add your post. The file name should contain Date and the url of the blog like [2019-04-11-hello-world.md](https://github.com/gopesh-sharma/personal-website/blob/master/_posts/2019-04-11-hello-world.md). 

I hope you understand now that how easy is to setup your blog. Please let me know if you stuck in any step, I am happy to help. 

Thanks for reading.


