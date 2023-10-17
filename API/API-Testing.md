# Wordpress RESTful API Testing

This API is used to get and update user data and product data.

## Basic URL

`https://teamjoyful.buzz`

## 1. JWT-Auth Login

This is used to get a JWT token upon successful authentication.

### Endpoint

`/wp-json/jwt-auth/v1/token`

### Request Type

`POST`

### Response

`JASON` - a JWT-Auth token

### Body

- **user** - This is the user name, for example **tester**
- **password** - This is the password, for example **123456**

### Example Request
```
POST https://teamjoyful.buzz/wp-json/jwt-auth/v1/token
Body:
{
    "username":"tester",
    "password":"123456"
}
```

### Example Responses
```
{
    "token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJodHRwczovL3d3dy50ZWFtam95ZnVsLmJ1enoiLCJpYXQiOjE2OTYzMzI3MTQsIm5iZiI6MTY5NjMzMjcxNCwiZXhwIjoxNjk2OTM3NTE0LCJkYXRhIjp7InVzZXIiOnsiaWQiOiI0In19fQ.j9WVZCyH8GkM84mFL1QvdQ_p_RmF2xNY5qUKfBmNIP0",
    "user_email": "tester@test.com",
    "user_nicename": "tester",
    "user_display_name": "tester retset"
}
```
## 2. Get User ID

This is used to get the id of the current user

### Endpont

`/wp-json/wp/v2/users/me`

### Request Type

`GET`

### Response

`JASON`

### Header

- **JWT-Auth token**

### Example Request
```
GET https://teamjoyful.buzz/wp-json/wp/v2/users/me
Header:
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJodHRwczovL3d3dy50ZWFtam95ZnVsLmJ1enoiLCJpYXQiOjE2OTYzMzI3MTQsIm5iZiI6MTY5NjMzMjcxNCwiZXhwIjoxNjk2OTM3NTE0LCJkYXRhIjp7InVzZXIiOnsiaWQiOiI0In19fQ.j9WVZCyH8GkM84mFL1QvdQ_p_RmF2xNY5qUKfBmNIP0
```

### Example Responses
```
{
    "id": 4,
    "name": "tester retset",
    "url": "http://www.tester.test",
    "description": "",
    "link": "https://www.teamjoyful.buzz/author/tester/",
    "slug": "tester",
    "avatar_urls": {
        "24": "https://secure.gravatar.com/avatar/0db53901eca1472a8997a38a24b38d06?s=24&d=mm&r=g",
        "48": "https://secure.gravatar.com/avatar/0db53901eca1472a8997a38a24b38d06?s=48&d=mm&r=g",
        "96": "https://secure.gravatar.com/avatar/0db53901eca1472a8997a38a24b38d06?s=96&d=mm&r=g"
    },
    "meta": [],
    "is_super_admin": true,
    "woocommerce_meta": {
        "variable_product_tour_shown": "",
        "activity_panel_inbox_last_read": "",
        "activity_panel_reviews_last_read": "",
        "categories_report_columns": "",
        "coupons_report_columns": "",
        "customers_report_columns": "",
        "orders_report_columns": "",
        "products_report_columns": "",
        "revenue_report_columns": "",
        "taxes_report_columns": "",
        "variations_report_columns": "",
        "dashboard_sections": "",
        "dashboard_chart_type": "",
        "dashboard_chart_interval": "",
        "dashboard_leaderboard_rows": "",
        "homepage_layout": "\"two_columns\"",
        "homepage_stats": "",
        "task_list_tracked_started_tasks": "{\"payments\":1}",
        "help_panel_highlight_shown": "",
        "android_app_banner_dismissed": ""
    },
    "_links": {
        "self": [
            {
                "href": "https://www.teamjoyful.buzz/wp-json/wp/v2/users/4"
            }
        ],
        "collection": [
            {
                "href": "https://www.teamjoyful.buzz/wp-json/wp/v2/users"
            }
        ]
    }
}
```


## 3. Get User Profile

This is used to get user profile.

### Endpoint

`/wp-json/wc/v3/customers/$id`

### Request Type

`GET`

### Response

`JASON` - a user profile

### Header

- **JWT-Auth token**

### Example Request
```
GET https://teamjoyful.buzz/wp-json/wc/v3/customers/4
Header:
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJodHRwczovL3d3dy50ZWFtam95ZnVsLmJ1enoiLCJpYXQiOjE2OTYzMzI3MTQsIm5iZiI6MTY5NjMzMjcxNCwiZXhwIjoxNjk2OTM3NTE0LCJkYXRhIjp7InVzZXIiOnsiaWQiOiI0In19fQ.j9WVZCyH8GkM84mFL1QvdQ_p_RmF2xNY5qUKfBmNIP0
```

### Example Responses
```
{
    "id": 4,
    "date_created": "2023-08-13T02:45:22",
    "date_created_gmt": "2023-08-13T02:45:22",
    "date_modified": "2023-10-03T10:55:13",
    "date_modified_gmt": "2023-10-03T10:55:13",
    "email": "tester@test.com",
    "first_name": "tester",
    "last_name": "retset",
    "role": "administrator",
    "username": "tester",
    "billing": {
        "first_name": "tester",
        "last_name": "retset",
        "company": "ANU",
        "address_1": "9 Cimba Lane",
        "address_2": "ANU",
        "city": "CBR",
        "postcode": "2601",
        "country": "AU",
        "state": "ACT",
        "email": "tester@test.com",
        "phone": "00000"
    },
    "shipping": {
        "first_name": "tester",
        "last_name": "retset",
        "company": "11",
        "address_1": "9 Cimba Lane",
        "address_2": "11",
        "city": "CBR",
        "postcode": "2601",
        "country": "AF",
        "state": "ACT",
        "phone": "00001"
    },
    "is_paying_customer": true,
    "avatar_url": "https://secure.gravatar.com/avatar/0db53901eca1472a8997a38a24b38d06?s=96&d=mm&r=g",
    "meta_data": [
        {
            "id": 143,
            "key": "wc_last_active",
            "value": "1696291200"
        },
        {
            "id": 147,
            "key": "community-events-location",
            "value": {
                "ip": "31.14.252.0"
            }
        },
        {
            "id": 150,
            "key": "meta-box-order_product",
            "value": {
                "side": "product_catdiv,tagsdiv-product_tag,woocommerce-product-images,postimagediv,submitdiv",
                "normal": "woocommerce-product-data,,,,,,postcustom,slugdiv,postexcerpt,commentsdiv",
                "advanced": ""
            }
        },
        {
            "id": 198,
            "key": "woocommerce_admin_homepage_layout",
            "value": "\"two_columns\""
        },
        {
            "id": 545,
            "key": "wpforms_dismissed",
            "value": {
                "edu-admin-notice-bar": 1692782159
            }
        },
        {
            "id": 608,
            "key": "screen_layout_product",
            "value": "2"
        },
        {
            "id": 662,
            "key": "woocommerce_admin_task_list_tracked_started_tasks",
            "value": "{\"payments\":1}"
        }
    ],
    "_links": {
        "self": [
            {
                "href": "https://www.teamjoyful.buzz/wp-json/wc/v3/customers/4"
            }
        ],
        "collection": [
            {
                "href": "https://www.teamjoyful.buzz/wp-json/wc/v3/customers"
            }
        ]
    }
}
```




