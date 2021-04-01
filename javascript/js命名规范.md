**js命名规范**

ECMAScript 规范中标识符采用驼峰大小写格式，驼峰命名法由小(大)写字母开始，后续每个单词首字母都大写。根据首字母是否大写，分为两种方式：

1. Pascal Case	大驼峰式命名法：首字母大写。eg：StudentInfo、UserInfo、ProductInfo
2. Camel Case	小驼峰式命名法：首字母小写。eg：studentInfo、userInfo、productInfo

标识符，则包括变量、函数名、类名、属性名和函数或类的参数，每个命名方法又略有不同，下面详细解释一下：

**1.项目命名**

全部采用小写方式， 以下划线分隔。

示例：

```
my_project_name
```

**2.目录命名**

参照项目命名规则；有复数结构时，要采用复数命名法。

示例：

```
scripts, styles, images, data_models
```

**3.JS文件命名**

1. 变量：必须采用小驼峰式命名法。

命名规范：前缀应当是名词。(函数的名字前缀为动词，以此区分变量和函数)

命名建议：尽量在变量名字中体现所属类型，如:length、count等表示数字类型；而包含name、title表示为字符串类型。

``` javascript
// 好的命名方式
 let maxCount = 10;
 let tableTitle = 'LoginTable';
 // 不好的命名方式
 let setCount = 10;
 let getTitle = 'LoginTable';
```

1. 常量：必须采用**全大写**的命名，且**单词以_分割**，常量通常用于ajax请求url，和一些不会改变的数据

命名规范：使用大写字母和下划线来组合命名，下划线用以分割单词。

``` javascript
const MAX_COUNT = 10;
const URL = 'http://www.foreverz.com';
```

1. 函数

- 命名方法：小驼峰式命名法。
- 命名规范：前缀应当为动词。
- 命名建议：可使用常见动词约定

| **动词** | **含义**                     | **返回值**                                            |
| -------- | ---------------------------- | ----------------------------------------------------- |
| can      | 判断是否可执行某个动作(权限) | 函数返回一个布尔值。true：可执行；false：不可执行     |
| has      | 判断是否含有某个值           | 函数返回一个布尔值。true：含有此值；false：不含有此值 |
| is       | 判断是否为某个值             | 函数返回一个布尔值。true：为某个值；false：不为某个值 |
| get      | 获取某个值                   | 函数返回一个非布尔值                                  |
| set      | 设置某个值                   | 无返回值、返回是否设置成功或者返回链式对象            |
| load     | 加载某些数据                 | 无返回值或者返回是否加载完成的结果                    |

``` javascript
// 是否可阅读
function canRead(): boolean {
    return true;
}
// 获取名称
function getName(): string {
	return this.name;
}
```



1. 类 & 构造函数

命名方法：大驼峰式命名法，首字母大写。

命名规范：前缀为名称。

示例：

``` javascript
class Person {
  public name: string;
  constructor(name) {
   this.name = name;
  }
 }
 const person = new Person('mevyn');
```

1. 类的成员

类的成员包含：

- 公共属性和方法：跟变量和函数的命名一样。
- 私有属性和方法：前缀为_(下划线)，后面跟公共属性和方法一样的命名方式。

示例：

``` javascript
class Person {
  private _name: string;
  constructor() { }
  // 公共方法
  getName() {
   return this._name;
  }
  // 公共方法
  setName(name) {
   this._name = name;
  }
 }
 const person = new Person();
 person.setName('mervyn');
 person.getName(); // ->mervyn
```

