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

## ClickWhale REST API

The ClickWhale plugin provides a REST API to manage links and track clicks.

**Base URL**

https://example.com/wp-json/clickwhale/v1/

## Authentication
As an authentication method API uses standard WordPress [Application Passwords](https://make.wordpress.org/core/2020/11/05/application-passwords-integration-guide/)

Authorization instructions:
- in WP admin area create an Application Password for WP User
- in request header use `Basic Authentication` authorization scheme.

## Available API Routes

### 1. Get a list of all ClickWhale links

**Request**

```
GET /links
```

**Response**

```json
[
  {
    "id": 8,
    "slug": "promo/test/5",
    "url": "https://example.com/promo/test/5",
    "target_url": "https://clickwhale.pro/pricing/",
    "title": "Test link from API",
    "redirection": 302,
    "link_target": "self",
    "nofollow": true,
    "sponsored": false,
    "description": "Test link description",
    "categories": "1,3",
    "clicks": 15,
    "author": 2,
    "created_by_api": true,
    "created_at": "2025-08-04 14:20:51",
    "updated_at": "2025-08-04 14:20:51"
  },
  {
    "id": 7,
    "slug": "link-aawp",
    "url": "https://example.com/link-aawp",
    "target_url": "https://aawp.de/?ref=25",
    "title": "AAWP link",
    "redirection": 308,
    "link_target": "blank",
    "nofollow": true,
    "sponsored": true,
    "description": "AAWP link description",
    "categories": "",
    "clicks": 35,
    "author": 3,
    "created_by_api": false,
    "created_at": "2025-08-01 14:00:29",
    "updated_at": "2025-11-24 19:31:54"
  }
]
```

*This endpoint was added in ClickWhale v2.5.0*

### 2. Get ClickWhale link by slug or ID

**Request**

```
GET /links/{slug_or_id}
```

Parameters:
- _required_: slug_or_id — Unique slug of ClickWhale link (string) or numeric ID (integer)

Example Payloads:

```
GET /links/12
GET /links/promo-offer
```

Response codes:
- 200 OK — link object
- 400 Bad Request — invalid slug or ID
- 404 Not Found — link not found

**Response**

```json
{
"id": 8,
  "slug": "promo/test/5",
  "url": "https://example.com/promo/test/5",
  "target_url": "https://clickwhale.pro/pricing/",
  "title": "Test link from API",
  "redirection": 301,
  "link_target": "self",
  "nofollow": true,
  "sponsored": false,
  "description": "Test link description",
  "categories": "1,3",
  "clicks": 15,
  "author": 2,
  "created_by_api": true,
  "created_at": "2025-08-04 14:20:51",
  "updated_at": "2025-08-04 14:20:51"
}
```

*This endpoint was added in ClickWhale v2.5.0*

### 3. Create ClickWhale link

**Request**

```
POST /links
```

Parameters:
- _required_:
  - slug — Unique slug of ClickWhale link (string)
  - title — Link title (string)
  - target_url — Target URL (string)
- _optional_:
    - redirection — Redirection type (integer)
    - link_target — Link target (string)
    - nofollow — Mark links as nofollow & noindex (boolean)
    - sponsored — Mark links as sponsored (boolean)
    - description — Link description (string)
    - categories — Link categories (array of integers)

Example Payload:

```
POST /links
```

Body (JSON):

```json
{
  "slug": "new-offer",
  "title": "New Offer",
  "target_url": "https://clickwhale.pro/?utm_campaign=test",
  "redirection": 301,
  "link_target": "self",
  "nofollow": false,
  "sponsored": false,
  "description": "Landing for new campaign",
  "categories": [1, 3]
}
```

Response codes:
- 200 OK — link object
- 400 Bad Request — invalid slug
- 409 Conflict — slug already exists as:
  - ClickWhale link
  - ClickWhale link page
  - post (incl. WordPress posts, pages and custom post types)
  - taxonomy
  - custom endpoint, rewrite rule, .htaccess rule, etc.
- 500 Internal Server Error — failed to create link

**Response**

```json
{
"id": 9,
  "slug": "new-offer",
  "url": "https://example.com/new-offer",
  "target_url": "https://clickwhale.pro/?utm_campaign=test",
  "title": "Test link from API",
  "redirection": 301,
  "link_target": "self",
  "nofollow": false,
  "sponsored": false,
  "description": "Landing for new campaign",
  "categories": "1,3",
  "clicks": 0,
  "author": 2,
  "created_by_api": true,
  "created_at": "2025-08-05 17:37:44",
  "updated_at": "2025-08-05 17:37:44"
}
```

*This endpoint was added in ClickWhale v2.5.0*

## REST API Schema

ClickWhale link object has the following properties:

| Field       | Type    | Context | Required | Constraints                                       | Description                                             |
|-------------|---------|---------|----------|---------------------------------------------------|---------------------------------------------------------|
| slug_or_id  | string  | view    | no       | Max length: 255                                   | Unique slug or numeric ID of ClickWhale link            |
| slug        | string  | edit    | yes      | Max length: 255                                   | Unique slug of ClickWhale link                          |
| target_url  | string  | edit    | yes      | Format: URI. Min length: 1. Max length: 1000      | Target URL                                              |
| title       | string  | edit    | yes      | Min length: 1. Max length: 255                    | Link title                                              |
| redirection | integer | edit    | no       | One of: `301`, `302`, `303`, `307`, `308`         | Redirection type. Defaults to plugin settings           |
| link_target | string  | edit    | no       | One of: `self`, `blank`                           | Link target. Defaults to plugin settings                |
| nofollow    | boolean | edit    | no       |                                                   | Mark link as nofollow & noindex. Default: false         |
| sponsored   | boolean | edit    | no       |                                                   | Mark link as sponsored. Default: false                  |
| description | string  | edit    | no       | Max length: 255                                   | Short link description. Default: ''                     |
| categories  | array   | edit    | no       | Max items: 100. Array of unique positive integers | List of category IDs. Example: `[1, 2, 5]`. Default: [] |

## Plugin Documentation
More information about the plugin functionality can be found in the official [plugin documentation](https://clickwhale.pro/docs/).
