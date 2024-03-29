# SCSS

>$ sass input.scss output.css
 
- [Syntax](#syntax)
- [Development](#development)

## Syntax

### Nested rules

    tag1 {
      param1: value1;
      tag2 { 
        param2: value2;
      }
    }
    
>⇓

    tag1 {
      param1: value1;
    }
    tag1 tag2 {
      param2: value2;
    }

### & parent selector

As prefix:

    a {
      font-weight: bold;
      &:hover { text-decoration: underline; }
      &.important { color: red; }
    }

>⇓

    a {
      font-weight: bold;
    }
    a:hover { 
      text-decoration: underline; 
    }
    a.important { color: red; }
    
As suffix:

    a {
      font-weight: bold;
      body.accessi & { font-weight: normal; }
    }

>⇓

    a {
      font-weight: bold;
    }
    body.accessi a { 
      font-weight: normal; 
    }
    
### Nested properties

    .mytypo {
      font: {
        family: arial;
        size: 12px;
        weight: bold;
      }
      padding: {
        left: 0px;
        bottom: 4px;
      }
    }

>⇓

    .mytypo {
      font-family: arial;
      font-size: 12px;
      font-weight: bold;
      padding-left: 0px;
      padding-bottom: 4px;
    }

### Comments

    /* CSS comment, 
     * removed only in compressed mode */
    
    // always removed comment, 
    // useful for developers
    
    /*! never removed comment, 
     *  useful for copyright notices */

### Variables

Variables Syntax

    $my-color: #ff0088;
    
    p.colored {
      // - and _ are interchangeable
      color: $my_color
    }

Global vs local variables

    $global-size: 4px;
    p {
      font-size: $global-size;
      $local-color: #FF0088;
      color: $local-color;
    }
    
Interpolation: #{}

    $name: foo;
    $attr: border;
    p.#{$name} {
      #{$attr}-color: blue;
    }

>⇓

    p.foo {
      border-color: blue; 
    }

### Partials

**_part1.scss**

    tag1 {
      param1: value1;
    }

**_part2.scss**

    tag2 {
      param2: value2;
    }
    
**_part3.scss**

    subtag {
      paramsub: subvalue;
    }
    
**file.scss**

    @import "part1";
    @import "part2";
    tag3 {
      param3: value3;
      @import "part3";
    }


>⇓

    tag1 {
      param1: value1;
    }
    tag2 {
      param2: value2;
    }
    tag3 {
      param3: value3;
    }
    tag3 subtag {
      paramsub: subvalue;
    }

### Variable default value in partial

**_module.scss**

    $alert-color: red !default;
    .alert {
      color: $alert-color;
    }
    
**with_default_value.scss**

    @import "module";
    
    
**with_custom_value.scss**

    $alert-color: orange;
    @import "module";
    
### Mixin

    @mixin clearfix {
      height: 100%;
      overflow: auto;
    }
    
    tag { 
      background-color: red;
      @include clearfix;
    }

>⇓

    tag {
      background-color: red;
      height: 100%;
      overflow: auto;
    }

### Mixin with arguments

    @mixin colorify($color, $bgcolor, $border) {
      color: $color;
      background-color: $bgcolor;
      border: 1px solid $border;
    }
    .alert {
      @include colorify(#FF0000, #00FF00, #0000FF);
    }

>⇓

    .alert {
      color: #FF0000;
      background-color: #00FF00;
      border: 1px solid #0000FF;
    }

### Arguments: default, named

    @mixin colorify($color, $bgcolor: transparent, $border: #808080) {
      color: $color;
      background-color: $bgcolor;
      border: 1px solid $border;
    }
    .alert {
      @include colorify($color: #FF0000, $bgcolor: #00FF00);
    }

>⇓

    .alert {
      color: #FF0000;
      background-color: #00FF00;
      border: 1px solid #808080;
    }

### Extend

    $color1: #004000;
    $color2: #008000;
    $color3: #00FF00;
    
    .tag1 {
      border: 1px solid #A0A0A0;
      background-color: #808080;
      color: $color1;
    }
    
    .tag2 {
      @extend .tag1;
      color: $color2;
    }
    
    .tag3 {
      @extend .tag1;
      color: $color3;
    }
    
>⇓

    .tag1, .tag2, .tag3 {
      border: 1px solid #A0A0A0;
      background-color: #808080;
      color: #004000;
    }
    
    .tag2 {
      color: #008000;
    }
    
    .tag3 {
      color: #00FF00;
    }

### Placeholder (to extend)

    $color1: #004000;
    $color2: #008000;
    $color3: #00FF00;

    %tag {
      border: 1px solid #A0A0A0;
      background-color: #808080;
    }

    .tag1 {
      @extend %tag;
      color: $color1;
    }

    .tag2 {
      @extend %tag;
      color: $color2;
    }

    .tag3 {
      @extend %tag;
      color: $color3;
    }

>⇓

    .tag1, .tag2, .tag3 {
      border: 1px solid #A0A0A0;
      background-color: #808080;
    }
    
    .tag1 {
      color: #004000;
    }
    
    .tag2 {
      color: #008000;
    }
    
    .tag3 {
      color: #00FF00;
    }

### Mixin vs Placeholder

 - With parameters: include mixin
 - With parameters: prefer to extend placeholder

### Media Queries

Nested media queries

    .wrapper {
      width: 960px;
      @media only screen {
          @media (min-width: 1400px) {
            width: 1400px;
          }
          @media (max-width: 450px) {
            width: 300px;
          }
      }
    }

>⇓

    .wrapper {
      width: 960px;
    }
    @media only screen and (min-width: 1400px) {
      .wrapper {
        width: 1400px;
      }
    }
    @media only screen and (max-width: 450px) {
      .wrapper {
        width: 300px;
      }
    }

Media queries with mixins

    $phone: 450px;
    $large-screen: 1400px;
    
    @mixin at-least($device-width) {
      @media only screen and (min-width: $device-with) {
        @content;
      }
    }
    @mixin until($device-width) {
      @media only screen and (max-width: $device-with - 1) {
        @content;
      }
    }
    
    .wrapper {
      width: 960px;
      @include at-least($large-screen) {
        width: $large-screen;
      }
      @include until($phone) {
        width: 300px;
      }
    }

>⇓

    .wrapper {
      width: 960px;
    }
    @media only screen and (min-width: 1400px) {
      .wrapper {
        width: 1400px;
      }
    }
    @media only screen and (max-width: 449px) {
      .wrapper {
        width: 300px;
      }
    }

## Development

### Types

    $ sass -i
    >> type-of($val)
    "string"

 - string
 - number
 - color
 - bool
 - list
 - null

#### String type

`astring` or `"astring"` or `'astring'`

#### Number type

`14` or `14px` or `14%` etc

#### Color type

`red` or `#F0F0F0` or `rgb(40, 40, 40)` or `rgba(40, 40, 40, 0.5)` or `hsl(30deg, 50%, 90%)` or `hsla(30deg, 50%, 90%, 0.5)`

#### Boolean type

`true` or `false`

#### List type

`val1 val2 val3` or `val1, val2, val3` or `(val1 val2 val3)` or `(val1, val2, val3)`

#### Null type

    $val: null;
    tag1 {
      param1: $val; // param1 will be removed (and tag1 because empty)
    }
    
    tag2 {
      param2: null; // null is considered the string "null"
    }

>⇓

    tag2 {
      param2: null;
    }

### control directives

#### if

    @if condition {
     // 
    }
    @else if condition {
      // 
    }
    @else {
      // 
    }
    
#### for

    @for $i from 1 to 10 {
      // executed with i = 1 ... 9
    }
    
    @for $i from 1 through 10 {
      // executed with i = 1 ... 10
    }
    
#### while

    $i: 10;
    @while $i > 0 {
      // 
      $i: $i - 1;
    } 
  
#### each

    @each $elt in $list-variable {
      // Executed for each element in list
    }
    

### Functions

#### Definition

    @function my-function($a, $b) {
      @return ($a + $b) / 2;
    }
    .class {
      width: my-function(1px, 9px);
    }
    

>⇓


    .class {
      width: 5px;
    }

#### Core functions

http://sass-lang.com/documentation/frames.html#!Sass/Script/Functions.html
