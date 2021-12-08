---
title: Creating a Free Markdown Dev Blog Using Github Pages and Google Cloud
date: 2021-12-08 23:27:00 +0100
categories: [GitHub Pages, Google Cloud]
tags: [dev blog, markdown, github pages, google cloud, chirpy jekyll theme, jekyll, disqus, git]
---

In this post, we set up a responsive and interactive dev blog using [GitHub Pages](https://pages.github.com/){:target="_blank"} in combination with the [Chirpy Jekyll Theme](https://github.com/cotes2020/jekyll-theme-chirpy){:target="_blank"} and [Google Cloud](https://cloud.google.com/){:target="_blank"}. The blog has the same setup as [vanSoest.dev](https://vansoest.dev){:target="_blank"}, featuring *viewcounts*, *tags*, *categories*, a *search bar* and a *dark theme* toggle. The blog should provide any developer with a portfolio to easily and estetically collect sharable guides, notes, thoughts and opinions using [Markdown](https://github.github.com/gfm/){:target="_blank"}. What's more, the blog is completely free, and without licensing restrictions.

In an overview, we 
[setup a GitHub Pages repository with Chirpy Jekyll Theme](#markdown-dev-blog--github-pages) and
[configure *viewcounts*, *analytics* and a *CDN* with Google Cloud](#cdn-and-analytics--google-cloud).

## Markdown Dev Blog \| GitHub Pages

*Source: [Getting Started \| Chirpy](https://chirpy.cotes.info/posts/getting-started/){:target="_blank"}*

In the steps below, we setup our main dev blog website. In short, we set up a [GitHub Pages](https://pages.github.com/){:target="_blank"} repository with the [Chirpy Jekyll Theme](https://github.com/cotes2020/jekyll-theme-chirpy){:target="_blank"}. Optionally, we may set a custom domain with a certificate for *https*.

### Creating a GitHub Account

In order to continue, we need a [GitHub](https://github.com/){:target="_blank"} account, [sign up here](https://github.com/signup){:target="_blank"}. In the steps below, the *GitHub account name* is refered to as `<github_username>`. Moreover, in case a [custom domain](#custom-domain) is not desired, note that the blog url will match `<github_username>.github.io`.

### Setting up GitHub Pages
We use [Chirpy Jekyll Theme](https://github.com/cotes2020/jekyll-theme-chirpy){:target="_blank"} as our [GitHub Pages](https://pages.github.com/){:target="_blank"} theme. As mentioned by [Getting Started \| Chirpy](https://chirpy.cotes.info/posts/getting-started/){:target="_blank"}, there are two ways to set up the blog.

- [Using the Chirpy Starter](https://github.com/cotes2020/chirpy-starter/generate){:target="_blank"}

- [Forking on GitHub](https://github.com/cotes2020/jekyll-theme-chirpy/fork){:target="_blank"}

For simplicity, we use the first step. Accordingly, follow the steps below. 

1. [Sign in](https://github.com/login){:target="_blank"} on GitHub.

2. [Create a new repository from chirpy-starter](https://github.com/cotes2020/chirpy-starter/generate){:target="_blank"}.

    - Set the *Owner* to the GitHub user `<github_username>`.
    - Set the *Repository Name* to `<github_username>.github.io`.
    - Set to public (default).
    - Uncheck *Include all branches* (default).

    ![Screenshot when creating a new repository from chirpy-starter](/posts/2021-12-08-creating-a-dev-blog/create-a-new-repository-from-chirpy-starter.png)

3. Now, we have successfully created the repository. [Clone](https://github.com/git-guides/git-clone) the repository locally.
    
    >If you are unfamiliar with *git*, checkout [GitHub Git Guide](https://github.com/git-guides){:target="_blank"} and [git-scm](https://git-scm.com/){:target="_blank"}.

4. In the local repository folder, set the value for property `url` of `_config.yml` to `https://<github_username>.github.io`, then [push](https://github.com/git-guides/git-push){:target="_blank"} the change to trigger a [GitHub Action](https://docs.github.com/en/actions){:target="_blank"} to publish the blog.

6. Head to `https://github.com/<github_username>/<github_username>.github.io/actions`, or *Actions* on the repository. Check the publishing status. After a few minutes, once resolved, a new branch `gh-pages` is added to the repository on [GitHub](https://github.com){:target="_blank"}, which contains the built source of the blog page.

    ![Screenshot of resolved publishing action for the blog page.](/posts/2021-12-08-creating-a-dev-blog/resolved-publishing-action-for-the-blog-page.png)

7. Head to `https://github.com/<github_username>/<github_username>.github.io/settings/pages` or *Settings* > *Pages* on the repository, and set the page source as follows.

    - Set *Branch* to `gh-pages`.
    - Set *Folder* to `/(root)`.
    - Click *Save*.

    ![Screenshot when setting branch gh-page as source.](/posts/2021-12-08-creating-a-dev-blog/set-gh-pages-root-as-source.png)

8. After a few minutes, head to `https://<github_username>.github.io` and view the new website.

With the steps above, we have created the base dev blog website. For the rest of this section, we look into personalisation. In short, we 
[set up a custom domain](#custom-domain) and 
[modify the page's *css* and *favicon*](#page-customisation).

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

    ![Screenshot of setting custom domain](/posts/2021-12-08-creating-a-dev-blog/set-custom-domain.png)

4. Set the value of property `url` of `_config.yml` to the desired domain name (e.g. `https://vansoest.dev`).

5. Type the configured custom domain in the browser, and view the blog.

### Page Customisation

In the steps below, we customise the blog's stylesheet and favicon. To do so, add the folder `/assets/` relative to the project folder. 

For reference, after [customising the css](#css) and [changing the favicon](#favicon) below, the project structure should match the example project structure below.

```txt
📦<github-username>.github.io
 ┣ ...
 ┣ 📂assets
 ┃ ┣ 📂css
 ┃ ┃ ┗ 📜style.scss
 ┃ ┗ 📂img
 ┃ ┃ ┗ 📂favicons
 ┃ ┃ ┃ ┣ 📜android-chrome-192x192.png
 ┃ ┃ ┃ ┣ 📜android-chrome-512x512.png
 ┃ ┃ ┃ ┣ 📜apple-touch-icon.png
 ┃ ┃ ┃ ┣ 📜browserconfig.xml
 ┃ ┃ ┃ ┣ 📜favicon-16x16.png
 ┃ ┃ ┃ ┣ 📜favicon-32x32.png
 ┃ ┃ ┃ ┣ 📜favicon.ico
 ┃ ┃ ┃ ┣ 📜mstile-150x150.png
 ┃ ┃ ┃ ┗ 📜site.webmanifest
 ┣ ...
```

#### CSS

We customise the stylesheet with a new *SCSS* file. With this file, we overwrite and add *CSS* rules.

1. Relative to the project folder, create folder `/assets/css/` and file `/assets/css/style.scss`.

2. Create a new file `assets/css/style.scss`, and add the contents of Chirpy's [style.scss](https://raw.githubusercontent.com/cotes2020/jekyll-theme-chirpy/master/assets/css/style.scss){:target="_blank"}.

In this new file, any css customisation we may add under the file's contents. For further reference, consider the [avatar size modification](https://github.com/larsvansoest/larsvansoest.github.io/blob/88bbbe415c93c904c49b0f0c87ea4a836be7ff14/assets/css/style.scss#L14){:target="_blank"} of [vanSoest.dev](https://vansoest.dev){:target="_blank"}.

#### Favicon

*Source: [Customize the Favicon \| Chirpy](https://chirpy.cotes.info/posts/customize-the-favicon/){:target="_blank"}*

To change the blog's favicon, prepare a square image of size 512x512 or more, and follow the steps below.

1. Relative to the project folder, create folders `/assets/img/` and `/assets/img/favicons/`.

2. Create new files `assets/img/favicons/browserconfig.xml` and `assets/img/favicons/site.webmanifest`, and add the contents of Chirpy's [browserconfig.xml](https://raw.githubusercontent.com/cotes2020/jekyll-theme-chirpy/master/assets/img/favicons/browserconfig.xml){:target="_blank"} and [site.webmanifest](https://raw.githubusercontent.com/cotes2020/jekyll-theme-chirpy/master/assets/img/favicons/site.webmanifest){:target="_blank"} respectively.

3. Upload the square image to [Real Favicon Generator](https://realfavicongenerator.net/){:target="_blank"}. Scroll down and click *Generate your Favicons and HTML code*. Download the package by clicking *Favicon Package*.

4. Extract and add all `.png` and `.ico` files to `/assets/img/favicons/`.

For reference, check if the folder structure matches the one illustrated [above](#page-customisation).

### Running Locally
 
To preview the blog locally, follow the steps below.

1. Based on the operating system, follow the instructions at [Installation \| Jekyll](https://jekyllrb.com/docs/installation/){:target="_blank"}.

2. Relative to the project folder, execute `bundle install` and `bundle exec jekyll s`.

3. To view the blog, navigate to `localhost:4000` or `127.0.0.1:4000` in the web browser.

> Once satisfied, to publish the changes, do not forget to commit the changes to the remote repository.

## CDN and Analytics \| Google Cloud

In the steps below, we use [Google Cloud](https://cloud.google.com/){:target="_blank"} for *engagement analytics*, *pageviews* and as *content delivery network (CDN)* for our website. The options below are optional. Skip this section to run the blog without these features. 

### Pageviews with Google Analytics

To enable pageviews, we follow the steps in [Enable Google Pageviews \| Chirpy](https://chirpy.cotes.info/posts/enable-google-pv/){:target="_blank"}. However, with the [release of Google Analytics 4](https://support.google.com/analytics/answer/10089681?hl=en){:target="_blank"}, the first step of the guide is outdated. Therefore, we overwrite this step with the instructions below. Subsequently, we continue with the next step of the guide.

1. Head to [analytics.google.com](https://analytics.google.com/){:target="_blank"} and login or click on *Start Measuring*.

2. Create or use an existing account.

3. Create a property, enable the *Create a Universal Analytics* property, click on *Create both a Google Analytics 4 and a Universal Analytics property*.

![Screenshot of creating a universal analytics property](/posts/2021-12-08-creating-a-dev-blog/create-universal-analytics-property.png)

With both the [Google Analytics 4](https://support.google.com/analytics/answer/10089681?hl=en){:target="_blank"} and [Universal Analytics](https://support.google.com/analytics/answer/10269537?hl=en){:target="_blank"} properties created, follow all steps of [Enable Google Page Views \| Chirpy](https://chirpy.cotes.info/posts/enable-google-pv/#create-data-stream){:target="_blank"} starting from *Create Data Stream*.

### CDN with Google Cloud

Last but not least, we set up the *content delivery network (CDN)* for our images. With the steps below, with [Google Cloud](https://cloud.google.com/){:target="_blank"}, we set up a load balancer to cache and deliver imagery for our posts. In the steps below, we use a subdomain of the [custom domain](#custom-domain) to enable *https*.

> For the steps below, if prompted to enable a required api (e.g. *Compute Engine API*), enable it to continue.

#### Creating a Project

For starters, we need a *project* to construct our infrastructure.

1. Navigate to [console.cloud.google.com](https://console.cloud.google.com){:target="_blank"} and login or create an account.

2. Create a new project, and select it as the current project.

#### Creating a Bucket

Below, we set up a *bucket* to store the blog's content and make it public.

1. With the project selected, type `storage` in the search bar and click on *Cloud Storage* and *Create Bucket*.

2. Enter a name for your bucket, and select the desired region(s). Leave the rest of the options as default. Click *Create*.

3. Now, we set the bucket to public. In the bucket details view, click *Permissions* and, click *Add*. Add `allUsers` as principals and select the role `Storage Object Viewer`. When prompted, click *Allow Public Access*.

![Screenshot of setting the bucket access to public](/posts/2021-12-08-creating-a-dev-blog/enable-public-bucket-access.png)

> Enabling public access for a bucket will make its contents accessible to the internet. Be mindful when uploading files.

#### Setting up the Loadbalancer

In the steps below, we setup *cloud CDN* with a load balancer, which can be accessed through the subdomain `loadbalancer` of our custom domain.

1. With the project selected, type `load balancer` in the search bar and click on *Load Balancing*.

2. Click *Create Load Balancer* and *Start Configuration* for *HTTP(S) Load Balancing*. Keep the default options, and continue.

4. Configure the *Load Balancer* as follows.

    - Enter a name for the load balancer.

    - At *Backend configuration*, select the dropdown for *Backend services & backend buckets* and *Create a Backend Bucket*. Enter a name, select [the bucket created above](#creating-a-bucket), select *Enable Cloud CDN*, keep the default options, and click *Create*.

        ![Screenshot of creating a backend bucket](/posts/2021-12-08-creating-a-dev-blog/create-backend-bucket.png)

    - At *Host and path rules*, set mode at *Simple host and path rule*, hosts as *Any unmatched (default)*, paths as *Any unmatched (default)* and at *Backends*, select the backend bucket created above.

    - At *Frontend configuration*, enter a name, and select *https* as protocol. At *certificate*, click *Create a New Certificate*, enter a name, select *Create Google-managed certificate*, enter `loadbalancer.<custom_domain>` at domains, and click *Create*.

5. Review the load balancer's settings, and click *Create*.

6. At the created load balancer's details, find the load balancer's ip-adress. Modify the *DNS* of your custom domain by adding the following rule.

    | Type | Name | Value |
    |:----:|:----:|:-----:|
    | A | loadbalancer | `<loadbalancer_ip>` |

    > It might take around 1 or 2 hours for the DNS and certificate to resolve.

7. In `_config.yml` of the project, set the value for property `image_cdn` to `https://loadbalancer.<custom_domain>`.

#### Upload and Use Imagery

After the steps above, to use images in our posts, we can upload them to our bucket and reference them in our markdown file.

To upload images to the bucket, select the project in [Google Cloud](https://cloud.google.com/){:target="_blank"}. Then, type `storage` in the search bar and click on *Cloud Storage*. Select the [bucket created above](#creating-a-bucket), and set up a file structure as desired. We can reference any uploaded image in our post with `![alt text](/relative/path/to/image)`.

## Notes

### Disqus Comment Sections

The [Chirpy Jekyll Theme](https://github.com/cotes2020/jekyll-theme-chirpy){:target="_blank"} comes with the option to set up a comment section by [Disqus](https://disqus.com/){:target="_blank"}. However, [Disqus](https://disqus.com/){:target="_blank"}' free plan users must show advertisements inside the comment sections. What's more, watermarks from the company can only be removed with a pro plan. Therefore, this feature is left out of the guide. In the future, we might integrate an open-source comment section with the theme.