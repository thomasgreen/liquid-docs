# Component Tag

The `component` tag is used to include other components into current liquid template. 

Example Liquid Template:
```php
<% component 'organism/product-accordions',
   productAccordions: productAccordions %>
<% endcomponent %>
``` 

**You must close the component tag with `<% endcomponent %>`, otherwise it will not work.**

You can pass in content between the `component` and `endcomponent` tags. This will be rendered by liquid, and will be available in a liquid variable called `slot`.

For example, you might have a componenent 