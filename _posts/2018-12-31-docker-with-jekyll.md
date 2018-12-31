---
layout: post
title: Running Jekyll locally with Docker
date: 2018-12-31
author: anuj
categories: [docker, jekyll]
image: assets/images/jekyll_docker.png
featured: false
hidden: false
---

This website is built with static site generator Jekyll and hosted on GitHub pages. 

There are a number of reasons why this combination is awesome:

- I can write everything in Markdown
- The workflow has source control baked in
- The website is automatically updated by pushing to master

But using Jekyll has its own downsides, for example you have to make sure you have the correct version of Ruby and some gems installed. Setting it up takes a couple of steps and installations. This is where Docker comes into play.

I have been using docker since one year now and I am absolutely in love with it.  You can run Jekyll from inside a Docker container, you won’t have to bother with installing a specific version of Ruby or with different versions of gems you already had installed potentially clashing. You could of course use the Ruby Version Manager, but working with Docker really is a breeze.

The rest of this post assumes you have Docker and Docker Compose installed and have some basic knowledge of working with it. Docker has some great getting started guides for [Linux](https://docs.docker.com/linux/), [Mac](https://docs.docker.com/mac/) and [Windows](https://docs.docker.com/windows/).

There is a Docker image available on [Docker Hub](https://hub.docker.com/r/jekyll/jekyll/). Let’s take advantage of that! Suppose you have downloaded or cloned the jekyll repository into  `~/jekyll-site/`. To use the Jekyll image without having to pass in the required options every time we would create a container and will make use of Docker Compose. Create a file called  `docker-compose.yml` in `~/jekyll-site` with these contents:

```dockerfile
jekyll:
    image: jekyll/jekyll
    command: jekyll serve --watch --drafts --incremental
    ports:
        - 4000:4000
    volumes:
        - .:/srv/jekyll
```

- With `image: jekyll/jekyll:pages` you indicate you want to use Jekyll’s Jekyll image tagged with “pages”. This is a specific image suited for GitHub pages.
- The part after `command:` is the command to execute in the container. The `jekyll serve` command starts Jekyll’s builtin development web server. The options `--watch` and `--incremental` instruct Jekyll to automatically regenerate the HTML when a file is changed. `--drafts` lets you view posts those are in the `_drafts` directory.
- `4000:4000` forwards port 4000 of the container to your local port 4000.
- Finally, on the last line you map the current directory (`~/jekyll-site/`) to `/srv/jekyll/`. That’s where the images is configured to go looking for a Jekyll site.

That’s all! You can start your Jekyll site by browsing to your site’s directory using a terminal and doing `docker-compose up`.

If you go to [http://0.0.0.0:4000](http://0.0.0.0:4000/) (or the IP of your VM if you’re running Docker on OS X or Windows) in a browser you’ll see your Jekyll site. Easy, right?

When you want to shut-down the site, use `Ctrl+C` then run `docker-compose down`.