<div align="center"><h1><a href="https://www.galaxyweblinks.com/" target="_blank">Galaxy Weblinks</a></h1></div>

# Gravity Form(WP) - Code Snippets

![Licence](https://img.shields.io/badge/Unlicense-red)

## Overview

This is a list of useful **WordPress** and **Gravity Form** code snippets and functions that I often reference to enhance or clean up my sites. 

**Note:** Please be careful and make backups!

## Gravity Form

- [Send entry data to third-party](#send-entry-data-to-third-party)

---

## Gravity Form

### Send entry data to third-party

```php
/**
 * This example demonstrates a simple approach to posting submitted entry data to a third party application.
 */
add_action( 'gform_after_submission', 'post_to_third_party', 10, 2 );
function post_to_third_party( $entry, $form ) {
 
    $endpoint_url = 'https://thirdparty.com';
    $body = array(
        'first_name' => rgar( $entry, '1.3' ),
        'last_name' => rgar( $entry, '1.6' ),
        'message' => rgar( $entry, '3' ),
        );
    GFCommon::log_debug( 'gform_after_submission: body => ' . print_r( $body, true ) );
 
    $response = wp_remote_post( $endpoint_url, array( 'body' => $body ) );
    GFCommon::log_debug( 'gform_after_submission: response => ' . print_r( $response, true ) );
}
```

---

## License

This repository is unlicense[d], so feel free to fork.
