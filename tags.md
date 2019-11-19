# Tags


Tags create the logic and control flow for templates. They are denoted by less than / greater than and percent signs: <% and %>.

The markup used in tags does not produce any visible text. This means that you can assign variables and create conditions and loops without showing any of the Liquid logic on the page.

```php
<% if user %>
  Hello <%= user.name %>!
<% endif %>
```

Output: 
```
Hello Adam!
```



Tags can be categorized into three types:
 - Control flow
 - Iteration
 - Variable assignments


## Control flow

Control flow tags can change the information Liquid shows using programming logic.

### if
 
Executes a block of code only if a certain condition is `true`.
 
Input:
```
<% if product.title == "Awesome Shoes" %>
  These shoes are awesome!
<% endif %>
```

Output:
```
These shoes are awesome!
```

### unless
 
The opposite of `if` – executes a block of code only if a certain condition is not met.
 
Input:
```
<% unless product.title == "Awesome Shoes" %>
  These shoes are not awesome.
<% endunless %>
```

Output:
```
These shoes are not awesome.
```

This would be the equivalent of doing the following:

```
<% if product.title != "Awesome Shoes" %>
  These shoes are not awesome.
<% endif %>
```

### elseif / else

Adds more conditions within an `if` or `unless` block.

Input: 
```
<!-- If customer.name = "anonymous" -->
<% if customer.name == "kevin" %>
  Hey Kevin!
<% elseif customer.name == "anonymous" %>
  Hey Anonymous!
<% else %>
  Hi Stranger!
<% endif %>
```

Output:
```
Hey Anonymous!
```

### case/when

Creates a switch statement to compare a variable with different values. `case` initializes the switch statement, and `when` compares its values.

Input:
```
<% assign handle = "cake" %>
<% case handle %>
  <% when "cake" %>
     This is a cake
  <% when "cookie" %>
     This is a cookie
  <% else %>
     This is not a cake nor a cookie
<% endcase %>
```

Output:
```
This is a cake
```

## Iteration
Iteration tags run blocks of code repeatedly.

### for

Repeatedly executes a block of code. For a full list of attributes available within a for loop, see forloop (object).

Input:
```
<% for product in collection.products %>
  <%= product.title %>
<% endfor %>
```

Output:
```
hat shirt pants
```

### break

Causes the loop to stop iterating when it encounters the `break` tag.

Input:
```
<% for i in (1..5) %>
  <% if i == 4 %>
    <% break %>
  <% else %>
    <%= i %>
  <% endif %>
<% endfor %>
```

Output:
```
1 2 3
```

### continue

Causes the loop to skip the current iteration when it encounters the `continue` tag.

Input:
```
<% for i in (1..5) %>
  <% if i == 4 %>
    <% continue %>
  <% else %>
    <%= i %>
  <% endif %>
<% endfor %>
```

### limit (for parameter)

Limits the loop to the specified number of iterations.

Input:
```
<!-- if array = [1,2,3,4,5,6] -->
<% for item in array limit:2 %>
  <%= item %>
<% endfor %>
```

Output:
```
1 2
```

### offset (for parameter)

Limits the loop to the specified number of iterations.

Input:
```
<!-- if array = [1,2,3,4,5,6] -->
<% for item in array offset:2 %>
  <%= item %>
<% endfor %>
```

Output:
```
3 4 5 6
```

### range (for parameter)

Defines a range of numbers to loop through. The range can be defined by both literal and variable numbers.

Input:
```
<% for i in (3..5) %>
  <%= i %>
<% endfor %>

<% assign num = 4 %>
<% for i in (1..num) %>
  <%= i %>
<% endfor %>
```

Output:
```
3 4 5
1 2 3 4
```

### cycle

Loops through a group of strings and prints them in the order that they were passed as arguments. Each time `cycle` is called, the next string argument is printed.

`cycle` must be used within a for loop block.

Input:
```
<% cycle "one", "two", "three" %>
<% cycle "one", "two", "three" %>
<% cycle "one", "two", "three" %>
<% cycle "one", "two", "three" %>
```

Output:
```
one
two
three
one
```

`cycle` accepts a “cycle group” parameter in cases where you need multiple `cycle` blocks in one template. If no name is supplied for the cycle group, then it is assumed that multiple calls with the same parameters are one group.

Input:
```
<% cycle "first": "one", "two", "three" %>
<% cycle "second": "one", "two", "three" %>
<% cycle "second": "one", "two", "three" %>
<% cycle "first": "one", "two", "three" %>
```


Output:
```
one
one
two
two
```

## Component

The `component` tag is used to include other components into current liquid template. 

Example use of `component` tag:
```
<% component 'organism/product-accordions',
   productAccordions: productAccordions %>
<% endcomponent %>
``` 

**You must close the component tag with `<% endcomponent %>`, otherwise it will not work.**

### Slot Content
You can pass in content between the `component` and `endcomponent` tags. This will be rendered by liquid, and will be available in a liquid variable called `slot`.

For example, you might have a `component` that contains the following:

```
<% component 'header' %>
This is example slot content
<% endcomponent %>
```

And a `component` called `header.liquid` that contains:

```
<header>
    <%= slot %>
</header>
```

This will render:

```
<header>
    This is example slot content
</header>
```

## Icon

The `icon` tag will render out an svg icon from the `media/icons` folder.

Input:
```
<% icon 'loading/spin' class: 'spin', size: 64 %>
```

Props can be passed in after the name, comma seperated. 

**You may pass in liquid variables as the icon name by not wrapping the name in `'`**

Example:
Input:
```
<% assign foo = 'arrow-up' %>
<% icon foo class: 'spin', size: 64 %>
```

## Assigning Variables

Variable tags create new Liquid variables.

### assign

Creates a new variable.

Input:
```
<% assign my_variable = false %>
<% if my_variable != true %>
  This statement is valid.
<% endif %>
```

Output:
```
This statement is valid.
```

Wrap a variable value in quotations `"` to save it as a string.

Input:
```
<% assign foo = "bar" %>
<%= foo %>
```

Output:
```
bar
```

#### Using ternary operator

You can use a ternary operator within an `assign` tag, instead of using verbose `if/else`.

Example:
```
<% assign basketCount = 4  %>
<% assign hiddenState = basketCount > 0 ? '' : 'hidden' %>
```

This will set `hiddenState` to an empty string, as `basketCount` is greater than 4.

`assign` ternary supports **one** logical operator either `and` (which is the same as a php `&&`), and `or` (a php `||`).

Example:
```
<% assign basketCount = 4  %>
<% assign bar = true  %>
<% assign hiddenState = (bar and basketCount > 0) ? '' : 'hidden' %>
```

The following comparison operators are available to use:

| Operators     | Name                      |   
| ------------- | ------------------------- |
| `==`          | Equal                     |
| `!=`          | Not equal                 |
| `===`         | Identical                 |
| `!==`         | Not identical             |
| `>`           | Greater than              |
| `>=`          | Greater than or equal to  |
| `<`           | Less than                 |
| `<=`          | Less than or equal to     |

### capture

Captures the string inside of the opening and closing tags and assigns it to a variable. Variables created through `capture` are strings.

Input:
```
<% capture my_variable %>I am being captured.<% endcapture %>
<%= my_variable %>
```

Output:
```
I am being captured.
```

Using `capture`, you can create complex strings using other variables created with `assign`:

Input:
```
<% assign favorite_food = "pizza" %>
<% assign age = 35 %>

<% capture about_me %>
I am <%= age %> and my favorite food is <%= favorite_food %>.
<% endcapture %>

<%= about_me %>
```

Output:
```
I am 35 and my favourite food is pizza.
```

## ampstate

Allows the input of amp state. 

Input:
```
<% ampstate 'productList' src:endpoint, [src]:endpoint %>
  {
    "Data":true
  }
<% endampstate %>
```

Output:
```
<amp-state
  id="'productList'"
  src="/amp/listings/products/c:1"
  [src]="/amp/listings/products/c:1" >
</amp-state>
```

Tag parameters are in key/value pairs, separated by a comma.

The key does not need to be enclosed in quotes and will always be output as a literal string.

The value does need to be enclosed in quotes if it is to be interpreted as a literal string - if it is not enclosed in quotes it will be interpreted as a Liquid variable. Integers are always interpreted as a literal string.

Input:
```
<% ampstate 'product' src:endpoint, count:5, title='Please Wait' %>
  {
    "Spinner":true
  }
<% endampstate %>
```

Output:
```
<amp-state
  id="'product'"
  src="/amp/listings/products/c:1"
  count="5"
  title="Please Wait" >
  <script type="application/json">
    {
      "Spinner":true
    }
  </script>
</amp-state>
```

## ampmacro

//todo
