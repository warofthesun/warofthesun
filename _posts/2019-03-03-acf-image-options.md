---
layout: post
title:  "ACF Images - The Missing Manual"
date:   2019-03-03 13:49:23 -0500
categories: wordpress
---

Advanced Custom Fields has a useful total of 3 different ways to work with images and a rather less useful total of 0 clear explanations for how to use them.

My goal here is to (as best I can) compile all of the answers to all of the many pleading questions asked on this topic around the web. Many thanks to all the people in ACF's support forums, StackExchange, and StackOverflow who provided the original answers to all of these questions.

## Image URL

The most straightforward of the three options is good old `Image URL` which (thankfully) works exactly as you would expect. Of course being the easiest to use also makes it the least useful so let's go ahead and deal with it first.

The `Image URL` does no more nor less that what it says on the bottle, ie returns the image url. Want to specify a size? Too bad. Want to add in alt text? Guess again. Want to display an image at full size? This is your lucky day. Just drop this in your code and move on with your life.

```
<img src="<?php the_field('image_field_name'); ?>" alt="" />
```



If you wanted to get "fancy" you could make the image optional by wrapping the whole business in an `if` conditional like so…

```
<?php if( get_field('image_field_name') ) : ?>
  <img src="<?php the_field('image_field_name'); ?>" alt="" />
<?php endif; ?>
```

That's about it. You are now an expert in the lowly `Image URL` ACF option. Use this anywhere you don't need fine control or alt text, something like a background image would be a good fit.
<br>

---

<br>
Ok, that was fun but now it's time to step the complexity (and usefulness) up a notch with…

## Image ID

Probably the coolest feature of `Image ID` is that this one will generate the `srcset` attribute for that extra responsive image goodness. You can't use `Image ID` for background images (since it prints out all of the image html) but if you want the ultimate in image responsiveness, this one's your huckleberry.

Using `Image ID` takes a little more set up but it's still pretty straightforward once you get going. Bare bones code looks like this…

```
<?php

$image = get_field('image_test');
$size = 'large'; // (or thumbnail, or medium, or custom, the Image ID sizes are your oyster)

  if( $image ) {
  	echo wp_get_attachment_image( $image, $size );
  }

?>
```
This code will churn out all of this handiness

```
<img width="1024" height="681" src="http://localhost:8200/wp-content/uploads/2019/02/header-1024x681.jpg"
class="attachment-large size-large"
srcset="http://localhost:8200/wp-content/uploads/2019/02/header-1024x681.jpg 1024w,
http://localhost:8200/wp-content/uploads/2019/02/header-300x200.jpg 300w,
http://localhost:8200/wp-content/uploads/2019/02/header-768x511.jpg 768w,
http://localhost:8200/wp-content/uploads/2019/02/header-150x100.jpg 150w,
http://localhost:8200/wp-content/uploads/2019/02/header-900x600.jpg 900w,
http://localhost:8200/wp-content/uploads/2019/02/header-736x490.jpg 736w,
http://localhost:8200/wp-content/uploads/2019/02/header.jpg 1479w"
sizes="(max-width: 1024px) 100vw, 1024px" itemprop="image">
```
all ready for ultimate responsiveability.

As you can see, `Image ID` is built on the standard `wp_get_attachment_image` so all of the normal WordPress options associated with that function are available. This means including alt text is done like so…

```
<?php

$image = get_field('image_test');
$size = 'large'; // (thumbnail, medium, large, full or custom size)
$alt = get_post_meta($post->ID, '_wp_attachment_image_alt', true);

  if( $image ) {
  	echo wp_get_attachment_image( $image, $size, $alt );
  }

?>
```

You can get more complex with this one if you want but if that's the case you should just go ahead and use the (drumroll please)…
<br>

---

<br>

## Image Array

For the absolute ultimate in control (and if you don't need `srcset`) you just can't do better than the `Image Array` option. As the name would suggest `Image Array` gives you access to the whole array of options associated with your image upload.

`Image Array` takes a little more effort to get set up but once you do you pretty much have total control. Let's take a look at a common example (and one I personally use all the time) so you can see what we're dealing with here.

```
<?php

$image = get_field('image_test');

if( !empty($image) ):
  // vars
	$url = $image['url'];
	$title = $image['title'];
	$alt = $image['alt'];
	$caption = $image['caption'];

	// thumbnail
	$size = 'large';
	$thumb = $image['sizes'][ $size ];
	$width = $image['sizes'][ $size . '-width' ];
	$height = $image['sizes'][ $size . '-height' ]; ?>

  <a href="<?php echo $url; ?>" title="<?php echo $title; ?>">
  	<img src="<?php echo $thumb; ?>" alt="<?php echo $alt; ?>" width="<?php echo $width; ?>" height="<?php echo $height; ?>" />
  </a>

<?php endif; ?>
```

In this example I've set `vars` for the image attachment's `url`, `title`, `alt`, and `caption`.

Once we have those we can mix and match them in anyway we want just by echoing out the correct variable.

I didn't include the caption in the example but if I wanted to all I would need to do is something like, `<p class="wp-caption-text"><?php echo $caption; ?></p>` and boom, instant caption.

If you need to see the full list of options available to you in the array just include a `var_dump` after your initial image variable. Plop this in and get more results than is strictly necessary.

```
<?php

$image = get_field('image_test');

  echo '<pre>';
	 var_dump( $image );
  echo '</pre>';

?>
```

Include the `pre` tags so it's formatted a little more nicely and you'll be swimming in options like you're Scrooge McDuck.

<br>

---

<br>

### And that's it!

You're now a bonafide ACF Image Options Expert (certificate is in the mail) you're mom is gonna be so proud.

There's more to each of these options (except for silly little `Image URL` of course) than we've covered here but this should be enough to get you moving.

I hope this has been helpful!
