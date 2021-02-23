## 一、表单概述

表单（`<form>`）用来收集用户提交的数据，发送到服务器。比如，用户提交用户名和密码，让服务器验证，就要通过表单。表单提供多种控件，让开发者使用，具体的控件种类和用法请参考 HTML 语言的教程。本章主要介绍 JavaScript 与表单的交互。

```js
<form action="/handling-page" method="post">
  <div>
    <label for="name">用户名：</label>
    <input type="text" id="name" name="user_name" />
  </div>
  <div>
    <label for="passwd">密码：</label>
    <input type="password" id="passwd" name="user_passwd" />
  </div>
  <div>
    <input type="submit" id="submit" name="submit_button" value="提交" />
  </div>
</form>


//生成formdata对象的的时候，键名是控件的name属性，键值是控件的value属性，

```

## 二、FormData 对象

### 1、FormData概述

`FormData()`首先是一个构造函数，用来生成表单的实例。

```js
 let formData = new FormData();
      console.log('formData==',typeof formData )
// output:formData== object   从这里我们能够看出来formData从属于对象类型
/*FormData()构造函数的参数是一个 DOM 的表单元素，构造函数会自动处理表单的键值对。这个参数是可选的，如果省略该参数，就表示一个空的表单。比如上面我们就创造了一个空的表单
*/

    <label for="username">用户名：</label>
      <input type="text" id="username" name="username" value="这是一个春天">
    </div>
    <div>
      <label for="useracc">账号：</label>
      <input type="text" id="useracc" name="useracc">
    </div>
    <div>
      <label for="userfile">上传文件：</label>
      <input type="file" id="userfile" name="userfile">
    </div>
    <input type="submit" value="Submit!">
  </form>



var myForm = document.getElementById('myForm');
console.log('myForm',myForm)
var formData = new FormData(myForm);
console.log('formData',formData)

// 获取某个控件的值
formData.get('username') // ""
console.log(formData.get('username') )
//output: '这是一个春天'
// 设置某个控件的值
formData.set('username', '张三');

formData.get('username') // "张三"

//这里我们就在已有form表单基础上对form表单进行处理
```

### 2、FormData实例方法

（1）FormData.get(key)：获取指定键名对应的键值，参数为键名。如果有多个同名的键值对，则返回第一个键值对的键值。如下

```js
let formData = new FormData();
      formData.append('file', this.fileList[0].raw);
      console.log('formData',formData)
      console.log('formDataGet',formData.get('file'))
/*
输出为 ：
formDataGet File {uid: 1611904922450, name: "IP导入模板 (1).xlsx", lastModified: 1611904897426, lastModifiedDate: Fri Jan 29 2021 15:21:37 GMT+0800 (中国标准时间), webkitRelativePath: "", …}*/
```

（2）FormData.getAll(key)：返回一个数组，表示指定键名对应的所有键值。如果有多个同名的键值对，数组会包含所有的键值。

```js
   let formData = new FormData();
      formData.append('file', this.fileList[0].raw);
       formData.append('file', 1);
      console.log('formData',formData)
      console.log('formDataGet',formData.getAll('file'))
输出为 ：
/*formDataGet [File，1]*/ //把值放在了数组里面。
```

(3)FormData.set(key, value)：设置指定键名的键值，参数为键名。如果键名不存在，会添加这个键值对，否则会更新指定键名的键值。如果第二个参数是文件，还可以使用第三个参数，表示文件名。

```js
   let formData = new FormData();
       formData.append('file', 3);
      formData.set('file', this.fileList[0].raw);
      console.log('formData',formData)
      console.log('formDataGet',formData.getAll('file'))
输出为 ：
/*formDataGet [File]*/ //把值放在了数组里面。能够看到后面的值直接把老值覆盖了
```

(4)FormData.append(key, value)：添加一个键值对。如果键名重复，则会生成两个相同键名的键值对。如果第二个参数是文件，还可以使用第三个参数，表示文件名。

```js
   let formData = new FormData();
      formData.append('file', this.fileList[0].raw);
       formData.append('file', 1);
      console.log('formData',formData)
      console.log('formDataGet',formData.getAll('file'))
输出为 ：
/*formDataGet [File，1]*/ //把值放在了数组里面。能够看到这时候有两个值，（这里吧formData当做一个特殊对象去理解）
```

（5）FormData.delete(key)：删除键值对，参数为键名。

```js
   let formData = new FormData();
       formData.append('file', 3);
      formData.append('file', this.fileList[0].raw);
      console.log('formData',formData)
      console.log('formDataGet',formData.getAll('file'))
输出为 ：
/*formDataGet []*/ //会把所有的都删除空,不是只删除一个
```

（6）FormData.has(key)：返回一个布尔值，表示是否具有该键名的键值对。

```js
 let formData = new FormData();
       formData.append('file', 3);
      formData.append('file', this.fileList[0].raw);
  
      console.log('formDataGet',formData.has('file'))
输出为 ：
/*formDataGet true*/ 
```

(7)FormaData.keys()返回一个遍历器对象，用于for...of循环遍历所有的键名。

```js
  formData.append('file', 3);
      formData.append('file', this.fileList[0].raw);
  
     for (var key of formData.keys()) {
       console.log(key);
        }
输出为 ：
/*file file*/ //打印了两次，侧面证明了内部是用两个键存储的
```

（8）FormData.values()：返回一个遍历器对象，用于for...of循环遍历所有的键值。

```js
 let formData = new FormData();
       formData.append('file', 3);
      formData.append('file', 4);
  
     for (var value of formData.values()) {
       console.log(value);
        }
输出为 ：
/*3, 4*/
```

(9)FormData.entries()：返回一个遍历器对象，用于for...of循环遍历所有的键值对。如果直接用for...of循环遍历 FormData 实例，默认就会调用这个方法.

```js

   let formData = new FormData();
       formData.append('file', 3);
      formData.append('file', this.fileList[0].raw);
  
     for (var pair of formData.entries()) {
       console.log(pair);
        }
输出为 ：
/*(2) ["file", "3"]
(2) ["file", File]*/ //数组的形式输出
```

## 三、表单的enctype 属性

表单能够用四种编码，向服务器发送数据。编码格式由表单的`enctype`属性决定。

假定表单有两个字段，分别是`foo`和`baz`，其中`foo`字段的值等于`bar`，`baz`字段的值是一个分为两行的字符串。

### 1、get

如果表单使用`GET`方法发送数据，`enctype`属性无效，数据将以 URL 的查询字符串发出。

```js
?foo=bar&baz=The%20first%20line.%0AThe%20second%20line
```

### 2、application/x-www-form-urlencoded

如果表单用`POST`方法发送数据，并省略`enctype`属性，那么数据以`application/x-www-form-urlencoded`格式发送（因为这是默认值）

### 3、text/plain

如果表单使用`POST`方法发送数据，`enctype`属性为`text/plain`，那么数据将以纯文本格式发送。

### 4、multipart/form-data

如果表单使用`POST`方法，`enctype`属性为`multipart/form-data`，那么数据将以混合的格式发送。这种格式也是文件上传的格式。

## 四、文件上传

用户上传文件，也是通过表单。具体来说，就是通过文件输入框选择本地文件，提交表单的时候，浏览器就会把这个文件发送到服务器。

此外，还需要将表单`<form>`元素的`method`属性设为`POST`，`enctype`属性设为`multipart/form-data`。其中，`enctype`属性决定了 HTTP 头信息的`Content-Type`字段的值，默认情况下这个字段的值是`application/x-www-form-urlencoded`，但是文件上传的时候要改成`multipart/form-data`。

```js
<form method="post" enctype="multipart/form-data">
  <div>
    <label for="file">选择一个文件</label>
    <input type="file" id="file" name="myFile" multiple>
  </div>
  <div>
    <input type="submit" id="submit" name="submit_button" value="上传" />
  </div>
</form>
```

上面的 HTML 代码中，file 控件的`multiple`属性，指定可以一次选择多个文件；如果没有这个属性，则一次只能选择一个文件。