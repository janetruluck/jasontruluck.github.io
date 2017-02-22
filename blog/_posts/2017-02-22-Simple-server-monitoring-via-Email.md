---
layout: post
title:  "Simple server monitoring via Email"
date:   2017-02-22 18:00:00
categories: server administration raild monitoring mailgun quicktip
---

So I recently setup a basic home media server to store the usual stuff. On this server I setup 2 Raid Arrays:

- ~3 TB 4 disk Raid 5
- ~6 TB 3 disk Raid 5

Both of these raid arrays were setup as a software raid using [MDADM][mdadm]. Nothing special. The server itself is 
completely headless and has ssh access, but I don't want to have to periodically log into the server to check 
the health of the raid arrays. So thinking up some simple ideas email came to mind. Initially I setup [Postfix][postfix]
and got things sending locally, but things quickly fell apart when trying to send to my gmail account. Turns out
gmail requires a bit more (namely TLS support and SASL). After spending a good hour trying it out my time box had \
expired and I started looking into alternatives.

Enter [Mailgun][mailgun] (or really any other of the million email services that support curl). This really should have been
obvious from the start but hey, messing with Postfix for the first time in forever was pretty fun at first. [Mailgun][mailgun] has
some advantages for this situation. As of writing this it has:

- 10,000 Free emails per month, plenty for my dinky home server
- Sending messages via cURL
- Can use a custom domain if you want (but you don't have to)
- No need to enter a credit card

And some cons (sort of):

- Free account (without credit card) requires Authorized Recipients, not really an issue for me since I only want 
to send things to myself. This really isn't an issue for small teams either.
- Limited log retention, again not really an issue for my case
- Limited to 5 domains, not an issue

So creating a [Mailgun][mailgun] account with pretty much no information entered on my part and no credit card 
I get everything I need. Awesome! And that 10,000 email limit might as well be 1,000,000 - I will only be 
sending ~60 per month.

So intros and rationalizations aside, setting up the actual monitoring is dead simple. Like I mentioned before I really
only want to get some info on the health of the raid arrays which simply enough can be done using the command:

{% highlight ruby %}
cat /proc/mdstat
{% endhighlight %}

which gives us something like:

{% highlight %}
Personalities : [raid6] [raid5] [raid4] [linear] [multipath] [raid0] [raid1] [raid10]
md0 : active raid5 sdd[2] sde[4] sdc[1] sdb[0]
      2929893888 blocks super 1.2 level 5, 512k chunk, algorithm 2 [4/4] [UUUU]
      bitmap: 0/8 pages [0KB], 65536KB chunk

md1 : active raid5 sdf[0] sdg[1] sdh[3]
      5860270080 blocks super 1.2 level 5, 512k chunk, algorithm 2 [3/3] [UUU]
      bitmap: 0/22 pages [0KB], 65536KB chunk

unused devices: <none>
{% endhighlight %}

Perfect! Now we have to do is get this sending out to our email, which can be done using the [Mailgun][mailgun] API:

{% highlight ruby %}
curl -s --user 'api:YOUR_API_KEY' \
    https://api.mailgun.net/v3/YOUR_DOMAIN_NAME/messages \
    -F from='Excited User <mailgun@YOUR_DOMAIN_NAME>' \
    -F to=YOU@YOUR_DOMAIN_NAME \
    -F to=bar@example.com \
    -F subject='Raid Update' \
    -F text="$(cat /proc/mdstat)"
{% endhighlight %}

Just fill in the blanks for the [Mailgun][mailgun] api info and you are good to go. The main meat is the section:

{% highlight ruby %}
    -F text="$(cat /proc/mdstat)"
{% endhighlight %}

This just throws the output we have above into the text of the email. Simple. Now all we have to do is 
throw this in a script and get it running automatically, we will use a cron job for this. Just create 
a file like `/home/USER_NAME/bin/raid_update.sh`and add:

{% highlight ruby %}
#!/bin/sh
PATH=/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin

curl -s --user 'api:YOUR_API_KEY' \
    https://api.mailgun.net/v3/YOUR_DOMAIN_NAME/messages \
    -F from='Excited User <mailgun@YOUR_DOMAIN_NAME>' \
    -F to=YOU@YOUR_DOMAIN_NAME \
    -F to=bar@example.com \
    -F subject='Raid Update' \
    -F text="$(cat /proc/mdstat)"
{% endhighlight %}

You can test this script just to make sure it works using: `sh /home/USER_NAME/bin/raid_update.sh`. It 
is nice to throw the `PATH` in there just so cron can use what it needs. Nothing special here though. 
Now make a crontab job by running: `crontab -e` and add:

{% highlight ruby %}
30 9 * * *   root    /bin/bash /home/USER_NAME/bin/raid_update.sh
{% endhighlight %}

Now every morning at 09:30 you should get an email telling you how your raid array is doing! This could 
easily be made prettier and monitor more things, pretty much any command you can run in the terminal is just
being forwarded to your email. Nice.

[mdadm]: https://linux.die.net/man/8/mdadm
[postfix]: http://www.postfix.org/postfix-manuals.html
[mailgun]: https://www.mailgun.com/
