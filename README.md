# Axis-Lang
A programming language running on JVM.
Powerful, Flexible, Safe and Simple.


## Features

#### Support JSON in the original code
```javascript
onject : Object = {
  'key0' : value0
  'key1' : value1
  ...
}
```

#### Strong and static Type 


#### Few system types
only Boolean, Number, Decimal, String, Object and Array

#### Default value for arguments
such as 
```javascript
function(arg0 : Type0, arg1 : Type1 = null),
```
then you can call 
```javascript
function(arg0 = value0)
```
or 
```javascript
function(arg0 = value0, arg1 = value1)
```

#### Default and anonymous getter and setter functions for fields
such as
```javascript
name : String{
  @Override
  () {
    return this
  }

  @Override
  (value) { 
    this = value
  }
}? = null
```

#### Expand fields for Types when declearing for variables
such as
```javascript
user : User{
  'isFriend' : Boolean 
}
```

#### Multiple extends, such as
```javascript
class User : Object, isCorrect {
}
```

<br />
#### No 'static', replaced with UPPER_CASE names <br />
#### No 'new' and no constructor, replaced with Type{} <br />
#### No interface, replaced with Object and function in package level <br />
