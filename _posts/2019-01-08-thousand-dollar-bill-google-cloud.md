---
layout: post
title: We messed up and racked a $4,000 bill with GCE, tips on how you can prevent that
date: 2019-01-08
author: anuj
categories: [cloud, gce]
image: assets/images/lost_cloud.jpg
featured: false
hidden: false
---

`2nd January 2019`, I was sitting there wondering how 2018 felt like a few days rather than 12 months, when I receive a message on my phone:

![sms_message]({{ site.baseurl }}/assets/images/sms_message.jpeg)

That's right, more than `$1000` were just deducted from my account by Google Cloud. I thought this might have been a mistake and quickly turned to my laptop, opened my Google cloud account to find even more horrifying news. All our Start Up credits (worth `$3000`) had vanished plus we owed Google `$1000` which just got deducted from my account.

So WTF just happened? Well, lets rewind a little bit. Last year we had applied for a program named [Google Cloud for Startups](https://cloud.google.com/developers/startups/) where you receive GCP credits worth `$3000` if you are selected.

Well, we got in and that gave us a lot of money so that we could freely experiment with all the services provided by Google Cloud. And we did! We played around with App Engine, Compute Engine, Cloud functions, Cloud Storage, Cloud SQL, VPC network, Cloud Builder and what not. Within a month or two, we had 3 projects each running atleast 5 of the services mentioned above. We kept eyes on our bills for the first 2 months and they came out to be in the range of $20-$40 each month. That was nothing in comparison to what we had in our credits, so we stopped caring about the monthly bills anymore.

And this is where we were mistaken. On `3rd November, 2018`, I was experimenting with NLP libraries when I found [DrQA](https://github.com/facebookresearch/DrQA). DrQA is an awesome system for reading comprehension applied to open-domain question answering. It is basically "machine reading at scale" (MRS). It has a document retriever which is `~13GB` in size and some pre trained models which are `~25GB` when untarred. So I quickly spun up a Compute Engine VM instance and started the training, but soon I ran out of space and memory. I tried increasing the space, then the memory and it still ran out of it. Frustrated, I created a new VM instance with more than `~250GB` space (and this is SSD we are talking about), a lot of RAM, 16 vCPUs and something I shouldn't have added. A GPU. Which one? `NVIDIA Tesla P100`!

I played around with the VM, trained the models and did some testing. Soon, I realized this is not what I was looking for and went to sleep. I FORGOT to turn the instance DOWN! Well, the rest is just history. We ended up with a 3 month bill adding upto a total worth of `$4000` for something we just used for a day.

# Some valuable lessons:

No matter if you are a devops guy, a startup co-founder, just someone experimenting with the cloud or a 5 year old, just remember these tips:

### 1. Always setup billing budgets, ALWAYS!

Most of the cloud providers have something known as [Budget Alerts](https://cloud.google.com/billing/docs/how-to/budgets). This is to help you with project planning and controlling costs. Setting a budget alert lets you track how your spend is growing toward a particular amount. Go set this up.

### 2. Never let your secret keys wander in the open

![github_meme]({{ site.baseurl }}/assets/images/github_meme.jpg)

There have been a lot of cases([#1](https://wptavern.com/ryan-hellyers-aws-nightmare-leaked-access-keys-result-in-a-6000-bill-overnight), [#2](https://dev.to/juanmanuelramallo/i-was-billed-for-14k-usd-on-amazon-web-services-17fn), [#3](https://www.theregister.co.uk/2015/01/06/dev_blunder_shows_github_crawling_with_keyslurping_bots/)) especially on AWS where people commited their secret keys in open source projects. Do not do this! Make sure you check what you commit and try to have a `.gitignore` file.

### 3. Never set easy passwords for anything

Hackers are always running bots which are basically trying to access common ports on cloud provider's IP addresses with common usernames and passwords like `admin` and `root`. Definitely avoid using usernames and passwords like [these](https://github.com/danielmiessler/SecLists). This also brings us to our 4th point.

### 4. Try to avoid opening network access to any IP

Network/IP filters are very important. Avoid opening your network firewalls to something like: `0.0.0.0/0`. I know you are in a hurry, I know you have to get that MVP up ASAP, but taking this extra step will save you a lot of time and effort for the future and will also keep you secure.

### 5. Monitor and turn it down

Once in a while, do monitor your instance health for problems and check if you actually need that instance or not. If not, it's best to just stop it, restarting it wouldn't take much time and you get all your data back. Plus you will not be billed for something you didn't use.

That’s all! Go ahead and make sure your money is safe.
