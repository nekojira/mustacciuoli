#Mustacciuoli

Brings **[Mustache](http://mustache.github.io/) template engine** to [WordPress](http://www.wordpress.org) in one of its [PHP](https://github.com/bobthecow/mustache.php) and one  of its [Javascript](https://github.com/twitter/hogan.js) implementations so you can use Mustache template files in your theme development and reuse the same template files across two languages. 

An interesting case use would be combining Mustache with [WordPress JSON API](http://wp-api.org/) to supply data to template files for example.  

## Installation

Install the plugin as you would with any WordPress plugin in your `wp-content/plugins/` directory or equivalent.   

 **Note:** If you are cloning or manually downloading this git repo from GitHub, make sure to init git submodule dependencies with:

    $ git submodule init
    $ git submodule update

This step is not necessary if you happen to download this plugin from WordPress.org.

Once installed, activate Mustacciuoli from WordPress plugins dashboard page and you're ready to go, Mustache PHP library will be instantiated already and Hogan.js script enqueued (Hogan.js is used instead of Mustache.js as the former seems to be better maintained by Twitter and faster). 

## Usage (PHP)

To render a template file in PHP you need to follow [Mustache.php instructions](https://github.com/bobthecow/mustache.php/wiki), for example:

	global $mustache;
	$template = $mustache->loadTemplate( 'your-template-file' );
	echo $template->render( array( 'your-vars') );

Global variable `$mustache` is set by Mustacciuoli and holds the template engine instance, so you can reuse it in your template files without instantiating it again and again.

By default, Mustacciuoli will tell Mustache.php to look for template files in `/templates/views` directory of your active theme (eg. `/your-theme/templates/views`). Partials will be in `/templates/views/partials`. However, you can override these settings by hooking these two filters and providing a different path before loading and rendering any template, like so:

	add_filter( 'mustache_templates_path', function() { return 'your-path'; } );
	add_filter( 'mustache_partials_path', function() { return 'your-path/partials'; } );

## Usage (Javascript)

Follow [Hogan.js documentation](http://twitter.github.io/hogan.js/).

Don't need the JS support? If you want to disable Hogan, you can de-queue the script with this code in your theme `functions.php` file (an example):

	function dequeue_hogan_js() {
   		wp_dequeue_script( 'hogan' );
	}
	add_action( 'wp_print_scripts', 'dequeue_hogan_js', 100 );

You can use this method if you don't want Hogan.js but rather enqueue another implementation of Mustache, like [Mustache.js](https://github.com/janl/mustache.js).
