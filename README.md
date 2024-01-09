
# utility-package

## Over View of package 

* Wpmet Plugin Utility helper package for Banner, Notice, Stories, and Rating.
* We made a composer package for these classes together.
* By using this Helper package we are centralizing the feature of these Classes in a package manager
  (so that any update or modifications of these functionalities globally have  no need to give hand or keep any dependencies on our plugins)


## Installation

`` composer require wpmet/utility-package ``

* this command installs the latest version of our package.
* NB: we recommend you to use this package with PHP-Scoper. 

## Calling/Using feature example here :

```php 
/**
 * Show WPMET stories widget in the dashboard
 */
$filter_string = ''; // elementskit,metform-pro
$filter_string .= ((!in_array('elementskit/elementskit.php', apply_filters('active_plugins', get_option('active_plugins')))) ? '' : ',elementskit');
$filter_string .= (!class_exists('\MetForm\Plugin') ? '' : ',metform');
$filter_string .= (!class_exists('\MetForm_Pro\Plugin') ? '' : ',metform-pro');

\Wpmet\UtilityPackage\Stories\Stories::instance( 'elementskit-lite' )   # @plugin_slug
// ->is_test(true)                                                      # @check_interval
->set_filter( $filter_string )                                          # @active_plugins
->set_plugin( 'ElementsKit', 'https://wpmet.com/plugin/elementskit/' )  # @plugin_name  @plugin_url
->set_api_url( 'https://api.wpmet.com/public/stories/' )                # @api_url_for_stories
->call();

/**
 * Show elementskit Notice
 */
\Wpmet\UtilityPackage\Notice\Notice::instance( 'elementskit-lite', 'go-pro-noti2ce' )   # @plugin_slug @notice_name
->set_dismiss( 'global', ( 3600 * 24 * 300 ) )                                          # @global/user @time_period
->set_type( 'warning' )                                                                 # @notice_type
->set_html(
        '
        <div class="ekit-go-pro-notice">
            <strong>Thank you for using ElementsKit Lite.</strong> To get more amazing 
            features and the outstanding pro ready-made layouts, please get the 
            <a style="color: #FCB214;" target="_blank" 
            href="https://wpmet.com/elementskit-pricing">Premium Version</a>.
        </div>
    '
    )                                                                                     # @notice_massage_html
->call();

/**
 * Show WPMET banner (codename: jhanda)
 */
\Wpmet\UtilityPackage\Banner\Banner::instance( 'elementskit-lite' )     # @plugin_slug
// ->is_test(true)                                                      # @check_interval
->set_filter( ltrim( $filter_string, ',' ) )                            # @active_plugins
->set_api_url( 'https://api.wpmet.com/public/jhanda' )                  # @api_url_for_banners
->set_plugin_screens( 'edit-elementskit_template' )                     # @set_allowed_screen
->set_plugin_screens( 'toplevel_page_elementskit' )                     # @set_allowed_screen
->call();

/**
 * Ask for Ratings 
 */
\Wpmet\UtilityPackage\Rating\Rating::instance('metform')                    # @plugin_slug
->set_plugin_logo('https://ps.w.org/metform/assets/icon-128x128.png')       # @plugin_logo_url
->set_plugin('Metform', 'https://wpmet.com/wordpress.org/rating/metform')   # @plugin_name  @plugin_url
->set_allowed_screens('edit-metform-entry')                                 # @set_allowed_screen
->set_allowed_screens('edit-metform-form')                                  # @set_allowed_screen
->set_allowed_screens('metform_page_metform_get_help')                      # @set_allowed_screen
->set_priority(30)                                                          # @priority
->set_first_appear_day(7)                                                   # @time_interval_days
->set_condition(true)                                                       # @check_conditions
->call();

/**
 * Show apps menu for others wpmet plugins
 */
\Wpmet\UtilityPackage\Apps\Apps::instance()->init('metform')                # @text_domain
->set_parent_menu_slug('metform-menu')                                      # @plugin_slug
->set_submenu_name('Apps')                                                  # @submenu_name (optional- default: Apps)
->set_section_title('My Custom title')                                      # @section_title (optional)
->set_section_description('My custom description')                          # @section_description (optional)
->set_items_per_row(4)                                                      # @items_per_row (optional- default: 6)
->set_plugins(                                                              # @plugins
  [
    'getgenie/getgenie.php' => [
      'name'  => esc_html__( 'GetGenie', 'metform' ),
      'url'   => 'htages_urtps://wordpress.org/plugins/getgenie/',
      'icon'  => , //image url,
      'desc'  => esc_html__( 'GetGenie is an AI-powered Content & SEO Assistant. It will assist you in adding a Social Login, Social Counter, and Social Login to your website.', 'metform' ),
    ],
    'shopengine/shopengine.php' => [
      'name'  => esc_html__( 'ShopEngine', 'metform' ),
      'url'   => 'https://wordpress.org/plugins/shopengine/',
      'icon'  => , //image url
      'desc'  => esc_html__( 'Completely customize your WooCommerce WordPress.', 'metform' ),
    ]
  ]
)
->call();

```

