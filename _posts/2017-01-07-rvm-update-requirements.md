---
layout: post
title:  "RVM update requirements script"
date:   2017-01-07 13:20:45 +0300
categories: linux ruby rvm
---

I usually use RVM for managing different versions of Ruby installations in my machine. RVM typically requires a sucessful update of your system prior to installing Ruby. To be specific, in Debian there is a function `requirements_debian_update_system` that updates the system automatically and in case of any errors, the installation process will break. This fuction resides in a **bash** script and usually it takes the following path in your **home** directory. In my home directory, here is the path of the requirements directory which has the requirements of ubuntu distro in the `ubuntu` bash script:

`/YOURHOMEDIR/.rvm/src/rvm/scripts/functions/requirements/ubuntu`

`apt-get update` returns 0 on normal operatoins while it returns 100 on error. This piece of information is stated clearly in `man apt-get` diagnoastics's section:

{% highlight bash %}
apt-get returns zero on normal operation, decimal 100 on error
{% endhighlight %}

Here's a snapshot of the function code ([on github]):

{% highlight bash %}
requirements_debian_update_system()
{
  # execute the update command and if not successful "using the doublepipe operator", execute the following block.
  __rvm_try_sudo apt-get --quiet --yes update ||
  {
    # get the return value of the update command using '$?'
    \typeset __ret=$?
    case ${__ret} in
      (100)
        rvm_error "There has been error while updating 'apt-get', please give it some time and try again later.
404 errors should be fixed for rvm to proceed. Check your sources configured in:
    /etc/apt/sources.list
    /etc/apt/sources.list.d/*.list
"
        ;;
    esac
    return ${__ret}
  }
}
{% endhighlight %}

By looking at the code, you have either to fix your update source in order to get `0` return value ofthe update system function or bypass the code in the function in order to skip the update verification.

[on github]: https://github.com/rvm/rvm/blob/bee47675c34a4ba96f849017ea06ce00efcbfc7a/scripts/functions/requirements/ubuntu#L39