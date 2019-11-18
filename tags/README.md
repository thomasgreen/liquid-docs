# Tags


Tags create the logic and control flow for templates. They are denoted by less than / greater than and percent signs: <% and %>.

The markup used in tags does not produce any visible text. This means that you can assign variables and create conditions and loops without showing any of the Liquid logic on the page.

```php
<% if user %>
  Hello <%= user.name %>!
<% endif %>
```