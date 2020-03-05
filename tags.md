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

## Portal & Portalprint

You can define a section of code to be printed out further on in the liquid process. For example, modals may need to printed at the bototm of the body etc.

To define a portal, just pass in a name as an arguement:
```
<% portal 'test_portal' %>
        This should print after the create account button
    <% endportal %>
```

To print it out:
```
<% portalprint 'test_portal' %>
```
Just use `portalprint` with the same name. The code blocks are made global, so can be injected more than once if needed. 


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


## analytics

For amp analytics to work we need to store each product list on a page, before we can render it into the amp analytics component.

This is a two stage process. Firstly, we use the `analyticshook` tag to store the list. 

```
<% analyticshook
    type: 'trigger',
    action: 'product-impression'
    listName: 'test',
    products: products.products
 %>
<% endanalyticshook %>
```

This stores the products data in a list called `test` to be used in the Product Impression type. This can be used multiple times in different lists (eg product litsings page and recently viewed).

We can then pull the data out of the hook using a filter. 

```
<% assign data = 'product-impression' | analyticshook %>
```

We can then use the `analytics` liquid tag to create the trigger. This trigger is then stored in an array.

```

<% analytics 'productImpression' type:'trigger' %>
    {
        "on": "visible",
        "request": "pageview",
        "extraUrlParams": {
            <% for list in data %>
                <% assign listId = forloop.index %>
                "il<%= listId %>nm": "<%= list.listName %>",
                <% for product in list.products %>
                    "il<%= listId %>pi<%= forloop.index %>id": "<%= product.parent_product_id %>",
                    "il<%= listId %>pi<%= forloop.index %>nm": "<%= product.title %>",
                    "il<%= listId %>pi<%= forloop.index %>ca": "<%= product.category %>",
                    "il<%= listId %>pi<%= forloop.index %>br": "<%= product.brand %>",
                    "il<%= listId %>pi<%= forloop.index %>pr": "<%= product.current_price.numeric %>",
                    "il<%= listId %>pi<%= forloop.index %>ps": <%= product.index | plus: 1 %><% if forloop.last == false %>,<% endif %>
                <% endfor %>
            <% endfor %>
        }
    }
<% endanalytics %>
```

This data can then be rendered in the `amp-analytics` tag.

```
<amp-analytics type="googleanalytics" data-credentials="include">
    <script type="application/json">
        {
            "vars" : {
                "account": "<%= ga_property %>"
            },
            "requests" : {
                "transaction": "${host}/collect?${basePrefix}&t=transaction${baseSuffix}",
                "transactionItem": "${host}/collect?${basePrefix}&t=item${baseSuffix}"
            },
            "triggers": <% analyticsrender %>
        }
    </script>
</amp-analytics>
```

## formbuilder

### Building a form
To build a form you will need a pre-existing FormBuilder form. If you do not have one there is existing [documentation](https://github.com/VisualsoftUK/vscommerce3/tree/master/modules/formbuilder) to describe form setup. If following the documentation bear in mind that only the library, config and `error_messages.php` language files are required, everything else is handled by the liquid templates. Please bear in mind that any changes to existing FormBuilder files will need to maintain backwards compatibility.

The `FormBuilder` library has been modified slightly for AMP forms. Instead of using the `view()` method to render a form it uses the `ampView()` method. See the `TagFormbuilder` class for more details on how we render a form for AMP.

### Loading a form
Within a liquid template, you ca call a form like this. You could also load the form with `Component::loadComponent` in PHP if required.

`<% formbuilder 'amp_register' action_url:'/amp/customer/register', id:'register_form', form_success_redirect:'/customer', action_type:'action-xhr', form_target:'_top' button_type:'submit', button_text:'submit_register' %>`

It is way neater to define the parameters in the FormBuilder config file for the form though, so the above tag could end up looking like this:

`<% formbuilder 'amp_register' %>`

If the FormBuilder config file included the following (id, action_type, form_target, button_type, action_url and button_text can normally be ommitted as they have sensible defaults):

```
// AMP default configuration
$config['amp'] = [
    'action_url' => '/amp/customer/register',
    'id' => 'register_form',
    'form_success_redirect' => '/customer',
    'action_type' => 'action-xhr',
    'form_target' => '_top',
    'button_type' => 'submit',
    'button_text' => 'submit_register',
];
```

The resulting form looks like this:

@TODO: insert FormBuilder image.

The FormBuilder class performs the following tasks:
* The input config file is loaded from the `amp_register` FormBuilder config.
* Form action is set by the `action_url` parameter, and rather than just placing this value in an `action=""` attribute, it's placed inside the type specified by the `action_type` parameter. **This is an optional field and defaults to /amp/form/submit, which will work for most FormBuilder forms.**
* The form ID is set to `register_form` - this ID is also reused for the submit button by default. **This is an optional field and under most circumstances should be omitted.**
* A custom form parameter, in this case `form_target` is set and passed to the form with the value of "`_top`". This is not a standard attribute that is coded into FormBuilder, any parameter with a name that starts `form_` will have the `form_` prefix removed and added to the form tag as a parameter. **This is an optional field that defaults to "_top".**
* `Form_success_redirect` is the URL the customer will be sent to when they successfully submit a form (including passing validation). Don't provide the domain in this, just provide the relative path to the URL you'd like to be redirected to.
* `Button_type` is passed to the form's submit button. This is a custom parameter - you can dynamically add any attribute to a button by specifying it with the `button_` prefix. You could pass any button attribute to the button in this way. **This is an optional field that defaults to "submit"**
* `Button_text` is a special button attribute used to control the text written on the button as well as the button's `aria-label`. In the above example we specify `submit_register` as the button text, which loads in the `formbuilder.button.submit_register` language string as the text on the button, and `formbuilder.button.submit_register_aria` as the `aria-label`.

Not specified above: you can override the form classes by specifying a `form_class` attribute. This will override the default classes.

### Palette
#### Liquid Palette config file
FormBuilder works with the liquid_palette config file. You can specify changes in the normal way. As an example, to modify a form's palette for a form called 'login':

`$config['login'] = [/* Palette */];'`

You can address inputs on a form. Button, for example, is addressed like this:

`$config['login']['button'] = [/* Palette */];'`

Most other inputs have a wrapper and will need the wrapper name added:

`$config['login']['default']['input'] = [/* Palette */];'`

#### FormBuilder config / Liquid tag
You may also modify the palette variables in the FormBuilder config file for the form, or directly when you call the liquid tag. If you'd like to add palette variables to the form you may do so like this:

FormBuilder config:

`$config['amp']['palette'] => 'form';`

Or add to the liquid tag:

`<% formbuilder 'amp_register' palette:'form' %>`

You can also add palette variables to an individual input type on a form. In this example I'll be replacing the palette variables for the button as I'd like to it be styled as a CTA (call to action)

FormBuilder config:

`$config['amp']['palette-input-button'] => 'btn, btn--cta, btn--large';`

Or add to the liquid tag:

`<% formbuilder 'amp_register' palette-input-button:'btn, btn--cta, btn--large' %>`

### Wrappers
Wrappers work in pretty much the same way as they do in the standard FormBuilder functionality. They're just a little view that prints out the input with some HTML around it. Most form inputs have `'wrapper' => 'default_wrapper'` defined in their config, but there are some exceptions. In the above case, we remove the _wrapper postfix and look for a wrapper template. In this case, it's found in `templates/responsive/amp/views/components/formbuilder/wrappers/default.liquid`

### Validating and processing a form
We lose a lot of frontend FormBuilder functionality with AMP, namely the ability to validate the form using JavaScript. HTML5 validation still works, but it's nowhere near as thorough or stylable. That being said, we do retain server-side validation from FormBuilder.

Very little needs to be said about server-side validation. If you send your form to the `amp/form/submit` endpoint it'll handle all the form validation and processing. If, for whatever reason, you need to make your own endpoint for a specific form this endpoint is probably a good starting place.

### Accessing form data from an input
You may need to access form data from an input. You can do this via a liquid tag. If you wanted to echo the form
identifier, for example, you can access it by calling the form_identifier method: `<%= form.form_identifier %>`

Form methods accessible from inputs:
 - form_identifier
 - form_template
 - form_success_redirect
 - action_type
 - action_url
 - form_method
 - form_options
 - form_type
 - identifier
 - referer

### Extending a form

#### Input types
Inside the FormBuilder config for the form each input has an `input_type` field. For example, the email input is of type "`email_input`". We remove the `_input` from the end of this value and look for the liquid template, in this case `templates/responsive/amp/views/components/formbuilder/inputs/default.liquid`. Note that we load `default.liquid`, which is the default template for all inputs. If `email.liquid` existed it would be loaded instead.

#### Form config files
There would be nothing stopping you from extending a form by copying the config and modifying it. In the above example `amp_register` is a cut-down version of `register`. It's been moved to its own config file with its own library to allow for a two-field registration form that would have been incompatible with the existing library. 

#### New form template
Form templates are stored in `templates/responsive/amp/views/components/formbuilder/forms`, and by default we load the `default.liquid` template. If, for whatever reason, you need to customise this template for just one form you can override it. Simply add a new liquid template to this directory with the same name as your form. In the above example we load the `amp_register` form, so if I wanted to have a unique form template for this I'd create an `amp-register.liquid` template file. Note that the config file uses underscores whereas the liquid template uses hyphens.

If you have a form in multiple locations on a site and you want one instance of the form to have a different template to the rest of the forms you can specify this when you call the Liquid tag, like this:

`<% formbuilder 'amp_register' template:'amp_register_checkout' %>`
