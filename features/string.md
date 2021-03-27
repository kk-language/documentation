# String

String can be constructed using double quotes:

```coffeescript
let my_name
  : String
  = "Bob"
```

### String Interpolation

Expression can be interpolated into strings using `#{}` , for example:

```coffeescript
let my_cat : String = "Joo"
let my_dog : String = "Bibi"
do "I like #{my_cat} and #{my_dog}"
```

{% hint style="info" %}
Interpolated expressions must be String

For example, the following code will result in compile error:

```coffeescript
let my_age : Integer = 2
do "I am #{my_age} years old"
#          ^^^^^^ Expected String, got Integer
```
{% endhint %}

