# hyde-bootstrap
A modification of [twitter/bootstrap][] for use with [hyde/hyde][].

This is a fork of [auzigog/hyde-bootstrap][] with some additions and
modifications to cooperate with [aims-group/webshooter][].

## Usage
You'll need to have Hyde to run this (`pip install hyde`). Once you do, just

    hyde gen

in this directory, then open `deploy/index.html` in a web browser.
Alternatively, run `hyde serve` to run a webserver with `deploy` as the root
directory.

## Editing
There are a variety of templates that you can subclass using
`{% extends "templatename.j2" %}` on the top of your article or post.

  * `base.j2` contains the bulk of the layout logic, but not the best for
    subclassing because it doesn't have any grid attached to it.
  * `columns.j2` has a main content area and a sidebar with links to content
    within the page.
  * `topbar.j2` adds a top bar to the base layout
  * `hero.j2` places the bootstrap "hero" area on the page (good for a home
    page)

### Bootstrap dropdown menus
Generating Bootstrap's native dropdown menus programmatically is supported in
`site.yaml`. To make a navigation link a dropdown menu, just put a list of links
where you'd normally put a URL:

    context:
        data:
            menu:
                - title: Home
                  url: index.html
                - title: About
                  url: about.html
                - title: Related projects
                  url:
                    - title: Project Red
                      url: project_red.html
                    - title: Project Blue
                      url: project_blue.html

Currently this is not supported recursively.

### Carousels
To implement a carousel, make a list of items similar to navigation items like
so:

    context:
      data:
        carousel:
          - caption: Test caption
            image: /path/to/background/image.png
          - caption: Another test caption
            image: /path/to/another/image.png

You'll have to set your carousel's div and arrows manually, then call
`render_carousel_items`:

    <div class="carousel-inner">
      <div class="item peopleCarouselImg active"><!-- class of active since it's the first item -->
        <div class="carousel-caption">
          <p>This is your initial carousel item, if you choose to specify one. It behaves
          just like every other item but is the first to be displayed.</p>
        </div>
      </div>
      {% from "macros.j2" import render_carousel_items with context %}
      {{ render_carousel_items(carousel) }}
    </div>
    <a class="carousel-control left" href="#hero-carousel" data-slide="prev">&lsaquo;</a>
    <a class="carousel-control right" href="#hero-carousel" data-slide="next">&rsaquo;</a>

See [aims-group/uvcdat-site][]'s index.html for a live example.

## Updating your site changes from hyde-bootstrap
When hyde-bootstrap inevitably gets updated after after you've generated your
site and you want to incorporate some or all of those changes into your site,
you can use `git cherry-pick` to selectively grab the commits you want.

Let's assume your website is its own git repository, with no more ties to the
hyde-bootstrap.

First, set up hyde-bootstrap as a remote and fetch its changes:

    git remote add hyde-bootstrap git://github.com/aims-group/hyde-bootstrap
    git fetch hyde-bootstrap

Fetching doesn't overwrite anything or try to merge any code (pulling does).
Once you've fetched changes, you can cherry-pick the ones you want. If you just
want the changes performed by the most recent commit, you can do

    git cherry-pick hyde-bootstrap/master

Otherwise you can specify a specific commit hash or use one of several other
ways of specifying commits. See `man git-cherry-pick` for more info.

When you're done, just `git commit`, add to the prepopulated commit message if
you like, and you're ready to push.

### Adding Content
Look at all of the `.html` files in the `content` directory for an example of
how to begin adding your own content.

### Adding CSS
To extend the CSS of a given page, use the `{% block css %}{% endblock %}`
block. You can do this with a `<style>` block or a `<link>` to a CSS file.

### Publishing
To publish the site, first edit site.yaml to match your preferred publishing
(github, sftp, etc). See the Hyde README for details. Then run

    hyde publish -c prod.yaml

Use prod.yaml makes it easy to switch your `site.config.mode` variable to
`"production"` which can enable production-only elements of your site. In the
default hyde-bootstrap setup, analytics is only rendered in production mode.

## Versions
Built using:

  * [Hyde][hyde/hyde] [0.8.7](http://github.com/hyde/hyde/tree/696adac061ff040d5c5be1c629c94975c146f32a)
  * [Bootstrap][twitter/bootstrap] [2.3.2](http://github.com/twitter/bootstrap/tree/d9b502dfb876c40b0735008bac18049c7ee7b6d2)


## Notes
* There's a bit of code mixed in from the [h5bp/html5-boilerplate][] project for
  jQuery and and IE PNG fix.
* To update the js and css files of bootstrap to newer versions, run
  `./update-bootstrap-libs.sh`


## Credits
Built by [Jeremy Blanchard](http://blanchardjeremy.com).
Contributions by [Anand S](https://github.com/anandtrex).

[hyde/hyde]: https://github.com/hyde/hyde
[twitter/bootstrap]: https://github.com/twitter/bootstrap
[aims-group/webshooter]: https://github.com/aims-group/webshooter
[auzigog/hyde-bootstrap]: https://github.com/auzigog/hyde-bootstrap
[h5bp/html5-boilerplate]: https://github.com/h5bp/html5-boilerplate
[aims-group/uvcdat-site]: https://github.com/aims-group/uvcdat-site
