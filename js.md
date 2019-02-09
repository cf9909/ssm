# JavaScript

## JavaScript Objects

1. ### Create an object:

   var car = {type:"Fiat", model:"500", color:"white"};

   The **name:values** pairs in JavaScript objects are called **Property:Property Value**



2. ### Accessing Object Properties

   *objectName.propertyName*

   or

   *objectName["propertyName"]*

3. ### Object Methods

   ```js
   var person = {
     firstName: "John",
     lastName : "Doe",
     id       : 5566,
     fullName : function() {
       return this.firstName + " " + this.lastName;
     }
   };
   ```

4. ### Accessing Object Methods

   *objectName.methodName()*



## JavaScript Arrays

1. ### Creating an Array

   var cars = ["Saab", "Volvo", "BMW"];

   var person = [];
   person[0] = "John";
   person[1] = "Doe";
   person[2] = 46;

2. ### Using the JavaScript Keyword new

   var cars = new Array("Saab", "Volvo", "BMW");

3. ### Access the Elements of an Array

   var name = cars[0];

4. ### Changing an Array Element

   cars[0] = "Opel";

5. ### Arrays are Objects

   Arrays use **numbers** to access its "elements".

6. ### Array Properties and Methods

   ```js
   var x = cars.length;   // The length property returns the number of elements
   var y = cars.sort();   // The sort() method sorts arrays
   ```

7. ### Looping Array Elements

   ```js
   for (i = 0; i < cars.length; i++) {
     text += "<li>" + cars[i] + "</li>";
   }
   ```

   ```js
   You can also use the Array.forEach() function:
   
   cars.forEach(function (value) {
       text += "<li>" + value + "</li>";
   });
   ```

8. ### Adding Array Elements

   ```js
   The easiest way to add a new element to an array is using the push() method:
   
   var fruits = ["Banana", "Orange", "Apple", "Mango"];
   fruits.push("Lemon");    // adds a new element (Lemon) to fruits
   ```

9. ### The Difference Between Arrays and Objects

   In JavaScript, **arrays** use **numbered indexes**.  

   In JavaScript, **objects** use **named indexes**.

10. ### Avoid new Array()

    ```js
    Use [] instead.
    
    These two different statements both create a new empty array named points:
    
    var points = new Array();     // Bad
    var points = [];              // Good
    ```



## Event

- click 事件

  当点击元素时，会发生 click 事件



- blur 事件

  当元素失去焦点时发生 blur 事件



- focus 事件

  当元素获得焦点时，发生 focus 事件



- change 事件

  当元素的值发生改变时，会发生 change 事件。

  该事件仅适用于文本域（text field），以及 textarea 和 select 元素。



- submit 事件

  当提交表单时，会发生 submit 事件



- 完整的 key press 过程分为两个部分：1. 按键被按下；2. 按键被松开。

  当按钮被按下时，发生 keydown 事件。

  当按键(键盘)被松开时，发生 keyup 事件。它发生在当前获得焦点的元素上。



- 当鼠标指针从元素上移开时，发生 mouseout 事件。

  当鼠标指针位于元素上方时，会发生 mouseover 事件。

  大多数时候这两个事件一起使用。



## Functions

- parseFloat() 函数可解析一个字符串，并返回一个浮点数。



- indexOf() 方法可返回某个指定的字符串值在字符串中首次出现的位置。

对大小写敏感！如果要检索的字符串值没有出现，则该方法返回 -1。