# PHP Liquid

Liquid is a PHP port of the [Liquid template engine for Ruby](https://github.com/Shopify/liquid), which was written by Tobias Lutke. Although there are many other templating engines for PHP, including Smarty (from which Liquid was partially inspired), Liquid had some advantages that made porting worthwhile:

 * Readable and human friendly syntax, that is usable in any type of document, not just html, without need for escaping.
 * Quick and easy to use and maintain.
 * 100% secure, no possibility of embedding PHP code.
 * Clean OO design, rather than the mix of OO and procedural found in other templating engines.
 * Seperate compiling and rendering stages for improved performance.
 * Unit tested: Liquid is fully unit-tested. The library is stable and ready to be used in large projects.

Liquid was written to meet three templating library requirements: good performance, easy to extend, and simply to use.

## Basic Use

The main class is `Liquid::Template` class. There are two separate stages of working with Liquid templates: parsing and rendering. Here is a simple example:

```php
use Liquid\Template;

$template = new Template();
$template->parse("Hello, {{ name }}!");
echo $template->render(array('name' => 'World');

// Will echo
// Hello, World!
```

You can also pass a file name to the template, to render the contents of a file instead of passing in a string.

```php
$path = 'molecule/accessability';
$liquid->parseFile($liquidPath);
return $liquid->render($props);

```

## Truthy and Falsy

In programming, anything that returns `true` in a conditional is called truthy. Anything that returns `false` in a conditional is called falsy. All object types can be described as either truthy or falsy.

### Truthy

All values in Liquid are truthy except `nil` and `false`.

In the example below, the string “Tobi” is not a boolean, but it is truthy in a conditional:

```
<% assign tobi = "Tobi" %}

<% if tobi %}
  This condition will always be true.
<% endif %}
```

Strings, even when empty, are truthy. The example below will result in empty HTML tags if `settings.fp_heading` is empty:

Input:
```
<% if settings.fp_heading %>
  <h1><%= settings.fp_heading %></h1>
<% endif %>
```

Output:
```
<h1></h1>
```

### Falsy

The falsy values in Liquid are `nil` and `false`.

## Variations of Liquid

Liquid is a flexible, safe language, and is used in many different environments. Liquid was created for use in [Shopify](https://www.shopify.com) stores, and is also used extensively on [Jekyll](https://jekyllrb.com) websites. Over time, both Shopify and Jekyll have added their own objects, tags, and filters to Liquid. The most popular versions of Liquid that exist are **Liquid**, **Shopify Liquid**, and **Jekyll Liquid**.

### Visualsoft

This documentation describes the version of liquid in use at Visualsoft. This version of liquid is modified significantly for use in VS3.

The biggest change for the the Visualsoft version is the tags. Normal liquid uses `{%`, `%}`, `{{` and `}}`. This version instead uses `<%`, `%>` for tags, `<%=` and `%>` for printing variables.

### Shopify

Shopify always uses the latest version of Liquid as a base, but Shopify adds a significant number of objects, tags, and filters to Liquid for use in merchants’ stores. These include objects representing store, product, and customer information, and filters for displaying store data and manipulating storefront assets like product images.

Shopify’s version of Liquid is documented in the [Shopify Help Center](https://help.shopify.com/themes/liquid). If you want to try out Shopify’s version of Liquid, you can create a development store through the [Shopify Partner Dashboard](https://help.shopify.com/en/partners/dashboard/development-stores).

### Jekyll

[Jekyll](https://jekyllrb.com) is a static site generator, a command-line tool that creates websites by merging templates with content files. Jekyll uses Liquid as its template language, and adds a few objects, tags, and filters. These include objects representing content pages, tags for including snippets of content in others, and filters for manipulating strings and URLs.

Jekyll also powers [GitHub Pages](https://pages.github.com/), a web hosting service that lets you push a Jekyll installation to a GitHub repository and have the resulting website published. This website is built using GitHub Pages.

Jekyll might not be using the latest version of Liquid. This means that the tags and filters listed on this site may not work in Jekyll. Often the Jekyll project will wait for a stable release of Liquid rather than using a beta or release candidate version. To see what version of Liquid Jekyll is using, check the **runtime dependencies** section of [Jekyll’s gem page](https://rubygems.org/gems/jekyll).

Jekyll’s version of Liquid is documented in the [Liquid section of Jekyll’s documentation](https://jekyllrb.com/docs/liquid/). If you want to try out Jekyll’s version of Liquid, you can clone the Jekyll project or install Jekyll as a gem and test Liquid on a static site.

## Whitespace Control

In Liquid, you can include a hyphen in your tag syntax `<%=-`, `-%>`, `%-`, and `-%>` to strip whitespace from the left or right side of a rendered tag.

Normally, even if it doesn’t print text, any line of Liquid in your template will still print a blank line in your rendered HTML:

Input:
```
<% assign my_variable = "tomato" %>
<%= my_variable %>
```

Notice the blank line before “tomato” in the rendered template:
```

tomato
```

By including hyphens in your assign tag, you can strip the generated whitespace from the rendered template:
```

<%- assign my_variable = "tomato" -%>
<%= my_variable %>
```

Output:
```
tomato
```