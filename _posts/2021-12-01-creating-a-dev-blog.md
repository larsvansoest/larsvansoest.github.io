---
title: Creating a Free Markdown Dev Blog Using Github Pages and Google Cloud
date: 2021-12-02 12:07:00 +0100
categories: [GitHub Pages, Google Cloud]
tags: [dev blog, markdown, github pages, google cloud, chirpy jekyll theme, jekyll, disqus, git]
---

## Introduction

In this post, we set up a responsive and interactive dev blog using [GitHub Pages](https://pages.github.com/){:target="_blank"} in combination with the [Chirpy Jekyll Theme](https://github.com/cotes2020/jekyll-theme-chirpy){:target="_blank"} and [Google Cloud](https://cloud.google.com/){:target="_blank"}. The blog has the same setup as [vanSoest.dev](#), featuring *viewcounts*, *tags*, *categories*, a *search bar* and a *dark theme* toggle. The blog should provide any developer with a portfolio to easily and estetically collect sharable guides, notes, thoughts and opinions using [Markdown](https://github.github.com/gfm/){:target="_blank"}. What's more, the blog is completely free, and without licensing restrictions.

In an overview, we 
[setup a GitHub Pages repository with Chirpy Jekyll Theme](#markdown-dev-blog--github-pages),
[configure *viewcounts*, *analytics* and a *CDN* with Google Cloud](#pageviews-and-cdn--google-cloud), and 
[enable *comments* with Disqus](#discussion-sections--disqus).

## Markdown Dev Blog | GitHub Pages

*Source: [Getting Started | Chirpy](https://chirpy.cotes.info/posts/getting-started/){:target="_blank"}*

In the steps below, we setup our main dev blog website. In short, we set up a [GitHub Pages](https://pages.github.com/){:target="_blank"} repository with the [Chirpy Jekyll Theme](https://github.com/cotes2020/jekyll-theme-chirpy){:target="_blank"}. Optionally, we may set a custom domain with a certificate for *https*.

### Creating a GitHub Account
In order to continue, we need a [GitHub](https://github.com/){:target="_blank"} account, [sign up here](https://github.com/signup){:target="_blank"}. In the steps below, the *GitHub account name* is refered to as `<github_username>`. Moreover, in case a [custom domain](#custom-domain) is not desired, note that the blog url will match `<github_username>.github.io`.

### Setting up GitHub Pages
We use [Chirpy Jekyll Theme](https://github.com/cotes2020/jekyll-theme-chirpy){:target="_blank"} as our [GitHub Pages](https://pages.github.com/){:target="_blank"} theme. As mentioned by [Getting Started | Chirpy](https://chirpy.cotes.info/posts/getting-started/){:target="_blank"}, there are two ways to set up the blog.

- [Using the Chirpy Starter](https://github.com/cotes2020/chirpy-starter/generate){:target="_blank"}

- [Forking on GitHub](https://github.com/cotes2020/jekyll-theme-chirpy/fork){:target="_blank"}

For simplicity, we use the first step, as the latter is for custom development. Accordingly, follow the steps below. 

1. [Sign in](https://github.com/login){:target="_blank"} on GitHub.

2. [Create a new repository from chirpy-starter](https://github.com/cotes2020/chirpy-starter/generate){:target="_blank"}.

    - Set the *Owner* to the GitHub user `<github_username>`.
    - Set the *Repository Name* to `<github_username>.github.io`.
    - Set to public (default).
    - Uncheck *Include all branches* (default).

    ![Screenshot when creating a new repository from chirpy-starter](/posts/2021-12-01-creating-a-dev-blog/create-a-new-repository-from-chirpy-starter.png)

3. Now, we have successfully created the repository. [Clone](https://github.com/git-guides/git-clone) the repository locally.
    
    >If you are unfamiliar with *git*, checkout [GitHub Git Guide](https://github.com/git-guides){:target="_blank"} and [git-scm](https://git-scm.com/){:target="_blank"}.

4. In the local repository folder, set the value for property `url` of `_config.yml` to `https://<github_username>.github.io`, then [push](https://github.com/git-guides/git-push){:target="_blank"} the change to trigger a [GitHub Action](https://docs.github.com/en/actions){:target="_blank"} to publish the blog.

6. Head to `https://github.com/<github_username>/<github_username>.github.io/actions`, or *Actions* on the repository. Check the publishing status. After a few minutes, once resolved, a new branch `gh-pages` is added to the repository on [GitHub](https://github.com){:target="_blank"}, which contains the built source of the blog page.

    ![Screenshot of resolved publishing action for the blog page.](/posts/2021-12-01-creating-a-dev-blog/resolved-publishing-action-for-the-blog-page.png)

7. Head to `https://github.com/<github_username>/<github_username>.github.io/settings/pages` or *Settings* > *Pages* on the repository, and set the page source as follows.

    - Set *Branch* to `gh-pages`.
    - Set *Folder* to `/(root)`.
    - Click *Save*.

    ![Screenshot when setting branch gh-page as source.](/posts/2021-12-01-creating-a-dev-blog/set-gh-pages-root-as-source.png)

8. After a few minutes, head to `https://<github_username>.github.io` and view the new website.

With the steps above, we have created the base dev blog website. For the rest of this section, we look into personalisation. In short, we 
[set up a custom domain](#custom-domain) and 
[apply custom *css* to the page](#custom-css-and-running-locally). 
If these modifications are not necessary, skip to [CDN and Analytics | Google Cloud](#cdn-and-analytics--google-cloud).

### Custom Domain

In this section, we setup a custom *apex domain* with *https* support. Moreover, we configure the *DNS* by setting up *A-records* for the domain and its `www` subdomain. Subsequently, [GitHub](https://github.com){:target="_blank"} creates and maintains the required certificates for us. 

> For instructions on how to configure a subdomain and other alternative methods, see [Managing a custom domain for your GitHub Pages site - GitHub Docs](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site/managing-a-custom-domain-for-your-github-pages-site){:target="_blank"}.

1. To use a domain, we must own it. So, for starters, make sure to own the desired domain for the blog.

2. In the *DNS* settings of the domain, add the following *A-records*. 

    | Type | Name | Value |
    |:----:|:----:|:-----:|
    | A | @ | 185.199.108.153 |
    | A | @ | 185.199.109.153 |
    | A | @ | 185.199.110.153 |
    | A | @ | 185.199.111.153 |
    | A | www | `<github_username>`.github.io |

    > *DNS* changes can take up to 48 hours. If an error occurs, ensure the *A-records* are correct and wait for the changes to resolve.

3. Head to `https://github.com/<github_username>/<github_username>.github.io/settings/pages` or *Settings* > *Pages* on the repository. Under custom domain, enter the configured domain, and click save. 

    ![Screenshot of setting custom domain](/posts/2021-12-01-creating-a-dev-blog/set-custom-domain.png)

4. Set the value of property `url` of `_config.yml` to the desired domain name (e.g. `https://vansoest.dev`).

5. Type the configured custom domain in the browser, and view the blog.

### Custom CSS and Running Locally

## CDN and Analytics | Google Cloud
In the steps below, we use [Google Cloud](https://cloud.google.com/){:target="_blank"} for *engagement analytics*, *pageviews* and as *content delivery network (CDN)* for our website. To set up a page without these features.

### Pageviews

### CDN

## Discussion Sections | Disqus
