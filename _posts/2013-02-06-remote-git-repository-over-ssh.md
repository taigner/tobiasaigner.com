---
layout: post
title:  "Remote git Repository Over ssh"
date:   2013-02-06
categories: tools
---

A feature of git I think is useful sometimes is the ability to utilize the ssh protocol. Therefore, it is possible to work with a remote repository by using an existing ssh configuration.

## Server-Side

On the server-side a [bare repository][baregitrepo] has to be created

{% highlight bash %}
$ mkdir remote-project.git
$ cd remote-project.git
$ git init --bare
{% endhighlight %}

## Client-Side

In contrast, on the client side it is now possible to clone the remote repository

{% highlight bash %}
$ git clone ssh://user@server/path/to/remote-project.git
{% endhighlight %}

Afterwards, changes can be pushed to the server:

{% highlight bash %}
$ cd remote-project
<add and commit your changes>
$ git push origin master
{% endhighlight %}

<p></p>

[baregitrepo]:    https://www.kernel.org/pub/software/scm/git/docs/gitglossary.html
