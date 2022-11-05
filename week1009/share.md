## ARTS - Share

## 关于ES6箭头函数的this

源于忽然兴起尝试微信小程序开发，在一个文本预上加了一个点击事件，尝试获取微信收货地址，
首先是在模板文件文本域加点击事件：

```

<view class="usermotto" bindtap='clickMe'>
    <text class="user-motto">{{motto}}</text>
  </view>

```

然后在index.js里实现 clickMe 方法：

```
clickMe: function () {
    wx.chooseAddress({
      success: function(res) {
        this.setData({
          motto: res.detailInfo
        })
      }
    })
   
  },

```

发现控制台报错：

```

thirdScriptError
Cannot read property 'setData' of null;at pages/index/index clickMe function;at api openAddress success callback function
TypeError: Cannot read property 'setData' of null
    at success (http://127.0.0.1:28587/appservice/pages/index/index.js:17:13)
    、    
```
 
根据报错说明，```'setData' of null````， 调用 setData方法是null? this 是null？ 为了验证，就打出this

```

clickMe: function () {
    console.log(1, this)
    wx.chooseAddress({
      success: function(res) {
        console.log(2, this)
        this.setData({
          motto: res.detailInfo
        })
      }
    })
   
  },
  
  
1 e {data: {…}, __viewData__: {…}, __displayReporter: e, …}
index.js? [sm]:16 

2 null

```
可以看出第一个this不是空，第二个是空，于是就修改了代码, 在clickMe方法定义常量self = this, 然后在回调方法使用， 结果是正常的：

```

clickMe: function () {
    console.log(1, this)
    const self = this
    wx.chooseAddress({
      success: function(res) {
        console.log(2, this)
        self.setData({
          motto: res.detailInfo
        })
      }
    })
   
  },

```

之后参考其他实现，发现是用箭头函数实现这一块，结果居然与上面一样，于是就猜测可能箭头函数实现this和匿名函数实现的this不一样：

```

clickMe: function () {
    
    wx.chooseAddress({
      success: res => {
        this.setData({
          motto: res.detailInfo
        })
      }
    })
   
  },

```

于是查资料：
https://kekek.cc/post/arrow-this.html [1]

### ES6箭头函数中的this
ES6 开始引入了箭头函数，让代码看起来更简洁了。但是箭头函数传统函数不仅仅是写法上的不同，在this和arguments的处理方式也有着很大的区别。
### this是局部的
在箭头函数中this是局部的，它跟其它局部变量的常规处理是一致的。看下面的例子：

```

function outer (age) {
    (() => {
        (() => {
            console.log(this.name, age);
        })();
    })();
}

outer.bind({name: '小明'})(10);

// => 小明 10

```

这例子中我们函数共有三层嵌套，里面是两层箭头函数，这里我们在最里层箭头函数内部引用了this和一个局部变量age。对局局部变量age的查找规则我们很清楚，先在当前作用域查找，如果没有就去父作用域查找，一直到最顶层（window、global）。在箭头函数中对this的查找规则和age是一样的，箭头函数本身并没有一个自己的this，它要一直往上层找，直到找到this为止。

同样，对于arguments的处理也是一样的。需要注意的一点是箭头函数本身是没有arguments变量的。[2]

---

[1] ES6箭头函数中的this https://kekek.cc/post/arrow-this.html 

[2] ES6 箭头函数中的 this？你可能想多了（翻译）
 https://www.cnblogs.com/vajoy/p/4902935.html
 