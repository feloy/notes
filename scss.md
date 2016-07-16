# SCSS

    $ sass input.scss output.css
 
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

Syntax

    $my-color: #ff0088;
    
    p.colored {
      // - and _ are interchangeable
      color: $my_color
    }

Global vs local

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
