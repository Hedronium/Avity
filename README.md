# Avity
---
<img src="http://hedronium.github.io/Avity/images/varied_1.jpeg" style="width: 5em !important;">
Highly Customizable Identicon Generator for PHP.


# Installation  
Get it through composer cli.

```
composer require hedronium/avity
```

or by adding it to your `composer.json`
```JSON
{
    "require": {
        "hedronium/avity": "^1.0"
    }
}
```

and running `composer install`

# Getting Started
```PHP
use Hedronium\Avity\Avity;

$avity = Avity::init()->generate()->jpg()->toBrowser();
```

Thats it thats all you really need to generate an Identicon.
The above code will generate an Identicon based on Random values.




# Basic Customization

## Customizing Image Dimensions
You can call the `height($value)` and `width($value)` method on the `Avity` instance
after initialization. Like:

```PHP
$avity = Avity::init();

$avity->height(600)->width(500); // Long Vertical Identicon. WOW!

$avity->generate()->jpg()->toBrowser();
```

Yes, its a Fluent API, method chaining is cool!

![Varied Color](http://hedronium.github.io/Avity/images/vertical.jpeg)


## Customizing the Grid
You can call the `rows($value)` and `columns($value)` method on the `Avity` instance
after initialization. Like:

```PHP
$avity = Avity::init();

$avity->rows(3)->columns(3); // 3x3 Grid

$avity->generate()->jpg()->toBrowser();
```

![Varied Color](http://hedronium.github.io/Avity/images/3_by_3.jpeg)

## Padding
You can call the `padding($value)` method on the `Avity` instance

![Padded](http://hedronium.github.io/Avity/images/padded.jpeg)  

```PHP
$avity = Avity::init();

$avity->padding(100);

$avity->generate()->jpg()->toBrowser();
```

## Style Specific Customizations.
Often the style class used has specific methods that customize its behaviour
which are not directly available on the Avity object for such cases the style
instace can be fetched with the `style()` method on the `Avity` instance.

Like:

```PHP
$avity = Avity::init();

$avity->style()->variedColor()->spacing(10); // `spacing()` & `variedColor()` is a style specific method

$avity->generate()->jpg()->toBrowser();
```

![Varied Color](http://hedronium.github.io/Avity/images/varied_2.jpeg)
![Varied Color](http://hedronium.github.io/Avity/images/varied_1.jpeg)


# Generators
`Generators` are objects that generate numbers. These numbers are used by Layouts
to set blocks onto the grid.

Avity comes built in with two generators `Hash` & `Random`

To use a different generator you can pass in an associative array
of options with the `generator` key and the class bane as the value. Like:

```PHP
$avity = Avity::init([
    'generator' => \Hedronium\Avity\Generators\Hash::class
]);
```


available classes:

- `\Hedronium\Avity\Generators\Hash` (_default_)
- `\Hedronium\Avity\Generators\Random`



## The Hash Generator
Once you got the generator setup like above to pass in a value to hash you can call `hash()` on the `Avity` instance.
(it can be anything, like the user's username or email address or id)

```PHP
$avity = Avity::init([
    'generator' => \Hedronium\Avity\Generators\Hash::class
]);

$avity->hash('I like Bananas and I cannot lie.'); // I really like bananas.

$avity->generate()->jpg()->toBrowser();
```

This will generate the same identicon each time you give it the same value to hash.


# Layouts
`Layout` objects use `Generator` objects to set blocks onto the grid.
Avity comes with 3 built in `Layout` classes:

- `\Hedronium\Avity\Layouts\VerticalMirror` (_default_)
- `\Hedronium\Avity\Layouts\HorizontalMirror`
- `\Hedronium\Avity\Layouts\DiagonalMirror`

![Avity](http://hedronium.github.io/Avity/images/vertical_mirror.jpeg)
![Avity](http://hedronium.github.io/Avity/images/horizontal_mirror.jpeg)
![Avity](http://hedronium.github.io/Avity/images/diagonal_mirror.jpeg)

changing the layout class

```PHP
$avity = Avity::init([
    'layout' => \Hedronium\Avity\Layouts\DiagonalMirror::class
]);
```

# Styles
`Style` objects use `Layout` objects to draw the grid onto a canvas.
Avity comes with 4 built in `Style` classes:

- `\Hedronium\Avity\Styles\Square` (_default_)
- `\Hedronium\Avity\Styles\SquareCircle`
- `\Hedronium\Avity\Styles\Circle`
- `\Hedronium\Avity\Styles\Triangle`

![Avity](http://hedronium.github.io/Avity/images/vertical_mirror.jpeg)
![Avity](http://hedronium.github.io/Avity/images/circle.jpeg)
![Avity](http://hedronium.github.io/Avity/images/square_circle.jpeg)
![Avity](http://hedronium.github.io/Avity/images/triangle.jpeg)

changing the style class

```PHP
$avity = Avity::init([
    'layout' => \Hedronium\Avity\Styles\Triangle::class
]);
```

## spacing()
![Avity](http://hedronium.github.io/Avity/images/spaced.jpeg)   

All built in `Style` classes have a `spacing(_int_ $value)` method that can be
used to set the space between blocks.
Like:

```PHP
$avity = Avity::init();
$avity->style()->spacing(30);
```

## variedColor()
This ones a fun method available to all default `Style` classes. Just call it and see the magic happen.
Like:

```PHP
$avity = Avity::init();

$avity->style()->variedColor();

$avity->generate()->jpg()->toBrowser();
```

# Advanced Customization
## Custom Generator
A generator class should always extend `Hedronium\Avity\Generator`
example:

```PHP
<?php
use Hedronium\Avity\Generator;

/**
 * A Generator that uses php's `rand()` function.
 */
class YourRandGenerator extends Generator
{
    public function next($x, $y)
    {
        // You could use the $x & $y values to
        // return something specific but usually it souldn't matter.

        return rand();
    }
}
```

### Using a custom generator
```PHP
$avity = Avity::init([
    'generator' => YourRandGenerator::class
]);
```

it is possible that you need to pass construction parameters when instantiating
your generator, for such cases you may set a callback with constructs your generator
like:

```PHP
$avity = Avity::init([
    'generator' => function () {
        return new YourRandGenerator('Please be very random.');
    }
]);
```

## Custom Layouts
A generator class should always extend `Hedronium\Avity\Layout`
example:

```PHP
<?php
use Hedronium\Avity\Layout;

/**
 * A Generator that uses php's `rand()` function.
 */
class NoMirror extends Layout
{
    public function drawGrid(array $values)
    {
        // Sometimes, some style objects are not binary
        // they may draw more than one shape of block
        // thus the `$values` variable is passed in by the style.

        $grid = [];
        for ($y = 0; $y < $this->rows; $y++) {
            $grid[$y] = [];

            for ($x = 0; $x < $this->columns; $x++) {

                // should draw takes the `$values` and
                // returns a value based on
                // generator output

                $grid[$y][$x] = $this->shouldDraw($values);
            }
        }

        return $grid;
    }
}
```

Please check the source code for more details. (We promise its simple and readable.)

### Using a custom layout
```PHP
$avity = Avity::init([
    'generator' => NoMirror::class
]);
```

it is possible that you need to pass construction parameters when instantiating
your layout, for such cases you may set a callback with constructs your layout
like:

```PHP
$avity = Avity::init([
    'generator' => function ($generator) {
        return new NoMirror($generator, 'POTATO');
    }
]);
```

the callback receive the generator instance as it's very likely that you
will be generating the grid based on the generaotr output.
