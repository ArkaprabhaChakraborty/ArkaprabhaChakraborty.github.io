+++
showdate = true
title = "Setup a lovely little portfolio with hugo"
date = 2022-08-02
url = "/blog/portfolio_with_hugo"
+++

Hugo is an amazing tool. It's a powerful static site generator that can be used to create a beautiful websites. Hugo, is world's fastest static website engine. What I like about Hugo is it's speed and flexibility. 

For my blog I can just use some simple Markdown files, and Hugo build the site for me. This helps me to create a blog that is easy to manage. For starters I followed the [Hugo quick start tutorial](https://gohugo.io/getting-started/quick-start/) to setup a my portfolio website.

To start with hugo you need to install go. You can download the latest version of go from this [link](https://go.dev/dl/). After that follow the instructions on go website and install it [there is no point to write about how to install go here]. 

After installing go, you can install hugo. I have built hugo from source and you can do the same by running the following commands:

```bash
mkdir $HOME/src
cd $HOME/src
git clone https://github.com/gohugoio/hugo.git
cd hugo
go install
```

If you ever wanna "find" Hugo, you can find it whithin the src folder in your HOME directory, provided you followed the commands given above.

Now you can setup your special github repository, having the name `<Your Username>.github.io`, clone the repo and setup a hugo site using the following commands:

```bash
cd /path/to/<Your Username>.github.io
hugo new site .
``` 

This initiates a new site in the current directory. Now you can use any theme to decorate your site and start writing your content. I'll use archie theme for this tutorial. Credits [Athul Cyriac Ajay](https://github.com/athul).
What I did was copy all of content archie theme's layouts directory to my Hugo site's layouts directory, all content from archie's assets to my site's assets dircetory and all the static files from archie's static directory to my site's static directory. Simple :)


For blogs everything goes into the cThe ```publish_dir``` is the directory that you want to deploy.ontent folder. For example, if I want to create a blog called "My first blog" I can create a file called `myfirstblog.md` in the content folder. It's not necessary that the file name ends with `.md` but I used an md :).

I have two sections for my portfolio, blog and posts. These are some default directories auto-parsed by Hugo. I did this by adding the follwing to my setup.toml

```toml
[[menu.main]]
name = "Blogs"
url = "/blog"
weight = 2

[[menu.main]]
name = "Projects"
url = "/posts"
weight = 3
```

Since I have two sections I need an `_index.md` file in each of them. For more info see the content sections in Hugo [here](https://gohugo.io/content-management/sections/). 

We are more or less set. Yaay!
Now to publish your portfolio with github pages, you need to set up github actions. If you wanna manually publish your site, you can do that too. You can do that using the following commands:

```bash
hugo build 
```

This will start the build process. It will create a public directory called `public` in the current directory. This will contain your static site. You can copy the contents of this directory to your github pages branch. If you are going for this "easy" way trust me won't be easy for you in near future as updating your site would be difficult.

This is why I recommend you to use github actions. It's a simple and easy way to publish your site. Mainly we will be following the Hugo [docs](https://gohugo.io/hosting-and-deployment/hosting-on-github/) again. 

To start with github actions you need to create a .github/ workflow directory from your repository root ie. your protfolio websites's repository root. This directory will contain all the files that are required to run github actions. Now create a file named `gh-pages.yml` in this directory.

```yaml
name: GitHub Pages

on:
  push:
    branches:
      - main  # Set a branch to deploy
  pull_request:

jobs:
  deploy:
    runs-on: ubuntu-20.04
    concurrency:
      group: ${{ github.workflow }}-${{ github.ref }}
    steps:
      - uses: actions/checkout@v3

      - name: Setup Hugo
        uses: peaceiris/actions-hugo@v2
        with:
          hugo-version: '0.91.2'
          # extended: true

      - name: Build
        run: hugo --minify

      - name: Deploy
        uses: peaceiris/actions-gh-pages@v3
        if: ${{ github.ref == 'refs/heads/main' }}
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./public
```

Alrighty then! You are ready to go. 

# *WAIT WAIT WAIT!!!!* 

You don't know what the hell is actually happening in the gh-pages.yml file! Never trust blogs blindly :). Who knows what malicious intent the person writing the blog has :P.

Let's take you line by line through the yaml file. 
For the first part
```yaml
name: GitHub Pages
```
This is the workflow name. You can set to anything you want :). Really :).

The second section deals with the branches and "actions". 
```yaml
on:
  push:
    branches:
      - main  # Set a branch to deploy
  pull_request:
```
This is the "on" section. Here you can set the branches that you want to deploy. You can set multiple branches. I have chosen the main branch because I mostly don't work on main. I create to new branch to make changes and then merge them to main :) "A GOOD practice" :)

So whenever there is "push" to main branch, the deploy will be triggered. Now its not a good practice to run deployments directly, but it's aa blogpost site :), so I didn't wanna overcomplicate things.

Now comes the jobs section. This is where you can set the jobs. You can set multiple jobs. I have only one job which is deploy job. 
```yaml
jobs:
  deploy:
    runs-on: ubuntu-20.04
    concurrency:
      group: ${{ github.workflow }}-${{ github.ref }}
```
So the delpymnet runs on ubuntu-20.04. This is the OS that I have in my local setup too. You can set to anything you want. Then I have added a job level concurrency. This will allow other jobs in the workflow to proceed even if the concurrent job is ``pending``. 

The next section deals with the steps that are followed by the job. At first I create a local copy/snapshot of the current main branch using the [checkout](https://github.com/actions/checkout) action. This action checks-out your repository under ```$GITHUB_WORKSPACE```, so your workflow can access it. 
```yaml
    steps:
      - uses: actions/checkout@v3
```
Next we run a Setup Hugo action. This action will install Hugo and set up the hugo environment. Here I'm using the older Hugo version. You can set it whatever you want. For this we use the [actions-hugo](https://github.com/peaceiris/actions-hugo). Kudos to [peaceiris](https://github.com/peaceiris) for his/her/their awesome work.

```yaml
      - name: Setup Hugo
        uses: peaceiris/actions-hugo@v2
        with:
          hugo-version: '0.91.2'
          # extended: true
```
Next we run our Build action. This action will build the minified static site. I have chosen minify, you can choose not to opt for it. 
```yaml
      - name: Build
        run: hugo --minify
```
Next we run our Deploy action. This action will deploy the site to GitHub Pages. Here I'm using the [actions-gh-pages](https://github.com/peaceiris/actions-gh-pages). 
```yaml
      - name: Deploy
        uses: peaceiris/actions-gh-pages@v3
        if: ${{ github.ref == 'refs/heads/main' }}
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./public
```
The ```GITHUB_TOKEN``` is a unique token that GitHub automatically creates a unique GITHUB_TOKEN secret to use in your workflow. You can use the GITHUB_TOKEN to authenticate in a workflow run. When you enable GitHub Actions, GitHub installs a GitHub App on your repository. The GITHUB_TOKEN secret is a GitHub App installation access token. You can use the installation access token to authenticate on behalf of the GitHub App installed on your repository.
For more info see the [automatic token authentication](https://docs.github.com/en/actions/security-guides/automatic-token-authentication).

So in this section we take the ```public``` directory and deploy it to GitHub Pages. After that we trigger github pages deployment.

Yep that's it :). Please note that you have to change github pages default branch to gh-pages after your first push. If you enjoyed it then do follow me on twitter and on github :P.