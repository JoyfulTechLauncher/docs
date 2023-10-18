# WooCommerce-REST-API

This document is a introductory documnet for the WooCommerce-REST-API.

## 1. Basic Settings

### Requirements

WordPress permalinks must be set to something that is easily human readable at: **Settings** > **Permalinks**.

Day and name is a great default, but anything aside from Plain should work

![image](https://github.com/JoyfulTechLauncher/docs/assets/67152451/86772c0d-51dd-4f72-8ad2-e3a24f306808)

### Generate API keys

The WooCommerce REST API works on a key system to control access. These keys are linked to WordPress users on your website.

![image](https://github.com/JoyfulTechLauncher/docs/assets/67152451/d3d80730-c3d1-4acf-be41-aeec1f3acd50)

To create or manage keys for a specific WordPress user:

1. Go to: **WooCommerce** > **Settings** > **Advanced** > **REST API**.
2. Select **Add Key**. You are taken to the **Key Details** screen

![image](https://github.com/JoyfulTechLauncher/docs/assets/67152451/ea75ceae-dbbb-4c92-9cbf-a135f3fe35f2)

3. Add a **Description**.
4. Select the **User** you would like to generate a key for in the dropdown.
5. Select a level of access for this API key — **Read** access, **Write** access or **Read/Write** access.
6. Select **Generate API Key**, and WooCommerce creates API keys for that user.

Now that keys have been generated, you should see **Consumer Key** and **Consumer Secret** keys, a QRCode, and a Revoke API Key button.

![image](https://github.com/JoyfulTechLauncher/docs/assets/67152451/5ebc9a80-7e76-4cb5-b79e-672cd675f7e1)

The **Consumer Key** and **Consumer Secret** may be entered in the application using the WooCommerce API, and the app should also request your URL.

## 2. Authentications

### REST API keys

Pre-generated keys can be used to authenticate use of the REST API endpoints. New keys can be generated either through the WordPress admin interface or they can be auto-generated through an endpoint.

### Generating API keys in the WordPress admin interface

To create or manage keys for a specific WordPress user, go to **WooCommerce** > **Settings** > **Advanced** > **REST API**.

![image](https://github.com/JoyfulTechLauncher/docs/assets/67152451/e3f2ed4c-058a-4324-b168-794f1857cb48)

Click the **Add Key** button. In the next screen, add a description and select the WordPress user you would like to generate the key for. Use of the REST API with the generated keys will conform to that user's WordPress roles and capabilities.

Choose the level of access for this REST API key, which can be **Read access**, **Write access** or **Read/Write** access. Then click the **Generate API Key** button and WooCommerce will generate REST API keys for the selected user.

![image](https://github.com/JoyfulTechLauncher/docs/assets/67152451/c042a050-0142-44ad-83cd-b86090a96746)

Now that keys have been generated, you should see two new keys, a QRCode, and a Revoke API Key button. These two keys are your Consumer Key and Consumer Secret.

![image](https://github.com/JoyfulTechLauncher/docs/assets/67152451/960eef34-8b84-48bf-a1e3-16810b24558f)

If the WordPress user associated with an API key is deleted, the API key will cease to function. API keys are not transferred to other users.

### Auto generating API keys using our Application Authentication Endpoint

This endpoint can be used by any APP to allow users to generate API keys for your APP. This makes integration with WooCommerce API easier because the user only needs to grant access to your APP via a URL. After being redirected back to your APP, the API keys will be sent back in a separate POST request.

The following image illustrates how this works:

![image](https://github.com/JoyfulTechLauncher/docs/assets/67152451/5254b0e3-5cf2-41ad-93ba-41c9df95ac37)

### Creating an authentication endpoint URL

You must use the `/wc-auth/v1/authorize` endpoint and pass the above parameters as a query string.

### Example of how to build an authentication URL:

```
# Bash example
STORE_URL='http://example.com'
ENDPOINT='/wc-auth/v1/authorize'
PARAMS="app_name=My App Name&scope=read_write&user_id=123&return_url=http://app.com/return-page&callback_url=https://app.com/callback-endpoint"
QUERY_STRING="$(perl -MURI::Escape -e 'print uri_escape($ARGV[0]);' "$PARAMS")"
QUERY_STRING=$(echo $QUERY_STRING | sed -e "s/%20/\+/g" -e "s/%3D/\=/g" -e "s/%26/\&/g")

echo "$STORE_URL$ENDPOINT?$QUERY_STRING"
```

### Example of JSON posted with the API Keys

```
{
    "key_id": 1,
    "user_id": 123,
    "consumer_key": "ck_xxxxxxxxxxxxxxxx",
    "consumer_secret": "cs_xxxxxxxxxxxxxxxx",
    "key_permissions": "read_write"
}
```
# Main API used in the project

## 3. Index

By default, the API provides information about all available endpoints on the site. Authentication is not required to access the API index.

### HTTP request

`GET` `/wp-json/wc/v3`

### Input Example

```
GET https://app.com/wp-json/wc/v3
```

### Output Example

```
{
  "namespace": "wc/v3",
  "routes": {
    "/wc/v3": {
      "namespace": "wc/v3",
      "methods": [
        "GET"
      ],
      "endpoints": [
        {
          "methods": [
            "GET"
          ],
          "args": {
            "namespace": {
              "required": false,
              "default": "wc/v3"
            },
            "context": {
              "required": false,
              "default": "view"
            }
          }
        }
      ],
      "_links": {
        "self": "https://example.com/wp-json/wc/v3"
      }
    },
    "/wc/v3/coupons": {
      "namespace": "wc/v3",
      "methods": [
        "GET",
        "POST"
      ],
      "endpoints": [
        {
          "methods": [
            "GET"
          ],
          "args": {
            "context": {
              "required": false,
              "default": "view",
              "enum": [
                "view",
                "edit"
              ],
              "description": "Scope under which the request is made; determines fields present in response.",
              "type": "string"
            },
            "page": {
              "required": false,
              "default": 1,
              "description": "Current page of the collection.",
              "type": "integer"
            },
            "per_page": {
              "required": false,
              "default": 10,
              "description": "Maximum number of items to be returned in result set.",
              "type": "integer"
            },
            "search": {
              "required": false,
              "description": "Limit results to those matching a string.",
              "type": "string"
            },
            "after": {
              "required": false,
              "description": "Limit response to resources published after a given ISO8601 compliant date.",
              "type": "string"
            },
            "before": {
              "required": false,
              "description": "Limit response to resources published before a given ISO8601 compliant date.",
              "type": "string"
            },
            "exclude": {
              "required": false,
              "default": [],
              "description": "Ensure result set excludes specific IDs.",
              "type": "array",
              "items": {
                "type": "integer"
              }
            },
            "include": {
              "required": false,
              "default": [],
              "description": "Limit result set to specific ids.",
              "type": "array",
              "items": {
                "type": "integer"
              }
            },
            "offset": {
              "required": false,
              "description": "Offset the result set by a specific number of items.",
              "type": "integer"
            },
            "order": {
              "required": false,
              "default": "desc",
              "enum": [
                "asc",
                "desc"
              ],
              "description": "Order sort attribute ascending or descending.",
              "type": "string"
            },
            "orderby": {
              "required": false,
              "default": "date",
              "enum": [
                "date",
                "id",
                "include",
                "title",
                "slug"
              ],
              "description": "Sort collection by object attribute.",
              "type": "string"
            },
            "code": {
              "required": false,
              "description": "Limit result set to resources with a specific code.",
              "type": "string"
            }
          }
        },
        {
          "methods": [
            "POST"
          ],
          "args": {
            "code": {
              "required": true,
              "description": "Coupon code.",
              "type": "string"
            },
            "amount": {
              "required": false,
              "description": "The amount of discount. Should always be numeric, even if setting a percentage.",
              "type": "string"
            },
            "discount_type": {
              "required": false,
              "default": "fixed_cart",
              "enum": [
                "percent",
                "fixed_cart",
                "fixed_product"
              ],
              "description": "Determines the type of discount that will be applied.",
              "type": "string"
            },
            "description": {
              "required": false,
              "description": "Coupon description.",
              "type": "string"
            },
            "date_expires": {
              "required": false,
              "description": "The date the coupon expires, in the site's timezone.",
              "type": "string"
            },
            "date_expires_gmt": {
              "required": false,
              "description": "The date the coupon expires, as GMT.",
              "type": "string"
            },
            "individual_use": {
              "required": false,
              "default": false,
              "description": "If true, the coupon can only be used individually. Other applied coupons will be removed from the cart.",
              "type": "boolean"
            },
            "product_ids": {
              "required": false,
              "description": "List of product IDs the coupon can be used on.",
              "type": "array",
              "items": {
                "type": "integer"
              }
            },
            "excluded_product_ids": {
              "required": false,
              "description": "List of product IDs the coupon cannot be used on.",
              "type": "array",
              "items": {
                "type": "integer"
              }
            },
            "usage_limit": {
              "required": false,
              "description": "How many times the coupon can be used in total.",
              "type": "integer"
            },
            "usage_limit_per_user": {
              "required": false,
              "description": "How many times the coupon can be used per customer.",
              "type": "integer"
            },
            "limit_usage_to_x_items": {
              "required": false,
              "description": "Max number of items in the cart the coupon can be applied to.",
              "type": "integer"
            },
            "free_shipping": {
              "required": false,
              "default": false,
              "description": "If true and if the free shipping method requires a coupon, this coupon will enable free shipping.",
              "type": "boolean"
            },
            "product_categories": {
              "required": false,
              "description": "List of category IDs the coupon applies to.",
              "type": "array",
              "items": {
                "type": "integer"
              }
            },
            "excluded_product_categories": {
              "required": false,
              "description": "List of category IDs the coupon does not apply to.",
              "type": "array",
              "items": {
                "type": "integer"
              }
            },
            "exclude_sale_items": {
              "required": false,
              "default": false,
              "description": "If true, this coupon will not be applied to items that have sale prices.",
              "type": "boolean"
            },
            "minimum_amount": {
              "required": false,
              "description": "Minimum order amount that needs to be in the cart before coupon applies.",
              "type": "string"
            },
            "maximum_amount": {
              "required": false,
              "description": "Maximum order amount allowed when using the coupon.",
              "type": "string"
            },
            "email_restrictions": {
              "required": false,
              "description": "List of email addresses that can use this coupon.",
              "type": "array",
              "items": {
                "type": "string"
              }
            },
            "meta_data": {
              "required": false,
              "description": "Meta data.",
              "type": "array",
              "items": {
                "type": "object",
                "properties": {
                  "id": {
                    "description": "Meta ID.",
                    "type": "integer",
                    "context": [
                      "view",
                      "edit"
                    ],
                    "readonly": true
                  },
                  "key": {
                    "description": "Meta key.",
                    "type": "string",
                    "context": [
                      "view",
                      "edit"
                    ]
                  },
                  "value": {
                    "description": "Meta value.",
                    "type": "string",
                    "context": [
                      "view",
                      "edit"
                    ]
                  }
                }
              }
            }
          }
        }
      ],
      "_links": {
        "self": "https://example.com/wp-json/wc/v3/coupons"
      }
    },
    "/wc/v3/coupons/(?P<id>[\\d]+)": {
      "namespace": "wc/v3",
      "methods": [
        "GET",
        "POST",
        "PUT",
        "PATCH",
        "DELETE"
      ],
      "endpoints": [
        {
          "methods": [
            "GET"
          ],
          "args": {
            "id": {
              "required": false,
              "description": "Unique identifier for the resource.",
              "type": "integer"
            },
            "context": {
              "required": false,
              "default": "view",
              "enum": [
                "view",
                "edit"
              ],
              "description": "Scope under which the request is made; determines fields present in response.",
              "type": "string"
            }
          }
        },
        {
          "methods": [
            "POST",
            "PUT",
            "PATCH"
          ],
          "args": {
            "id": {
              "required": false,
              "description": "Unique identifier for the resource.",
              "type": "integer"
            },
            "code": {
              "required": false,
              "description": "Coupon code.",
              "type": "string"
            },
            "amount": {
              "required": false,
              "description": "The amount of discount. Should always be numeric, even if setting a percentage.",
              "type": "string"
            },
            "discount_type": {
              "required": false,
              "enum": [
                "percent",
                "fixed_cart",
                "fixed_product"
              ],
              "description": "Determines the type of discount that will be applied.",
              "type": "string"
            },
            "description": {
              "required": false,
              "description": "Coupon description.",
              "type": "string"
            },
            "date_expires": {
              "required": false,
              "description": "The date the coupon expires, in the site's timezone.",
              "type": "string"
            },
            "date_expires_gmt": {
              "required": false,
              "description": "The date the coupon expires, as GMT.",
              "type": "string"
            },
            "individual_use": {
              "required": false,
              "description": "If true, the coupon can only be used individually. Other applied coupons will be removed from the cart.",
              "type": "boolean"
            },
            "product_ids": {
              "required": false,
              "description": "List of product IDs the coupon can be used on.",
              "type": "array",
              "items": {
                "type": "integer"
              }
            },
            "excluded_product_ids": {
              "required": false,
              "description": "List of product IDs the coupon cannot be used on.",
              "type": "array",
              "items": {
                "type": "integer"
              }
            },
            "usage_limit": {
              "required": false,
              "description": "How many times the coupon can be used in total.",
              "type": "integer"
            },
            "usage_limit_per_user": {
              "required": false,
              "description": "How many times the coupon can be used per customer.",
              "type": "integer"
            },
            "limit_usage_to_x_items": {
              "required": false,
              "description": "Max number of items in the cart the coupon can be applied to.",
              "type": "integer"
            },
            "free_shipping": {
              "required": false,
              "description": "If true and if the free shipping method requires a coupon, this coupon will enable free shipping.",
              "type": "boolean"
            },
            "product_categories": {
              "required": false,
              "description": "List of category IDs the coupon applies to.",
              "type": "array",
              "items": {
                "type": "integer"
              }
            },
            "excluded_product_categories": {
              "required": false,
              "description": "List of category IDs the coupon does not apply to.",
              "type": "array",
              "items": {
                "type": "integer"
              }
            },
            "exclude_sale_items": {
              "required": false,
              "description": "If true, this coupon will not be applied to items that have sale prices.",
              "type": "boolean"
            },
            "minimum_amount": {
              "required": false,
              "description": "Minimum order amount that needs to be in the cart before coupon applies.",
              "type": "string"
            },
            "maximum_amount": {
              "required": false,
              "description": "Maximum order amount allowed when using the coupon.",
              "type": "string"
            },
            "email_restrictions": {
              "required": false,
              "description": "List of email addresses that can use this coupon.",
              "type": "array",
              "items": {
                "type": "string"
              }
            },
            "meta_data": {
              "required": false,
              "description": "Meta data.",
              "type": "array",
              "items": {
                "type": "object",
                "properties": {
                  "id": {
                    "description": "Meta ID.",
                    "type": "integer",
                    "context": [
                      "view",
                      "edit"
                    ],
                    "readonly": true
                  },
                  "key": {
                    "description": "Meta key.",
                    "type": "string",
                    "context": [
                      "view",
                      "edit"
                    ]
                  },
                  "value": {
                    "description": "Meta value.",
                    "type": "string",
                    "context": [
                      "view",
                      "edit"
                    ]
                  }
                }
              }
            }
          }
        },
        {
          "methods": [
            "DELETE"
          ],
          "args": {
            "id": {
              "required": false,
              "description": "Unique identifier for the resource.",
              "type": "integer"
            },
            "force": {
              "required": false,
              "default": false,
              "description": "Whether to bypass trash and force deletion.",
              "type": "boolean"
            }
          }
        }
      ]
    },
    "/wc/v3/coupons/batch": {
      "namespace": "wc/v3",
      "methods": [
        "POST",
        "PUT",
        "PATCH"
      ],
      "endpoints": [
        {
          "methods": [
            "POST",
            "PUT",
            "PATCH"
          ],
          "args": {
            "code": {
              "required": false,
              "description": "Coupon code.",
              "type": "string"
            },
            "amount": {
              "required": false,
              "description": "The amount of discount. Should always be numeric, even if setting a percentage.",
              "type": "string"
            },
            "discount_type": {
              "required": false,
              "enum": [
                "percent",
                "fixed_cart",
                "fixed_product"
              ],
              "description": "Determines the type of discount that will be applied.",
              "type": "string"
            },
            "description": {
              "required": false,
              "description": "Coupon description.",
              "type": "string"
            },
            "date_expires": {
              "required": false,
              "description": "The date the coupon expires, in the site's timezone.",
              "type": "string"
            },
            "date_expires_gmt": {
              "required": false,
              "description": "The date the coupon expires, as GMT.",
              "type": "string"
            },
            "individual_use": {
              "required": false,
              "description": "If true, the coupon can only be used individually. Other applied coupons will be removed from the cart.",
              "type": "boolean"
            },
            "product_ids": {
              "required": false,
              "description": "List of product IDs the coupon can be used on.",
              "type": "array",
              "items": {
                "type": "integer"
              }
            },
            "excluded_product_ids": {
              "required": false,
              "description": "List of product IDs the coupon cannot be used on.",
              "type": "array",
              "items": {
                "type": "integer"
              }
            },
            "usage_limit": {
              "required": false,
              "description": "How many times the coupon can be used in total.",
              "type": "integer"
            },
            "usage_limit_per_user": {
              "required": false,
              "description": "How many times the coupon can be used per customer.",
              "type": "integer"
            },
            "limit_usage_to_x_items": {
              "required": false,
              "description": "Max number of items in the cart the coupon can be applied to.",
              "type": "integer"
            },
            "free_shipping": {
              "required": false,
              "description": "If true and if the free shipping method requires a coupon, this coupon will enable free shipping.",
              "type": "boolean"
            },
            "product_categories": {
              "required": false,
              "description": "List of category IDs the coupon applies to.",
              "type": "array",
              "items": {
                "type": "integer"
              }
            },
            "excluded_product_categories": {
              "required": false,
              "description": "List of category IDs the coupon does not apply to.",
              "type": "array",
              "items": {
                "type": "integer"
              }
            },
            "exclude_sale_items": {
              "required": false,
              "description": "If true, this coupon will not be applied to items that have sale prices.",
              "type": "boolean"
            },
            "minimum_amount": {
              "required": false,
              "description": "Minimum order amount that needs to be in the cart before coupon applies.",
              "type": "string"
            },
            "maximum_amount": {
              "required": false,
              "description": "Maximum order amount allowed when using the coupon.",
              "type": "string"
            },
            "email_restrictions": {
              "required": false,
              "description": "List of email addresses that can use this coupon.",
              "type": "array",
              "items": {
                "type": "string"
              }
            },
            "meta_data": {
              "required": false,
              "description": "Meta data.",
              "type": "array",
              "items": {
                "type": "object",
                "properties": {
                  "id": {
                    "description": "Meta ID.",
                    "type": "integer",
                    "context": [
                      "view",
                      "edit"
                    ],
                    "readonly": true
                  },
                  "key": {
                    "description": "Meta key.",
                    "type": "string",
                    "context": [
                      "view",
                      "edit"
                    ]
                  },
                  "value": {
                    "description": "Meta value.",
                    "type": "string",
                    "context": [
                      "view",
                      "edit"
                    ]
                  }
                }
              }
            }
          }
        }

```


## 4. Customers

The customer API allows you to create, view, update, and delete individual, or a batch, of customers.

### Customer properties

| Attribute | Type | Description |
| --- | --- | --- |
| `id` | integer | Unique identifier for the resource.    `READ-ONLY` |
| `date_created` | date-time | The date the customer was created, in the site's timezone.    `READ-ONLY` |
| `date_created_gmt` | date-time | The date the customer was created, as GMT.    `READ-ONLY` |
| `date_modified` | date-time | The date the customer was last modified, in the site's timezone.    `READ-ONLY` |
| `date_modified_gmt` | date-time |	The date the customer was last modified, as GMT.    `READ-ONLY` |
| `email` | string | The email address for the customer.    `MANDATORY` |
| `first_name` | string	| Customer first name. |
| `last_name` |	string | Customer last name. |
| `role` | string | Customer role.   `READ-ONLY` |
| `username` | string |	Customer login name. |
| `password` | string | Customer password.    `WRITE-ONLY` |
| `billing` | object | List of billing address data. |
| `shipping` | object |	List of shipping address data. |
| `is_paying_customer` | bool |	Is the customer a paying customer?    `READ-ONLY` |
| `avatar_url` | string | Avatar URL.   `READ-ONLY` |
| `meta_data` |	array |	Meta data. |

- Other details see: https://woocommerce.github.io/woocommerce-rest-api-docs/?shell#customer-properties

### Create a customer

`POST` `/wp-json/wc/v3/customers`

### Input Example

```
POST https://app.com/wp-json/wc/v3/customers
```

### Retrieve a customer

`GET` `/wp-json/wc/v3/customers/<id>`

### Input Example

```
GET https://app.com/wp-json/wc/v3/customers/25
```

### Output Example
JSON response example:
```
{
  "id": 25,
  "date_created": "2017-03-21T16:09:28",
  "date_created_gmt": "2017-03-21T19:09:28",
  "date_modified": "2017-03-21T16:09:30",
  "date_modified_gmt": "2017-03-21T19:09:30",
  "email": "john.doe@example.com",
  "first_name": "John",
  "last_name": "Doe",
  "role": "customer",
  "username": "john.doe",
  "billing": {
    "first_name": "John",
    "last_name": "Doe",
    "company": "",
    "address_1": "969 Market",
    "address_2": "",
    "city": "San Francisco",
    "state": "CA",
    "postcode": "94103",
    "country": "US",
    "email": "john.doe@example.com",
    "phone": "(555) 555-5555"
  },
  "shipping": {
    "first_name": "John",
    "last_name": "Doe",
    "company": "",
    "address_1": "969 Market",
    "address_2": "",
    "city": "San Francisco",
    "state": "CA",
    "postcode": "94103",
    "country": "US"
  },
  "is_paying_customer": false,
  "avatar_url": "https://secure.gravatar.com/avatar/8eb1b522f60d11fa897de1dc6351b7e8?s=96",
  "meta_data": [],
  "_links": {
    "self": [
      {
        "href": "https://example.com/wp-json/wc/v3/customers/25"
      }
    ],
    "collection": [
      {
        "href": "https://example.com/wp-json/wc/v3/customers"
      }
    ]
  }
}
```
### List all customers

`GET` `/wp-json/wc/v3/customers`

- Details for attributes : https://woocommerce.github.io/woocommerce-rest-api-docs/?shell#list-all-customers

### Input Example

```
GET https://app.com/wp-json/wc/v3/customers
```

### Output Example

```

```
### Update a customer

`PUT` `/wp-json/wc/v3/customers/<id>`

```
curl -X PUT https://example.com/wp-json/wc/v3/customers/25 \
    -u consumer_key:consumer_secret \
    -H "Content-Type: application/json" \
    -d '{
  "first_name": "James",
  "billing": {
    "first_name": "James"
  },
  "shipping": {
    "first_name": "James"
  }
}'
```

### Delete a customer

`DELETE` `/wp-json/wc/v3/customers/<id>`

## 5. Orders

- Properties of orders see: https://woocommerce.github.io/woocommerce-rest-api-docs/?shell#order-properties

### Create an order

`POST` `/wp-json/wc/v3/orders`

### Retrieve an order

`GET` `/wp-json/wc/v3/orders/<id>`

### List all orders

`GET` `/wp-json/wc/v3/orders`

- Parameters of orders see: https://woocommerce.github.io/woocommerce-rest-api-docs/?shell#list-all-orders

### Update an Order

`PUT` `/wp-json/wc/v3/orders/<id>`

```
curl -X PUT https://example.com/wp-json/wc/v3/orders/727 \
    -u consumer_key:consumer_secret \
    -H "Content-Type: application/json" \
    -d '{
  "status": "completed"
}'
```

### Delete an order

`DELETE` `/wp-json/wc/v3/orders/<id>`

## 6. Products

- Products properties see: https://woocommerce.github.io/woocommerce-rest-api-docs/?shell#product-properties
  
### Create a product

`POST` `/wp-json/wc/v3/products`

- example of how to create a product:
  
```
curl -X POST https://example.com/wp-json/wc/v3/products \
    -u consumer_key:consumer_secret \
    -H "Content-Type: application/json" \
    -d '{
  "name": "Premium Quality",
  "type": "simple",
  "regular_price": "21.99",
  "description": "Pellentesque habitant morbi tristique senectus et netus et malesuada fames ac turpis egestas. Vestibulum tortor quam, feugiat vitae, ultricies eget, tempor sit amet, ante. Donec eu libero sit amet quam egestas semper. Aenean ultricies mi vitae est. Mauris placerat eleifend leo.",
  "short_description": "Pellentesque habitant morbi tristique senectus et netus et malesuada fames ac turpis egestas.",
  "categories": [
    {
      "id": 9
    },
    {
      "id": 14
    }
  ],
  "images": [
    {
      "src": "http://demo.woothemes.com/woocommerce/wp-content/uploads/sites/56/2013/06/T_2_front.jpg"
    },
    {
      "src": "http://demo.woothemes.com/woocommerce/wp-content/uploads/sites/56/2013/06/T_2_back.jpg"
    }
  ]
}'


```
### Retrieve a product

`GET` `/wp-json/wc/v3/products/<id>`

- example of how to retrieve a product
  
```
curl https://example.com/wp-json/wc/v3/products/794 \
    -u consumer_key:consumer_secret
```

### List all products

`GET` `/wp-json/wc/v3/products`

- parameters of products see: https://woocommerce.github.io/woocommerce-rest-api-docs/?shell#list-all-products

### Update a product

`PUT` `/wp-json/wc/v3/products/<id>`

### Delete a product

`DELETE` `/wp-json/wc/v3/products/<id>`


