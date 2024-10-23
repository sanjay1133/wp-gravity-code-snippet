<div align="center"><h1><a href="https://www.galaxyweblinks.com/" target="_blank">Galaxy Weblinks</a></h1></div>

# Gravity Form(WP) - Code Snippets

![Licence](https://img.shields.io/badge/Unlicense-red)

## Overview

This is a list of useful **WordPress** and **Gravity Form** code snippets and functions that I often reference to enhance or clean up my sites. 

**Note:** Please be careful and make backups!

## Gravity Form

- [Send entry data to third-party](#send-entry-data-to-third-party)
- [Check entry spam status](#check-entry-spam-status)

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

### Check entry spam status

```php
/**
 * This example shows how you can check if the entry has been marked as spam and prevent the rest of your function from running.
 */
add_action( 'gform_after_submission', 'action_gform_after_submission_spam_check', 10, 2 );
function action_gform_after_submission_spam_check( $entry, $form ) {
    if ( rgar( $entry, 'status' ) === 'spam' ) {
        return;
    }
 
    // The code that you want to run for submissions which aren't spam.
}
```

---

## License

This repository is unlicense[d], so feel free to fork.
