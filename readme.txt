=== ScoreRender ===
Contributors: abelcheung
Tags: music, music notation, music typesetting, score, abc, mup, lilypond, guido, pmw
Requires at least: 2.6
Tested up to: 2.9.2
Stable tag: 0.3.4

Renders inline sheet music fragments in excerpts, posts, pages and comments.

== Description ==

ScoreRender is a Wordpress plugin for rendering sheet music fragments into images.  It supports converting fragments in excerpts, posts, pages and (optionally) comments.  Currently it supports 5 music notations: ABC, Guido, Lilypond, Mup and Philip's Music Writer.

For latest version, detailed usage instructions and demo cases, please visit [ScoreRender official site](http://scorerender.abelcheung.org/). Requires PHP5, ImageMagick and various programs to generate music score.

== Installation ==

###Prerequisite###

1. **PHP 5.x** (PHP4 compatibility is dropped since 0.2; please visit [offical site](http://scorerender.abelcheung.org/) if ancient version using PHP4 is needed)
1. **ImageMagick >= 6.3.6-2** (due to usage of `-flatten` option). Newer version is better, since a bug about detecting PostScript transparency is fixed (updating pending in this area)
1. **Music rendering programs** must also be installed on the same machine web server is running. For example, to support Lilypond fragments, Lilypond >= 2.8.1 must be installed in web server. Most notations require explicit program to render, except GUIDO notation which fetches remote images instead. Refer to [installation page](http://scorerender.abelcheung.org/installation/) for more detail.

###New install###

1. Install any prerequisite programs as noted above.
1. Extract archive, and copy 'scorerender' folder to `wp-content/plugins/`, keeping the folder structure intact.
1. Login to WordPress and enable the plugin in admin interface.
1. Configure ScoreRender under the ScoreRender tab of the Options page.
1. In Option -> Writing, check if this option is turned on:

       "WordPress should correct invalidly nested XHTML automatically"

   It must be turned off if you intent to render Lilypond and Mup fragments,
   since this option will convert "<<" and ">>" into "< <" and "> >"
   correspondingly, thus destroying the music content and cause render error.

###Upgrade###

1. Deactivate the plugin in WordPress admin page.
1. Remove the whole plugin folder.
1. Upload new plugin and activate again.

== Frequently Asked Questions ==

= It just complains about some obscure error! =

The error code indicates the kind of error in some degree. There are comments inside wp-scorerender.php indicating what kind of error it is. If you can't make any heads and tails out of PHP code, feel free to [contact the author](http://me.abelcheung.org/aboutme/).

= Why music score fragments are not rendered at all? =

* Check if the beginning tag and ending tag of your music score fragment are correct.
* It will only be rendered if logged in user has *unfiltered_html* capability, i.e. the user has *Administrator* or *Editor* role in WordPress. User can ask blog admin to boost their capabilities if needed.
* Some programs required for certain notation may be not working or missing.

= Is any ABC notation compatible program also supported? =

Since 0.2, [abcm2ps](http://moinejf.free.fr/) will be the only one supported. This is a design decision. If you **REALLY** want to use other similar programs, you are on your own, though modifying the code to support others is not very hard. Take a look at `is_notation_usable()` method in `notation/abc.php`.

= I want to remove cache for 1 image and re-render, but how can I determine which is which? =

Right now this is still impossible. Management of cache is planned in future, but can't say when.

= Images using Guido notation seems blurred. =

This may not be fully fixable, because setting font attributes may not be possible for all text. After image resizing, they can be rendered smaller / larger than desired.

= How to debug my fragment when posting? =

Simply answer: don't do that if possible. The best way is render it privately in your computer first, then post the content, rather than needlessly spending lots of time on trial and error.
Long answer: There is no easy method for debugging a fragment yet. If there is no choice but perform trial and error, please search for this line in `wp-scorerender.php`:

`	define (SR_DEBUG, FALSE);`

Change `FALSE` to `TRUE`, then resubmit the content again. It has 2 purposes:

1. Erraneous fragments are not deleted from server temp folder, and you can try manually rendering the fragment using command line to see what's wrong.
1. Full command line for rendering is now shown on blog, so that you can check out if command line argument is wrong.

= How can I install Philip's Music Writer? =

Only by downloading source from its official website and compile the program yourself. The author failed to notice any binary package for Windows or Linux as of Feb 2010, except an outdated RPM package for Mandriva Linux.

Please refer to the documents inside PMW tarball on how to compile its source code.

= I discovered a bug. How can I notify the author? =

You can either [submit bug report to Googlecode](http://code.google.com/p/scorerender/issues/list) or [contact the author](http://me.abelcheung.org/aboutme/).

== Screenshots ==

Please visit [ScoreRender official site](http://scorerender.abelcheung.org/) for screenshots.

== License ==

* This plugin is licensed under GNU AGPL v3.
* IE Alpha Fix is licensed under LGPL v2.1 or later.
* Zero Clipboard is licensed under LGPL.
* jQuery Color Picker is dual licensed under MIT and GPL.

== Upgrade Notice ==

= 0.3.3 =

* Safe mode for Lilypond notation is once again usable.
* Users of more recent ImageMagick version will find that transparency for certain notations are fixed.
* Also fix a bug prevented disabling of notation.

= 0.3.2 =

* Users of Lilypond 2.12.x should upgrade due to broken command line invocation.

== Changelog ==

= 0.3.3 =

* Change license to AGPL v3.
* 'Show source' setting is moved to 'Contents' admin section.
* BUG FIX: Admin form html tags incorrectly nested.
* BUG FIX: Notation was not deactivated even when program name is not filled.
* BUG FIX: Restore safe mode for Lilypond, use precise version detection to determine command line argument.
* BUG FIX: PostScript transparency shall be properly detected for PMW and Mup on recent ImageMagick versions.
* BUG FIX: Prevents PMW from reading config file.
* FEATURE: Add icon for admin form title (on recent WP versions).

= 0.3.2 =

* BUG FIX: Fix invocation for LilyPond 2.12.x

= 0.3.1 =

* FEATURE: Show image dimension in output.
* BUG FIX: Fix line break when showing score source code under Windows.
* FEATURE: Better autodetection of program.
* BUG FIX: program availability checking.

= 0.3.0 =

* FEATURE: Philip's Music Writer notation support.
* FEATURE: IE Alpha fix has been incorporated, which provides translucent PNG support for IE 5.5 / 6.x. Thus drop IE PNG transparency warning altogether. 
* FEATURE: Zero Clipboard has been incorporated, which provides cross platform copy and paste via flash. Warning about non-IE browser during copy and paste is removed.
* FEATURE: Better support of installation on web hosting, where disabling certain PHP functions is common practise.
* Rendering or not also depends on 'unfiltered_html' WordPress capability.
* Refactor functions and files, so admin page is only included when needed, and PHP class no longer access global variables.

= 0.2.1 =

* BUG FIX: Toggling IE PNG transparency warning option was ineffective.

= 0.2.0 =

* Revamp admin page and simplify options.
* Allows limiting number of score fragments and length of fragment.
* Allows showing music source code when image is clicked.
* Better Windows support.
* Mandates abcm2ps must be used for ABC notation support.

= 0.1.3 =

* Add WordPress nonce protection.

= 0.1.2 =

* Fix image rendering during ImageMagick conversion process.

= 0.1.1 =

* Fix transparency of images generated by Lilypond.
* Issue warning if *correct invalidly nested XHTML automatically* option is checked, instead of turning the option off.

= 0.1.0 =

* Initial release.
