---
layout: post
title:  "Continuous Testing in the Browser"
date:   2013-10-26
categories: tools
---

When developing in JavaScript I think it is beneficial to have short feedback cycles. For instance, every time I change or write a unit test I want to see the results immediately. The same applies for changes in the application itself.

I came across a handy tool named [xdotool][xdotool]. It simulates keyboard or mouse presses and sends them to a window.

The following bash script sends a reload command to a window named Jasmine Spec Runner every time a file is changed in the directories src and spec.

{% highlight bash %}
#!/bin/bash

while true; do
  xdotool search --name "Jasmine Spec Runner" key --clearmodifiers "CTRL+R";
  inotifywait -r -qq -e modify -e delete src spec;
done
{% endhighlight %}

Both xdotool and inotifywait work on Linux based systems. However, equivalent tools or ports may exist for Windows or Mac OS X.

[xdotool]: https://www.semicomplete.com/projects/xdotool/
