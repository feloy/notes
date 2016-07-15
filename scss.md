# SCSS

    $ sass input.scss output.css
 
## Syntax

### Nesting

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
