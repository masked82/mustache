Mustache.php
============

A [Mustache](http://mustache.github.com/) implementation in PHP.

[![Build Status](https://secure.travis-ci.org/bobthecow/mustache.php.png?branch=dev)](http://travis-ci.org/bobthecow/mustache.php)


Additions
---------

The view helpers now get access to the context object:

```php
<?php
$mustache = new Mustache_Engine(
    array(
        'helpers' => array(
            'displayName' => function($source, \Mustache_LambdaHelper $lambdaHelper, \Mustache_Context $context) {
                // Access the current context:
                $contextValue = $context->last();
                return isset($contextValue->name) ? $contextValue->name : '';
            },
            'changeName' => function($source, \Mustache_LambdaHelper $lambdaHelper, \Mustache_Context $context) {
                // Remove the current context:
                $contextValue = $context->pop();
                // Change the context value:
                $contextValue->name = 'New Name';
                // Save the context value
                $context->push($contextValue);
                return $lambdaHelper->render($source);
            },
        ),
    )
);
```

Usage
-----

```php
<?php
echo $mustache->render(
    'Name:
    {{#person}}
        <p>
            {{#displayName}}{{/displayName}}
        </p>
        <p>
            {{#changeName}}
                The name is changed, but the last name is still {{lastName}}.
            {{/changeName}}
        </p>
        <p>
            {{#displayName}}{{/displayName}}
        </p>
    {{/person}}',
    array(
        'person' => array(
            'name' => 'Some Name',
            'lastName' => 'Last Name'
        )
    )
);
```

The result would be:

```html+jinja
Name:

Some Name
The name is changed, but the last name is still Last Name.
New Name

```


And That's Not All!
-------------------

Read [the Mustache.php documentation](https://github.com/bobthecow/mustache.php/wiki/Home) for more information.