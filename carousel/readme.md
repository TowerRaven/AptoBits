# AptoCarousel [jQuery Plugin]
The adaptable CSS3 transitions powered carousel, in your hands.

## Dependencies
* jQuery
* A fairly modern browser

## Implementation
AptoCarousel relies heavily on CSS and CSS3 transitions, using two classes and
an effective third to create the rotation. So yes, it requires jQuery (JavaScript)
but there aren't really any suitable ways around that.

There is a "no-js" class which requires removing by script, this can be used to
style the carousel without the script kicking in so its still usable by users who
might have blocked scripts.

### Reasoning
I wasn't happy that most carousels stick a lot of inline code which will often
override any styling, because in the nature of cascading styles, the last style
is the one getting the attention, and inline style is usually the gobbiest.

In short, I wanted to create a carousel which leaves the majority of design still
in the developers/designers hands, it doesn't force dimensions or layout upon the
user so it can look pretty much however you want it to.

So, with the magic of transitions its now possible to style a carousel, which
from first sights behaves smoother. Anyhow, enough waxing lyrical…

## Usage
Include aptocarousel.js and style.css and create your HTML markup. A later feature
may allow some automation here but for now its all in your hands, do something
like the following…

    <div id="thecarousel" class="no-js carousel">
        <ul class="slides">
          <li class="slide"><div class="content">Slide #1 Content</div></li>
          <li class="slide"><div class="content">Slide #2 Content</div></li>
          <li class="slide"><div class="content">Slide #3 Content</div></li>
        </ul>
        <div class="aptocarousel-controls">
          <button type="button" class="previous">Previous</button>
          <button type="button" class="next">Next</button>
    </div>

Note that it doesn't care if its a list or more divs. It doesn't want sizes or
specify what should be in each slide. That is all under your control.

Once you're happy with that follow it up with the script.

    $('#thecarousel').aptocarousel();

Finally the CSS, the crucial thing here is that the carousel slides container
must be positioned absolute, and the slides themselves. This opens up the
possibilities for CSS transitions. Anyhow, details below the code…

    html, body {
      z-index: 1;
    }

The above ensures we have something to hide slides behind. The z-index will play
a big part in layering our slides.

    #thecarousel.carousel {
      width: 300px;
      height: 200px;
      padding: 2em;
    }

    #thecarousel.carousel .slides {
      height: 180px;
      position: absolute;
    }

    #thecarousel.carousel.no-js .slides {
      /**** Backup style for no JS ****/
    }

    #thecarousel.carousel .aptocarousel-controls {
      margin-top: 180px;
      height: 20px;
    }

This section covers the carousel main, the .slides container (our ul in this
example), note the .slides container is absolute, but not positioned (so it
maintains its original location). The height of the carousel is the slides +
controls; but ultimately this depends on your own preferences and styling,
and is as such in your complete control.

You could adjust the carousel size to allow controls to fit around, but this may
require further positioning of the .slides container. Anyhow, you get the idea.

    #thecarousel.carousel .slide .content {
      width: 300px;
      height: 180px;
      overflow: hidden;
    }

    #thecarousel.carousel .slide {
      position: absolute;
      bottom: 0px;
      display: inline-block;
      float: left;
      width: 300px;
      height: 180px;
      overflow: hidden;
      visibility: visible;
      transition: height .0s, visibility .0s;
      z-index: 1;
    }

    #thecarousel.carousel.no-js .slide {
      /**** Backup style for no JS ****/
    }

    #thecarousel.carousel .slide.cloaked {
      visibility: hidden;
      height: 0%;
      transition: height 1s, visibility 1s;
      z-index: 2;
    }

    #thecarousel.carousel .slide.approaching {
      visibility: hidden;
      transition: height 1s, visibility 1s;
      z-index: 3;
    }

First, why the slide .content div? This is primarily for the overflow: hidden,
the transition relies more on adjusting width/height than visibility - so by
hiding overflow content and giving it a fixed size it doesn't resize any text
when its parent container alters size; but still gives the illusion of sliding
out of sight.

All slides are positioned absolutely, and by positioning them to a side you can
make them "slide" out of view by tying the transition in - height (for top/bottom) or
width (left/right); giving the .cloaked a height or width of 0%.

Likewise tying it to a corner and adding a height and width of 0% with transition
for both height and width would make them disappear into a corner.

The z-index of cloaked should be lower than that of the approaching, and Likewise
the normal slide should be below the cloaked. So from bottom to top

    .slide (no class) < .slide.cloaked < .slide.approaching

Additionally ensure all 3 are kept as neighbours, and move them above any other
z-indexed elements so they appear on the surface. Don't bury your carousel, the
wooden horses do not like it, not one bit.

A .less version is included should that option be preferable.

## Tips & Observations
The larger the transitioning axis (so if wider and sliding left/right
for example), increase the transition time and time between slides, larger
dimensions look odd when sliding too fast. While 1 second might work for a
300x180 slide it might need 1.5s for a 600x180, etc.

## Supported Browsers
Otherwise read as…
> I made this in Firefox, but haven't gotten around to testing
it in anything else yet.

Or…
> Browsers it should theoretically work in.

* Firefox (16+)
* Chrome (version 26+)
* Internet Explorer (10+)
* Opera (12.10+)
* Safari (6.1+)

## Upcoming Features
* Custom style for previous button to "reverse" the slide effect, improving
the carousel style.
* Support for item buttons in addition to the current next/previous
* Implementation of settings for the aptocarousel function, they exist but are
effectively "hard-coded"
* Mayhaps I'll switch to jQuery .timer instead of setInterval
