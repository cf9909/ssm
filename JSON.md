Sublime Text格式化JSON快捷键：Ctrl + Alt + J

```
{
  "employees": [
    {
      "firstName": "John",
      "lastName": "Doe"
    },
    {
      "firstName": "Anna",
      "lastName": "Smith"
    },
    {
      "firstName": "Peter",
      "lastName": "Jones"
    }
  ]
}
对象 "employees" 是包含三个对象的数组。每个对象代表一条关于某人（有姓和名）的记录。
```

- **数据**使用名/值对表示。
- 使用大括号保存**对象**，每个名称后面跟着一个 ':'（冒号），名/值对使用 ,（逗号）分割。
- 使用方括号保存**数组**，数组值使用 ,（逗号）分割。

## 在 Java 中使用 JSON

JSON.parseObject，是将JSON字符串转化为相应的对象；

JSON.toJSONString(map)，则是将对象转化为JSON字符串。



JSON: **J**ava**S**cript **O**bject **N**otation.

JSON is a syntax for storing and exchanging data.

JSON is text, written with JavaScript object notation.



## Sending Data

If you have data stored in a JavaScript object, you can convert the object into JSON, and send it to a server

```js
var myObj = {name: "John", age: 31, city: "New York"};
var myJSON = JSON.stringify(myObj);
```



## Receiving Data

If you receive data in JSON format, you can convert it into a JavaScript object

```js
var myJSON = '{"name":"John", "age":31, "city":"New York"}';
var myObj = JSON.parse(myJSON);
```



## JSON Data - A Name and a Value

```json
Values in JSON 

{ "name":"John" }//Strings

{ "age":30 }//Numbers

{
"employee":{ "name":"John", "age":30, "city":"New York" }
}//objects

{
"employees":[ "John", "Anna", "Peter" ]
}//arrays

{ "sale":true }//booleans

{ "middlename":null }//null
```

The JSON format is almost identical to JavaScript objects.

In JSON, *keys* must be strings, written with double quotes:

In JavaScript, keys can be strings, numbers, or identifier names:

```js
JavaScript
{ name:"John" }
```



# JSON Objects

```json
{ "name":"John", "age":30, "car":null }

myObj = {
  "name":"John",
  "age":30,
  "cars": {
    "car1":"Ford",
    "car2":"BMW",
    "car3":"Fiat"
  }
 }//nested JSON objects
```

## Accessing Object Values

```js
myObj = { "name":"John", "age":30, "car":null };
x = myObj.name;

x = myObj["name"];
```

## Looping an Object

```js
for (x in myObj) {
  document.getElementById("demo").innerHTML += x;
}
```

## Modify Values

```js
myObj.cars.car2 = "Mercedes";

myObj.cars["car2"] = "Mercedes";
```



## Delete Object Properties

```js
delete myObj.cars.car2;
```

