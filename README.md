# Axis-Lang
A programming language running on JVM.
Powerful, Flexible, Safe and Simple.


## Features

#### Support JSON like Schema
```javascript
Map object : {
  'key0' : value0
  'key1' : value1
  ...
}
```

#### Strong and static Type 


#### Few system types
only Any, Bool, Int, Num, Char, Map and List

#### Safe type
```javascript
Int id : 0
Char name : null

id.toChar() // '0'

name.length //won't execute and won't throw NullPoninterExeption
name.toUpperCase() //won't execute and won't throw NullPoninterExeption
```

#### Default value for arguments
such as 
```javascript
function(Type0 arg0, Type1 arg1 : null)
```
then you can call 
```javascript
function(arg0 : value0)
```
or 
```javascript
function(
  arg0 : value0
  arg1 : value1
)
```

#### Default and anonymous getter and setter functions for fields
such as
```javascript
Char name {
  @Override
  Char get() {
    return name
  }

  @Override
  Char set(Char value) {
    name : value
    return this
  }
} : null
```
private, protected or public fields have no getter or setter functions.

#### Expand fields
such as
```javascript
user.'isFriend' : true //isFriend is not a field decleared in User, so it must be covered with ''
LOG(
  tag : User.class.getSimpleName()
  message : 'id = ' + user.id + '; isFriend = ' +  user.'isFriend'
)
```

#### Package level funcions
such as
```javascript
package org.axis.api

abtract Bool isCorrect()

LOG(
  Char tag
  Char message
) {
  ...
}
```
only suppor public abstract and public static functions.

#### Multiple extends and support Maps and functions
such as
```javascript
class Map User implements isCorrect { // public class User extends Map implements Interface$isCorrect {

  @Override
  Bool isCorrect() {
    return true
  }
}
```
the first one must be an Map Type, and the after Map Type can only supply CONSTANS and abstract functions.


#### '=' equal for any Types
```javascript
b = true // b.equals(true)
i = 0 // i.equals(0)
s = '' // s.equals('')
obj = {} // obj.equals({})
arr = [] // arr.equals([])
user = User{} // user.equals(new User())
user = User{ // user.equals(new User().setId(1).setName('tommy'))
  id : 1 // setId(1)
  name : 'tommy' // setName('tommy')
}
```

### '+', '-' between Lists
```javascript
List arr : [1, 2, 3]

//forbidden  arr +: 4 // arr.add(4)

PRINT(arr) // [1, 2, 3, 4]

arr +: [2, 5, 6] //arr.addAll([2, 5, 6])

PRINT(arr) // [1, 2, 3, 4, 2, 5, 6]

arr -: <0, 1> //arr.remove(0);  arr.remove(1);

PRINT(arr) // [3, 4, 2, 5, 6]

arr -: [5] //arr.remove([5]);

PRINT(arr) // [3, 4, 2, 6]

arr -: 2 //arr.remove((Map) 2)

PRINT(arr) // [3, 4, 6]

//forbidden  arr -: {0, 2} //arr.remove(0) arr.remove(2)

PRINT(arr.0) // 3

PRINT(arr.'0') // 3

PRINT(arr.3) // throw IndexOutOfBoundsException('index : 3, arr.length : 3, index >: arr.length !')

PRINT(arr.'3') // null

//forbidden  PRINT(arr.'a') //the index must be a number

arr.4 : 4 // throw IndexOutOfBoundsException('index : 4, arr.length : 3, index > arr.length !')

arr.'4' : 4

PRINT(arr.'4') // 4

PRINT(arr) // [3, 4, 6, null, 4]
```

### '+', '-' between Maps
```javascript
Map obj : {
  'id' : 1
  'sex' : 0
  'name' : null
}

obj +: {
  'name'  : 'test'
  'phone' : '123456789'
} // obj.putAll({'name' : 'test', 'phone': '123456789'})

PRINT(obj) // { 'id' : 1, 'sex' : 0, 'name' : 'test', 'phone' : '123456789' }

obj -: <'sex'> //obj.remove('sex')

PRINT(obj) // { 'id' : 1, 'name' : 'test', 'phone' : '123456789' }

obj -: 1 //obj.removeValue(1);

PRINT(obj) // { 'name' : 'test', 'phone' : '123456789' }

obj -: ['123456789'] //obj.removeValues(['123456789']);

PRINT(obj) // { 'name' : 'test' }

PRINT(obj.name) // test

PRINT(obj.'name') // test

//forbidden  PRINT(obj.tag) // throw NotFoundException('could not find the key "tag" in obj !')

PRINT(obj.'tag') // null

//forbidden  obj.tag : 'Java'

obj.'tag' : 'Java'

PRINT(obj.'tag') // Java
```


#### forEach
List
```javascript
Char[] NAMES : [
  'name0'
  'name1'
  'name2'
]

NAMES.forEach(
  Int index
  Char item
) {
  LOG(
    tag : 'FOR_EACH'
    message : item
  )
}
```

Map
```javascript
Map object : {
  'key0' : value0
  'key1' : value1
  'key2' : value2
}

object.forEach(
  Char key
  Any value
) {
  LOG(
    tag : 'FOR_EACH'
    message : 'key = ' + key + '; value = ' + value
  )
}
```

#### Assign value for final fields on any time
declare a Type 
```javascript
class Map User {
  final Int id
  final Char name
}
```
then call
```javascript
User user : {
  id : 1
  name : null
}
```
or
```javascript
User user : {} // User user = new User();
user.sex : 0 // user.setSex(0);
user.{
  id : 1 // user.setId(1);
  name : null // user.setName(null);
}
```

#### No 'static'
replaced with UPPER_CASE names. <br />
static class
```javascript
class Map Outter {
  class Map INNER {
  }
}
```
static funciton
```javascript
MAIN(Char[] args) {
}
```
static field
```javascript
final Char TAG : 'Axis'
```

#### No 'new' and no constructor
replaced with Type{}, such as
```javascript
User user : User{}
```

#### No 'void' for functions
```javascript
call() {
}

Char callBack() {
  return 'Title'
}
```
```javascript
call()
Char title : callBack()
```


#### No interface
replaced with Map and function in package level
```javascript
abstract refresh(List array)

class AdapterViewCallback {
  View createView(
    Int type
    ViewGroup parent
  )
  
  bindView(
    Int type
    ItemView itemView
    Int position
  )
}
```

```javascript
class Adapter BaseAdapter implements AdapterViewCallback, refresh {
  @Override
  View createView(
    Int type
    ViewGroup parent
  ) {
    return DemoView{}
  }
  
  @Override
  bindView(
    Int type
    ItemView itemView
    Int position
  ) {
    itemView.bindView(
      type : getItemViewType(position : position)
      itemView : getItem(position : position)
      position : position
    )
  }
  
  @Override
  refresh(List array) {
    this.array : when (array =) {
      (null) : []
      () : List.OF(array : array)
    }
  }
}
```


#### If
replace if-else if-else, switch-case
```javascript
if () {
  (a = 1) {
    ...
  }
  (a < 1 & a != -1) {
    ...
  }
  () { //else
    ...
  }
}
```

```javascript
if (a) {
  (= 1) {
    ...
  }
  (< 1) {
    ...
  }
  () { //else
    ...
  }
}
```

```javascript
if (a =) {
  (1) {
    ...
  }
  (2 | 3) {
    ...
  }
  () { //else
    ...
  }
}
```


```javascript
if (a = 1) {
  ...
}
```

#### Callback
##### Abstract Method
1.Define an abstract method:
```javascript
run()
```
2.Define a method with argument, and the type of the argument is a abstract method above:
```javascript
runOnUiThread (run action) {
  ...
}
```
3.Then you can call:
```javascript
runOnUiThread (
  /* run */ action : () { //type was a generated comment. No name means default name 'run'.
    ...
  }
)
```

##### Interface
1.Define an interface:
```javascript
interface Runnable {
  run()
}
```
2.Define a method with argument, and the type of the argument is a interface above:
```javascript
runOnUiThread (Runnable action) {
  ...
}
```
3.Then you can call:
```javascript
runOnUiThread (
  /* Runnable */ action : { //type was a generated comment. No name means default name 'Runnable'.
    run () {
      ...
    }
  }
)
```

#### Thread
childThread:
```javascript
childThread { // in a automatically recycler thread pool
  // run on child thread
}
```
mainThread:
```javascript
childThread {
  // run on child thread
  
  mainThread {
       // run on main thread
  }
}
```
