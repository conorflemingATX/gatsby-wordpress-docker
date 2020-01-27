- Create a new menu:
  Appearance > Menus > Create New

- Add Menu Endpoint to the REST API:
  In Plugins > Add New search for WP API Menus by Fulvio Notarstefano and add then activate it.
  Remember that the menus endpoint that it adds is at "wp-json/wp-api-menus/v2/menus"

- Add a custom post type to the theme you are using:
  Go to Appearance > Theme Editor > Theme Functions and use the below code as an example:

```php
// If no thumbnail support in theme.
add_theme_support('post-thumbnails');

// Create our custom post type
function create_custom_post_type(){
  register_post_type('portfolio',
                    array(
                      'labels' => array(
                        'name' => __('Portfolio'),
                        'singular_name' => __('Portfolio')
                      ),
                      'public' => true
                      'show_in_admin_bar' => true,
                      'show_in_rest' => true // ! Important for us
                    ));

  // Add fields and types to the post type.
  add_post_type_support('portfolio', array('thumbnail', 'excerpt'));
  // thumbnail and except should be enough for most without getting into acf.
  // The only other one I really have to worry about is possibly putting 'hierarchical => true' and then enabling page-attributes.
  // but that really is for pages.
}

// Add custom post type to init hook in WP
add_action('init', 'create_custom_post_type');

/*
  Post types supported in 'add_post_type_support':
  * 'title'
  * 'editor' (content)
  * 'author'
  * 'thumbnail'
  * 'excerpt'
  * 'trackbacks'
  * 'custom-fields'
  * 'comments'
  * 'revisions'
  * 'page-attributes'
  * 'post-formats'
*/
```

- Add Templates to distinguish between pages in WP.
  Since although we are using Gatsby for styling, we are defining pages through wordpress it is useful to be able to send Gatsby more information about how to render the page from WP, rather than having every page be rendered through a single page template.
  This is an alternative to defining multiple special pages in the pages folder in Gatsby src, we can define multiple different templates for pages and allow them to be selected in WP.
  To do so, we need to add the following code in a PHP file to the root folder of our theme.
  In this case it is in wordpress > engine > wp-content > themes.
  When you place a template named in a comment into this folder, you can set it under the parent attribute option in Wordpress Admin > pages, and then query for template in Gatsby-node.js where we create pages.
  From there the template option can be handled in the pages template.

```php
<?php /* Template name: Portfolio Items Below Content */
```
