---
layout: post
title:  "Imagemagick text"
date:   2018-07-04 11:43:00 -0400
categories: imagemagick stuff
---

To draw a basic bit of text:
{% highlight shell %}
convert -size $width_in_pixels_of_image x$height_in_pixels_of_image
        xc:$background_color
        -pointsize $font_size
        -draw "fill '$text_color'
                    text $x_position,$y_position
                    'the text output'"
        $output_filename
{% endhighlight %}
where $text_color is a hex color, like "#FFFFFF"  
(opacity can be specified too, like "#FFFFFF55")  
and $background_color is a color (like 'lightblue') or 'none' if no background color  
and $x_position and $y_position are the lower left corner of the text,  
relative to the upper left corner of the image  
(0,0 is the upper left corner of the image, and will make the text not appear, as it would be positioned just above the upper left corner)  

Any number of text objects can be added after the `-draw`:
{% highlight shell %}
convert -size $width_in_pixels_of_image x$height_in_pixels_of_image
        xc:$background_color
        -pointsize $font_size
        -draw "fill '$text_color'
                    text $x_position,$y_position
                    'the text output'"
              "fill '$text_color'
                    text $x_position_2,$y_position_2
                    'another text output'"
        $output_filename
{% endhighlight %}
NOTE: for readability, I have space between one "fill ..." and the next "fill ...", but
_this will cause an error_. Imagemagick expects a filename after the space if you do this. Don't have
spaces between your "fill ..."s

It is also possible to specify multiple text objects with multiple `-draw` statements:  

{% highlight shell %}
convert -size $width_in_pixels_of_image x$height_in_pixels_of_image
        xc:$background_color
        -pointsize $font_size
        -draw "fill '$text_color'
                    text $x_position,$y_position
                    'the text output'"
        -draw "fill '$text_color'
                    text $x_position_2,$y_position_2
                    'another text output'"
        $output_filename
{% endhighlight %}

Why would you do this? One reason might be to allow different `gravity`s to be applied to different text objects

To back up a little, `gravity` works like this:
{% highlight shell %}
convert -size $width_in_pixels_of_image x$height_in_pixels_of_image
        xc:$background_color
        -pointsize $font_size
        -gravity center
        -draw "fill '$text_color'
                    text $x_position,$y_position
                    'the text output'"
        -draw "fill '$text_color'
                    text $x_position_2,$y_position_2
                    'another text output'"
        $output_filename
{% endhighlight %}
`gravity` can be NorthWest, North, NorthEast, West, Center, East, SouthWest, South, or SouthEast.  
If no gravity is specified, default is NorthWest.  
`gravity` controls where position 0,0 is in the image. With `-gravity northwest`, 0,0 is the upper
left corner of the image, and positions move right and down. With `-gravity center`, 0,0 is the center of the image. With `-gravity southeast`, 0,0 is the lower right corner of the image, and positions move left and up.  
Note that `gravity` doesn't just change the coordinate system of the image, it also changes where on
the text object the text is positioned relative to the image.  

For example, with `-gravity northwest`, text is positioned relative to the lower left corner.  
With `-gravity southeast`, text is positioned relative to the lower right corner.  

With multiple `-draw` statements, each `-draw` can have its own `gravity`
{% highlight shell %}
convert -size $width_in_pixels_of_image x$height_in_pixels_of_image
        xc:$background_color
        -pointsize $font_size
        -gravity center
        -draw "fill '$text_color'
                    text $x_position,$y_position
                    'the text output'"
        -gravity southeast
        -draw "fill '$text_color'
                    text $x_position_2,$y_position_2
                    'another text output'"
        $output_filename
{% endhighlight %}

Within each `-draw`, you can have multiple `text` objects, which will have the same gravity
{% highlight shell %}
convert -size $width_in_pixels_of_image x$height_in_pixels_of_image
        xc:$background_color
        -pointsize $font_size
        -gravity center
        -draw "fill '$text_color'
                    text $x_position,$y_position
                    'the text output'"
              "fill '$text_color'
                    text $x_position,$y_position
                    'more text output'"
        -gravity southeast
        -draw "fill '$text_color'
                    text $x_position_2,$y_position_2
                    'another text output'"
              "fill '$text_color'
                    text $x_position,$y_position
                    'also text output'"
        $output_filename
{% endhighlight %}

Now, let's say that you want to use a different font.  
That can be specified like so:
{% highlight shell %}
convert -size $width_in_pixels_of_image x$height_in_pixels_of_image
        xc:$background_color
        -pointsize $font_size
        -font Lato-Bold
        -draw "fill '$text_color'
                    text $x_position,$y_position
                    'the text output'"
        $output_filename
{% endhighlight %}

To check which fonts are available, try `convert -list font`