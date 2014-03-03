Sass icon fonts
===============

Sass 3.2 integration for modern icon webfonts.

 * [Elusive] - *271 icons, Aristeides Stathopoulos, SIL license, native at ?px*
 * [Entypo] v2.0 - *241 icons, Daniel Bruce, SIL license, native at 40px*
 * [Entypo Social][Entypo] v2.0 - *46 icons*
 * [Font Awesome] v3.0 - *249 icons, Dave Gandy, CC BY 3.0, native at 14px*
 * [Typicons] - *88 icons, Stephen Hutchings, CC BY-SA 3.0, native at 24px*
 * [Foundation Icons] v2 - *General set icons, ZURB, MIT Open Source License, native at ?px*

[Elusive]: https://github.com/aristath/elusive-iconfont
[Font Awesome]: http://fortawesome.github.com/Font-Awesome/
[Entypo]: http://www.entypo.com/
[Typicons]: http://typicons.com/
[Foundation Icons]: http://www.zurb.com/playground/foundation-icons

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

``` scss
@import "entypo";
@include en-font;
```

Then use the `*-icon` mixin:

``` sass
button.add
  +fa-icon(plus, 24px)
```

``` scss
button.add {
  @include en-icon("plus", 24px);
}
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

Why?
----

 * So that you define icons in your CSS, not in your HTML.
 * So that your CSS becomes leaner by not adding definitions for icons you don't 
 use.

Sass_icon_fonts uses Sass placeholder classes to make sure that you don't get 
CSS definitions for unneeded buttons. You see, this modest snippet:

``` sass
button.add
  +fa-icon(plus)

button.delete
  +fa-icon(trash)
```

...produces this output CSS below. Note that there isn't anything else here but 
the two buttons. Compare this with font-awesome's default Sass file :)

``` css
button.add:before, button.delete:before {
  line-height: 1em;
  font-family: FontAwesome;
  font-weight: normal;
  font-style: normal;
  display: inline-block;
  text-decoration: none;
  vertical-align: middle;
  text-rendering: optimizeLegibility !important;
  -webkit-font-smoothing: antialiased !important; }

button.add:before {
  content: "\f067"; }

button.delete:before {
  content: "\f014"; }
```

Dev notes
---------

 * To contribute, just edit the `.sass` files. The `icons.json` and `index.html`
 are all auto-generated.
 * See `icons.json` for a manifest of available icons.
 * Typing `rake` updates the `index.html` reference.

Acknowledgements
----------------

Files in this repo (Sass stylesheets, build script, reference HTML) all (c) 
  2013, Rico Sta Cruz and its contributors, all released under the MIT License.

Actual font files are not part of this project, and are available in their respective licenses.
