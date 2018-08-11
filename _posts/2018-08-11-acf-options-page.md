- - - -
layout: post
title:  “Creating an ACF Options Page“
date:   2018-08-11 13:49:23 -0500
categories: wordpress acf
- - - -
[Advanced Custom Fields](advancedcustomfields.com) just never stops finding new ways to be cool. Or rather, I just can’t stop discovering new ways in which ACF is even cooler than I thought.

The most recent (accidental) discovery I’ve made is how to create a built-in options page for your site. This is incredibly convenient as the options page is “Universal” meaning you can call its fields from any page on the site without having to set up a custom loop.

::Sidebar - This is in contrast to how I had previously created “Options Pages” which was to have an actual page titled ‘Options’ that I would then reference with a custom query like so::
```
<?php $custom_query = new WP_Query('pagename=footer');
					while($custom_query->have_posts()) : $custom_query->the_post(); ?>
```

