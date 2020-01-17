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
only Any, Bool, Int, Num, Str, Map and List

#### Type hint
if NAME_AUTHOR, User and an instance for it was defined like this:
```
final Str NAME_AUTHOR : 'Lemon'
class User : {
  Int id
  Str name
}
User user : {}
```
then when user or its fields were called, the IDE will automatically generate the Type behind instances:
user`@User`.name`@Str` : NAME_AUTHOR`@Str`
user`@User` : User{ // user.equals(new User().setId(1).setName('tommy'))
  id@Int : 1 // setId(1)
  name@Str : 'tommy' // setName('tommy')
}

#### Safe type
```javascript
Int id : 0
Str name : null

id@Int.toStr() // '0'

name@Str.length@Int //won't throw NullPoninterExeption but return null
name@Str.toUpperCase() //won't throw NullPoninterExeption but return null 

User user = null
user@User.name@Str : name@Str //automatically create user and name in user when they are null and the expression is for assign
PRINT(msg : user@User.toStr()) //{ name : null }
```

#### Default value for arguments
such as 
```javascript
function(Type0 arg0, Type1 arg1 : null)
```
then you can call 
```javascript
function(*arg0 : *value0)
```
or 
```javascript
function(
  arg0@Type0 : value0
  arg1@Type1 : value1
)
```

#### Default and anonymous getter and setter functions for fields
such as
```javascript
Str name {
  @Override
  Str get() {
    return name@Str
  }

  @Override
  Str set(Str value) {
    name@Str : value
    return this@User
  }
} : null
```

private, protected or public fields have no getter or setter functions.

#### Expand fields
such as
```javascript
user@User.'isFriend' : true //isFriend is not a field decleared in User, so it must be covered with ''
LOG(
  tag@Str : User.class@Class.getSimpleName()
  msg@Str : 'id = ' + user@User.id@Int + '; isFriend = ' + user@User.'isFriend'
)
```

#### Package level funcions
such as
```javascript
package org.axis.api

abtract Bool isCorrect()

LOG(
  Str tag
  Str msg
) {
  ...
}
```

only suppor public abstract and public static functions.

#### Multiple extends and support Maps and functions
such as
```javascript
class User : Map, isCorrect { // public class User extends Map implements Interface$isCorrect {

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
  id@Int : 1 // setId(1)
  name@Str : 'tommy' // setName('tommy')
}
```

### '+', '-' between Lists
```javascript
List<Int> list : [1, 2, 3]

//forbidden  list +: 4 // list.add(4)

PRINT(msg : list@List<Int>) // [1, 2, 3, 4]

list@List<Int> +: [2, 5, 6] //list.addAll([2, 5, 6])

PRINT(msg : list@List<Int>) // [1, 2, 3, 4, 2, 5, 6]

list@List<Int> -: <0, 1> //list.remove(0);  list.remove(1);

PRINT(msg : list@List<Int>) // [3, 4, 2, 5, 6]

list@List<Int> -: [5] //list.remove([5]);

PRINT(msg : list@List<Int>) // [3, 4, 2, 6]

list@List<Int> -: 2 //list.remove((Map) 2)

PRINT(msg : list@List<Int>) // [3, 4, 6]

//forbidden  list -: {0, 2} //list.remove(0) list.remove(2)

PRINT(msg : list@List<Int>.0@Int) // 3

PRINT(msg : list@List<Int>.'0'@Int) // 3

PRINT(msg : list@List<Int>.3@Int) // throw IndexOutOfBoundsException('index : 3, list.length : 3, index >: list.length !')

PRINT(msg : list@List<Int>.'3'@Int) // null

//forbidden  PRINT(list.'a') //the index must be a number

list@List<Int>.4@Int : 4 // throw IndexOutOfBoundsException('index : 4, list.length : 3, index > list.length !')

list@List<Int>.'4'@Int : 4

PRINT(msg : list@List<Int>.'4'@Int) // 4

PRINT(msg : list@List<Int>) // [3, 4, 6, null, 4]
```

### '+', '-' between Maps
```javascript
Map map : {
  'id' : 1
  'sex' : 0
  'name' : null
}

map@Map +: {
  'name'  : 'test'
  'phone' : '123456789'
} // map.putAll({'name' : 'test', 'phone': '123456789'})

PRINT(msg : map@Map) // { 'id' : 1, 'sex' : 0, 'name' : 'test', 'phone' : '123456789' }

map@Map -: <'sex'> //map.remove('sex')

PRINT(msg : map@Map) // { 'id' : 1, 'name' : 'test', 'phone' : '123456789' }

map@Map -: 1 //map.removeValue(1);

PRINT(msg : map@Map) // { 'name' : 'test', 'phone' : '123456789' }

map@Map -: ['123456789'] //map.removeValues(['123456789']);

PRINT(msg : map@Map) // { 'name' : 'test' }

PRINT(msg : map@Map.name) // error, undefined filed name in map@Map!

PRINT(msg : map@Map.'name') // test

//forbidden  PRINT(map.tag) // throw NotFoundException('could not find the key "tag" in map !')

PRINT(msg : map@Map.'tag') // null

//forbidden  map.tag : 'Java'

map@Map.'tag' : 'Java'

PRINT(msg : map@Map.'tag') // Java
```


#### for each
List
```javascript
Str[] NAMES : [
  'name0'
  'name1'
  'name2'
]

NAMES.each(
  Int index
  Str item
) {
  LOG(
    tag@Str : 'FOR_EACH'
    msg@Str : item@Str
  )
}
```

Map
```javascript
Map map<Str, Any> : {
  'key0' : value0@Int
  'key1' : value1@Str
  'key2' : value2@List
}

mapMap<Str, Any>.each(
  Str key
  Any value
) {
  LOG(
    tag@Str : 'FOR_EACH'
    msg@Str : 'key = ' + key@Str + '; value = ' + value
  )
}
```

#### Assign value for final fields on any time
declare a Type 
```javascript
class User : Map {
  final Int id
  final Str name
}
```
then call
```javascript
User user : {
  id@Int : 1
  name@Str : null
}
```
or
```javascript
User user : {} // User user = new User();
user@User.sex@Int : 0 // user.setSex(0);
user@User.{
  id@Int : 1 // user.setId(1);
  name@Str : null // user.setName(null);
}
```

#### Short and repeatable variable or constant names in the same scope
```javascript
Int id
List<Int> id

getId() {
  return id@Int // @Int is a type annotation generated by IDE, highlight and uneditable, generate code when export
}
setIdList(List<Int> id) {
  this.id@List : id@List // @List is a type annotation generated by IDE, highlight and uneditable, generate code when export
}
getFromIdList(Int position) {
  return id@List.get(positoin@Int : position@Int)
}
```

If the type of a variable or a constant was changed, IDE will automatically change all the type annotations about it. <br />
For example, you edit 'List<Int> id' and change it to 'Map<Int, Int> id', <br />
then all codes of 'id@List' will automatically become 'id@Map'.
```javascript
Map<Int, Int> id

setIdList(List<Int> id) {
  this.id@Map : id@List // Error！Type mismatch between id@Map and id@List ！
}
getFromIdList(Int position) {
  return id@Map.get(key@Int : position@Int)
}
```

#### No 'static'
replaced with UPPER_CASE names. <br />
static class
```javascript
class Outter : Map {
  class INNER : Map {
  }
}
```
static funciton
```javascript
MAIN(Str[] args) {
}
```
static field
```javascript
final Str TAG : 'Axis'
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

Str callBack() {
  return 'Title'
}

call()
Str title : callBack()
```


#### No package
Auto set package for Axis files. And you don't need to write such code in Java
java
package org.axis.lang;

And if the path of an Axis file changed(Myabe the file was moved to another folder), <br />
you don't need to edit the code above.


#### Interface
replaced with Map and function in package level
```javascript
abstract refresh(List list)

interface AdapterViewCallback {
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
class BaseAdapter<T> : Adapter, AdapterViewCallback, refresh {
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
      type@Int : getItemViewType(position@Int : position@Int)
      itemView@ItemView : getItem(position@Int : position@Int)
      position@Int : position@Int
    )
  }
  
  @Override
  refresh(List<T> list) {
    this.list@List<T> : when (list@List<T> =) {
      (null) : []
      () : List.OF(list@Listable : list@List<T>)
    }
  }
}
```


#### If
replace if-else if-else, switch-case

if
```javascript
if a@Int = 1 {  // if
   ...
}
```

if-else
```javascript
if a@Int = 1 {  // if
   ...
}  {  // else
   ...
}
```

if-else-else if
```javascript
if
   a@Int = 1 {  // if
    ...
}  a@Int < 1 & a@Int != -1 {  // else if
    ...
}  { //else
    ...
}
```

if-else-else if
```javascript
if a@Int
   = 1 {  // if
   ...
}  < 1 {  // else if
   ...
}  {  //else
   ...
}
```

if-else-else if
```javascript
if a@Int =
   1 {  // if
   ...
}  2 | a@Int = 3 {  // else if
   ...
}  {  //else
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
  action@run : () { //type was a generated comment. No name means default name 'run'.
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
  action@Runnable : { //type was a generated comment. No name means default name 'Runnable'.
    run() {
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
