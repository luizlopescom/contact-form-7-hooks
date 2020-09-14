# Contact Form 7 Hooks, functions and JS Scripts

Here you can find some [Contact Form 7](https://wordpress.org/plugins/contact-form-7/) actions and filters (hooks) available for use. The last used version is 4.2.1.

## Filters

- `wpcf7_contact_form_default_pack`

```php
/**
 * Description
 *
 * @param $contact_form
 * @param $args
 * @return $contact_form
 */
add_filter('wpcf7_contact_form_default_pack', function($contact_form, $args) {
    return $contact_form;
});
```

- `wpcf7_contact_form_properties`

```php
/**
 * Description
 *
 * @param $properties
 * @param $cf7
 * @return $properties
 */
add_filter('wpcf7_contact_form_properties', function($properties, $cf7) {
    return $properties;
});
```

- `wpcf7_form_action_url`

```php
/**
 * Used to change the form action URL
 *
 * @param $url the current URL
 * @return string The new URL you want
 */
add_filter('wpcf7_form_action_url', function($url) {
    return $url;
});
```

- `wpcf7_form_id_attr`

```php
/**
 * Change de form HTML id value
 *
 * @param $html_id The current id value
 * @return string The new id
 */
add_filter('wpcf7_form_id_attr', function($html_id) {
    return $html_id;
});
```

- `wpcf7_form_name_attr`

```php
/**
 * Change de form HTML name
 *
 * @param $html_name The actual name
 * @return string The new name
 */
add_filter('wpcf7_form_name_attr', function($html_name) {
    return $html_name;
});
```

- `wpcf7_form_class_attr`

```php
/**
 * Change de form class name
 *
 * @param $html_class The actual class name
 * @return string The new class name
 */
add_filter('wpcf7_form_class_attr', function($html_class) {
    return $html_class;
});
```

- `wpcf7_form_enctype`

```php
/**
 * Change de form enctype tag
 *
 * @param $html_enctype An empty string
 * @return string The new enctype tag value
 */
add_filter('wpcf7_form_enctype', function($html_enctype = '') {
    return $html_enctype;
});
```

- `wpcf7_form_novalidate`

```php
/**
 * Description
 *
 * @param $support_html5 The result of wpcf7_support_html5() function call
 * @return @novalidate
 */
add_filter('wpcf7_form_novalidate', function($support_html5) {
    return null;
});
```

- `wpcf7_form_response_output`

```php
/**
 * Filter the form response output
 *
 * @param $output 
 * @param $class 
 * @param $content 
 * @param $cf7 
 * @return @output
 */
add_filter('wpcf7_form_response_output', function($output, $class, $content, $cf7) {
    return $output;
});
```

- `wpcf7_validation_error`

```php
/**
 * Return the validation HTML code message for a field error
 *
 * @param $error The HTML like '<span role="alert" class="wpcf7-not-valid-tip">%s</span>
 * @param $name The field name
 * @param $cf7 The CF7 object
 * @return @error The new HTML error
 */
add_filter('wpcf7_validation_error', function($error, $name, $cf7) {
    return $error;
});
```

## Manual Functions

```php
/**
 * Contact form 7 remove span
 *
 * NOT TESTED
 */
add_filter('wpcf7_form_elements', function($content) {
    $content = preg_replace('/<(span).*?class="\s*(?:.*\s)?wpcf7-form-control-wrap(?:\s[^"]+)?\s*"[^\>]*>(.*)<\/\1>/i', '\2', $content);

    $content = str_replace('<br />', '', $content);
        
    return $content;
});
```

```php
/*
 * Disable CF7 Refill
 * useful when cache is enabled
 */
function branode_disable_wpcf7_refill() {
	global $wp_scripts;
	$handle      = 'contact-form-7';
	$object_name = 'wpcf7';
	$data        = $wp_scripts->get_data( $handle, 'data' );
	if ( ! empty( $data ) ) {
		if ( ! is_array( $data ) ) {
			$data = json_decode( str_replace( 'var ' . $object_name . ' = ', '', substr( $data, 0, - 1 ) ), true );
		}
		foreach ( $data as $key => $value ) {
			$localized_data[ $key ] = $value;
		}
		unset($localized_data['cached']);
		$wp_scripts->add_data( $handle, 'data', '' );
		wp_localize_script( $handle, $object_name, $localized_data );
	}
}
add_action( 'wpcf7_enqueue_scripts', 'branode_disable_wpcf7_refill' );
```

## Scripts
```javascript
/*
 * DOM Events
 * Run scripts on form submit
 * Reference: https://contactform7.com/dom-events/
 */
document.addEventListener( 'wpcf7mailsent', function( event ) { //Use mailsent instead of submit to prevent false positives
  if ( '123' == event.detail.contactFormId ) { //CF7 form ID - append as many forms as you need changing the ID
	//Generic example
	alert( "The contact form ID is 123." );
	
	//GTM Google Tag Manager Event example
	dataLayer.push({
		'event': 'Success' //event name
	});
  }
}, false );
```
