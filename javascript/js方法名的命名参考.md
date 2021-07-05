# js方法名的命名参考

### dom操作

| 单词   | 释义             | 说明                                                         |
| ------ | ---------------- | ------------------------------------------------------------ |
| append | 添加，添加到     | 将新元素加载到已有的dom中                                    |
| remove | 删除、移除       | 删除指定的dom元素                                            |
| empty  | 清空             | 清空已有的dom元素内的元素                                    |
| show   | 展示、显示       | 例如 showLoading()                                           |
| hide   | 隐藏             | 例如 hideLoading()                                           |
| init   | 初始化           | 通常用于将html中没有的元素第一次加载到页面中，例如 initMainTable() |
| render | 渲染、装裱、装潢 | 通常用于给已有的元素加载、渲染样式 renderSelect()            |
| reload | 重载             | 通常用于重新加载页面中已有的元素 reloadMainTable()           |

### 事件操作

| 单词    | 释义           | 说明                       |
| ------- | -------------- | -------------------------- |
| bind    | 绑定、绑定事件 | 例如 bindBtnClickEvent()   |
| unbind  | 解绑、解绑事件 | 例如 unbindBtnClickEvent() |
| trigger | 触发、调用     | 例如 triggerBtnClick()     |
| event   | 事件           | 例如 bindBtnClickEvent()   |

### 方法注释常见分类

| 单词   | 释义         | 说明             |
| ------ | ------------ | ---------------- |
| render | 渲染         | //[render]方法名 |
| util   | 功用         | //[util]方法名   |
| ajax   | 发送http请求 | //[ajax]方法名   |
| event  | 事件         | //[event]方法名  |

