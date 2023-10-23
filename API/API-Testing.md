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
## 4. Product Listing

This is used to get all the product data.

### Endpoint

`/wp-json/wc/v3/products`

### Request Type

`GET`

### Response

`JASON`

### Header

- **JWT-Auth token**

### Example Request
```
GET https://teamjoyful.buzz/wp-json/wc/v3/products
Header:
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJodHRwczovL3d3dy50ZWFtam95ZnVsLmJ1enoiLCJpYXQiOjE2OTgwNTMxNTMsIm5iZiI6MTY5ODA1MzE1MywiZXhwIjoxNjk4NjU3OTUzLCJkYXRhIjp7InVzZXIiOnsiaWQiOiI0In19fQ.8-1i78ZEf77HJeoyJvrnjOWSTDLrjN-zOfxQGmPIaMk
```

### Example Responses
```
[
    {
        "id": 62,
        "name": "New Balance Q Speed 5 Inch 2 in 1 Short Men's Shorts Sport",
        "slug": "new-balance-q-speed-5-inch-2-in-1-short-mens-shorts-sport",
        "permalink": "https://www.teamjoyful.buzz/product/new-balance-q-speed-5-inch-2-in-1-short-mens-shorts-sport/",
        "date_created": "2023-08-28T20:03:55",
        "date_created_gmt": "2023-08-28T20:03:55",
        "date_modified": "2023-08-28T20:03:55",
        "date_modified_gmt": "2023-08-28T20:03:55",
        "type": "simple",
        "status": "publish",
        "featured": false,
        "catalog_visibility": "visible",
        "description": "<p>The lightweight New Balance Q Speed 5 Inch 2 in 1 Short is ideal for everything from track workouts to marathons. The men’s running shorts combine a supportive built-in liner short and a breezy woven shell to create a stylish and functional garment. Drop in and zippered pockets provide easily accessible storage for tech, snacks and other small items.</p>\n",
        "short_description": "",
        "sku": "",
        "price": "58",
        "regular_price": "80",
        "sale_price": "58",
        "date_on_sale_from": null,
        "date_on_sale_from_gmt": null,
        "date_on_sale_to": null,
        "date_on_sale_to_gmt": null,
        "on_sale": true,
        "purchasable": true,
        "total_sales": 1,
        "virtual": false,
        "downloadable": false,
        "downloads": [],
        "download_limit": -1,
        "download_expiry": -1,
        "external_url": "",
        "button_text": "",
        "tax_status": "taxable",
        "tax_class": "",
        "manage_stock": false,
        "stock_quantity": null,
        "backorders": "no",
        "backorders_allowed": false,
        "backordered": false,
        "low_stock_amount": null,
        "sold_individually": false,
        "weight": "",
        "dimensions": {
            "length": "",
            "width": "",
            "height": ""
        },
        "shipping_required": true,
        "shipping_taxable": true,
        "shipping_class": "",
        "shipping_class_id": 0,
        "reviews_allowed": true,
        "average_rating": "0.00",
        "rating_count": 0,
        "upsell_ids": [],
        "cross_sell_ids": [],
        "parent_id": 0,
        "purchase_note": "",
        "categories": [
            {
                "id": 18,
                "name": "Man",
                "slug": "man"
            }
        ],
        "tags": [],
        "images": [
            {
                "id": 63,
                "date_created": "2023-08-28T20:03:51",
                "date_created_gmt": "2023-08-28T20:03:51",
                "date_modified": "2023-08-28T20:03:51",
                "date_modified_gmt": "2023-08-28T20:03:51",
                "src": "https://www.teamjoyful.buzz/wp-content/uploads/2023/08/s-l500.jpg",
                "name": "s-l500",
                "alt": ""
            }
        ],
        "attributes": [
            {
                "id": 3,
                "name": "Brand",
                "position": 0,
                "visible": true,
                "variation": false,
                "options": [
                    "New Balance"
                ]
            },
            {
                "id": 1,
                "name": "Color",
                "position": 1,
                "visible": true,
                "variation": false,
                "options": [
                    "Black"
                ]
            },
            {
                "id": 5,
                "name": "Condition",
                "position": 2,
                "visible": true,
                "variation": false,
                "options": [
                    "New"
                ]
            },
            {
                "id": 4,
                "name": "Gender",
                "position": 3,
                "visible": true,
                "variation": false,
                "options": [
                    "Male"
                ]
            },
            {
                "id": 2,
                "name": "Size",
                "position": 4,
                "visible": true,
                "variation": false,
                "options": [
                    "xs"
                ]
            }
        ],
        "default_attributes": [],
        "variations": [],
        "grouped_products": [],
        "menu_order": 0,
        "price_html": "<del aria-hidden=\"true\"><span class=\"woocommerce-Price-amount amount\"><bdi><span class=\"woocommerce-Price-currencySymbol\">&#36;</span>80.00</bdi></span></del> <ins><span class=\"woocommerce-Price-amount amount\"><bdi><span class=\"woocommerce-Price-currencySymbol\">&#36;</span>58.00</bdi></span></ins>",
        "related_ids": [
            48,
            56,
            28,
            52,
            44
        ],
        "meta_data": [],
        "stock_status": "instock",
        "has_options": false,
        "_links": {
            "self": [
                {
                    "href": "https://www.teamjoyful.buzz/wp-json/wc/v3/products/62"
                }
            ],
            "collection": [
                {
                    "href": "https://www.teamjoyful.buzz/wp-json/wc/v3/products"
                }
            ]
        }
    }
  ] 
```
## 5. Get Categories

This is used to get all the category data.

### Endpoint

`/wp-json/wc/v3/products/categories`

### Request Type

`GET`

### Response

`JASON`

### Header

- **JWT-Auth token**

### Example Request
```
GET https://teamjoyful.buzz/wp-json/wc/v3/products/categories
Header:
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJodHRwczovL3d3dy50ZWFtam95ZnVsLmJ1enoiLCJpYXQiOjE2OTgwNTMxNTMsIm5iZiI6MTY5ODA1MzE1MywiZXhwIjoxNjk4NjU3OTUzLCJkYXRhIjp7InVzZXIiOnsiaWQiOiI0In19fQ.8-1i78ZEf77HJeoyJvrnjOWSTDLrjN-zOfxQGmPIaMk
```

### Example Responses
```
[
    {
        "id": 22,
        "name": "Accessories",
        "slug": "accessories",
        "parent": 0,
        "description": "",
        "display": "default",
        "image": {
            "id": 258,
            "date_created": "2023-10-01T08:07:15",
            "date_created_gmt": "2023-10-01T08:07:15",
            "date_modified": "2023-10-01T08:07:15",
            "date_modified_gmt": "2023-10-01T08:07:15",
            "src": "https://www.teamjoyful.buzz/wp-content/uploads/2023/10/product-accessory1-600x600-1.jpg",
            "name": "product-accessory1-600&#215;600",
            "alt": ""
        },
        "menu_order": 1,
        "count": 3,
        "_links": {
            "self": [
                {
                    "href": "https://www.teamjoyful.buzz/wp-json/wc/v3/products/categories/22"
                }
            ],
            "collection": [
                {
                    "href": "https://www.teamjoyful.buzz/wp-json/wc/v3/products/categories"
                }
            ]
        }
    },
    {
        "id": 19,
        "name": "Children",
        "slug": "children",
        "parent": 0,
        "description": "",
        "display": "default",
        "image": {
            "id": 263,
            "date_created": "2023-10-01T10:09:37",
            "date_created_gmt": "2023-10-01T10:09:37",
            "date_modified": "2023-10-01T10:09:37",
            "date_modified_gmt": "2023-10-01T10:09:37",
            "src": "https://www.teamjoyful.buzz/wp-content/uploads/2023/10/截屏2023-10-01-下午9.08.35.png",
            "name": "截屏2023-10-01 下午9.08.35",
            "alt": ""
        },
        "menu_order": 2,
        "count": 1,
        "_links": {
            "self": [
                {
                    "href": "https://www.teamjoyful.buzz/wp-json/wc/v3/products/categories/19"
                }
            ],
            "collection": [
                {
                    "href": "https://www.teamjoyful.buzz/wp-json/wc/v3/products/categories"
                }
            ]
        }
    }
]
```
## 6. Get Specific Product Using Product ID

This is used to get one specific product data.

### Endpoint

`/wp-json/wc/v3/products/$id`

### Request Type

`GET`

### Response

`JASON`

### Header

- **JWT-Auth token**

### Example Request
```
GET https://teamjoyful.buzz/wp-json/wc/v3/products/62
Header:
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJodHRwczovL3d3dy50ZWFtam95ZnVsLmJ1enoiLCJpYXQiOjE2OTgwNTMxNTMsIm5iZiI6MTY5ODA1MzE1MywiZXhwIjoxNjk4NjU3OTUzLCJkYXRhIjp7InVzZXIiOnsiaWQiOiI0In19fQ.8-1i78ZEf77HJeoyJvrnjOWSTDLrjN-zOfxQGmPIaMk
```

### Example Responses
```
{
    "id": 62,
    "name": "New Balance Q Speed 5 Inch 2 in 1 Short Men's Shorts Sport",
    "slug": "new-balance-q-speed-5-inch-2-in-1-short-mens-shorts-sport",
    "permalink": "https://www.teamjoyful.buzz/product/new-balance-q-speed-5-inch-2-in-1-short-mens-shorts-sport/",
    "date_created": "2023-08-28T20:03:55",
    "date_created_gmt": "2023-08-28T20:03:55",
    "date_modified": "2023-08-28T20:03:55",
    "date_modified_gmt": "2023-08-28T20:03:55",
    "type": "simple",
    "status": "publish",
    "featured": false,
    "catalog_visibility": "visible",
    "description": "<p>The lightweight New Balance Q Speed 5 Inch 2 in 1 Short is ideal for everything from track workouts to marathons. The men’s running shorts combine a supportive built-in liner short and a breezy woven shell to create a stylish and functional garment. Drop in and zippered pockets provide easily accessible storage for tech, snacks and other small items.</p>\n",
    "short_description": "",
    "sku": "",
    "price": "58",
    "regular_price": "80",
    "sale_price": "58",
    "date_on_sale_from": null,
    "date_on_sale_from_gmt": null,
    "date_on_sale_to": null,
    "date_on_sale_to_gmt": null,
    "on_sale": true,
    "purchasable": true,
    "total_sales": 1,
    "virtual": false,
    "downloadable": false,
    "downloads": [],
    "download_limit": -1,
    "download_expiry": -1,
    "external_url": "",
    "button_text": "",
    "tax_status": "taxable",
    "tax_class": "",
    "manage_stock": false,
    "stock_quantity": null,
    "backorders": "no",
    "backorders_allowed": false,
    "backordered": false,
    "low_stock_amount": null,
    "sold_individually": false,
    "weight": "",
    "dimensions": {
        "length": "",
        "width": "",
        "height": ""
    },
    "shipping_required": true,
    "shipping_taxable": true,
    "shipping_class": "",
    "shipping_class_id": 0,
    "reviews_allowed": true,
    "average_rating": "0.00",
    "rating_count": 0,
    "upsell_ids": [],
    "cross_sell_ids": [],
    "parent_id": 0,
    "purchase_note": "",
    "categories": [
        {
            "id": 18,
            "name": "Man",
            "slug": "man"
        }
    ],
    "tags": [],
    "images": [
        {
            "id": 63,
            "date_created": "2023-08-28T20:03:51",
            "date_created_gmt": "2023-08-28T20:03:51",
            "date_modified": "2023-08-28T20:03:51",
            "date_modified_gmt": "2023-08-28T20:03:51",
            "src": "https://www.teamjoyful.buzz/wp-content/uploads/2023/08/s-l500.jpg",
            "name": "s-l500",
            "alt": ""
        }
    ],
    "attributes": [
        {
            "id": 3,
            "name": "Brand",
            "position": 0,
            "visible": true,
            "variation": false,
            "options": [
                "New Balance"
            ]
        },
        {
            "id": 1,
            "name": "Color",
            "position": 1,
            "visible": true,
            "variation": false,
            "options": [
                "Black"
            ]
        },
        {
            "id": 5,
            "name": "Condition",
            "position": 2,
            "visible": true,
            "variation": false,
            "options": [
                "New"
            ]
        },
        {
            "id": 4,
            "name": "Gender",
            "position": 3,
            "visible": true,
            "variation": false,
            "options": [
                "Male"
            ]
        },
        {
            "id": 2,
            "name": "Size",
            "position": 4,
            "visible": true,
            "variation": false,
            "options": [
                "xs"
            ]
        }
    ],
    "default_attributes": [],
    "variations": [],
    "grouped_products": [],
    "menu_order": 0,
    "price_html": "<del aria-hidden=\"true\"><span class=\"woocommerce-Price-amount amount\"><bdi><span class=\"woocommerce-Price-currencySymbol\">&#36;</span>80.00</bdi></span></del> <ins><span class=\"woocommerce-Price-amount amount\"><bdi><span class=\"woocommerce-Price-currencySymbol\">&#36;</span>58.00</bdi></span></ins>",
    "related_ids": [
        56,
        46,
        54,
        28,
        44
    ],
    "meta_data": [],
    "stock_status": "instock",
    "has_options": false,
    "_links": {
        "self": [
            {
                "href": "https://www.teamjoyful.buzz/wp-json/wc/v3/products/62"
            }
        ],
        "collection": [
            {
                "href": "https://www.teamjoyful.buzz/wp-json/wc/v3/products"
            }
        ]
    }
}
```
