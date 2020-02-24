# WP2 - resume-plugin

Using this example wordpress shortcode create a custom plugin and display it as a resume format.

Helpers:

* ShortCode : [thomasdavis_resume]

* [Template(s)](https://speckyboy.com/free-html-resume-templates/)

* [FAQ Page](https://www.monster.ca/career-advice/article/top-resume-questions)

The main file:

<details><summary>Resume/Resume.php</summary><br>

```
<?php 
defined( 'ABSPATH' ) or die( 'Nope, not accessing this' );

/*
Plugin Name: Thomas Davis Resume
Plugin URI:  https://github.com/jinolacson/WP2-resume-plugin
Description: Automates the creation of Thomas Davis resume from external api using shortcode.
Version:     1.0.0
Author:      Jino Lacson
Author URI:  https://boom.camp
License:     GPL2
License URI: https://www.gnu.org/licenses/gpl-2.0.html
*/

class Resume {

	/**
	 * External and point
	 * @var string
	 */
	private $url = "https://gist.githubusercontent.com/thomasdavis/c9dcfa1b37dec07fb2ee7f36d7278105/raw/eb7968eb551bee9e3136b420394549b9680439d4/resume.json";

	public function __construct()
	{
		/**
		 * Wordpress Hook to Register ShortCode and display custom menu
		 */
    	add_action('init', array($this,'register_shortcode'));     
    	add_action('admin_menu', array($this,'resume_menu'));

    	/**
    	 * Your Styles and Scripts
    	 */
    	add_action('admin_enqueue_scripts', array($this,'enqueue_admin_scripts_and_styles')); 
		add_action('wp_enqueue_scripts', array($this,'enqueue_public_scripts_and_styles'));  

	}

	/**
	 * Activation hook
	 */
	public function plugin_activate()
	{
		//Add any logic here
    	flush_rewrite_rules();
	}

	/**
	 * Deactivation hook
	 */
	public function plugin_deactivate()
	{
		//Add any logic here
    	flush_rewrite_rules();
	}

	/**
	 * Unistall Hook
	 */
	public function plugin_uninstall()
	{
		//Add any logic here
	}

	/**
	 * Custom Menus
	 */
	public function resume_menu()
	{    
		$page_title = 'Thomas Davis Resume';   
		$menu_title = 'Thomas Resume';   
		$capability = 'manage_options';   
		$menu_slug  = 'resume-page';   
		$function   =  array($this,'resume_main_menu');   
		$icon_url   = 'dashicons-media-code';   
		$position   = 4;    

	    add_menu_page(
	    	__($page_title), 
	    	__($menu_title),
	    	$capability,
	    	$menu_slug,
	    	$function,
	    	$icon_url,
	    	$position 
	   ); 

	   add_submenu_page(
	   	'resume-page', 
	   	__('Resume Faqs Title'), 
	   	__('Resume Faqs'), 
	   	'manage_options', 
	   	'resume-faq', 
	   	array($this,'resume_faq_menu')
	   );
	} 

	/**
	 * Preview of Resume Format
	 */
	public function resume_main_menu()
	{ 

		echo __("<h1>This is the preview of ".$this->fetch_api()->basics->name." Resume"."</h1>");

		try {

            /**
             * Complete your source code here..
             */
            
        } catch (Exception $e) {

            echo __('Caught exception: '.  $e->getMessage(). "\n");
        }

	}

	/**
	 * Frequently ask questions about resume
	 */
	public function resume_faq_menu()
	{
		echo __("<h1>Frequently ask Questions</h1>");

		/**
		 * Complete your source code here..
		 */
		
	}

	/**
	 * Fnction to register the shortcode
	 */
	public function register_shortcode()
	{
		add_shortcode('thomasdavis_resume', array($this,'display_resume'));
	}

	/**
	 * Function to display resume in public site
	 */
	public function display_resume()
	{

		echo __("<h2>".$this->fetch_api()->basics->name." Resume"."</h2>");

		/**
		 * Your code to display Thomas Davis Resume in Wordpress public site 
		 */
	}

	/**
	 * Function to enqueue scripts and styles
	 */
	public function enqueue_admin_scripts_and_styles()
	{

		/**
		 * Your external CSS and JS for wordpress backend dashboard
		 */
		
	}

	/**
	 * Function to enqueue scripts and styles
	 */
	public function enqueue_public_scripts_and_styles()
	{

		/**
		 * Your external CSS and JS for wordpress public site
		 */
		
	}

	/**
	 * Function to fetch external endpoint
	 */
	public function fetch_api()
	{
		$response = wp_remote_get($this->url);
        $data = json_decode($response['body']);
        return $data;
	}
}


if(class_exists("Resume"))
{
	$resume = new Resume();
}

/**
 * Settings of plugin
 */
register_activation_hook(__FILE__, array($resume,'plugin_activate'));     
register_deactivation_hook(__FILE__, array($resume,'plugin_deactivate')); 
register_uninstall_hook(__FILE__, 'plugin_uninstall');  
?>
```
</details>

# Requirements

* A way to preview resume in wordpress admin dashboard.

* A way to display resume in wordpress public site.

* A fully functional `activate`, `deactivate` and `uninstall` options.

* A responsive resume layout.

* A `Resume.zip`

Optional

* Custom post page for resume.

* A shortcode `[link href="#"]Go to my resume[/link]` that will redirect to Thomas Davis resume page.

* A class that extends `Wp_enqueue_script` and `Wp_enqueue_style`.

# Finished

Submit a link to your fork of this repository to the Google Classroom assignment related to this project.
