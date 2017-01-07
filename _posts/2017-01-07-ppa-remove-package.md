---
layout: post
title:  "Remove PPA from apt-get update sources"
date:   2017-01-07 13:20:45 +0300
categories: linux ppa
---
For custom PPAs, you might face an issue during the `apt-get update` in which you get an error for not being able to fetch the package from the source. However, packages such RVM does require a clean source update file where there should be no errors at all.

Let's first list down the updae errors:

{% highlight bash %}
sudo apt-get update | grep -i "Failed"
{% endhighlight %}

For demonestration purposes, here's a sample error:

{% highlight bash %}
W: Failed to fetch http://ppa.launchpad.net/groovy-dev/groovy/ubuntu/dists/trusty/main/binary-amd64/Packages  404  Not Found

W: Failed to fetch http://ppa.launchpad.net/groovy-dev/groovy/ubuntu/dists/trusty/main/binary-i386/Packages  404  Not Found
{% endhighlight %}

It seems that Groovy sources aren't available at this time due to the 404 error mentioned there. Now, simply were remove these "Not Found" PPAs using the following:

{% highlight bash %}
sudo add-apt-repository --remove ppa:groovy-dev/groovy
{% endhighlight %}

Please keep in mind that PPA url contains the `http://ppa.launchpad.net/TEAM-NAME/PPA-NAME` followed by the OS/distro information. When you remove the PPA, make sure that you follow the correct naming convension e.g. `ppa:TEAM-NAME/PPA-NAME`

Check out the [Jekyll docs][jekyll-docs] for more info on how to get the most out of Jekyll. File all bugs/feature requests at [Jekyll’s GitHub repo][jekyll-gh]. If you have questions, you can ask them on [Jekyll Talk][jekyll-talk].

[jekyll-docs]: http://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/
