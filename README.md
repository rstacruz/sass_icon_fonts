Sass icon fonts
===============

Sass 3.2 integration for modern icon webfonts.

 * [Entypo] v2.0 - *241 icons, Daniel Bruce, SIL license, native at 40px*
 * [Entypo Social][Entypo] v2.0 - *46 icons*
 * [Font Awesome] v3.0 - *249 icons, Dave Gandy, CC BY 3.0, native at 14px*
 * [Typicons] - *88 icons, Stephen Hutchings, CC BY-SA 3.0, native at 24px*

[Font Awesome]: http://fortawesome.github.com/Font-Awesome/
[Entypo]: http://www.entypo.com/
[Typicons]: http://typicons.com/

Why is it needed?
-----------------

The Sass/CSS files that these fonts provide usually give you a lot of cruft, and 
defines all the classes in one giant file.

Me, I prefer to not have them in my CSS files unless I need them.

Usage
-----

First, include the font-face definition somewhere like so:

``` sass
@import font-awesome
+fa-font
```

Then use the `*-icon` mixin:

``` sass
button.add
  +fa-icon(plus, 24px)
```

Mixin usage
-----------

Each of the files provides a `*-icon` mixin:

``` sass
+fa-icon(music)
+fa-icon(music, 24px)        /* 24px size */
```

It also accepts some keyed arguments. You can mix and match these together.

``` sass
+fa-icon(music, 24px, $color: #333)
+fa-icon(music, 24px, $margin: 3px)    /* Margin-right */
+fa-icon(music, 24px, $shadow: 0 1px 1px rgba(black, 0.2))    /* Text shadow */
```

You may also specify a `$top` value to compensate for any mis-alignment.
This nudges the icon by that many pixels from the top.

``` sass
+fa-icon(music, 24px, $top: 2px)
```

See the individual files for more info.

Dev notes
---------

 * See `icons.json` for a manifest of available icons.
 * Typing `rake` updates the `index.html` reference.

