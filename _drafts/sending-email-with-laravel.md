---
layout: post
title: Sending Email with Laravel
permalink: "/sending-email-with-laravel/"
---
There may be times when you want to automatically send an email when something has happened in your app; maybe a user published a blog post and you want to send them a message saying that their new post is live. You may also want to receive an email yourself if something you deem important has taken place; maybe users can purchase items and you want to be notified whenever that happens.  

[Laravel](https://laravel.com/) is a framework for creating web applications using PHP, and this post will go over how to send emails when your app's models change by using Observer and Mailable classes.  

---