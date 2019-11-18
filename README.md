# PHP Liquid

Liquid is a PHP port of the [Liquid template engine for Ruby](https://github.com/Shopify/liquid), which was written by Tobias Lutke. Although there are many other templating engines for PHP, including Smarty (from which Liquid was partially inspired), Liquid had some advantages that made porting worthwhile:

 * Readable and human friendly syntax, that is usable in any type of document, not just html, without need for escaping.
 * Quick and easy to use and maintain.
 * 100% secure, no possibility of embedding PHP code.
 * Clean OO design, rather than the mix of OO and procedural found in other templating engines.
 * Seperate compiling and rendering stages for improved performance.
 * Easy to extend with your own "tags and filters":https://github.com/harrydeluxe/php-liquid/wiki/Liquid-for-programmers.
 * 100% Markup compatibility with a Ruby templating engine, making templates usable for either.
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

To find more examples have a look at the `examples` directory or at the original Ruby implementation repository's [wiki page](https://github.com/Shopify/liquid/wiki).