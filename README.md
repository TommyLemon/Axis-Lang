# Axis-Lang
A programming language running on JVM.
Powerful, Flexible, Safe and Simple.


## Features

#### Support JSON like Schema
```javascript
onject : Object = {}.(
  'key0' = value0
  'key1' = value1
  ...
)
```

#### Strong and static Type 


#### Few system types
only Boolean, Number, Decimal, String, Object and Array

#### Safe type
```javascript
id : Number! = 0
name : String? = null
```

#### Default value for arguments
such as 
```javascript
function(arg0 : Type0, arg1 : Type1 = null)
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
    return name
  }

  @Override
  (value) { 
    name = value
  }
}? = null
```
private, protected or public fields have no getter or setter functions.

#### Expand fields
such as
```javascript
user.('isFriend' = true) //isFriend is not a field decleared in User, so it must be covered with ''
LOG(
  tag = User.class.getSimpleName()
  message = 'id=' + user.id + '; isFriend=' +  user.('isFriend')
)
```

#### Package level funcions
such as
```javascript
package org.axis.api

abtract isCorrect() : Boolean!

LOG(tag : String, message : String) {
  ...
}
```
only suppor public abstract and public static functions.

#### Multiple extends and support Objects and functions
such as
```javascript
class User : Object, isCorrect {

  @Override
  isCorrect() : Boolean! {
    return true
  }
}
```
the first one must be an Object Type, and the after Object Type can only supply CONSTANS and abstract functions.


#### '==' between any Types
```javascript
b == true
i == 0
s == ''
obj == {}
arr == []
user == User{}
user == User{}.(
  id = 1
  name = 'tommy'
)
```

### '+', '-' between Arrays
```javascript
arr : Array = [1, 2, 3]

//forbidden  arr += 4 // arr.add(4)

PRINT(arr) // [1, 2, 3, 4]

arr += [5, 6] //arr.addAll([5, 6])

PRINT(arr) // [1, 2, 3, 4, 5, 6]

arr -= [2, 5] //arr.removeAll([2, 5])

PRINT(arr) // [1, 3, 4, 6]

arr -= 0 //arr.remove(0)

PRINT(arr) // [3, 4, 6]

//forbidden  arr -= {0, 2} //arr.remove(0) arr.remove(2)

PRINT(arr.0) // 3

PRINT(arr.'0') // 3

PRINT(arr.3) // throw IndexOutOfBoundsException('index = 3, arr.length = 2, index > arr.length !')

PRINT(arr.'3') // null
```

### '+', '-' between Objects
```javascript
obj : Object = {}.(
  'id' = 1
  'sex' = 0
  'name' = null
)

obj += {}.(
  'name'  = 'test'
  'phone' = '123456789'
) // obj.putAll({'name' : 'test', 'phone': '123456789'})

PRINT(obj) // { 'id' = 1, 'sex' = 0, 'name' = 'test', 'phone' = '123456789' }

obj -= 'sex' //obj.remove('sex')

PRINT(obj) // { 'id' = 1, 'name' = 'test', 'phone' = '123456789' }

//forbidden  obj -= ['id', 'phone'] //obj.remove('id')  obj.remove('phone')

PRINT(obj.name) // test

PRINT(obj.'name') // test

PRINT(obj.tag) // throw NotFoundException('could not find the key "tag" in obj !')

PRINT(obj.'tag') // null
```


#### forEach
Array
```javascript
NAMES : String[] = [
  'name0'
  'name1'
  'name2'
]

NAMES.forEach(
  index : Number!
  item : String?
) {
  LOG(
    tag = 'FOR_EACH'
    message = item
  )
}
```

Object
```javascript
object : Object = {
  'key0' = value0
  'key1' = value1
  'key2' = value2
}

object.forEach(
  key : String?
  value : Any?
) {
  LOG(
    tag = 'FOR_EACH'
    message = 'key=' + key + '; value=' + value
  )
}
```

#### Assign value for final fields on any time
declare a Type 
```javascript
class User : Object {
  final id : Number!
  final name : String?
}
```
the call
```javascript
user : User = User{
  id = 1
  name = null
}
```
or
```javascript
user : User = User{}
user.(
  id = 1
  name = null
)
```

#### No 'static'
replaced with UPPER_CASE names.
static class
```javascript
class Outter : Object {
  class INNER : Object {
  }
}
```
static funciton
```javascript
MAIN(args : String[]?) {
}
```
static field
```javascript
final TAG : String = 'Axis'
```

#### No 'new' and no constructor
replaced with Type{}, such as
```javascript
user : User = User{}
```

#### No 'void' for functions
```javascript
call() {
}

callBack() : String {
}
```
```javascript
call()
title : String = callBack()
```


#### No interface
replaced with Object and function in package level
```javascript
abstract refresh(array : Array?)

class AdapterViewCallback {
  createView(
    type : Number!
    parent : ViewGroup?
  ) : View
  
  bindView(
    type : Number!
    itemView : ItemView!
    position : Number!
  )
}
```

```javascript
class BaseAdapter : Adapter, AdapterViewCallback, refresh {
  @Override
  createView(
    type : Number!
    parent : ViewGroup?
  ) : View {
    return DemoView{}
  }
  
  @Override
  bindView(
    type : Number!
    itemView : ItemView!
    position : Number!
  ) {
    itemView.bindView(
      getItemViewType(position = position)
      getItem(position = position)
      position = position
    )
  }
  
  @Override
  refresh(array : Array?) {
    this.array = array == null ? [] : Array.OF(array = array)
  }
}
```

