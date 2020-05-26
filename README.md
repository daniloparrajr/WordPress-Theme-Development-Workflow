# __WordPress Theme Development__

## __Minimum Development Environment__

* Local Server - [Xampp](https://www.apachefriends.org/index.html) / [Wampp](http://www.wampserver.com/en/) / [Mampp](https://www.mamp.info/en/)
* IDE/Text editor - [NetBeans](https://netbeans.org/) / [Sublime](https://www.sublimetext.com/) / [VS code](https://code.visualstudio.com/) / [Atom](https://atom.io/)
* WordPress Local Installation - [Download WordPress](https://wordpress.org/download/)
* WordPress Site populated with Theme Unit Test data - [Theme Unit Test Data](https://codex.wordpress.org/Theme_Unit_Test)
* [Gulp](https://gulpjs.com/) or GUI applications [Prepros](https://prepros.io/) / [Scout](http://scout-app.io/) / [Koala](http://koala-app.com/) for task automation such as
  * Compiling SASS files
  * Minifying Scripts and Styles
  * Optimizing Images
  * Browser Reloading
* Various Browsers including [Chrome](https://www.google.com/chrome/index.html) and [Firefox](https://www.mozilla.org/en-US/firefox/new/) for testing

## __Gulp installation guide__

> Gulp is a command line task runner utilizing Node.js platform. It runs custom defined repetitious tasks and manages process automation.
> Listed below are the steps needed to run gulp on your machine

1. Install [Node](https://nodejs.org/en/)
2. Install [Gulp](https://gulpjs.com/) globally using the command line - `npm install gulp-cli -g`

## __SASS installation guide__

>Before you start using Sass you will need to install Ruby. The fastest way to get Ruby on your Windows computer is to use Ruby > [Ruby Installer](https://rubyinstaller.org/). It's a single-click installer that will get everything set up for you super fast.[More info](http://sass-lang.com/install)

- [Sass](http://sass-lang.com/). Ruby uses Gems to manage its various packages of code like Sass. In your terminal window type: - `gem install sass`

## __Get and install underscores(stater theme)__

Once WordPress is up and running on your computer, you need to get a copy of the Underscores theme. Underscores is a starters theme built specifically to be used as the starting point for new custom themes.

Go to your WordPress site, navigate to the backend. Go to Appearance and Themes. Click Add New, Upload Theme, and then, either grab the newly downloaded file from underscores.me. Choose File, Downloads, find your theme, open, Install Now, the theme is unpacked, installed, and from here You can activate it.

## __Install WordPress Theme Unit Test data__

This is a full set of content for a WordPress site that covers all the bases and provides all the weird scenarios you should test for. The Theme Unit Test Data is available in an xml file you can download from [Theme Unit Test Data](https://codex.wordpress.org/Theme_Unit_Test).

1. Go to WordPress back end. Tools > Import > WordPress > Install Now.
2. Run Importer then choose the file you just downloaded. !important check _Download and import file attachments_
3. Wait for the message _"All done, have fun!"_

## Setup WordPress for development

### __Install WordPress Developer Plugin__

When you develop themes or plugins it can be useful to have WordPress tell you what's going on and when something is not working right.
To make this possible, there are a handful of plugins that you can choose to install on your site for debugging purposes. All of these plugins have been bundled into one super plugin called [Developer](https://wordpress.org/plugins/developer/). Now Developer is quite an unusual plugin so let me show you how it works.

1. Go to the back end of your site, plugins and add new, and search for [Developer](https://wordpress.org/plugins/developer/)
2. Install and activate it. A modal will popup choose __theme for a self-hosted WordPress installation__
3. Install the following plugins - Debug bar, Monster widget, Regenerate thumbnails, and theme check.

### Install Show Current Template Plugin

[Show Current Template Plugin](https://wordpress.org/plugins/show-current-template/) is not bundled with WordPress Developer plugin. To install go to _Plugins > Add New_ and search for __Show Current Template__

### __WordPress WP_Debug__

*WP_Debug* enables debug mode which will identify and try to resolve issues in your code. And save queries logs the database queries in an array so you can review them. That means if something weird is happening and you need to see what the communication is between WordPress and the database, that is all saved and you can review it later.

Open `wp-config.php` located in the root installation of WordPress. Look for `define('WP_DEBUG', false);`
and set it to true `define('WP_DEBUG', true);`

### __Integrate Gulp to WordPress__

1. [Clone](https://github.com/danyaneh/WordPress-Gulp-Integration) or [Download](https://github.com/danyaneh/WordPress-Gulp-Integration/archive/master.zip) and move gulp-dev folder to your WordPress Theme directory _wp-content > themes_.
2. Go to the gulp-dev folder using the command line and type `npm install` this will download and install all packages needed.
3. Open _gulpfile.js_ and change the variable the following variables accordingly:

* themeName - Name of the theme your developing.
* siteName - Name of the site your developing.
* themeUrl - Url of the site your developing

```javascript
var themeName = 'themename',
    siteName = 'siteName',
    siteUrl = 'http://localhost/' + siteName,
    gulp = require('gulp'),
  // Prepare and optimize code etc
  autoprefixer = require('autoprefixer'),
  browserSync = require('browser-sync').create(),
  image = require('gulp-image'),
  jshint = require('gulp-jshint'),
  postcss = require('gulp-postcss'),
  sass = require('gulp-sass'),
  sourcemaps = require('gulp-sourcemaps'),
```

4. Still within gulp-dev folder in the command line type `gulp` to initialize all gulp task

## __Adding Google fonts switch by translator and resources hints__

Go the link and add it to your functions.php and make sure to replace `text_domain` with you theme text domain.
[WordPress Google Fonts Functions](https://gist.github.com/daniloparrajr/826063b377f94151c7bc54783873c752).

## __Adding SVG Icons to theme and Social Menus__

Why use SVG by the way? Here are some articles [Icon fonts vs SVG](https://css-tricks.com/icon-fonts-vs-svg/), [Seriously Don't Use Icon Fonts](https://cloudfour.com/thinks/seriously-dont-use-icon-fonts/)

If your convince to use SVG Icons for your theme lets add it now!

1. Require icon-funtions.php to your theme. - this file contains all the necessary functions for svg icons to work.
2. Add assets folder to your theme - this container all the svg icons that we will include to your theme.
3. Add body class _no-svg_ - we will assume that the browser does not support svg then we add fallbacks.
4. Add functions.js to your theme - this will check if the browser is supporting svg and adds _svg_ class to body tag
5. Add default styles for svg icons and svg fallbacks

If you follow all the neccessary steps we can now call inline svg icons using:
Make sure to find and replace all *text_domain* to your text domain
`text_domain_get_svg( array( 'icon' => 'icon-name' , 'fallback' => true ))`  
example implementation:  
`gleent_get_svg( array( 'icon' => 'arrow-long-right' , 'fallback' => true ));`

Add SVG icons to your social menu:
Make sure to have `link_before` and `link_after` contain's values so we can make sure icons will appear
to your menu instead of text.

~~~~php
  wp_nav_menu( array(
    'theme_location' => 'social',
    'menu_class'     => 'social-links-menu',
    'depth'          => 1,
    'link_before'    => '<span class="screen-reader-text">',
    'link_after'     => '</span>' . gleent_get_svg( array( 'icon' => 'chain' ) ),
  ) );
~~~~
