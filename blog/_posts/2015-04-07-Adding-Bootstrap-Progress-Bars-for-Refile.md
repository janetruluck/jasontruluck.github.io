---
layout: post
title:  "Adding Bootstrap Progress Bars for Refile"
date:   2015-04-07 17:48:40
categories: bootstrap refile images ui
---
<br/>

###Overview

The [Refile][refile-repo] gem is an awesome new file upload gem brought to us
by jnicklas and the people over at Elabs. Depending on the situation and use
case this can be an awesome replacement for [Carrierwave][carrierwave-repo]. It
offers a lot of really  cool features like:

* out of the box on the fly file manipulation
* direct uploads
* a javascript library to hook into events for the file

This will be a quick tutorial covering the integration of the
[Refile][refile-repo] gems Javascript API hooks with [Bootstrap
3's][bootstrap-repo] progress bars in a [Rails][rails-repo] application. This
will allow us to make some nice looking upload pages that provide great
feedback to users, especially for large file. This will work with any ruby
application (not just rails) as long as Bootstrap and Refile are supported.

Gems used:

* [Bootstrap][bootstrap-repo]
* [Refile][refile-repo]
* [Rails][rails-repo]

### Adding the Bootstrap Progress Bar

After adding Refile and Bootstrap to your Rails project you are ready to start
adding your progress bars. Within your form that you have an `attachment_field`
that you would like to track progress add a progress bar like so:

```html
<div class="progress">
  <div class="progress-bar" role="progressbar" aria-valuenow="0" aria-valuemin="0" aria-valuemax="100" style="width: 0%;">
    <span class="sr-only">0% Complete</span>
  </div>
</div>
```

### Adding the Refile JS library

Now you just need to add the Refile JS library to the project. You will want to
include this after `jquery` and `turbolinks`

```js
# app/assets/javascripts/application.js
//= require jquery
//= require jquery_ujs
//= require turbolinks
//= require refile
```

### Attaching hooks to make the progress bar work

Now that we have the Refile JS library in place we just need to hook it up to
our progress bar and start letting the user know what is going on. The Refile
JS exposes a lot of information when an requests takes place. Most notably for
our uses are the `uploaded` and `total` attributes from the `originalEvent`
that is returned. These will allow us to calculate the current progress
percentage really easly so we can communicate it to the end-user.

```JS
$(document).on("upload:start", "form", function(e) {
  $(this).find("input[type=submit]").attr("disabled", true);

  progressBar = $("#" + $(e.target).data("progressbar")).parent();
  progressBar.removeClass("hidden");
  progressBar.addClass("show");
});

$(document).on("upload:progress", "form", function(e) {
  // Get the progress bar to modify
  progressBar = $("#" + $(e.target).data("progressbar"));

  // Process upload details to get the percentage complete
  uploadDetail = e.originalEvent.detail;
  percentLoaded = uploadDetail.loaded;
  totalSize = uploadDetail.total;
  percentageComplete = Math.round((percentLoaded / totalSize) * 100);

  // Reflect the percentage on the progress bar
  progressBar.css("width", percentageComplete + "%");
  progressBar.text(percentageComplete + "%");
});

$(document).on("upload:success", "form", function(e) {
  progressBar = $("#" + $(e.target).data("progressbar"));
  progressBar.addClass("progress-bar-success");
});

$(document).on("upload:failure", "form", function(e) {
  progressBar = $("#" + $(e.target).data("progressbar"));
  progressBar.addClass("progress-bar-danger");
});

$(document).on("upload:complete", "form", function(e) {
  if(!$(this).find("input.uploading").length) {
    $(this).find("input[type=submit]").removeAttr("disabled");
  }
});
```

So lets break these down a bit and get into what we are doing on each event
that is delegated.

#### `upload:start`

This attaches to the start event for the current upload. Here is where we would
want to make the progress bar visible since we had it hidden until the user
uploads something. Also, we are disabling the submit button of the form so that
a user can not accidentally submit the form while the file is still in the
process of uploading.

#### `upload:progress`

This is the main event that we want to hook into for our progress bars. Here we
will grab the progress bar that we want to update. Then we will get the
information from the event that is returned to calculate the total percentage,
the `loaded` and `total` attributes. Then we do a simple calculation of
dividing the loaded by the total and multiplying by 100 to get the value as a
percentage. The last thing that we will have to do is communicate the change in
percentage back to the end user on the progress bar itself. This is done by
changing the `width` attribute via css and changing the text in the progress
bar to reflect the current percentage.

#### `upload:success`

This will be used to communicate a successful upload to the server. One thing
you may want to do on this event is change the color of the progress bar as we
are doing here to a color that indicates a successful upload.

#### `upload:failure`

This event is just like `upload:success` but is instead, you guessed it, for
failure. 

#### `upload:complete`

This event is similar to the `upload:complete` and `upload:success` events but
will actually fire before both of them and is only an indication of a completed
XHR request. Because of this it is a good event to hook into for things like
turning your submit button on. But be careful, this does not mean
that your transaction completed successfully! You should use the
`upload:success` event for that instead.

### Wrap up

Now that we have the HTML and Javascript all setup you are ready to go. When you 
navigate to your file upload and select a file you should see your awesome progress
bar showing the user their upload progress.

[bootstrap-repo]: https://github.com/twbs/bootstrap
[refile-repo]: https://github.com/refile/refile
[rails-repo]: https://github.com/rails/rails
[carrierwave-repo]: https://github.com/carrierwaveuploader/carrierwave
