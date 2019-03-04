---
layout: post
title:  "ACF Images - The Missing Manual"
date:   2019-03-03 13:49:23 -0500
categories: wordpress
---

Advanced Custom Fields has a useful total of 3 different ways to work with images and rather less useful total of 0 clear explanations for how to use them.

My goal here is to (as best I can) compile all of the answers to all of the many pleading questions asked on this topic around the web. Many thanks to all the people in ACF's support forums, StackExchange, and StackOverflow who provided the original answers to all of these questions.

## Image URL

The most straightforward of the three options is good old `Image URL` which (thankfully) works exactly as you would expect. Of course being the easiest to use also makes it the least useful so let's go ahead and deal with it first.

The `Image URL` does no more nor less that what it says on the bottle, ie returns the image url. Want to specify a size? Too bad. Want to add in alt text? Guess again. Want to display an image at full size? This is your lucky day. Just drop this in your code and call it a day.

```
<img src="<?php the_field('image_field_name'); ?>" alt="" />
```

If you wanted to get "fancy" you could make the image optional by wrapping the whole business in an `if` conditional like so…

```
<?php if( get_field('image_field_name') ) : ?>
  <img src="<?php the_field('image_field_name'); ?>" alt="" />
<?php endif; ?>
```

That's about it. You are now an expert in the lowly `Image URL` ACF option.
