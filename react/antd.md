# Table

1、columns:表格列的配置描述（必选）

2、dataSource：数据数组（表格中行的数据来源）

3、rowKey：表格行 key 的取值，可以是字符串或一个函数

4、render：生成复杂数据的渲染函数，参数分别为当前行的值，当前行数据，行索引（回调函数的第一个参数由，dataIndex决定。如果没有写就是正行的数据，如果写了，就是指定的那一个）

5、bordered:决定表格是不是有边框

6、pagination：决定是不是分页显示

```
pagination={{ pageSize: 4, showQuickJumper: true }}
```

​	（1）pageSize：每页条数

​	（2）total：数据总数

​	（3）onChange：页码改变的回调，参数是改变后的页码及每页条数

​	（4）current:当前页数

7、width:直接写在列里面指定列的宽度

# Selected选择器

1、onChange：选中 option，或 input 的 value 变化（combobox 模式下）时，调用此函数

```
function(value, option:Option/Array<Option>)
```



### 	Option

# Message

1、message.success(content, [duration], onClose)：成功的信息提示

2、message.error(content, [duration], onClose)：错误的信息提示

# Card

1、title：左侧标题

2、extra：卡片右上角的操作区域

# Form

1、item左右两边宽度的设置

```
 const formItemLayout = {
      labelCol: { span: 2 },//设置左边lable的宽度，总共为24份
      wrapperCol: { span: 8 },
    };
<Form onSubmit={this.handleSubmit} {...formItemLayout}> //将前面规定的表单左边和右边的宽度应用上
<Item label="商品名称">label规定了右边部分的内容
```

2、Form.create()()：将一个组件包装为form组件

```
export default Form.create()(CategoryForm);//将CategoryForm包装为form组件
```

3、resetFields：重置一组输入控件的值（为 `initialValue`）与状态，如不传入参数，则重置所有组件。

4、getFieldDecorator：单个的表单验证

```
getFieldDecorator("roleName",{
            initialValue: "",
            rules: [{ required: true, message: "必须输入角色名称" }]
          })(<Input type="text" placeholder="请输入角色名称"></Input>)
```

5、validateFields:提交时的统一表单验证，回调参数为必填参数

```
 validateFields((err,roleName)=>{
      if(!err){
        const result
      }
    })
```



# upLoad

1、action：上传的地址，返回值是个Promise

2、name：发到后台的文件参数名，默认为file

3、fileList：已经上传的文件列表（受控）（状态里面的数组）

4、onPreview：点击文件链接或预览图标时的回调，控制是不是显示大图

5、onChange：文件状态改变的回调，接受三个参数

```
{
  file: { /* ... */ },
  fileList: [ /* ... */ ],
  event: { /* ... */ },
}
 （1）onChange对应的回调:
 handleChange = ({file,fileList}) => {//回调函数传过来一个对象，这里进行解构赋值，顺序可以变，对象											的属性名不可以变
      console.log(obj)
      this.setState({ fileList })
  };
  （2）file里面的对象这个对象里面有如下的属性，我们可以通过判断状态，把response里面的内容给他加到file属性上面，从而让他能够具有和原生file一样的属性，这里还要注意一点：// file与fileList中最后一个file代表同一个图片信息的对象(不是同一个)。通常需要进行如下的赋值：
      file = fileList[fileList.length-1]
  		file:对象的属性
  	{
   	uid: 'uid',      // 文件唯一标识，建议设置为负数，防止和内部产生的 id 冲突
   	name: 'xx.png'   // 文件名
   	status: 'done', // 状态有：uploading done error removed
   	response: '{"status": "success"}', // 服务端响应内容
   	linkProps: '{"download": "image"}', // 下载链接额外的 HTML 属性
			}
```

# Input输入框

1、disable：设置输入框是不是禁用状态，默认为false

# Modal

1、title:标题

2、visible：控制modal是不是可见

# Tree

1、defaultExpandAll：默认展开所有树节点

2、 checkedKeys：选中复选框的树节点

3、onCheck：点击复选框触发