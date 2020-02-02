# Axis-Lang
A programming language running on JVM. <br />
Powerful, Flexible, Safe and Simple. <br />
[Designing...]


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
if NAME_AUTHOR, User and an instance of User was defined like this:
```
final Str NAME_AUTHOR : 'Lemon'
class User : {
  Int id
  Str name
  Any extra
}
User user : {}
```
then when user or its fields were called, the IDE will automatically generate the Type behind instances: <br />
&emsp;  user`@User`.name`@Str` : NAME_AUTHOR`@Str` <br />
&emsp;  user`@User` : { `// user = new User().setId(1).setName("tommy");` <br />
&emsp;&emsp;  id`@Int` : 1 `// user.setId(1);` <br />
&emsp;&emsp;  name`@Str` : 'tommy' `// user.setName("tommy");` <br />
&emsp;&emsp;  extra : null `//no hint for Any, don't need it.  user.setExtra(null);` <br />
&emsp;  } <br />


#### Safe type
```javascript
Int id : 0
Str name : null

id@Int.toStr()@Str // '0'

name@Str.length@Int //won't throw NullPoninterExeption but return null
name@Str.toUpperCase()@Str //won't throw NullPoninterExeption but return null 

User user = null
user@User.name@Str : name@Str //automatically create user and name in user when they are null and the expression is for assign
PRINT(msg : user@User) //{ name : null }
```


#### Indexes for arrays
Positive index:
```javascript
arr.0  // arr[0]
arr.1  // arr[1]

arr.-1  // arr[arr.length - 1]
// arr.-0  //Forbidden, throw UnsupportedSyntaxError
```
If the index is out of arr's bounds [0, arr.length - 1]，it will return null instead of throw IndexOutOfBoundsException. <br />
If you want to stop the process when the case above happen, then you can:
```javascript
if arr.length@Int <= index@Int {
  # IndexOutOfBoundsException(msg@Str : 'arr.length <= index! index is out of bounds!')
}
```


#### Default value for arguments
such as 
```javascript
function(Type0 arg0, Type1 arg1 : null) {
  ...
}
```
then you can call 
```javascript
function(arg0@Type0 : value0@Type0)
```
or 
```javascript
function(
  arg0@Type0 : value0@Type0
  arg1@Type1 : value1@Type1
)
```


#### Return
the keyword 'return' is replaced with '^' <br />
such as
```javascript
function() {
  ^  //return;
}

@NotNull
Any getNotNull(Any arg) {
  ^ arg = null ? {} ; arg  //return arg == null ? new Object() : arg;
}
```
If you write 'return', the IDE will recommend '^'.


#### Lambda
Lambda is an anonymous callback
```javascript
function()
  ^() {
  ^()  //callback.callback();
}
function()  //这里比较难判断是声明还是调用，所以回调函数的 括号 () 也不能省
  () {  //调用 function 时代码提示，一起自动生成
  //do something
}

getNotNullAync(Any in)
  ^(@NotNull Any out) {
  ^(out : in = null ? {} ; in)  //callback.callback(in == null ? new Object() : in);
}
getNotNullAync(in : null)
  (@NotNull Str out) {  //调用 getNotNullAync 时代码提示，一起自动生成
  //do something
}

Bool getNotNullAync(Any in)
  ^(@NotNull Any out) {
  ^(out : in = null ? {} ; in)  //callback.callback(in == null ? new Object() : in);
  ^ in != null  //return in != null;
}
Bool handled = getNotNullAync(in : null)
  (@NotNull Str out) {  //调用 getNotNullAync 时代码提示，一起自动生成
  //do something
}

```


#### Throw
the keyword 'throw' is replaced with '#' <br />
such as
```javascript
Any get(
  Listable list
  Int position
) # NullPoninterExeption, IndexOutOfBoundsException  {
  if list@Listable = null {
    # NullPoninterExeption(msg@Str : 'list can not be null!')  //throw new NullPoninterExeption("list can not be null!");
  }
  if position@Int = null {
    # NullPoninterExeption(msg@Str : 'position can not be null!')  //throw new NullPoninterExeption("position can not be null!");
  }
  if position@Int < 0 | position@Int >= list@Listable.length@Int {
    # IndexOutOfBoundsException(msg@Str : 'position is out of bounds of list!')  //throw new IndexOutOfBoundsException("position is out of bounds of list!");
  }

  ^ list@Listable.get(position@Int : position@Int)
}
```
If you write 'throw' or 'throws', the IDE will recommend '#'.


#### Break
the keyword 'break' is replaced with '>>' <br />
such as
```javascript
while true {  //replace  while(true) { ... }
  PRINT(msg : 'while...')
  >>  //break;
}
```
If you write 'break', the IDE will recommend '>>'.


#### Continue
the keyword 'continue' is replaced with '<<' <br />
such as
```javascript
until false {  //replace  do {...} while(...);
  PRINT(msg : 'do while...')
  Thread.sleep(time@Int : 1000)
  <<  //continue;
}
```
If you write 'continue', the IDE will recommend '<<'.


#### Default and anonymous getter and setter functions for fields
such as
```javascript
Str name {
  @Override
  Str get() {
    ^ name@Str  //return name;
  }

  @Override
  Str set(Str value) {
    name@Str : value
    ^ this@User  //return this;
  }
} : null
```

private, protected or public fields have no getter or setter functions.


#### Expand fields
such as
```javascript
user@User.'isFriend' : true //isFriend is not a field decleared in User, so it must be covered with ''
LOG(
  tag@Str : User.class@Class.getSimpleName()@Str
  msg@Str : 'id = ' + user@User.id@Int + '; isFriend = ' + user@User.'isFriend'
)
```


#### Package level funcions
such as
```javascript
package axis.api

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
    ^ true
  }
}
```

the first one must be an Map Type, and the after Map Type can only supply CONSTANS and abstract functions.


#### '=' equal for any Types
```javascript
b@Bool = true // b.equals(true)
i@Int = 0 // i.equals(0)
s@Str = '' // s.equals('')
map@Map = {} // map.equals({})
list@List = [] // list.equals([])
user@User = User{} // user.equals(new User())
user@User = User{ // user.equals(new User().setId(1).setName('tommy'))
  id@Int : 1 // setId(1)
  name@Str : 'tommy' // setName('tommy')
}
```


### '+', '-' between Lists
```javascript
List<Int> list : [
  1
  2
  3
]

list +: 4 // list.add(4)

PRINT(msg : list@List<Int>) // [1, 2, 3, 4]

list@List<Int> +: [
  2
  5
  6
] //list.addAll([2, 5, 6])

PRINT(msg : list@List<Int>) // [1, 2, 3, 4, 2, 5, 6]

list@List<Int> -: <0> //list.remove(0);

PRINT(msg : list@List<Int>) // [3, 4, 2, 5, 6]

list@List<Int> -: [
  0
  5
] //list.remove((Object) 0);  list.remove((Object) 5);

PRINT(msg : list@List<Int>) // [3, 4, 2, 6]

list@List<Int> -: 2 //list.remove((Object) 2)

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
  ^ id@Int // @Int is a type annotation generated by IDE, highlight and uneditable, generate code when export
}
setIdList(List<Int> id) {
  this.id@List : id@List // @List is a type annotation generated by IDE, highlight and uneditable, generate code when export
}
getFromIdList(Int position) {
  ^ id@List.get(positoin@Int : position@Int)
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
  ^ id@Map.get(key@Int : position@Int)
}
```
#### No Comma ',' and no semicolon ';'
replaced with escape character in many cases such as field and function declearations, value initiations, function calls. <br />
```javascript
Str name : 'Axis'
Int version : 1
Map extra : {
  'type' : 'Language'
  'for' : 'Programming'
}
  
LOG(
  Str tag
  Str msg
) {
  ...
}

LOG(
  tag@Str : 'Axis'
  msg@Str : 'is a much better programming language than Java running on JVM.'
)
```

  
#### No 'static'
replaced with UPPER_CASE names. <br />
static class
```javascript
class Outter : Map {  //public class Outter extends Map {
  class INNER : Map {  //public static class INNER extends Map {
  }
}
```
static funciton
```javascript
MAIN(Str[] args : null) {  //public static void main(Str[] args) {
}
```
static field
```javascript
final Str TAG : 'Axis'  //public static final String TAG = "Axis";
```


#### No 'new' and no constructor
replaced with Type{}, such as
```javascript
User user : User{}  //User user = new User();
```


#### No 'void' for functions
```javascript
call() {  //public void call() {
}

Str callBack() {  //public String cacallBackll() {
  ^ 'Title'
}

call()  //call();
Str title : callBack()  //String title = callBack();
```


#### No package
Auto set package for Axis files. And you don't need to write such code in Java
java
package org.axis.lang;

And if the path of an Axis file changed(Myabe the file was moved to another folder), <br />
you don't need to edit the code above.


#### No varargs
in Java, sometimes you need to use varargs to reduce codes like: <br />
declear:
```java
public Object invoke(Object obj, Object... args) throws IllegalAccessException, IllegalArgumentException, InvocationTargetException {
  ...
}
```
then call:
```java
invoke(info, 1, 'Aixs', null);
```

while varargs is replaced with \[...] in Axis: <br />
declear:
```javascript
Any invoke(
  Any obj
  List args
) # IllegalAccessException, IllegalArgumentException, InvocationTargetException {
  ...
}
```
you can call:
```javascript
invoke(
  obj : info@Info
  args@List : [
    1
    'Aixs'
    null
  ]
)
```


#### Interface
1.Define an interface:
```javascript
interface Runnable {
  run()
}
```
2.Define a method with argument, and the type of the argument is a interface above:
```javascript
runOnUiThread(Runnable action) {
  ...
}
```
3.Then you can call:
```javascript
runOnUiThread(
  action@Runnable : { //type was a generated comment. No name means default name 'Runnable'.
    run() {
      ...
    }
  }
)
```

Another expample:
```javascript
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
can be replaced function in package level
```javascript
abstract refresh(List list)
```

```javascript
class BaseAdapter<T> : Adapter, AdapterViewCallback, refresh {
  override View createView(  //override is a keyword generated by IDE，you don't need and aren't allowed to write it
    Int type
    ViewGroup parent
  ) {
    ^ DemoView{}
  }
  
  override bindView(  //override is a keyword generated by IDE，you don't need and aren't allowed to write it
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
  
  override refresh(List<T> list) {  //override is a keyword generated by IDE，you don't need and aren't allowed to write it
    if list@List<T> = null {
       this.list@List<T> : []
    }  {
      this.list@List<T> : list
    }
  }

}
```


#### If
replace if-else if-else, switch-case

if
```javascript
if a = 1 {  // if
   ...
}
```

if-else
```javascript
if a = 1 {  // if
   ...
}  {  // else
   ...
}
```

if-else-else if
```javascript
if a =
   1 {  // if
   ...
}  2 | a = 3 {  // else if
   ...
}  {  // else
   ...
}
```

if-else-else if
```javascript
if a
   = 1 {  // if
   ...
}  < 1 {  // else if
   ...
}  {  // else
   ...
}
```

if-else-else if
```javascript
if
   a = 1 {  // if
    ...
}  a < 1 & a != -1 {  // else if
    ...
}  { // else
    ...
}
```


#### Instanceof
replaced with 'is'
```javascript
true is Bool  //true
0 is Str  //false
```
If you write 'instanceof', the IDE will recommend 'is'.


#### Synchonized
replaced with 'sync'
```javascript
sync(this) {
  //dosomething
}

sync(Any.class) {
  //dosomething
}
```
If you write 'synchonized', the IDE will recommend 'sync'.


#### Callback

##### Abstract Method
1.Define an abstract method:
```javascript
run()
```
2.Define a method with argument, and the type of the argument is a abstract method above:
```javascript
runOnUiThread(run action) {
  ...
}
```
3.Then you can call:
```javascript
runOnUiThread(
  action@run : () {  // type was a generated comment. No name means default name 'run'.
    ...
  }
)
```


#### Thread
childThread:
```javascript
childThread {  // in a automatically recycler thread pool
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
