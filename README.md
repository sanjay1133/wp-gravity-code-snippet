<div align="center"><h1><a href="https://www.galaxyweblinks.com/" target="_blank">Galaxy Weblinks</a></h1></div>

# Gravity Form(WP) - Code Snippets

![Licence](https://img.shields.io/badge/Unlicense-red)

## Overview

A curated collection of helpful code snippets and functions designed to optimize and improve the functionality of **WordPress** and **Gravity Form** websites.

**Note:** Please be careful and make backups!

## Gravity Form

- [Send entry data to third-party](#send-entry-data-to-third-party)
- [Check entry spam status](#check-entry-spam-status)
- [Create an Events Calendar plugin event](#create-an-events-calendar-plugin-event)

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

### Create an Events Calendar plugin event

```php
/**
 * This example demonstrates how the gform_after_submission hook and the tribe_create_event(https://theeventscalendar.com/function/tribe_create_event/) function can be used to create an event in the Events Calendar(https://theeventscalendar.com/product/wordpress-events-calendar/) plugin.
 */
add_action( 'gform_after_submission', function ( $entry ) {
    if ( ! function_exists( 'tribe_create_event' ) ) {
        return;
    }
 
    $start_date = rgar( $entry, '4' );
    $start_time = rgar( $entry, '5' );
    $end_date   = rgar( $entry, '6' );
    $end_time   = rgar( $entry, '7' );
 
    $args = array(
        'post_title'            => rgar( $entry, '1' ),
        'post_content'          => rgar( $entry, '2' ),
        'EventAllDay'           => (bool) rgar( $entry, '3.1' ),
        'EventHideFromUpcoming' => (bool) rgar( $entry, '3.2' ),
        'EventShowInCalendar'   => (bool) rgar( $entry, '3.3' ),
        'feature_event'         => (bool) rgar( $entry, '3.4' ),
        'EventStartDate'        => $start_date,
        'EventStartTime'        => $start_time ? Tribe__Date_Utils::reformat( $start_time, 'H:i:s' ) : null,
        'EventEndDate'          => $end_date,
        'EventEndTime'          => $end_time ? Tribe__Date_Utils::reformat( $end_time, 'H:i:s' ) : null,
    );
 
    GFCommon::log_debug( 'gform_after_submission: tribe_create_event args => ' . print_r( $args, 1 ) );
    $event_id = tribe_create_event( $args );
    GFCommon::log_debug( 'gform_after_submission: tribe_create_event result => ' . var_export( $event_id, 1 ) );
} );
```

---

## License

This repository is unlicense[d], so feel free to fork.
