Sass icon fonts
===============

Sass 3.2 integration for modern icon webfonts.

 * [Entypo] (241 icons, Daniel Bruce, SIL license, native at 40px)
 * [Entypo Social][Entypo] (46 icons)
 * [Font Awesome] (249 icons, Dave Gandy, CC BY 3.0, native at 14px)
 * [Typicons] (88 icons, Stephen Hutchings, CC BY-SA 3.0, native at 24px)

Why is it needed?
-----------------

The Sass/CSS files that these fonts provide usually give you a lot of cruft, and 
defines all the classes in one giant file.

Me, I prefer to not have them in my CSS files unless I need them.

Usage
-----

First, include the font-face definition somewhere like so:

    @import font-awesome
    +fa-font

Then use the `*-icon` mixin:

    button.add
      +fa-icon(plus, 24px)

See the individual files for more info.

[Font Awesome]: http://fortawesome.github.com/Font-Awesome/
[Entypo]: http://www.entypo.com/
[Typicons]: http://typicons.com/

