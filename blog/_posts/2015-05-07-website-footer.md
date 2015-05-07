---
title: Website Footer
layout: blogpost
posted: 05-07-2015
updated: 05-07-2015
description: Oh, the headaches involved when you just want to add a simple footer to a website.
---

While putting together this very website, I wanted to add a simple footer bar to the bottom of every page. I wanted it to follow all content on the page, while also never being above the bottom of the browser window if there was less than a full page's worth of content. It sounds like a simple task, but it wasn't immediately apparent how to go about such a layout in the best way.

After searching online, I discovered a remarkable lack of solutions considering how simple and common of a problem this is. The best and most commonly cited solution I could find was by a certain [Ryan Fait](http://ryanfait.com/resources/footer-stick-to-bottom-of-page/). This solution, while a thorough and robust solution, required 4 divs and a non-trivial amount of CSS to implement. Additionally, I found the explanation of the solution to be confusing at best.

I loathe the short "do it because it works" methodology. As such, I investigated exactly how HTML and CSS percentage sizing works, and did further research online jumping from forum to forum. Doing the kind of stuff that makes you feel like you might accomplish something at any minute, only to leave you disappointed when you see the same useless information in a slightly different form.

Eventually, I gathered enough information to take another, informed, crack at the problem myself. This is what I came up with. As far as I know, this solution has probably been implemented countless of times on the web, but the exact components and requirements aren't clearly laid out in any one place. Cue the creation of this blog post.

Here are the minimum requirements for a footer using this method:
{% highlight html %}
html { height:100%; }
body { min-height:100%; padding-bottom:[footer-height]; position:relative; }
footer { position:absolute; bottom:0; }
{% endhighlight %}

This requires that you know the height of your footer in pixels (or em, or whatever). All other pure HTML and CSS solutions that I've found have that same restriction though, so that's not surprising.
Additionally, be aware that the footer is considered part of the body, so any absolute positioning with respect to the bottom of the body must take the footer into account. This seems intuitive enough in my humble opinion, but it's still an important aspect to be aware of.

### How it works:
  * Setting the `height` of `html` to `100%` forces the webpage display area to take up 100% of the browser window height, regardless of how much content there is. This is necessary because the `min-height` property only works if `html` has a "hard-coded" `height` attribute.
  * Setting `body` `min-height` to `100%` forces the body to take up at least 100% of the html height (which is the height of the browser window). If the content is longer than the browser window, the body will expand accordingly, because we are only using `min-height` here.
  * Setting `body` `padding-bottom` to the height of the footer prevents the body from allowing anything in the space that will be occupied by the footer, unless it is positioned there absolutely.
  * Setting `body` `position` to `relative` does absolutely nothing to the size or location of the `body` itself. However, it is necessary because setting the `footer`'s position to absolute causes the `footer`'s position to be determined by the next container up the chain that has had it's position attribute hard-set in some way (or `html` if none have). Without setting this attribute in `body`, the `footer` would be placed "absolute" with respect to the `html` container instead, leading to it being positioned over any content that overflows the browser window height.
  * Setting `footer` `position` to `absolute` ignores all padding and margin settings, and simply places the box in some "absolute" position with respect to it's first container that has also had its `position` attribute set.
  * Setting `footer` `bottom` to `0` causes it to be placed at the very bottom of `body` in this specific case. Which is precisely where the `padding-bottom` is.

### Other Rambling:

HTML consists of boxes (or containers) containing other boxes. At the lowest level, the boxes contain simple content, such as text. The size and location of each box is determined by many things: the size of the box containing it, the location of the box containing it, the boxes and content within it, the other boxes and content within the box containing it, its own CSS, the CSS of the box containing it, and potentially the CSS of any other boxes that contain that box in any way. Needless to say, this gets very complicated, because the size and location of a box is dependent on the size and location of its container, and the size and location of the container is dependent on the size and location of its contents.
