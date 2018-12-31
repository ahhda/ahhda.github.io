---
title: Getting HTTPS on AWS using ngrok
date: 2017-01-20 21:46:00 +05:30
author: anuj
tags:
- jugaad
- ngrok
- https
layout: post
---

Recently while working on a messenger bot project I required an HTTPS server (Facebook requires the endpoint to run over https). But enabling encrypted HTTPS on an AWS web server is a time-consuming task.

### Ngrok to the rescue!
Ngrok is a tool that helps you set up secure tunnels to localhost. Ngrok gives web accessible URLs and tunnels all traffic from that URL to our localhost! Easy, peasy!

### Getting HTTPS on AWS
Download the ngrok tool by running the below command on your server:

```
wget https://bin.equinox.io/c/4VmDzA7iaHb/ngrok-stable-linux-amd64.zip
unzip ngrok-stable-linux-amd64.zip
```

Now we need to run the ngrok tool in the background.
Use the below command to run in background:

```
./ngrok http 8000 > /dev/null &
```

Awesome! Now we have HTTPS tunneling to our localhost port 8000. But what is the URL?

To get the urls type curl into your localhost port 4040.

```
curl  http://localhost:4040/api/tunnels
```

You can find the HTTP and HTTPS links in the output.

Enjoy your HTTPS tunnel. 
