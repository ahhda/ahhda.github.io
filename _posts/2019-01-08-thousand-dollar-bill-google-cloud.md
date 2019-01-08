---
layout: post
title: We messed up and racked a $4,000 bill with GCE, tips on how you can prevent that
date: 2019-01-08
author: anuj
categories: [cloud, gce]
image: assets/images/lost_cloud.jpg
featured: true
hidden: true
---

`2nd January 2019`, I was sitting there wondering how 2018 felt like a few days rather than 12 months, when I receive a message on my phone:

![sms_message]({{ site.baseurl }}/assets/images/sms_message.jpeg)

Thats right more than `$1000` were just cut from my account by Google Cloud. I thought this might have been a mistake and quickly turn to my laptop, open my google cloud account to find even more horrifying news. All our Start up credits (worth `$3000`) had vanished plus we owed google `$1000` which just got deducted from my account.

So WTF just happened? Well, lets rewind a little bit. Last year we had applied for a program known as [Google Cloud for Startups](https://cloud.google.com/developers/startups/) where if you are selected you receive GCP credits worth `$3000`. 

Well, we got in and now we had a lot of money so that we could freely experiment with all the services provided by Google Cloud. And we did! We played around with App Engine, Compute Engine, Cloud functions, Cloud Storage, Cloud SQL, VPC network, Cloud Builder and what not. Within a month or two we had 3 google projects each running atleast 5 of the services mentioned here. We kept eyes on our bills for first 2 months and they came out to be in the range of $20-$40 each month. This was nothing in comparison to what we had, so we stopped caring about the monthly bills anymore.

And this is where we were mistaken, on `3rd November, 2018` I was experimenting with NLP libraries when I found [DrQA](https://github.com/facebookresearch/DrQA). DrQA is an awesome system for reading comprehension applied to open-domain question answering. It is basically "machine reading at scale" (MRS). It has a document retriever which is `~13GB` in size and some pre trained model which are `~25GB` when untarred. So I quickly spin up a Compute Engine VM instance and start the training, but I quickly run out of space and memory. I try increasing the space, then the memory and it still ran out of it. Frustrated I created a new VM instance with more than `~250GB` space (and this is SSD we are talking about), a lot of RAM, 16 vCPUs and something I shouldn't have added. A GPU. Which one? `NVIDIA Tesla P100`!

Well the rest is just history.

# Some valuable lessons:

No matter if you are a devops guy, a startup co founder, just someone experimenting with the cloud or a 5 year old, just remember these tips:

### 1. Always setup billing budgets, ALWAYS!

Most of the cloud providers have something known as [Budget Alerts](https://cloud.google.com/billing/docs/how-to/budgets). This is to help you with project planning and controlling costs. Setting a budget alert lets you track how your spend is growing toward a particular amount. Go set this up.

### 2. Never let your secret keys wander in the open

![github_meme]({{ site.baseurl }}/assets/images/github_meme.jpg)

There have been a lot of cases([1](https://wptavern.com/ryan-hellyers-aws-nightmare-leaked-access-keys-result-in-a-6000-bill-overnight), [2](https://dev.to/juanmanuelramallo/i-was-billed-for-14k-usd-on-amazon-web-services-17fn), [3](https://www.theregister.co.uk/2015/01/06/dev_blunder_shows_github_crawling_with_keyslurping_bots/)) especially on AWS where people commited their secret keys on open source projects. Do not do this! Make sure you check what you commit and try to have a `.gitignore` file.

### 3. Never set easy passwords for anything

Hackers are always running bots which are basically trying to access common ports on cloud providers IP addresses with common usernames and passwords like `admin` and `root`. Definitely avoid using usernames and passwords like [these](https://github.com/danielmiessler/SecLists). This also brings us to our 4th point.

### 4. Try to avoid opening network access to any IP

Network/IP filters are very important. Avoid opening your network firewalls to something like: `0.0.0.0/0`. I know you are in a hurry, I know you have to get that MVP up ASAP, but taking this extra step will save you a lot of time and effort for the future and also keep you secure

That’s all! Go ahead and Save your money!
