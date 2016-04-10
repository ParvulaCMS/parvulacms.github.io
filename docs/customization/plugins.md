---
layout: doc
title: Plugins
section: customization
---

# Plugin structure

All plugins must be located in the `plugins` folder and must have a php file and class named as the plugin name.

Structure of a basic plugin:

```_
└── plugins
    └── MyPlugin
        └── MyPlugin.php
```

Code of a basic plugin:

```php
<?php
namespace Plugin\MyPlugin;

use Parvula\Plugin;

class MyPlugin extends Plugin {
	// ...
}

```

# The plugin class

Each plugin should inherit the Plugin class (`Parvula\Plugin`).

The Plugin class has some useful method to help you to develop a new plugin easily.

{:.table .table-striped}
| Function                      | Description |
| ----------------------------- | ----------- |
| `getPath()`                   | Get the current plugin path (useful for the backend/php part) |
| `getUri()`                    | Get the current plugin URI (useful for the client/view part) |
| `appendToHeader($html, $add)` | Append string to the header element (`<head>`) |
| `appendToBody($html, $add)`   | Append string to the header element (`<body>`) |

## The plugin callbacks

Each plugin also have a "cycle of live". Each time, those callbacks functions will be called.

{:.table .table-striped}
| Callback function               | Description |
| ---------------------           | ----------- |
| `onBootstrap(Parvula $app)`     | The first function to be called, useful to get services in $app.
| `onLoad()`                      | On load |
| `onRouter(Router $router)`      | To handle the router. Useful to add or override routes |
| `onDispatch(string $method, string $uri)` | |
| `onUri(string $uri)`            | ex: `/pictures/islands?test` //TODO check |
| `onSlug(string $slug)`          | ex: `pictures/islands` |
| `onPage(Page $page)`            | When the page mapped to the slug is loaded |
| `onPreRender(string $layout)`   | Before the layout is rendered |
| `onPostRender(string $output)`  | After the layout is rendered |
| `on404(Page $page)`             | When the page is not found |
| `onEnd()`                       | The last callback called |

## Hello world example

If we want to add a route (`/hello`):

```php
<?php
namespace Plugin\MyPlugin;

use Parvula\Plugin;

class MyPlugin extends Plugin
{
	/**
	 * Add a new route
	 * It will handle '/hello' route and print "Hello world !"
	 */
	function onRouter(&$router) {
		$router->get('/hello', function () {
			return 'Hello world !';
		});
	}
}
```

---

If we want to change the content of the page:

```php
<?php
namespace Plugin\MyPlugin2;

use Parvula\Plugin;

class MyPlugin2 extends Plugin {
	function onPage(&$page) {
		$page->content = 'Hello world !';
	}
}
```
