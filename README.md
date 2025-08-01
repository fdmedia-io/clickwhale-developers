# Developers Documentation for ClickWhale

This documentation provides a collection of useful code snippets for developers working with the [ClickWhale WordPress plugin](https://wordpress.org/plugins/clickwhale/).

## Available Hooks

### Link Page: Header
Use this hook if you want to execute an action at the top of a ClickWhale link page.

```php
add_action('clickwhale/link_page_head', function( $link_page, $link_page_id, $user_id = 0 ) {
    // Do something
}, 3, 10 );
```
The hook provides information about the viewed link page, the link page ID and the user ID (if logged in).

*This hook was added in ClickWhale v2.4.0*

### Link Page: Footer
Use this hook if you want to execute an action at the bottom of a ClickWhale link page.

```php
add_action('clickwhale/link_page_footer', function( $link_page, $link_page_id, $user_id = 0 ) {
    // Do something
}, 3, 10 );
```
The hook provides information about the viewed link page, the link page ID and the user ID (if logged in).

*This hook was added in ClickWhale v2.4.0*

### Link Clicks
Use this hook if you want to execute an action when a ClickWhale link was clicked, no matter where it is placed.

```php
add_action('clickwhale/link_clicked', function( $link, $link_id, $user_id = 0 ) {
    // Do something
}, 3, 10 );
```
The hook provides information about the clicked link and the user ID (if logged in).

*This hook was added in ClickWhale v2.4.5*

### Link Clicks (Link Page)
Use this hook if you want to execute an action when a ClickWhale link was clicked on a link page.

```php
add_action('clickwhale/link_page_link_clicked', function( $link, $link_id, $linkpage, $linkpage_id, $user_id = 0 ) {
    // Do something
}, 5, 10 );
```
The hook provides information about the clicked link, as well as the viewed link page, the link page ID and the user ID (if logged in).

*This hook was added in ClickWhale v2.4.5*

## Available API Routes
tba

## Plugin Documentation
More information about the plugin functionality can be found in the official [plugin documentation](https://clickwhale.pro/docs/).
