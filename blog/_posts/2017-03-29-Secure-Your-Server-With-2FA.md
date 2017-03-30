---
layout: post
title:  "Secure Your Server with 2FA"
date:   2017-03-29 18:00:00
categories: server administration security two-factor
---

Being the tinfoil hatter that I am and after realizing (unsurprisingly) my home
server was getting bombarded with SSH attempts I decided to implement Two
Factor authentication just for some peace of mind. The server itself is already
hardened a bit with a sane SSH config, fail2ban and iptables but you can never
be too careful right?

The merits of 2FA are already well documented and this was so easy that I see
no reason not to do it, especially on production systems. I already use Google
Authenticator for my other 2FA needs and was pleasantly surprised to find out
they also have an awesome PAM module `libpam-google-authenticator`.

Just a quick note, this guide is written under the assumption of an
Ubuntu/Debian based system but the instructions should vary little for other
OS's. I will also assume we have a fresh SSH config so we can walk through that
a bit too.

First things first we need to install the package and run through the setup:
{% highlight ruby %}
sudo apt-get install libpam-google-authenticator
{% endhighlight %}

*Don't forget to copy down the emergency keys!*

Next up we have to edit or `pam.d` file for SSH to require the new package. To
do this we want to modify `/etc/pam.d/sshd` using your editor of choice and
add this to the *top* of the file:
{% highlight ruby %}
auth required pam_google_authenticator.so
{% endhighlight %}

It is important to note that you should definitely use `required` here. You can
take a look at the [PAM][pam] docs for the
different options but required holds the benefit of not only bouncing a user
out if they fail this method but it does not immediately reveal that was the
method that bounced them, adding a but more over `requisite`.

Next up we want to update our `/etc/ssh/sshd_config` adding/modifying:

{% highlight ruby %}
ChallengeResponseAuthentication yes
UsePAM yes
{% endhighlight %}

Now assuming this is a fresh installation of SSH and you are just using
username and password authentication this will work without any problems. But
lets be honest, you shouldn't be doing that when public keys are so easy to
setup. Once you generate your public key head beack to the
`/etc/ssh/sshd_config` and add/change these options:

{% highlight ruby %}
PasswordAuthentication no # Disables password authentication
PermitEmptyPasswords no # Prevents empty passwords, shouldn't matter now but eh
PermitRootLogin no # Prevents root user from authenticating
{% endhighlight %}

Now you have your server setup with SSH public key authentication and snazy 2FA.
Your so excited to try it out so you disconnect and try SSHing in only to find
that you are never prompted for a token... thats not right. Unfortunately
public key auth does not play well with PAM, it pretty much just bypasses it
(public keys are secure right??). Well fret not, there is a solution. You will
have to update to at least `OpenSSH v6.2` for this but you should already be
there. Next add this to your `/etc/ssh/sshd_config`:

{% highlight ruby %}
AuthenticationMethods publickey,keyboard-interactive:pam
{% endhighlight %}

The `,` here is important. This tells ssh to run these check sequentially
allowing us to use both publickey and 2FA authentication.

So now if we give this a go you should see a nice prompt for your token after
your public key auth is done. Success!!

[pam]: https://linux.die.net/man/5/pam.d
