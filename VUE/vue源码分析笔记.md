# 一、Vue的数据代理

vue是通过数据代理的方式，让我们可以直接在vm实例对象上面，直接访问data里面的属性的。

```
const vm = new MVVM({
      el: '#test',
      data: {
        name: 'atguigu2'
      }
    })
    类似这种代码，通常来说，我们如果想要访问data里面的name属性，则需需要通过this.data.name来访问，但是我们能够看到，我们并没有采用这种形式，而是直接使用this.name的形式来读取或者是修改name的属性值，这里面用到的就是数据代理。具体的代码实现如下：

function MVVM(options) {
    this.$options = options;
    //（1）这里能够看到，当我们传入一个配置对象的时候，这个函数会取出里面的data并且把data对象，保存在this._data上面。
    var data = this._data = this.$options.data;
    //（2）将data中的所有属性给他遍历出来
    Object.keys(data).forEach(function(key) {
        （3）调用实现属性代理的函数
        me._proxy(key);
    });
	（4）代理的具体实现，其传入了一个key。通过（3）我们知道，这个key就是data对象里面的每一个属性，其把这个data里面的每一个属性，都添加成了实例的属性，注意：这里给实例添加属性的方式，是通过描述符的形式取定义的，this上面添加的属性，其实读取的还是this_data里面对应的属性值，这里是通过getter函数和setter函数进行设置的。
    _proxy: function(key) {
        var me = this;
        Object.defineProperty(me, key, {
            configurable: false,
            enumerable: true,
            get: function proxyGetter() {
                return me._data[key];
            },
            set: function proxySetter(newVal) {
                me._data[key] = newVal;
            }
        });
    }
};
```

# 二、vue源码的模板解析部分

vue中的模板解析指令主要是通过函数compile来实现。下面的过程论述了compile函数式如何解析模板的

```
在compil函数中，其做的第一步就是根据el选择器器把它放在fragment中，首先做的是将这个节点保存在compile大的实例对象上面。保存好了之后其对el对象的处理先放在fragment中，然后进行处理，将他们放在fragment容器中是采用node2Fragement()这个函数
function Compile(el) {
    this.$el = this.isElementNode(el) ? el : document.querySelector(el);
    if (this.$el) {
        this.$fragment = this.node2Fragment(this.$el);
        this.init();
        this.$el.appendChild(this.$fragment);
    }
}
Compile.prototype = {
	init: function() { this.compileElement(this.$fragment); },
    node2Fragment: function(el) {
        var fragment = document.createDocumentFragment(), child;
        // 将原生节点拷贝到fragment
        while (child = el.firstChild) {
            fragment.appendChild(child);
        }
        return fragment;
    }
};
```

# 三、Vue的数据绑定



进行数据绑定的第一步就是监视数据的变化，在vue当中，对于数据的监视都是通过observe函数进行的。

```
// 对data中所有层次属性进行监视/劫持
    observe(data, this);
  //上一步执行之后，在这里执行的是new Observe
  function observe(value, vm) {
    if (!value || typeof value !== 'object') {
        return;
    }

    // 创建对应的监视器对象
    return new Observer(value);
};
//new Observe的代码会执行构造函数
function Observer(data) {
    // 保存data对象
    this.data = data;
    // 开启监视流程
    this.walk(data);
}

Observer.prototype = {
    walk: function(data) {
        var me = this;
        // 遍历data中每个属性
        Object.keys(data).forEach(function(key) {
            // 给data重新定义响应式属性
            me.defineReactive(data, key, data[key]);
        });
    },

    defineReactive: function(data, key, val) {
        // 创建一个对应的dep对象(订阅器)
        var dep = new Dep();
        // 通过递归调用实现所有层次属性的监视
        var childObj = observe(val);

        // 重新定义属性
        Object.defineProperty(data, key, {
            enumerable: true, // 可枚举
            configurable: false, // 不能再define
            get: function() { // 建立dep与watcher之间的关系
                if (Dep.target) {
                    dep.depend();
                }
                return val;
            },
            set: function(newVal) { // 监视数据变化, 去更新界面
                if (newVal === val) {
                    return;
                }
                val = newVal;
                // 如果新的值是object的话，对内部属性进行监听
                childObj = observe(newVal);
                // 通过dep订阅器通知所有watcher订阅者
                dep.notify();
            }
        });
    }
};
//从这里我们能够看出来，dep.notify()所做的就是通知所有的订阅者watcher。那么问题来了，watch什么时候出现的

Dep.prototype = {
    addSub: function(sub) {
        this.subs.push(sub);
    },

    depend: function() {
        Dep.target.addDep(this);
    },

    removeSub: function(sub) {
        var index = this.subs.indexOf(sub);
        if (index != -1) {
            this.subs.splice(index, 1);
        }
    },

    notify: function() {
        // 遍历每个订阅者watcher
        this.subs.forEach(function(sub) {
            sub.update();
        });
    }
};
在compile文件中我们能够看到，watcher是在编译模板的时候就创建了
    bind: function(node, vm, exp, dir) {
        // 根据指令名得到对应的更新节点的函数
        var updaterFn = updater[dir + 'Updater'];
        // 执行更新函数去更新节点(第一次, ==> 初始化显示)
        updaterFn && updaterFn(node, this._getVMVal(vm, exp));
        // 创建用于更新节点的watcher对象
        new Watcher(vm, exp, function(value, oldValue) {
            // 更新对应的节点
            updaterFn && updaterFn(node, value, oldValue);
        });
    },
在mvvm.js中我们能能够看到是deeper创建的时候watcher还没有创建，因此初始化的时候watcher与deeper并没有建立联系，也就是说，这两者的关系是在模板编译之后才建立的
/* 
相当于Vue的构造函数
options: 配置对象
*/
function MVVM(options) {
    // 将配置对象保存到vm上
    this.$options = options;
    // 将data对象保存到vm和变量data上
    var data = this._data = this.$options.data;
    // 将vm保存到变量me上
    var me = this;
    // 遍历data中所有属性
    Object.keys(data).forEach(function(key) {
        // 对指定属性实现数据代理
        me._proxy(key);
    });

    // 对data中所有层次属性进行监视/劫持
    observe(data, this);  //这里已经有了deeper

    // 创建编译对象
    this.$compile = new Compile(options.el || document.body, this) //这里才可能有watcher
}
//在watcher.js文件中，我们能够看到watcher实例存在了dep函数的target属性上面
function Watcher(vm, exp, cb) {
    this.cb = cb;
    this.vm = vm;
    this.exp = exp;
    this.depIds = {}; // 保存n个对应的dep对象, 属性值为dep对象, 属性名为id值
    this.value = this.get();
}

Watcher.prototype = {
    update: function() {
        this.run();
    },
    run: function() {
        var value = this.get();
        var oldVal = this.value;
        if (value !== oldVal) {
            this.value = value;
            // 调用用于更新节点的回调函数
            this.cb.call(this.vm, value, oldVal);
        }
    },
    addDep: function(dep) {
        // 如果关系还没有建立过
        if (!this.depIds.hasOwnProperty(dep.id)) {
            // 建立dep到watcher的关系
            dep.addSub(this);
            // 建立watcher到dep的关系
            this.depIds[dep.id] = dep;
        }
    },
    get: function() {
        Dep.target = this;    把wathcer实例存在了dep函数的target属性上面
        var value = this.getVMVal();
        Dep.target = null;
        return value;
    },

    getVMVal: function() {
        var exp = this.exp.split('.');
        var val = this.vm._data;
        exp.forEach(function(k) {
            val = val[k];
        });
        return val;
    }
};
//再次返回observe.js注意看下面的代码
        Object.defineProperty(data, key, {
            enumerable: true, // 可枚举
            configurable: false, // 不能再define
            get: function() { // 建立dep与watcher之间的关系
                if (Dep.target) {
                    dep.depend();
                }
                return val;
            },
            set: function(newVal) { // 监视数据变化, 去更新界面
                if (newVal === val) {
                    return;
                }
                val = newVal;
                // 如果新的值是object的话，对内部属性进行监听
                childObj = observe(newVal);
                // 通过dep订阅器通知所有watcher订阅者
                dep.notify();
            }
        });
    }
};

Dep.prototype = {
    addSub: function(sub) {
        this.subs.push(sub);
    },

    depend: function() {
        Dep.target.addDep(this);  这时候，dep.target已经变成了watcher
    },

    removeSub: function(sub) {
        var index = this.subs.indexOf(sub);
        if (index != -1) {
            this.subs.splice(index, 1);
        }
    },

    notify: function() {
        // 遍历每个订阅者watcher
        this.subs.forEach(function(sub) {
            sub.update();
        });
    }
};

Dep.target = null;
//返回watcher.js

    addDep: function(dep) {
        // 如果关系还没有建立过
        if (!this.depIds.hasOwnProperty(dep.id)) {
            // 建立dep到watcher的关系
            dep.addSub(this);    两者之间正式建立了练习 
            // 建立watcher到dep的关系
            this.depIds[dep.id] = dep;
        }
    },
    
    
    我们记得前面两者如果建立了联系之后，只要一set也就是数据发生了变化就会调用dep.notify我们再返回看看obeserve.js部分
    function Dep() {
    this.id = uid++;
    this.subs = [];
}

Dep.prototype = {
    addSub: function(sub) {
        this.subs.push(sub);
    },

    depend: function() {
        Dep.target.addDep(this);
    },

    removeSub: function(sub) {
        var index = this.subs.indexOf(sub);
        if (index != -1) {
            this.subs.splice(index, 1);
        }
    },

    notify: function() {
        // 遍历每个订阅者watcher
        this.subs.forEach(function(sub) {
            sub.update();  这时候里面的每个sub就是watcher，我们到watcher.js中看看，这个updata更新函数做了什么
        });
    }
};

Dep.target = null;

返回watcher.js
Watcher.prototype = {
    update: function() {
        this.run();
    },
    run: function() {
        var value = this.get();
        var oldVal = this.value;
        if (value !== oldVal) {
            this.value = value;
            // 调用用于更新节点的回调函数
            this.cb.call(this.vm, value, oldVal);  这个回调函数需要返回compil.js看，就是new watcher的时候，传入的那个回调函数。这个时候我们可以回头看一下
        }
    },
    addDep: function(dep) {
        // 如果关系还没有建立过
        if (!this.depIds.hasOwnProperty(dep.id)) {
            // 建立dep到watcher的关系
            dep.addSub(this);
            // 建立watcher到dep的关系
            this.depIds[dep.id] = dep;
        }
    },
    get: function() {
        Dep.target = this;
        var value = this.getVMVal();
        Dep.target = null;
        return value;
    },

    getVMVal: function() {
        var exp = this.exp.split('.');
        var val = this.vm._data;
        exp.forEach(function(k) {
            val = val[k];
        });
        return val;
    }
};

//再次返回compile.js这个文件，找到创建watcher创建的部分
  bind: function(node, vm, exp, dir) {
        // 根据指令名得到对应的更新节点的函数
        var updaterFn = updater[dir + 'Updater'];
        // 执行更新函数去更新节点(第一次, ==> 初始化显示)
        updaterFn && updaterFn(node, this._getVMVal(vm, exp));
        // 8.我们看到了这个回调函数长什么样子
        new Watcher(vm, exp, function(value, oldValue) {
            //9.回掉函数，调用了updateFn方法，从这个bind函数的第二行可以看出，就是updater系列方法，这三个参数，node没有传所以会沿着作用域链查找，所以直接找到了bind的node参数，形成了闭包，所以就可以直接更新节点了
            updaterFn && updaterFn(node, value, oldValue);
        });
    },
        
    /* 
*/
var updater = {
    /* 更新节点的textContent属性 */
    textUpdater: function(node, value) {
        node.textContent = typeof value == 'undefined' ? '' : value;
    },
    
    /* 更新节点的innerHTML属性 */
    htmlUpdater: function(node, value) {
        node.innerHTML = typeof value == 'undefined' ? '' : value;
    },
    
    /* 更新节点的className属性 */
    classUpdater: function(node, value, oldValue) {
        // 得到静态类名
        var className = node.className;
        // 将静态类名拼接到动态类名, 指定为新的类名
        node.className = className ? className + ' ' + value : value
    },
    
    /* 更新节点的value属性 */
    modelUpdater: function(node, value, oldValue) {
        node.value = typeof value == 'undefined' ? '' : value;
    }
};
```

