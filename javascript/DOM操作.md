**获取元素**

``` javascript
document.getElementById(id);

document.getElementsByTagName(tag);
```

 

**获取或设置属性**

``` javascript
element.getAttribute(attribute);

element.setAttribute(attribute,value);
```



**获取或设置****HTML****内容**

``` javascript
element.innerHTML;

element.innerHTML = "new html";
```

或

``` javascript
var h1 = document.createElement("h1");

element.appendChild(h1);
```

 

**添加文本**

``` javascript
var text = document.createTextNode("text");

element.appendChild(text);
```



**获取或设置CSS样式**

``` javascript
element.style.property;

element.style.property = value;
```



**给元素添加事件**

``` javascript
element.onclick = function() {};

element.onmouseover = function() {};

element.attachEvent("onclick", function() {});

element.attachEvent("onmouseover", function() {});
```



**IE8+****的方法**

 

**获取元素**

 ``` javascript
document.getElementsByClassName(classname);

document.querySelector("#demo");

document.querySelectorAll(".demo");
 ```



**获取或设置文本**

``` javascript
element.textContent;

element.textContent = "new text";
```



**绑定事件与解除绑定**

``` javascript
element.addEventListener("click", function() {});

element.removeEventListener("click", function() {});
```