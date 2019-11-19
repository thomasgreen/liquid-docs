# Filters



Filters change the output of a Liquid object. They are used within an output and are separated by a `|`.

Input:
```
{{ "/my/fancy/url" | append: ".html" }}
```

Output:
```
/my/fancy/url.html
```

Multiple filters can be used on one output. They are applied from left to right.

Input:
```
{{ "adam!" | capitalize | prepend: "Hello " }}
```

Output:
```
Hello Adam!
```

## Adding New Filters

Basically, they are just methods which take one parameter and return a modified string. Custom Filters are added to `Vs\Amp\Liquid\CustomFilters.php`.

Here is an example of a custom filter:

```php
public static function unique($input)
    {
        return $input . md5(uniqid(rand(), true));
    }
```

## Passing arguments to filters

Normally, a filter will take the string to the left of `|` as the input, but more arguments can be passed in as props. A good example of this is the `localisation` filter.

Input:
```
<%= 'product.free_delivery' | localisation foo: 'bar' %>
```

This will allow you to access an argument called `foo` for use in the php method. Use `func_get_args()` in your method to allow 0 to many arguments to the filter code.

Example:
```php
public static function localisation()
    {
        $args = func_get_args();
        $input = array_shift($args);
        return \Kohana::lang($input, $args);
    }
```

## Liquid Filters

For a list of liquid filters please consult [this list](https://shopify.github.io/liquid/filters).

## Custom Filters

The following list contains the filters that have been added to `CustomFilters.php` for use in VS3. These should only really be specific to VS3.

### Localisation

Takes the input and use `Kohana::lang` to return the language string contents

Input:
```
<%= 'product.free_delivery' | localisation %>
```

Output:
```
Free Delivery & Returns
```

### Unique

Appends a unique md5 string to the end of the input. Used to generate ID's for amp.

Input:
```
```

Output:
```
```