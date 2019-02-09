# jQuery

## Selectors

### attribute

- [attribute]

  可以根据元素的属性和属性值来选择元素

  ```css
  div[title]
  ```

- [attribute=value] 

  ```css
  [title="学号"]//要求必须和属性值完全匹配，属性值引号可以省略
  ```

- [attribute!=value]

- [attribute$=value]


### Basic

1. All Selector 

   ```css
   (“*”)
   ```

   Selects all elements.


2. Class Selector 

   ```css
   (“.class”)
   ```

   Selects all elements with the given class.



3. Element Selector

   ```css
    (“element”)
   ```

   Selects all elements with the given tag name.



4. ID Selector

   ```css
    (“#id”)
   ```

   Selects a single element with the given id attribute.



5. Multiple Selector 

   ```css
   (“selector1, selector2, selectorN”)
   ```

   Selects the combined results of all the specified selectors.

### 后代选择器

- 子选择器（直接儿子）

  ```css
  div > strong 
  ```

- 相邻兄弟选择器（第一个兄弟）

  ```css
  h3 + p
  ```

- 同级兄弟选择器

  ```css
  h3 ~ p 
  ```


## jQuery 选择器

| 选择器                                                       | 实例                       | 选取                                       |
| ------------------------------------------------------------ | -------------------------- | ------------------------------------------ |
| [*](http://www.w3school.com.cn/jquery/selector_all.asp)      | $("*")                     | 所有元素                                   |
| [#*id*](http://www.w3school.com.cn/jquery/selector_id.asp)   | $("#lastname")             | id="lastname" 的元素                       |
| [.*class*](http://www.w3school.com.cn/jquery/selector_class.asp) | $(".intro")                | 所有 class="intro" 的元素                  |
| [*element*](http://www.w3school.com.cn/jquery/selector_element.asp) | $("p")                     | 所有 <p> 元素                              |
| .*class*.*class*                                             | $(".intro.demo")           | 所有 class="intro" 且 class="demo" 的元素  |
|                                                              |                            |                                            |
| [:first](http://www.w3school.com.cn/jquery/selector_first.asp) | $("p:first")               | 第一个 <p> 元素                            |
| [:last](http://www.w3school.com.cn/jquery/selector_last.asp) | $("p:last")                | 最后一个 <p> 元素                          |
| [:even](http://www.w3school.com.cn/jquery/selector_even.asp) | $("tr:even")               | 所有偶数 <tr> 元素                         |
| [:odd](http://www.w3school.com.cn/jquery/selector_odd.asp)   | $("tr:odd")                | 所有奇数 <tr> 元素                         |
|                                                              |                            |                                            |
| [:eq(*index*)](http://www.w3school.com.cn/jquery/selector_eq.asp) | $("ul li:eq(3)")           | 列表中的第四个元素（index 从 0 开始）      |
| [:gt(*no*)](http://www.w3school.com.cn/jquery/selector_gt.asp) | $("ul li:gt(3)")           | 列出 index 大于 3 的元素                   |
| [:lt(*no*)](http://www.w3school.com.cn/jquery/selector_lt.asp) | $("ul li:lt(3)")           | 列出 index 小于 3 的元素                   |
| :not(*selector*)                                             | $("input:not(:empty)")     | 所有不为空的 input 元素                    |
|                                                              |                            |                                            |
| [:header](http://www.w3school.com.cn/jquery/selector_header.asp) | $(":header")               | 所有标题元素 <h1> - <h6>                   |
| [:animated](http://www.w3school.com.cn/jquery/selector_animated.asp) |                            | 所有动画元素                               |
|                                                              |                            |                                            |
| [:contains(*text*)](http://www.w3school.com.cn/jquery/selector_contains.asp) | $(":contains('W3School')") | 包含指定字符串的所有元素                   |
| [:empty](http://www.w3school.com.cn/jquery/selector_empty.asp) | $(":empty")                | 无子（元素）节点的所有元素                 |
| :hidden                                                      | $("p:hidden")              | 所有隐藏的 <p> 元素                        |
| [:visible](http://www.w3school.com.cn/jquery/selector_visible.asp) | $("table:visible")         | 所有可见的表格                             |
|                                                              |                            |                                            |
| s1,s2,s3                                                     | $("th,td,.intro")          | 所有带有匹配选择的元素                     |
|                                                              |                            |                                            |
| [[*attribute*\]](http://www.w3school.com.cn/jquery/selector_attribute.asp) | $("[href]")                | 所有带有 href 属性的元素                   |
| [[*attribute*=*value*\]](http://www.w3school.com.cn/jquery/selector_attribute_equal_value.asp) | $("[href='#']")            | 所有 href 属性的值等于 "#" 的元素          |
| [[*attribute*!=*value*\]](http://www.w3school.com.cn/jquery/selector_attribute_notequal_value.asp) | $("[href!='#']")           | 所有 href 属性的值不等于 "#" 的元素        |
| [[*attribute*$=*value*\]](http://www.w3school.com.cn/jquery/selector_attribute_end_value.asp) | $("[href$='.jpg']")        | 所有 href 属性的值包含以 ".jpg" 结尾的元素 |
|                                                              |                            |                                            |
| [:input](http://www.w3school.com.cn/jquery/selector_input.asp) | $(":input")                | 所有 <input> 元素                          |
| [:text](http://www.w3school.com.cn/jquery/selector_input_text.asp) | $(":text")                 | 所有 type="text" 的 <input> 元素           |
| [:password](http://www.w3school.com.cn/jquery/selector_input_password.asp) | $(":password")             | 所有 type="password" 的 <input> 元素       |
| [:radio](http://www.w3school.com.cn/jquery/selector_input_radio.asp) | $(":radio")                | 所有 type="radio" 的 <input> 元素          |
| [:checkbox](http://www.w3school.com.cn/jquery/selector_input_checkbox.asp) | $(":checkbox")             | 所有 type="checkbox" 的 <input> 元素       |
| [:submit](http://www.w3school.com.cn/jquery/selector_input_submit.asp) | $(":submit")               | 所有 type="submit" 的 <input> 元素         |
| [:reset](http://www.w3school.com.cn/jquery/selector_input_reset.asp) | $(":reset")                | 所有 type="reset" 的 <input> 元素          |
| [:button](http://www.w3school.com.cn/jquery/selector_input_button.asp) | $(":button")               | 所有 type="button" 的 <input> 元素         |
| [:image](http://www.w3school.com.cn/jquery/selector_input_image.asp) | $(":image")                | 所有 type="image" 的 <input> 元素          |
| [:file](http://www.w3school.com.cn/jquery/selector_input_file.asp) | $(":file")                 | 所有 type="file" 的 <input> 元素           |
|                                                              |                            |                                            |
| [:enabled](http://www.w3school.com.cn/jquery/selector_input_enabled.asp) | $(":enabled")              | 所有激活的 input 元素                      |
| [:disabled](http://www.w3school.com.cn/jquery/selector_input_disabled.asp) | $(":disabled")             | 所有禁用的 input 元素                      |
| [:selected](http://www.w3school.com.cn/jquery/selector_input_selected.asp) | $(":selected")             | 所有被选取的 input 元素                    |
| [:checked](http://www.w3school.com.cn/jquery/selector_input_checked.asp) | $(":checked")              | 所有被选中的 input 元素                    |



## 核心操作

### HTML  操作

1. 获取和设置内容和属性

   text() 返回或设置被选元素的文本内容。

   ```js
   $(selector).text()
   $(selector).text(content)
   ```



   html() 返回或设置被选元素的内容 (inner HTML)。

   ```js
   $(selector).html()
   $(selector).html(content)
   ```



   val() 返回或设置被选元素的值。

   ```js
   $(selector).val()
   $(selector).val(value)
   ```



   attr() 返回或设置被选元素的属性值。

   ```js
   $(selector).attr(attribute)
   $(selector).attr(attribute,value)
   ```

2. 添加和删除元素

   append() 方法在被选元素的结尾（仍然在内部）插入指定内容。

   prepend() 方法在被选元素的开头（仍位于内部）插入指定内容。

   after() 方法在被选元素后插入指定的内容。

   before() 方法在被选元素前插入指定的内容。

   remove() - 删除被选元素（及其子元素）：自身也删除

   empty() - 从被选元素中删除子元素：自身不删除（清空）

3. 获取和设置CSS

   ## 设置 CSS 属性

   ```js
   实例
   $("p").css("background-color","yellow");
   
   设置多个 CSS 属性实例
   $("p").css({"background-color":"yellow","font-size":"200%"});
   ```

4. 尺寸设置



## Traversing

### Ancestors

- parent() 方法返回被选元素的直接父元素。

- parents() 方法返回被选元素的所有祖先元素，它一路向上直到文档的根元素 (<html>)。

- parentsUntil() 方法返回介于两个给定元素之间的所有祖先元素。

### Descendants

- children() 方法返回被选元素的所有直接子元素。

- find() 方法返回被选元素的后代元素，一路向下直到最后一个后代。

### Siblings

- `siblings()` 返回被选元素的所有同胞元素。
- `next()` 返回被选元素的下一个同胞元素
- `prev() ` 
- `nextAll() ` 返回被选元素的所有跟随的同胞元素。
- `prevAll() ` 
- `nextUntil() ` 返回介于两个给定参数之间的所有跟随的同胞元素。
- `prevUntil() ` 

### Filtering

- first() 返回被选元素的首个元素。
- last() 返回被选元素的最后一个元素。
- eq() 返回被选元素中带有指定索引号的元素。索引号从 0 开始，因此首个元素的索引号是 0 而不是 1。
- filter() 允许您规定一个标准。不匹配这个标准的元素会被从集合中删除，匹配的元素会被返回。
- not() 返回不匹配标准的所有元素。

### each() 方法

```js
输出每个 li 元素的文本：

$("button").click(function(){
  $("li").each(function(){
    alert($(this).text())
  });
});
```



## prop

- prop( propertyName )

  Get the value of a property for the first element in the set of matched elements.

- prop( propertyName, value )

  Set one or more properties for the set of matched elements.



