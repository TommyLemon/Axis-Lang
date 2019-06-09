# Axis-Lang
A programming language running on JVM.
Powerful, Flexible, Safe and Simple.


## Features

#### Support JSON like Schema
```javascript
Map map : {
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

name.length //won't throw NullPoninterExeption but return null
name.toUpperCase() //won't throw NullPoninterExeption but return null 

User user = null
user.name : name //automatically create user and name in user when they are null and the expression is for assign
PRINT(user.toChar()) //{ name : null }
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
map = {} // map.equals({})
list = [] // list.equals([])
user = User{} // user.equals(new User())
user = User{ // user.equals(new User().setId(1).setName('tommy'))
  id : 1 // setId(1)
  name : 'tommy' // setName('tommy')
}
```

### '+', '-' between Lists
```javascript
List list : [1, 2, 3]

//forbidden  list +: 4 // list.add(4)

PRINT(list) // [1, 2, 3, 4]

list +: [2, 5, 6] //list.addAll([2, 5, 6])

PRINT(list) // [1, 2, 3, 4, 2, 5, 6]

list -: <0, 1> //list.remove(0);  list.remove(1);

PRINT(list) // [3, 4, 2, 5, 6]

list -: [5] //list.remove([5]);

PRINT(list) // [3, 4, 2, 6]

list -: 2 //list.remove((Map) 2)

PRINT(list) // [3, 4, 6]

//forbidden  list -: {0, 2} //list.remove(0) list.remove(2)

PRINT(list.0) // 3

PRINT(list.'0') // 3

PRINT(list.3) // throw IndexOutOfBoundsException('index : 3, list.length : 3, index >: list.length !')

PRINT(list.'3') // null

//forbidden  PRINT(list.'a') //the index must be a number

list.4 : 4 // throw IndexOutOfBoundsException('index : 4, list.length : 3, index > list.length !')

list.'4' : 4

PRINT(list.'4') // 4

PRINT(list) // [3, 4, 6, null, 4]
```

### '+', '-' between Maps
```javascript
Map map : {
  'id' : 1
  'sex' : 0
  'name' : null
}

map +: {
  'name'  : 'test'
  'phone' : '123456789'
} // map.putAll({'name' : 'test', 'phone': '123456789'})

PRINT(map) // { 'id' : 1, 'sex' : 0, 'name' : 'test', 'phone' : '123456789' }

map -: <'sex'> //map.remove('sex')

PRINT(map) // { 'id' : 1, 'name' : 'test', 'phone' : '123456789' }

map -: 1 //map.removeValue(1);

PRINT(map) // { 'name' : 'test', 'phone' : '123456789' }

map -: ['123456789'] //map.removeValues(['123456789']);

PRINT(map) // { 'name' : 'test' }

PRINT(map.name) // test

PRINT(map.'name') // test

//forbidden  PRINT(map.tag) // throw NotFoundException('could not find the key "tag" in map !')

PRINT(map.'tag') // null

//forbidden  map.tag : 'Java'

map.'tag' : 'Java'

PRINT(map.'tag') // Java
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
Map map : {
  'key0' : value0
  'key1' : value1
  'key2' : value2
}

map.forEach(
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

#### Short and repeatable variable or constant names in the same scop
```javascript
Int id
List<Int> id

getId() {
  return id@Int // @Int is a type annotation generated by IDE, highlight and uneditable, generate code when export
}
setIdList(List<Int> id) {
  this.id@List : id@List // @List is a type annotation generated by IDE, highlight and uneditable, generate code when export
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


#### No package
Auto set package for Axis files. And you don't need to write such code in Java
```java
package org.axis.lang;
```
And if the path of an Axis file changed(Myabe the file was moved to another folder),
you don't need to edit the code above.


#### No interface
replaced with Map and function in package level
```javascript
abstract refresh(List list)

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
  refresh(List list) {
    this.list : when (list =) {
      (null) : []
      () : List.OF(list : list)
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
