---
layout: post
title:  "Adding Bootstrap Progress Bars for Refile"
date:   2015-04-07 17:48:40
categories: bootstrap refile images ui
---
<br/>

###Overview

The [Refile][refile-repo] gem is an awesome new file upload gem brought to us by jnicklas and the people over at Elabs.
Depending on the situation and use case this can be an awesome replacement for [Carrierwave][carrierwave-repo]. It offers a
lot of really  cool features like:

* out of the box on the fly file manipulation
* direct uploads
* a javascript library to hook into events for the file

This will be a quick tutorial covering the integration of the [Refile][refile-repo] gems
Javascript API hooks with [Bootstrap 3's][bootstrap-repo] progress bars in a [Rails][rails-repo] application.
This will allow us to make some nice looking upload pages that provide great feedback to users, especially for large file.
This will work with any ruby application (not just rails) as long as Bootstrap and Refile are supported.

Gems used:

* [Bootstrap][bootstrap-repo]
* [Refile][refile-repo]
* [Rails][rails-repo]

### Adding the Bootstrap Progress Bar

After adding Refile and Bootstrap to your Rails project you are ready to start adding your progress bars. Within your form that
you have an `attachment_field` that you would like to track progress add a progress bar like so:

```html
<div class="progress">
  <div class="progress-bar" role="progressbar" aria-valuenow="60" aria-valuemin="0" aria-valuemax="100" style="width: 60%;">
    <span class="sr-only">60% Complete</span>
  </div>
</div>
```

{% highlight ruby linenos %}
config.before(:all) do
  DeferredGarbageCollection.start unless ENV["SKIP_DGC"]
end

config.after(:all) do
  DeferredGarbageCollection.reconsider unless ENV["SKIP_DGC"]
end
{% endhighlight %}

[bootstrap-repo]: https://github.com/twbs/bootstrap
[refile-repo]: https://github.com/refile/refile
[rails-repo]: https://github.com/rails/rails
[carrierwave-repo]: https://github.com/carrierwaveuploader/carrierwave
