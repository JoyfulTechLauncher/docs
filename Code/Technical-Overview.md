# Technical Overview



## [lib/common](https://github.com/JoyfulTechLauncher/joyfulfashionista_app_beta/blob/59c0bd77d384e0c241ac6623cf8e926ca9f9774b/lib/common)

### [/api](https://github.com/JoyfulTechLauncher/joyfulfashionista_app_beta/blob/59c0bd77d384e0c241ac6623cf8e926ca9f9774b/lib/common/api)

- APIs that contain multiple web requests, such as user registration and login, user address lookup, order lookup, coupon lookup, catalog and categories, etc.
- Its main functionality is realized relying on the [woocommerce](https://woocommerce.github.io/woocommerce-rest-api-docs/v3.html?shell#) API.

### [/components](https://github.com/JoyfulTechLauncher/joyfulfashionista_app_beta/blob/59c0bd77d384e0c241ac6623cf8e926ca9f9774b/lib/common/components)

- Here is the front-end content which defines some components of some pages.

### [/extension](https://github.com/JoyfulTechLauncher/joyfulfashionista_app_beta/blob/59c0bd77d384e0c241ac6623cf8e926ca9f9774b/lib/common/extension)

- Stores definitions of extensions such as colors, datetime, icons, lists, etc.

### [/i18n](https://github.com/JoyfulTechLauncher/joyfulfashionista_app_beta/blob/59c0bd77d384e0c241ac6623cf8e926ca9f9774b/lib/common/i18n)

- This section defines about multilingual support for Flutter applications.

### [/models](https://github.com/JoyfulTechLauncher/joyfulfashionista_app_beta/blob/59c0bd77d384e0c241ac6623cf8e926ca9f9774b/lib/common/models)

- This part builds a number of request models, such as user registration model, user login model, order query request model and so on. It is mainly used to encapsulate the parameters required when querying orders. Through this model, it is easier to organize and deliver network requests, making the code more tidy and easy to manage.
- This section also models the parsing of much of the returned information. It will pattern match the information returned by the API to extract the key information we need and use it for display.

### [/routers](https://github.com/JoyfulTechLauncher/joyfulfashionista_app_beta/blob/59c0bd77d384e0c241ac6623cf8e926ca9f9774b/lib/common/routers)

- Primarily used to define and manage navigation paths between pages or views in an application.
- Routing tables provide a structured way to organize and manage pages and views in an application, making the code more modular and easier to maintain.

### [/services](https://github.com/JoyfulTechLauncher/joyfulfashionista_app_beta/blob/59c0bd77d384e0c241ac6623cf8e926ca9f9774b/lib/common/services)

- Includes some basic settings for the software, such as initial language, initial theme, etc.
- Contains a shopping cart service that defines some basic shopping cart operations. For example, adding items to the cart, removing certain items from the cart, modifying the number of items in the cart, and so on.

### [/style](https://github.com/JoyfulTechLauncher/joyfulfashionista_app_beta/blob/59c0bd77d384e0c241ac6623cf8e926ca9f9774b/lib/common/style)

- This is where the theme, color, text, etc. of the software is defined.

### [/utils](https://github.com/JoyfulTechLauncher/joyfulfashionista_app_beta/blob/59c0bd77d384e0c241ac6623cf8e926ca9f9774b/lib/common/utils)

- Contains code of a tool nature to support the implementation of the main functionality or to provide some commonly used auxiliary functions. These tools cover a wide range of functionality from UI interactions (such as bottom popups and loading prompts), data conversion, encryption, image selection, permissions management, local storage, to input validation. Overall, the `utils` catalog provides a comprehensive toolset for applications, making common tasks and operations during development easier and more efficient.

### [/values](https://github.com/JoyfulTechLauncher/joyfulfashionista_app_beta/blob/59c0bd77d384e0c241ac6623cf8e926ca9f9774b/lib/common/values)

- Stores some keys as well as product category names and defines the location of some icon images.

### [/widgets](https://github.com/JoyfulTechLauncher/joyfulfashionista_app_beta/blob/59c0bd77d384e0c241ac6623cf8e926ca9f9774b/lib/common/widgets)

- The directory contains a series of files related to interface components. These files mainly define a variety of commonly used interface elements, such as buttons, checkboxes, dialog boxes, icons, and so on. With these predefined components, developers can more easily build and customize the application's user interface, ensuring a consistent and efficient interface. These components cover common interface requirements in application development, ranging from basic text and image display to complex input and navigation features.

  