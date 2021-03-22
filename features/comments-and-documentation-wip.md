# Comments & Documentation

### Single line comments

```python
# hello
```

### Multiline comments

```coffeescript
###
Roses are blue;
Violets are pink;
The sky is red;
And so it is.
###
```

### Documentation

Documentation also uses multiline comments. For example:

```coffeescript
###
The number π is a mathematical constant. 
It is defined as the ratio of a circle 's 
circumference to its diameter, 
and it also has various equivalent definitions. 
It appears in many formulas in all areas 
of mathematics and physics.
(Copied from Wikipedia)
###
let pi = 3.142
```

### Code in Documentation

Code snippet within documentation will be treated as normal code. Code snippet can be created by enclosing a section with triple backticks:  


```coffeescript
###
Example:
```
do `9.0.square`.should_be(`81`)
```
###
let square
  : | Float => Float
  = | n => n.multiply(2.0)
```

Reference: [https://documentation.divio.com/](https://documentation.divio.com/)

