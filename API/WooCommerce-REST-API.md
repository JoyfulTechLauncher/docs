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
[
  {
    "id": 26,
    "date_created": "2017-03-21T16:11:14",
    "date_created_gmt": "2017-03-21T19:11:14",
    "date_modified": "2017-03-21T16:11:16",
    "date_modified_gmt": "2017-03-21T19:11:16",
    "email": "joao.silva@example.com",
    "first_name": "João",
    "last_name": "Silva",
    "role": "customer",
    "username": "joao.silva",
    "billing": {
      "first_name": "João",
      "last_name": "Silva",
      "company": "",
      "address_1": "Av. Brasil, 432",
      "address_2": "",
      "city": "Rio de Janeiro",
      "state": "RJ",
      "postcode": "12345-000",
      "country": "BR",
      "email": "joao.silva@example.com",
      "phone": "(55) 5555-5555"
    },
    "shipping": {
      "first_name": "João",
      "last_name": "Silva",
      "company": "",
      "address_1": "Av. Brasil, 432",
      "address_2": "",
      "city": "Rio de Janeiro",
      "state": "RJ",
      "postcode": "12345-000",
      "country": "BR"
    },
    "is_paying_customer": false,
    "avatar_url": "https://secure.gravatar.com/avatar/be7b5febff88a2d947c3289e90cdf017?s=96",
    "meta_data": [],
    "_links": {
      "self": [
        {
          "href": "https://example.com/wp-json/wc/v3/customers/26"
        }
      ],
      "collection": [
        {
          "href": "https://example.com/wp-json/wc/v3/customers"
        }
      ]
    }
  },
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
]
```
### Update a customer

`PUT` `/wp-json/wc/v3/customers/<id>`

### Input Example

```
PUT https://app.com/wp-json/wc/v3/customers/25
```

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

### Output Example

```
{
  "id": 25,
  "date_created": "2017-03-21T16:09:28",
  "date_created_gmt": "2017-03-21T19:09:28",
  "date_modified": "2017-03-21T16:12:28",
  "date_modified_gmt": "2017-03-21T19:12:28",
  "email": "john.doe@example.com",
  "first_name": "James",
  "last_name": "Doe",
  "role": "customer",
  "username": "john.doe",
  "billing": {
    "first_name": "James",
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
    "first_name": "James",
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

### Delete a customer

`DELETE` `/wp-json/wc/v3/customers/<id>`

### Input Example

DELETE https://app.com/wp-json/wc/v3/customers/25

## 5. Orders

- Properties of orders see: https://woocommerce.github.io/woocommerce-rest-api-docs/?shell#order-properties

### Create an order

`POST` `/wp-json/wc/v3/orders`

### Input Example

```
POST https://app.com/wp-json/wc/v3/orders
```

```
curl -X POST https://example.com/wp-json/wc/v3/orders \
    -u consumer_key:consumer_secret \
    -H "Content-Type: application/json" \
    -d '{
  "payment_method": "bacs",
  "payment_method_title": "Direct Bank Transfer",
  "set_paid": true,
  "billing": {
    "first_name": "John",
    "last_name": "Doe",
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
    "address_1": "969 Market",
    "address_2": "",
    "city": "San Francisco",
    "state": "CA",
    "postcode": "94103",
    "country": "US"
  },
  "line_items": [
    {
      "product_id": 93,
      "quantity": 2
    },
    {
      "product_id": 22,
      "variation_id": 23,
      "quantity": 1
    }
  ],
  "shipping_lines": [
    {
      "method_id": "flat_rate",
      "method_title": "Flat Rate",
      "total": "10.00"
    }
  ]
}'
```

### Output Example

```
{
  "id": 727,
  "parent_id": 0,
  "number": "727",
  "order_key": "wc_order_58d2d042d1d",
  "created_via": "rest-api",
  "version": "3.0.0",
  "status": "processing",
  "currency": "USD",
  "date_created": "2017-03-22T16:28:02",
  "date_created_gmt": "2017-03-22T19:28:02",
  "date_modified": "2017-03-22T16:28:08",
  "date_modified_gmt": "2017-03-22T19:28:08",
  "discount_total": "0.00",
  "discount_tax": "0.00",
  "shipping_total": "10.00",
  "shipping_tax": "0.00",
  "cart_tax": "1.35",
  "total": "29.35",
  "total_tax": "1.35",
  "prices_include_tax": false,
  "customer_id": 0,
  "customer_ip_address": "",
  "customer_user_agent": "",
  "customer_note": "",
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
  "payment_method": "bacs",
  "payment_method_title": "Direct Bank Transfer",
  "transaction_id": "",
  "date_paid": "2017-03-22T16:28:08",
  "date_paid_gmt": "2017-03-22T19:28:08",
  "date_completed": null,
  "date_completed_gmt": null,
  "cart_hash": "",
  "meta_data": [
    {
      "id": 13106,
      "key": "_download_permissions_granted",
      "value": "yes"
    }
  ],
  "line_items": [
    {
      "id": 315,
      "name": "Woo Single #1",
      "product_id": 93,
      "variation_id": 0,
      "quantity": 2,
      "tax_class": "",
      "subtotal": "6.00",
      "subtotal_tax": "0.45",
      "total": "6.00",
      "total_tax": "0.45",
      "taxes": [
        {
          "id": 75,
          "total": "0.45",
          "subtotal": "0.45"
        }
      ],
      "meta_data": [],
      "sku": "",
      "price": 3
    },
    {
      "id": 316,
      "name": "Ship Your Idea &ndash; Color: Black, Size: M Test",
      "product_id": 22,
      "variation_id": 23,
      "quantity": 1,
      "tax_class": "",
      "subtotal": "12.00",
      "subtotal_tax": "0.90",
      "total": "12.00",
      "total_tax": "0.90",
      "taxes": [
        {
          "id": 75,
          "total": "0.9",
          "subtotal": "0.9"
        }
      ],
      "meta_data": [
        {
          "id": 2095,
          "key": "pa_color",
          "value": "black"
        },
        {
          "id": 2096,
          "key": "size",
          "value": "M Test"
        }
      ],
      "sku": "Bar3",
      "price": 12
    }
  ],
  "tax_lines": [
    {
      "id": 318,
      "rate_code": "US-CA-STATE TAX",
      "rate_id": 75,
      "label": "State Tax",
      "compound": false,
      "tax_total": "1.35",
      "shipping_tax_total": "0.00",
      "meta_data": []
    }
  ],
  "shipping_lines": [
    {
      "id": 317,
      "method_title": "Flat Rate",
      "method_id": "flat_rate",
      "total": "10.00",
      "total_tax": "0.00",
      "taxes": [],
      "meta_data": []
    }
  ],
  "fee_lines": [],
  "coupon_lines": [],
  "refunds": [],
  "_links": {
    "self": [
      {
        "href": "https://example.com/wp-json/wc/v3/orders/727"
      }
    ],
    "collection": [
      {
        "href": "https://example.com/wp-json/wc/v3/orders"
      }
    ]
  }
}
```

### Retrieve an order

`GET` `/wp-json/wc/v3/orders/<id>`

### Input Example

```
GET https://app.com/wp-json/wc/v3/orders/727
```

### Output Example

```
{
  "id": 727,
  "parent_id": 0,
  "number": "727",
  "order_key": "wc_order_58d2d042d1d",
  "created_via": "rest-api",
  "version": "3.0.0",
  "status": "processing",
  "currency": "USD",
  "date_created": "2017-03-22T16:28:02",
  "date_created_gmt": "2017-03-22T19:28:02",
  "date_modified": "2017-03-22T16:28:08",
  "date_modified_gmt": "2017-03-22T19:28:08",
  "discount_total": "0.00",
  "discount_tax": "0.00",
  "shipping_total": "10.00",
  "shipping_tax": "0.00",
  "cart_tax": "1.35",
  "total": "29.35",
  "total_tax": "1.35",
  "prices_include_tax": false,
  "customer_id": 0,
  "customer_ip_address": "",
  "customer_user_agent": "",
  "customer_note": "",
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
  "payment_method": "bacs",
  "payment_method_title": "Direct Bank Transfer",
  "transaction_id": "",
  "date_paid": "2017-03-22T16:28:08",
  "date_paid_gmt": "2017-03-22T19:28:08",
  "date_completed": null,
  "date_completed_gmt": null,
  "cart_hash": "",
  "meta_data": [
    {
      "id": 13106,
      "key": "_download_permissions_granted",
      "value": "yes"
    }
  ],
  "line_items": [
    {
      "id": 315,
      "name": "Woo Single #1",
      "product_id": 93,
      "variation_id": 0,
      "quantity": 2,
      "tax_class": "",
      "subtotal": "6.00",
      "subtotal_tax": "0.45",
      "total": "6.00",
      "total_tax": "0.45",
      "taxes": [
        {
          "id": 75,
          "total": "0.45",
          "subtotal": "0.45"
        }
      ],
      "meta_data": [],
      "sku": "",
      "price": 3
    },
    {
      "id": 316,
      "name": "Ship Your Idea &ndash; Color: Black, Size: M Test",
      "product_id": 22,
      "variation_id": 23,
      "quantity": 1,
      "tax_class": "",
      "subtotal": "12.00",
      "subtotal_tax": "0.90",
      "total": "12.00",
      "total_tax": "0.90",
      "taxes": [
        {
          "id": 75,
          "total": "0.9",
          "subtotal": "0.9"
        }
      ],
      "meta_data": [
        {
          "id": 2095,
          "key": "pa_color",
          "value": "black"
        },
        {
          "id": 2096,
          "key": "size",
          "value": "M Test"
        }
      ],
      "sku": "Bar3",
      "price": 12
    }
  ],
  "tax_lines": [
    {
      "id": 318,
      "rate_code": "US-CA-STATE TAX",
      "rate_id": 75,
      "label": "State Tax",
      "compound": false,
      "tax_total": "1.35",
      "shipping_tax_total": "0.00",
      "meta_data": []
    }
  ],
  "shipping_lines": [
    {
      "id": 317,
      "method_title": "Flat Rate",
      "method_id": "flat_rate",
      "total": "10.00",
      "total_tax": "0.00",
      "taxes": [],
      "meta_data": []
    }
  ],
  "fee_lines": [],
  "coupon_lines": [],
  "refunds": [],
  "_links": {
    "self": [
      {
        "href": "https://example.com/wp-json/wc/v3/orders/727"
      }
    ],
    "collection": [
      {
        "href": "https://example.com/wp-json/wc/v3/orders"
      }
    ]
  }
}
```

### List all orders

`GET` `/wp-json/wc/v3/orders`

- Parameters of orders see: https://woocommerce.github.io/woocommerce-rest-api-docs/?shell#list-all-orders

### Input Example

```
GET https://app.com/wp-json/wc/v3/orders
```

### Output Example

```
[
  {
    "id": 727,
    "parent_id": 0,
    "number": "727",
    "order_key": "wc_order_58d2d042d1d",
    "created_via": "rest-api",
    "version": "3.0.0",
    "status": "processing",
    "currency": "USD",
    "date_created": "2017-03-22T16:28:02",
    "date_created_gmt": "2017-03-22T19:28:02",
    "date_modified": "2017-03-22T16:28:08",
    "date_modified_gmt": "2017-03-22T19:28:08",
    "discount_total": "0.00",
    "discount_tax": "0.00",
    "shipping_total": "10.00",
    "shipping_tax": "0.00",
    "cart_tax": "1.35",
    "total": "29.35",
    "total_tax": "1.35",
    "prices_include_tax": false,
    "customer_id": 0,
    "customer_ip_address": "",
    "customer_user_agent": "",
    "customer_note": "",
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
    "payment_method": "bacs",
    "payment_method_title": "Direct Bank Transfer",
    "transaction_id": "",
    "date_paid": "2017-03-22T16:28:08",
    "date_paid_gmt": "2017-03-22T19:28:08",
    "date_completed": null,
    "date_completed_gmt": null,
    "cart_hash": "",
    "meta_data": [
      {
        "id": 13106,
        "key": "_download_permissions_granted",
        "value": "yes"
      },
      {
        "id": 13109,
        "key": "_order_stock_reduced",
        "value": "yes"
      }
    ],
    "line_items": [
      {
        "id": 315,
        "name": "Woo Single #1",
        "product_id": 93,
        "variation_id": 0,
        "quantity": 2,
        "tax_class": "",
        "subtotal": "6.00",
        "subtotal_tax": "0.45",
        "total": "6.00",
        "total_tax": "0.45",
        "taxes": [
          {
            "id": 75,
            "total": "0.45",
            "subtotal": "0.45"
          }
        ],
        "meta_data": [],
        "sku": "",
        "price": 3
      },
      {
        "id": 316,
        "name": "Ship Your Idea &ndash; Color: Black, Size: M Test",
        "product_id": 22,
        "variation_id": 23,
        "quantity": 1,
        "tax_class": "",
        "subtotal": "12.00",
        "subtotal_tax": "0.90",
        "total": "12.00",
        "total_tax": "0.90",
        "taxes": [
          {
            "id": 75,
            "total": "0.9",
            "subtotal": "0.9"
          }
        ],
        "meta_data": [
          {
            "id": 2095,
            "key": "pa_color",
            "value": "black"
          },
          {
            "id": 2096,
            "key": "size",
            "value": "M Test"
          }
        ],
        "sku": "Bar3",
        "price": 12
      }
    ],
    "tax_lines": [
      {
        "id": 318,
        "rate_code": "US-CA-STATE TAX",
        "rate_id": 75,
        "label": "State Tax",
        "compound": false,
        "tax_total": "1.35",
        "shipping_tax_total": "0.00",
        "meta_data": []
      }
    ],
    "shipping_lines": [
      {
        "id": 317,
        "method_title": "Flat Rate",
        "method_id": "flat_rate",
        "total": "10.00",
        "total_tax": "0.00",
        "taxes": [],
        "meta_data": []
      }
    ],
    "fee_lines": [],
    "coupon_lines": [],
    "refunds": [],
    "_links": {
      "self": [
        {
          "href": "https://example.com/wp-json/wc/v3/orders/727"
        }
      ],
      "collection": [
        {
          "href": "https://example.com/wp-json/wc/v3/orders"
        }
      ]
    }
  },
  {
    "id": 723,
    "parent_id": 0,
    "number": "723",
    "order_key": "wc_order_58d17c18352",
    "created_via": "checkout",
    "version": "3.0.0",
    "status": "completed",
    "currency": "USD",
    "date_created": "2017-03-21T16:16:00",
    "date_created_gmt": "2017-03-21T19:16:00",
    "date_modified": "2017-03-21T16:54:51",
    "date_modified_gmt": "2017-03-21T19:54:51",
    "discount_total": "0.00",
    "discount_tax": "0.00",
    "shipping_total": "10.00",
    "shipping_tax": "0.00",
    "cart_tax": "0.00",
    "total": "39.00",
    "total_tax": "0.00",
    "prices_include_tax": false,
    "customer_id": 26,
    "customer_ip_address": "127.0.0.1",
    "customer_user_agent": "mozilla/5.0 (x11; ubuntu; linux x86_64; rv:52.0) gecko/20100101 firefox/52.0",
    "customer_note": "",
    "billing": {
      "first_name": "João",
      "last_name": "Silva",
      "company": "",
      "address_1": "Av. Brasil, 432",
      "address_2": "",
      "city": "Rio de Janeiro",
      "state": "RJ",
      "postcode": "12345-000",
      "country": "BR",
      "email": "joao.silva@example.com",
      "phone": "(11) 1111-1111"
    },
    "shipping": {
      "first_name": "João",
      "last_name": "Silva",
      "company": "",
      "address_1": "Av. Brasil, 432",
      "address_2": "",
      "city": "Rio de Janeiro",
      "state": "RJ",
      "postcode": "12345-000",
      "country": "BR"
    },
    "payment_method": "bacs",
    "payment_method_title": "Direct bank transfer",
    "transaction_id": "",
    "date_paid": null,
    "date_paid_gmt": null,
    "date_completed": "2017-03-21T16:54:51",
    "date_completed_gmt": "2017-03-21T19:54:51",
    "cart_hash": "5040ce7273261e31d8bcf79f9be3d279",
    "meta_data": [
      {
        "id": 13023,
        "key": "_download_permissions_granted",
        "value": "yes"
      }
    ],
    "line_items": [
      {
        "id": 311,
        "name": "Woo Album #2",
        "product_id": 87,
        "variation_id": 0,
        "quantity": 1,
        "tax_class": "",
        "subtotal": "9.00",
        "subtotal_tax": "0.00",
        "total": "9.00",
        "total_tax": "0.00",
        "taxes": [],
        "meta_data": [],
        "sku": "",
        "price": 9
      },
      {
        "id": 313,
        "name": "Woo Ninja",
        "product_id": 34,
        "variation_id": 0,
        "quantity": 1,
        "tax_class": "",
        "subtotal": "20.00",
        "subtotal_tax": "0.00",
        "total": "20.00",
        "total_tax": "0.00",
        "taxes": [],
        "meta_data": [],
        "sku": "",
        "price": 20
      }
    ],
    "tax_lines": [],
    "shipping_lines": [
      {
        "id": 312,
        "method_title": "Flat rate",
        "method_id": "flat_rate:25",
        "total": "10.00",
        "total_tax": "0.00",
        "taxes": [],
        "meta_data": [
          {
            "id": 2057,
            "key": "Items",
            "value": "Woo Album #2 &times; 1"
          }
        ]
      }
    ],
    "fee_lines": [],
    "coupon_lines": [],
    "refunds": [
      {
        "id": 726,
        "refund": "",
        "total": "-10.00"
      },
      {
        "id": 724,
        "refund": "",
        "total": "-9.00"
      }
    ],
    "_links": {
      "self": [
        {
          "href": "https://example.com/wp-json/wc/v3/orders/723"
        }
      ],
      "collection": [
        {
          "href": "https://example.com/wp-json/wc/v3/orders"
        }
      ],
      "customer": [
        {
          "href": "https://example.com/wp-json/wc/v3/customers/26"
        }
      ]
    }
  }
]
```

### Update an Order

`PUT` `/wp-json/wc/v3/orders/<id>`

### Input Example

```
PUT https://app.com/wp-json/wc/v3/orders/727
```

```
curl -X PUT https://example.com/wp-json/wc/v3/orders/727 \
    -u consumer_key:consumer_secret \
    -H "Content-Type: application/json" \
    -d '{
  "status": "completed"
}'
```

### Output Example

```
{
  "id": 727,
  "parent_id": 0,
  "number": "727",
  "order_key": "wc_order_58d2d042d1d",
  "created_via": "rest-api",
  "version": "3.0.0",
  "status": "completed",
  "currency": "USD",
  "date_created": "2017-03-22T16:28:02",
  "date_created_gmt": "2017-03-22T19:28:02",
  "date_modified": "2017-03-22T16:30:35",
  "date_modified_gmt": "2017-03-22T19:30:35",
  "discount_total": "0.00",
  "discount_tax": "0.00",
  "shipping_total": "10.00",
  "shipping_tax": "0.00",
  "cart_tax": "1.35",
  "total": "29.35",
  "total_tax": "1.35",
  "prices_include_tax": false,
  "customer_id": 0,
  "customer_ip_address": "",
  "customer_user_agent": "",
  "customer_note": "",
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
  "payment_method": "bacs",
  "payment_method_title": "Direct Bank Transfer",
  "transaction_id": "",
  "date_paid": "2017-03-22T16:28:08",
  "date_paid_gmt": "2017-03-22T19:28:08",
  "date_completed": "2017-03-22T16:30:35",
  "date_completed_gmt": "2017-03-22T19:30:35",
  "cart_hash": "",
  "meta_data": [
    {
      "id": 13106,
      "key": "_download_permissions_granted",
      "value": "yes"
    },
    {
      "id": 13109,
      "key": "_order_stock_reduced",
      "value": "yes"
    }
  ],
  "line_items": [
    {
      "id": 315,
      "name": "Woo Single #1",
      "product_id": 93,
      "variation_id": 0,
      "quantity": 2,
      "tax_class": "",
      "subtotal": "6.00",
      "subtotal_tax": "0.45",
      "total": "6.00",
      "total_tax": "0.45",
      "taxes": [
        {
          "id": 75,
          "total": "0.45",
          "subtotal": "0.45"
        }
      ],
      "meta_data": [],
      "sku": "",
      "price": 3
    },
    {
      "id": 316,
      "name": "Ship Your Idea &ndash; Color: Black, Size: M Test",
      "product_id": 22,
      "variation_id": 23,
      "quantity": 1,
      "tax_class": "",
      "subtotal": "12.00",
      "subtotal_tax": "0.90",
      "total": "12.00",
      "total_tax": "0.90",
      "taxes": [
        {
          "id": 75,
          "total": "0.9",
          "subtotal": "0.9"
        }
      ],
      "meta_data": [
        {
          "id": 2095,
          "key": "pa_color",
          "value": "black"
        },
        {
          "id": 2096,
          "key": "size",
          "value": "M Test"
        }
      ],
      "sku": "Bar3",
      "price": 12
    }
  ],
  "tax_lines": [
    {
      "id": 318,
      "rate_code": "US-CA-STATE TAX",
      "rate_id": 75,
      "label": "State Tax",
      "compound": false,
      "tax_total": "1.35",
      "shipping_tax_total": "0.00",
      "meta_data": []
    }
  ],
  "shipping_lines": [
    {
      "id": 317,
      "method_title": "Flat Rate",
      "method_id": "flat_rate",
      "total": "10.00",
      "total_tax": "0.00",
      "taxes": [],
      "meta_data": []
    }
  ],
  "fee_lines": [],
  "coupon_lines": [],
  "refunds": [],
  "_links": {
    "self": [
      {
        "href": "https://example.com/wp-json/wc/v3/orders/727"
      }
    ],
    "collection": [
      {
        "href": "https://example.com/wp-json/wc/v3/orders"
      }
    ]
  }
}
```

### Delete an order

`DELETE` `/wp-json/wc/v3/orders/<id>`

### Input Example

```
DELETE https://app.com/wp-json/wc/v3/orders/727
```

### Output Example

```
{
  "id": 727,
  "parent_id": 0,
  "number": "727",
  "order_key": "wc_order_58d2d042d1d",
  "created_via": "rest-api",
  "version": "3.0.0",
  "status": "completed",
  "currency": "USD",
  "date_created": "2017-03-22T16:28:02",
  "date_created_gmt": "2017-03-22T19:28:02",
  "date_modified": "2017-03-22T16:30:35",
  "date_modified_gmt": "2017-03-22T19:30:35",
  "discount_total": "0.00",
  "discount_tax": "0.00",
  "shipping_total": "10.00",
  "shipping_tax": "0.00",
  "cart_tax": "1.35",
  "total": "29.35",
  "total_tax": "1.35",
  "prices_include_tax": false,
  "customer_id": 0,
  "customer_ip_address": "",
  "customer_user_agent": "",
  "customer_note": "",
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
  "payment_method": "bacs",
  "payment_method_title": "Direct Bank Transfer",
  "transaction_id": "",
  "date_paid": "2017-03-22T16:28:08",
  "date_paid_gmt": "2017-03-22T19:28:08",
  "date_completed": "2017-03-22T16:30:35",
  "date_completed_gmt": "2017-03-22T19:30:35",
  "cart_hash": "",
  "meta_data": [
    {
      "id": 13106,
      "key": "_download_permissions_granted",
      "value": "yes"
    },
    {
      "id": 13109,
      "key": "_order_stock_reduced",
      "value": "yes"
    }
  ],
  "line_items": [
    {
      "id": 315,
      "name": "Woo Single #1",
      "product_id": 93,
      "variation_id": 0,
      "quantity": 2,
      "tax_class": "",
      "subtotal": "6.00",
      "subtotal_tax": "0.45",
      "total": "6.00",
      "total_tax": "0.45",
      "taxes": [
        {
          "id": 75,
          "total": "0.45",
          "subtotal": "0.45"
        }
      ],
      "meta_data": [],
      "sku": "",
      "price": 3
    },
    {
      "id": 316,
      "name": "Ship Your Idea &ndash; Color: Black, Size: M Test",
      "product_id": 22,
      "variation_id": 23,
      "quantity": 1,
      "tax_class": "",
      "subtotal": "12.00",
      "subtotal_tax": "0.90",
      "total": "12.00",
      "total_tax": "0.90",
      "taxes": [
        {
          "id": 75,
          "total": "0.9",
          "subtotal": "0.9"
        }
      ],
      "meta_data": [
        {
          "id": 2095,
          "key": "pa_color",
          "value": "black"
        },
        {
          "id": 2096,
          "key": "size",
          "value": "M Test"
        }
      ],
      "sku": "Bar3",
      "price": 12
    }
  ],
  "tax_lines": [
    {
      "id": 318,
      "rate_code": "US-CA-STATE TAX",
      "rate_id": 75,
      "label": "State Tax",
      "compound": false,
      "tax_total": "1.35",
      "shipping_tax_total": "0.00",
      "meta_data": []
    }
  ],
  "shipping_lines": [
    {
      "id": 317,
      "method_title": "Flat Rate",
      "method_id": "flat_rate",
      "total": "10.00",
      "total_tax": "0.00",
      "taxes": [],
      "meta_data": []
    }
  ],
  "fee_lines": [],
  "coupon_lines": [],
  "refunds": [],
  "_links": {
    "self": [
      {
        "href": "https://example.com/wp-json/wc/v3/orders/727"
      }
    ],
    "collection": [
      {
        "href": "https://example.com/wp-json/wc/v3/orders"
      }
    ]
  }
}
```

## 6. Products

- Products properties see: https://woocommerce.github.io/woocommerce-rest-api-docs/?shell#product-properties
  
### Create a product

`POST` `/wp-json/wc/v3/products`

### Input Example

```
POST https://app.com/wp-json/wc/v3/products
```
  
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

### Output Example

```
{
  "id": 794,
  "name": "Premium Quality",
  "slug": "premium-quality-19",
  "permalink": "https://example.com/product/premium-quality-19/",
  "date_created": "2017-03-23T17:01:14",
  "date_created_gmt": "2017-03-23T20:01:14",
  "date_modified": "2017-03-23T17:01:14",
  "date_modified_gmt": "2017-03-23T20:01:14",
  "type": "simple",
  "status": "publish",
  "featured": false,
  "catalog_visibility": "visible",
  "description": "<p>Pellentesque habitant morbi tristique senectus et netus et malesuada fames ac turpis egestas. Vestibulum tortor quam, feugiat vitae, ultricies eget, tempor sit amet, ante. Donec eu libero sit amet quam egestas semper. Aenean ultricies mi vitae est. Mauris placerat eleifend leo.</p>\n",
  "short_description": "<p>Pellentesque habitant morbi tristique senectus et netus et malesuada fames ac turpis egestas.</p>\n",
  "sku": "",
  "price": "21.99",
  "regular_price": "21.99",
  "sale_price": "",
  "date_on_sale_from": null,
  "date_on_sale_from_gmt": null,
  "date_on_sale_to": null,
  "date_on_sale_to_gmt": null,
  "price_html": "<span class=\"woocommerce-Price-amount amount\"><span class=\"woocommerce-Price-currencySymbol\">&#36;</span>21.99</span>",
  "on_sale": false,
  "purchasable": true,
  "total_sales": 0,
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
  "stock_status": "instock",
  "backorders": "no",
  "backorders_allowed": false,
  "backordered": false,
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
  "related_ids": [
    53,
    40,
    56,
    479,
    99
  ],
  "upsell_ids": [],
  "cross_sell_ids": [],
  "parent_id": 0,
  "purchase_note": "",
  "categories": [
    {
      "id": 9,
      "name": "Clothing",
      "slug": "clothing"
    },
    {
      "id": 14,
      "name": "T-shirts",
      "slug": "t-shirts"
    }
  ],
  "tags": [],
  "images": [
    {
      "id": 792,
      "date_created": "2017-03-23T14:01:13",
      "date_created_gmt": "2017-03-23T20:01:13",
      "date_modified": "2017-03-23T14:01:13",
      "date_modified_gmt": "2017-03-23T20:01:13",
      "src": "https://example.com/wp-content/uploads/2017/03/T_2_front-4.jpg",
      "name": "",
      "alt": ""
    },
    {
      "id": 793,
      "date_created": "2017-03-23T14:01:14",
      "date_created_gmt": "2017-03-23T20:01:14",
      "date_modified": "2017-03-23T14:01:14",
      "date_modified_gmt": "2017-03-23T20:01:14",
      "src": "https://example.com/wp-content/uploads/2017/03/T_2_back-2.jpg",
      "name": "",
      "alt": ""
    }
  ],
  "attributes": [],
  "default_attributes": [],
  "variations": [],
  "grouped_products": [],
  "menu_order": 0,
  "meta_data": [],
  "_links": {
    "self": [
      {
        "href": "https://example.com/wp-json/wc/v3/products/794"
      }
    ],
    "collection": [
      {
        "href": "https://example.com/wp-json/wc/v3/products"
      }
    ]
  }
}
```

### Retrieve a product

`GET` `/wp-json/wc/v3/products/<id>`

### Input Example

```
GET https://app.com/wp-json/wc/v3/products/794
```

### Output Example

```
{
  "id": 794,
  "name": "Premium Quality",
  "slug": "premium-quality-19",
  "permalink": "https://example.com/product/premium-quality-19/",
  "date_created": "2017-03-23T17:01:14",
  "date_created_gmt": "2017-03-23T20:01:14",
  "date_modified": "2017-03-23T17:01:14",
  "date_modified_gmt": "2017-03-23T20:01:14",
  "type": "simple",
  "status": "publish",
  "featured": false,
  "catalog_visibility": "visible",
  "description": "<p>Pellentesque habitant morbi tristique senectus et netus et malesuada fames ac turpis egestas. Vestibulum tortor quam, feugiat vitae, ultricies eget, tempor sit amet, ante. Donec eu libero sit amet quam egestas semper. Aenean ultricies mi vitae est. Mauris placerat eleifend leo.</p>\n",
  "short_description": "<p>Pellentesque habitant morbi tristique senectus et netus et malesuada fames ac turpis egestas.</p>\n",
  "sku": "",
  "price": "21.99",
  "regular_price": "21.99",
  "sale_price": "",
  "date_on_sale_from": null,
  "date_on_sale_from_gmt": null,
  "date_on_sale_to": null,
  "date_on_sale_to_gmt": null,
  "price_html": "<span class=\"woocommerce-Price-amount amount\"><span class=\"woocommerce-Price-currencySymbol\">&#36;</span>21.99</span>",
  "on_sale": false,
  "purchasable": true,
  "total_sales": 0,
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
  "stock_status": "instock",
  "backorders": "no",
  "backorders_allowed": false,
  "backordered": false,
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
  "related_ids": [
    53,
    40,
    56,
    479,
    99
  ],
  "upsell_ids": [],
  "cross_sell_ids": [],
  "parent_id": 0,
  "purchase_note": "",
  "categories": [
    {
      "id": 9,
      "name": "Clothing",
      "slug": "clothing"
    },
    {
      "id": 14,
      "name": "T-shirts",
      "slug": "t-shirts"
    }
  ],
  "tags": [],
  "images": [
    {
      "id": 792,
      "date_created": "2017-03-23T14:01:13",
      "date_created_gmt": "2017-03-23T20:01:13",
      "date_modified": "2017-03-23T14:01:13",
      "date_modified_gmt": "2017-03-23T20:01:13",
      "src": "https://example.com/wp-content/uploads/2017/03/T_2_front-4.jpg",
      "name": "",
      "alt": ""
    },
    {
      "id": 793,
      "date_created": "2017-03-23T14:01:14",
      "date_created_gmt": "2017-03-23T20:01:14",
      "date_modified": "2017-03-23T14:01:14",
      "date_modified_gmt": "2017-03-23T20:01:14",
      "src": "https://example.com/wp-content/uploads/2017/03/T_2_back-2.jpg",
      "name": "",
      "alt": ""
    }
  ],
  "attributes": [],
  "default_attributes": [],
  "variations": [],
  "grouped_products": [],
  "menu_order": 0,
  "meta_data": [],
  "_links": {
    "self": [
      {
        "href": "https://example.com/wp-json/wc/v3/products/794"
      }
    ],
    "collection": [
      {
        "href": "https://example.com/wp-json/wc/v3/products"
      }
    ]
  }
}
```

### List all products

`GET` `/wp-json/wc/v3/products`

### Input Example

```
GET https://app.com/wp-json/wc/v3/products
```

### Output Example

```
[
  {
    "id": 799,
    "name": "Ship Your Idea",
    "slug": "ship-your-idea-22",
    "permalink": "https://example.com/product/ship-your-idea-22/",
    "date_created": "2017-03-23T17:03:12",
    "date_created_gmt": "2017-03-23T20:03:12",
    "date_modified": "2017-03-23T17:03:12",
    "date_modified_gmt": "2017-03-23T20:03:12",
    "type": "variable",
    "status": "publish",
    "featured": false,
    "catalog_visibility": "visible",
    "description": "<p>Pellentesque habitant morbi tristique senectus et netus et malesuada fames ac turpis egestas. Vestibulum tortor quam, feugiat vitae, ultricies eget, tempor sit amet, ante. Donec eu libero sit amet quam egestas semper. Aenean ultricies mi vitae est. Mauris placerat eleifend leo.</p>\n",
    "short_description": "<p>Pellentesque habitant morbi tristique senectus et netus et malesuada fames ac turpis egestas.</p>\n",
    "sku": "",
    "price": "",
    "regular_price": "",
    "sale_price": "",
    "date_on_sale_from": null,
    "date_on_sale_from_gmt": null,
    "date_on_sale_to": null,
    "date_on_sale_to_gmt": null,
    "price_html": "",
    "on_sale": false,
    "purchasable": false,
    "total_sales": 0,
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
    "stock_status": "instock",
    "backorders": "no",
    "backorders_allowed": false,
    "backordered": false,
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
    "related_ids": [
      31,
      22,
      369,
      414,
      56
    ],
    "upsell_ids": [],
    "cross_sell_ids": [],
    "parent_id": 0,
    "purchase_note": "",
    "categories": [
      {
        "id": 9,
        "name": "Clothing",
        "slug": "clothing"
      },
      {
        "id": 14,
        "name": "T-shirts",
        "slug": "t-shirts"
      }
    ],
    "tags": [],
    "images": [
      {
        "id": 795,
        "date_created": "2017-03-23T14:03:08",
        "date_created_gmt": "2017-03-23T20:03:08",
        "date_modified": "2017-03-23T14:03:08",
        "date_modified_gmt": "2017-03-23T20:03:08",
        "src": "https://example.com/wp-content/uploads/2017/03/T_4_front-11.jpg",
        "name": "",
        "alt": ""
      },
      {
        "id": 796,
        "date_created": "2017-03-23T14:03:09",
        "date_created_gmt": "2017-03-23T20:03:09",
        "date_modified": "2017-03-23T14:03:09",
        "date_modified_gmt": "2017-03-23T20:03:09",
        "src": "https://example.com/wp-content/uploads/2017/03/T_4_back-10.jpg",
        "name": "",
        "alt": ""
      },
      {
        "id": 797,
        "date_created": "2017-03-23T14:03:10",
        "date_created_gmt": "2017-03-23T20:03:10",
        "date_modified": "2017-03-23T14:03:10",
        "date_modified_gmt": "2017-03-23T20:03:10",
        "src": "https://example.com/wp-content/uploads/2017/03/T_3_front-10.jpg",
        "name": "",
        "alt": ""
      },
      {
        "id": 798,
        "date_created": "2017-03-23T14:03:11",
        "date_created_gmt": "2017-03-23T20:03:11",
        "date_modified": "2017-03-23T14:03:11",
        "date_modified_gmt": "2017-03-23T20:03:11",
        "src": "https://example.com/wp-content/uploads/2017/03/T_3_back-10.jpg",
        "name": "",
        "alt": ""
      }
    ],
    "attributes": [
      {
        "id": 6,
        "name": "Color",
        "position": 0,
        "visible": false,
        "variation": true,
        "options": [
          "Black",
          "Green"
        ]
      },
      {
        "id": 0,
        "name": "Size",
        "position": 0,
        "visible": true,
        "variation": true,
        "options": [
          "S",
          "M"
        ]
      }
    ],
    "default_attributes": [],
    "variations": [],
    "grouped_products": [],
    "menu_order": 0,
    "meta_data": [],
    "_links": {
      "self": [
        {
          "href": "https://example.com/wp-json/wc/v3/products/799"
        }
      ],
      "collection": [
        {
          "href": "https://example.com/wp-json/wc/v3/products"
        }
      ]
    }
  },
  {
    "id": 794,
    "name": "Premium Quality",
    "slug": "premium-quality-19",
    "permalink": "https://example.com/product/premium-quality-19/",
    "date_created": "2017-03-23T17:01:14",
    "date_created_gmt": "2017-03-23T20:01:14",
    "date_modified": "2017-03-23T17:01:14",
    "date_modified_gmt": "2017-03-23T20:01:14",
    "type": "simple",
    "status": "publish",
    "featured": false,
    "catalog_visibility": "visible",
    "description": "<p>Pellentesque habitant morbi tristique senectus et netus et malesuada fames ac turpis egestas. Vestibulum tortor quam, feugiat vitae, ultricies eget, tempor sit amet, ante. Donec eu libero sit amet quam egestas semper. Aenean ultricies mi vitae est. Mauris placerat eleifend leo.</p>\n",
    "short_description": "<p>Pellentesque habitant morbi tristique senectus et netus et malesuada fames ac turpis egestas.</p>\n",
    "sku": "",
    "price": "21.99",
    "regular_price": "21.99",
    "sale_price": "",
    "date_on_sale_from": null,
    "date_on_sale_from_gmt": null,
    "date_on_sale_to": null,
    "date_on_sale_to_gmt": null,
    "price_html": "<span class=\"woocommerce-Price-amount amount\"><span class=\"woocommerce-Price-currencySymbol\">&#36;</span>21.99</span>",
    "on_sale": false,
    "purchasable": true,
    "total_sales": 0,
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
    "stock_status": "instock",
    "backorders": "no",
    "backorders_allowed": false,
    "backordered": false,
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
    "related_ids": [
      463,
      47,
      31,
      387,
      458
    ],
    "upsell_ids": [],
    "cross_sell_ids": [],
    "parent_id": 0,
    "purchase_note": "",
    "categories": [
      {
        "id": 9,
        "name": "Clothing",
        "slug": "clothing"
      },
      {
        "id": 14,
        "name": "T-shirts",
        "slug": "t-shirts"
      }
    ],
    "tags": [],
    "images": [
      {
        "id": 792,
        "date_created": "2017-03-23T14:01:13",
        "date_created_gmt": "2017-03-23T20:01:13",
        "date_modified": "2017-03-23T14:01:13",
        "date_modified_gmt": "2017-03-23T20:01:13",
        "src": "https://example.com/wp-content/uploads/2017/03/T_2_front-4.jpg",
        "name": "",
        "alt": ""
      },
      {
        "id": 793,
        "date_created": "2017-03-23T14:01:14",
        "date_created_gmt": "2017-03-23T20:01:14",
        "date_modified": "2017-03-23T14:01:14",
        "date_modified_gmt": "2017-03-23T20:01:14",
        "src": "https://example.com/wp-content/uploads/2017/03/T_2_back-2.jpg",
        "name": "",
        "alt": ""
      }
    ],
    "attributes": [],
    "default_attributes": [
      {
        "id": 6,
        "name": "Color",
        "option": "black"
      },
      {
        "id": 0,
        "name": "Size",
        "option": "S"
      }
    ],
    "variations": [],
    "grouped_products": [],
    "menu_order": 0,
    "meta_data": [],
    "_links": {
      "self": [
        {
          "href": "https://example.com/wp-json/wc/v3/products/794"
        }
      ],
      "collection": [
        {
          "href": "https://example.com/wp-json/wc/v3/products"
        }
      ]
    }
  }
]
```

### Update a product

`PUT` `/wp-json/wc/v3/products/<id>`

### Input Example

```
PUT https://app.com/wp-json/wc/v3/products
```

```
curl -X PUT https://example.com/wp-json/wc/v3/products/794 \
    -u consumer_key:consumer_secret \
    -H "Content-Type: application/json" \
    -d '{
  "regular_price": "24.54"
}'
```

### Output Example

```
{
  "id": 794,
  "name": "Premium Quality",
  "slug": "premium-quality-19",
  "permalink": "https://example.com/product/premium-quality-19/",
  "date_created": "2017-03-23T17:01:14",
  "date_created_gmt": "2017-03-23T20:01:14",
  "date_modified": "2017-03-23T17:01:14",
  "date_modified_gmt": "2017-03-23T20:01:14",
  "type": "simple",
  "status": "publish",
  "featured": false,
  "catalog_visibility": "visible",
  "description": "<p>Pellentesque habitant morbi tristique senectus et netus et malesuada fames ac turpis egestas. Vestibulum tortor quam, feugiat vitae, ultricies eget, tempor sit amet, ante. Donec eu libero sit amet quam egestas semper. Aenean ultricies mi vitae est. Mauris placerat eleifend leo.</p>\n",
  "short_description": "<p>Pellentesque habitant morbi tristique senectus et netus et malesuada fames ac turpis egestas.</p>\n",
  "sku": "",
  "price": "24.54",
  "regular_price": "24.54",
  "sale_price": "",
  "date_on_sale_from": null,
  "date_on_sale_from_gmt": null,
  "date_on_sale_to": null,
  "date_on_sale_to_gmt": null,
  "price_html": "<span class=\"woocommerce-Price-amount amount\"><span class=\"woocommerce-Price-currencySymbol\">&#36;</span>24.54</span>",
  "on_sale": false,
  "purchasable": true,
  "total_sales": 0,
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
  "stock_status": "instock",
  "backorders": "no",
  "backorders_allowed": false,
  "backordered": false,
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
  "related_ids": [
    479,
    387,
    22,
    463,
    396
  ],
  "upsell_ids": [],
  "cross_sell_ids": [],
  "parent_id": 0,
  "purchase_note": "",
  "categories": [
    {
      "id": 9,
      "name": "Clothing",
      "slug": "clothing"
    },
    {
      "id": 14,
      "name": "T-shirts",
      "slug": "t-shirts"
    }
  ],
  "tags": [],
  "images": [
    {
      "id": 792,
      "date_created": "2017-03-23T14:01:13",
      "date_created_gmt": "2017-03-23T20:01:13",
      "date_modified": "2017-03-23T14:01:13",
      "date_modified_gmt": "2017-03-23T20:01:13",
      "src": "https://example.com/wp-content/uploads/2017/03/T_2_front-4.jpg",
      "name": "",
      "alt": ""
    },
    {
      "id": 793,
      "date_created": "2017-03-23T14:01:14",
      "date_created_gmt": "2017-03-23T20:01:14",
      "date_modified": "2017-03-23T14:01:14",
      "date_modified_gmt": "2017-03-23T20:01:14",
      "src": "https://example.com/wp-content/uploads/2017/03/T_2_back-2.jpg",
      "name": "",
      "alt": ""
    }
  ],
  "attributes": [],
  "default_attributes": [],
  "variations": [],
  "grouped_products": [],
  "menu_order": 0,
  "meta_data": [],
  "_links": {
    "self": [
      {
        "href": "https://example.com/wp-json/wc/v3/products/794"
      }
    ],
    "collection": [
      {
        "href": "https://example.com/wp-json/wc/v3/products"
      }
    ]
  }
}
```

### Delete a product

`DELETE` `/wp-json/wc/v3/products/<id>`

### Input Example

```
DELETE https://app.com/wp-json/wc/v3/products/794
```

```
curl -X DELETE https://example.com/wp-json/wc/v3/products/794?force=true \
    -u consumer_key:consumer_secret
```

### Output Example

```
{
  "id": 794,
  "name": "Premium Quality",
  "slug": "premium-quality-19",
  "permalink": "https://example.com/product/premium-quality-19/",
  "date_created": "2017-03-23T17:01:14",
  "date_created_gmt": "2017-03-23T20:01:14",
  "date_modified": "2017-03-23T17:01:14",
  "date_modified_gmt": "2017-03-23T20:01:14",
  "type": "simple",
  "status": "publish",
  "featured": false,
  "catalog_visibility": "visible",
  "description": "<p>Pellentesque habitant morbi tristique senectus et netus et malesuada fames ac turpis egestas. Vestibulum tortor quam, feugiat vitae, ultricies eget, tempor sit amet, ante. Donec eu libero sit amet quam egestas semper. Aenean ultricies mi vitae est. Mauris placerat eleifend leo.</p>\n",
  "short_description": "<p>Pellentesque habitant morbi tristique senectus et netus et malesuada fames ac turpis egestas.</p>\n",
  "sku": "",
  "price": "24.54",
  "regular_price": "24.54",
  "sale_price": "",
  "date_on_sale_from": null,
  "date_on_sale_from_gmt": null,
  "date_on_sale_to": null,
  "date_on_sale_to_gmt": null,
  "price_html": "<span class=\"woocommerce-Price-amount amount\"><span class=\"woocommerce-Price-currencySymbol\">&#36;</span>24.54</span>",
  "on_sale": false,
  "purchasable": true,
  "total_sales": 0,
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
  "stock_status": "instock",
  "backorders": "no",
  "backorders_allowed": false,
  "backordered": false,
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
  "related_ids": [
    479,
    387,
    22,
    463,
    396
  ],
  "upsell_ids": [],
  "cross_sell_ids": [],
  "parent_id": 0,
  "purchase_note": "",
  "categories": [
    {
      "id": 9,
      "name": "Clothing",
      "slug": "clothing"
    },
    {
      "id": 14,
      "name": "T-shirts",
      "slug": "t-shirts"
    }
  ],
  "tags": [],
  "images": [
    {
      "id": 792,
      "date_created": "2017-03-23T14:01:13",
      "date_created_gmt": "2017-03-23T20:01:13",
      "date_modified": "2017-03-23T14:01:13",
      "date_modified_gmt": "2017-03-23T20:01:13",
      "src": "https://example.com/wp-content/uploads/2017/03/T_2_front-4.jpg",
      "name": "",
      "alt": ""
    },
    {
      "id": 793,
      "date_created": "2017-03-23T14:01:14",
      "date_created_gmt": "2017-03-23T20:01:14",
      "date_modified": "2017-03-23T14:01:14",
      "date_modified_gmt": "2017-03-23T20:01:14",
      "src": "https://example.com/wp-content/uploads/2017/03/T_2_back-2.jpg",
      "name": "",
      "alt": ""
    }
  ],
  "attributes": [],
  "default_attributes": [],
  "variations": [],
  "grouped_products": [],
  "menu_order": 0,
  "meta_data": [],
  "_links": {
    "self": [
      {
        "href": "https://example.com/wp-json/wc/v3/products/794"
      }
    ],
    "collection": [
      {
        "href": "https://example.com/wp-json/wc/v3/products"
      }
    ]
  }
}
```
