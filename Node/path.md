## 一、path模块介绍

path模块是node当中的内置模块，主要是方便开发者对路径进行处理，

```
path的引入
import path from 'path';或者
const path = require('path');
```

## 二、path模块的一些常用方法

### 1、path.resolve（）

path.resolve():方法会把一个路径或路径片段的序列解析为一个绝对路径。

resolve把‘／’当成根目录找到根目录之后会对按照路径规则对后面的路径进行操作，如果没有找到根路径，就会把当前文件所在的跟路径找出来。如果参数中传入了多个根路径，那么就会以最后一个为准。具体可以看下面得到例子

```
const path1 = path.resolve('/a/b', '/c/d');
// 结果： /c/d。(两个跟路径输出最后一个)
const path2 = path.resolve('/a/b', '../c/d');
// 输出： /a/c/d   根路径为第一个参数，后面按照路径../出去是到了/a。然后找到/a下面的/c之后的/d
const path3=path.resolve('home','/foo/bar', '../baz')   
// 输出：'/foo/baz' 和第二个类似
const path4=path.resolve('home','./foo/bar', '../baz')  
 //  输出： '/home/foo/baz'   没有根路径一home作为根路径
```




