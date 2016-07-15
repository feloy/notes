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
    
