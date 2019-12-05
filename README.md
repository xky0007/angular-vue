# angular-vue
（施工中）
---
**学习Vue过程中，记下与Angular相比较的笔记**

### 开发环境
---
```
VS Code
Win-10 v1903
NodeJs: v10.14.1
npm: 6.4.1
angular/cli: 8.3.20
rxjs: 6.4.0
```

### 安装
---
#### Angular:

Angular的起源这里只简单的介绍一下，有兴趣的可以自行搜索。

AngularJs诞生于2009年，起先也是类似于现在VueJs一样的，只需要html页面中导入相应的script库，即可使用，有兴趣的可以稍微看看我的[AngularJS-1.6.6-Tutorial](https://github.com/xky0007/AngularJS-1.6.6-Tutorial#angularjs-166-tutorial)稍作了解。

从Angular2开始，Angular变成了一个完整的前端框架且主要使用TypeScript。之后为了与之前的AngularJs区分并且对Angular本身进行了很大的改变，跨过了Angular3，直接到了Angular4，并且计划每半年发布一次新版本。至今已是Angular8 （Angular9照计划应该已经推出，但是每次都会晚一些）。

1. 安装Angular需要首先安装NodeJs和npm，搜索一下nodejs即可找到安装包，windows下npm也会随着nodejs安装，安装完成后使用```node --version```和```npm --version```检查是否安装成功。
2. 打开终端（cmd,powershell,windows terminal都可以)，运行```npm install -g @angular/cli```，等待安装结束
3. 运行```ng --version```查看是否安装成功

#### vuejs

只有一步，导入```<script src="https://cdn.jsdelivr.net/npm/vue"></script>```即可。

### 使用
---
由于本人之前只用过Angular，vue的经验不多，具目前的使用来看大概分为几个部分，我将一一对比Angular和vuejs的代码。所有的代码我都会放在stackblitz: [vuejs](https://stackblitz.com/edit/js-ducfaz)和[angular](https://stackblitz.com/edit/angular-gswt3k)

1. 插值
2. 双向绑定
3. 组件事件
4. 根据传入值动态变化css
5. 条件、循环调用组件和组件复用
6. 将值传入子组件
7. 子组件事件上浮至父组件

#### 1. 插值

##### Vuejs:

index.html
```
<body>
	<div id="app">
		{{message}}
	</div>
</body>
```

index.js
```
// Import stylesheets
import './style.css';
import Vue from 'vue'

// Write Javascript code!
var vm = new Vue({
  el:'#app', //绑定到id为app的元素，不可使用body
  data:function(){ //插入值变量
    return{
      message:'hello world'
    }
  }
})
```
即可得到

![插值](./images/vue-插值.jpg)

##### angular

在stackblitz里可以直接创建一个空的angular项目，也可以本地使用ng new app 创建一个本地项目，这里推荐在stackblitz因为既快又方便。

app.component.ts //等价于index.js，使用ts编写
```
import { Component } from '@angular/core';

@Component({
  selector: 'my-app',
  templateUrl: './app.component.html',
  styleUrls: [ './app.component.css' ]
})
export class AppComponent  {
  message = 'hello world';
}
```

app.component.html
```
{{message}}
```
这里可能会奇怪为什么没有完整的body和其他的tag，原因在于angular已经帮你绑定好了，你会看到项目内存在一个index.html的文件，如果打开你会看到一行代码`<my-app>loading</my-app>`。这里的`my-app`与`app.component.ts`里`selector: 'my-app'`是一致的，而`my-app`就相当于vue中的根元素`<div id="app"> </div>`。

如果运行程序可以看到相同的结果

![angular-插值](./images/angular-插值.jpg)


#### 2. 双向绑定

在mvvm中，双向绑定是一个非常方便且很有必要的功能，在纯js中，如果我们有两个input让用户来输入他的邮箱和密码，再点击登录按钮登录，需要使用类似的代码
```
var email = document.getElementById('email').val();
var password = document.getElementById('password').val();

//code to send email & password 
```

而在angular和vue中，如果我们使用了双向绑定，变量会自动得到当前的值而非手动

##### vue

首先，我们需要在index.js中的data设一个变量，如果没有默认值可以设为空值`email:'x@zx.com'`

第二步，在index.html中添加如下代码
```
<!-- 双向绑定 -->
<hr/>
<div>
    <h4>双向绑定</h4>
    <input type="" v-model="email"/>
    <div>最新的email为：{{email}}</div>
</div>
```
![vue-双向绑定](./gif/vue-双向绑定.gif)

##### angular

在angular中，第一次双向绑定会比vue多一步，而之后则是一样的先设变量再绑定。这是因为angular中的模块较多，需要手动添加需要的模块。

1. 在app.module.ts中导入`import { FormsModule } from '@angular/forms';`，再在@NgModule里的imports中添加`FormsModule`
2. 添加email变量
3. 使用[(ngModel)]绑定

修改后的app.component.html代码如下
```
<hr />
<div>
	<h4>双向绑定</h4>
	<input type="" [(ngModel)]="email"/>
	<div>最新的email为：{{email}}</div>
</div>
```
可以发现代码几乎完全一样，唯一的却别在于`[(ngModel)]="email"`和`v-model="email"`。

#### 事件

与双相绑定一样，angular和vue的事件绑定也非常类似。

##### vue
1. 在Vue对象中添加新的field-methods，在methods中定义方法。修改后的js如下：
    ```
    var vm = new Vue({
    el:'#app',
    data:function(){
        return{
        message:'hello world',
        email:'x@zx.com',
        count: 0
        }
    },
    methods:{
        addOne:function(){
        this.count+=1;
        }
    }
    })
    ```
2. 添加对应的html并使用`v-on:click="addOne"`将按钮与事件绑定
    ```
    <!-- 事件 -->
    <hr/>
    <div>
      <h4>事件</h4>
      <button v-on:click="addOne">加1</button>
      <div>最新的count为：{{count}}</div>
    </div>
    ```
效果如下：

![vue-事件](./gif/vue-事件.gif)

##### angular

1. 在html文件中添加如下代码
    ```
    <!-- 事件 -->
    <hr/>
    <div>
    <h4>事件</h4>
    <button (click)="addOne()">加1</button>
    <div>最新的count为：{{count}}</div>
    </div>
    ```
2. 在ts文件中添加count变量并添加函数，修改后的`AppComponent` class如下：
    ```
    export class AppComponent  {
        message = 'hello world';
        email:string = 'x@zx.com';
        count:number = 0;

        addOne(){
            this.count++;
        }
    }
    ```
即可得到相同的效果，主要区别在于vue使用`v-on:click="addOne"`而angular使用`(click)="addOne()"`，*注意angular中的函数括号不可省略*

#### 条件、循环调用组件和组件复用

首先来看条件，从前面的插值、双向绑定和事件，能看出来vue和angular是非常相似的，这一点在条件和循环调用也是一样的。

##### vue

1. 在html文件中添加如下代码：
    ```
    <!-- 条件 -->
    <hr/>
    <div>
      <h4>条件</h4>
      <div v-if="showMsg">这是一条信息</div>
      <div v-else>这是另一条信息</div>
      <button @click="changeShowMsg">改变showMsg</button>
    </div>
    ```
2. 在Vue实例中添加showMsg变量和changeShowMsg函数如下
    ```
    showMsg: true
    ```
    ```
    changeShowMsg:function(){
      this.showMsg = !this.showMsg;
    }
    ```
**注意**当v-if的变量表达式为false时，dom中是看不到这个元素的。效果如下：
![vue-条件](./gif/vue-条件.gif)

###### angular

1. 在html中添加如下代码：
    ```
    <!-- 条件 -->
    <hr/>
    <div>
    <h4>条件</h4>
    <div *ngIf="showMsg;else anotherMsg">这是一条信息</div>
    <ng-template #anotherMsg>
        <div >这是另一条信息</div>
    </ng-template>
    <button (click)="changeShowMsg()">改变showMsg</button>
    </div>
    ```
2. 在ts文件中加入变量和方法如下：
    ```
    showMsg:boolean = true;
    ```
    ```
    changeShowMsg(){
        this.showMsg = !this.showMsg;
    }
    ```
这里我们发现，angular的else是和if放在一起的`*ngIf="showMsg;else anotherMsg"`。而在else的模板中需要使用指令赋予一个名字`#anotherMsg`，这样angular才会知道else去渲染什么模板。并且这个模板必须使用`ng-template`，在angular中`ng-template`并不会真正的生产一个tag，当渲染时，会自动去除。

这里可能会认为angular这里会复杂一些，但是根据我的理解，事实并非如此，我们对vue中的条件稍作修饰如下：

```
<!-- 条件2 -->
<hr/>
<div>
    <h4>条件2</h4>
    <template v-if="showMsg2">
    <div>这是一条信息</div>
    <div>这是一条信息</div>
    </template>
    <template v-else>
    <div>这是另一条信息</div>
    <div>这是另一条信息</div>
    </template>
    <button @click="changeShowMsg2">改变showMsg</button>
</div>
```

当我们的if-else模板中的tag多余一个，我们也必须使用`template`，这样就和angular基本一致了。唯一的区别在于angular的else是需要放在`*ngIf`语句中，而vue是分开的。