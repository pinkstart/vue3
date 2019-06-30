###1.Vue声明周期
A.beforeCreated：当new Vue实例的时候，还没有根据配置对象来进行Vue实例的具体配置时触发。
B.created:当Vue实例创建完毕的时候触发
C.beforeMount：在实例进行挂载的时候触发
D.mounted:将容器初始化完毕，将容器模板也准备完毕的时候触发。
我们一般会在Mounted声明周期时发送ajax获取需要的数据
E.beforeUpdate：在更新data中的数据之前触发。我们可以在beforeUpdate生命周期中验证用户更新的最新的
数据是否正确
F.updated:在数据更新完毕时触发。
G.beforeDestroy：在销毁Vue实例前触发，在此时我们可以去释放缓存，清空变量保存的数据。
H.destroyed：在销毁Vue实例后触发。

补充：
如何销毁Vue实例
this.$destroy();

###2.动态修改数组中内容
在一些情况下(当需要修改的那个元素是基本类型数据的时候)，去通过索引修改数组中的内容时，修改的效果无法在页面中显示出（没有响应式变化）。
如果出现了这种情况，我们可以使用特定的方式来进行数据的响应式修改
Vue.set(要修改的数组名,要修改的索引,"修改的值");
vm.$set(要修改的数组名,要修改的索引,"修改的值");

###3.创建Vue组件
创建全局组件
Vue.component("组件名称",{ 组件配置信息 });
例子：
使用button-counter组件
<div id="app">
    <button-counter></button-counter>
    <button-counter></button-counter>
    <button-counter></button-counter>

    <hello-world></hello-world>
    <hello-tom></hello-tom>
</div>
<script>
Vue.component("HelloWorld",{
    data:function(){
        return {
            msg:"呵呵"
        }
    },
    template:"<div>{{msg}}</div>"
})

//定义全局组件
Vue.component("button-counter",{
    data:function(){
        return {
            //此时返回的对象中包含的就是组件拥有的数据
            count:0
        }
    },
    //template是一个字符串，他就是组件的html结构代码
    template:`<div>
                <button @click="handle">
                点击了{{count}}次
                </button>
                <HelloWorld></HelloWorld>
            </div>`,
    //在组件中可以添加methods，methods中包含了组件需要调用的方法
    methods:{
        handle(){
            //this指的是button-counter组件对象
            this.count += 3;
        }
    }
})

var vm = new Vue({
    el:"#app",
    data:{

    },
    //在components中添加局部组件
    components:{
        "hello-tom":{
            data:function(){
                return {
                    msg:"tom is dead"
                }
            },
            template:"<div>{{msg}}</div>"
        }
    }
})
</script>

注意：
A.组件中的data和Vue实例中的data不一样，组件中的data它是一个函数。
B.组件中的template里面必须是一个根元素，根元素里面可以有多个子元素
C.可以使用模板字符串来编写template对应html结构字符串。
D.如果使用驼峰命名的形式来创建一个组件，如果在template模板字符串中，可以直接使用驼峰命名的组件来添加组件
如果在容器中添加这个驼峰命名的组件，那么必须将驼峰命名的组件更改为-形式的组件
建议使用横线命名方式来创建组件。

补充：
模板字符串：使用反引号来定义模板字符串
单引号字符串或者双引号字符串的问题：字符串内部无法支持换行，如果要去拼接变量的值，使用+进行拼接
var str =" 我的名字是"+name+",年龄是"+age;
模板字符串可以解决以上的问题，模板字符串支持换行，支持直接使用${}的方式来插入变量
var mStr = `我的名字是${name},
                年龄是${age} `

###4.父组件向子组件传递数据
子组件需要设置props数组来接收父组件传递的数据
例：
Vue.component("HelloWorld",{
    //子组件接收的数据的名称叫做count
    props:['count'],
    data:function(){
        return {
            msg:"呵呵"
        }
    },
    template:"<div>{{msg}}，点击了{{count}}次</div>"
})

父组件需要设置属性来传递数据
Vue.component("button-counter",{
    data:function(){
        return {
            //此时返回的对象中包含的就是组件拥有的数据
            count:0
        }
    },
    //template是一个字符串，他就是组件的html结构代码
    template:`<div>
                <button @click="handle">
                点击了{{count}}次
                </button>
                <!-- :count是属性绑定，属性绑定的值是数据count  -->
                <HelloWorld :count="count"></HelloWorld>
            </div>`,
    //在组件中可以添加methods，methods中包含了组件需要调用的方法
    methods:{
        handle(){
            //this指的是button-counter组件对象
            this.count += 3;
        }
    }
})

注意：
如果使用驼峰命名的形式来传递父组件的数据给子组件，如果在template模板字符串中，可以直接使用驼峰命名的方式进行传递
如果在容器中设置和传递属性数据，那么必须将驼峰命名的组件更改为横线形式的属性名字
建议使用横线命名方式来设置传递数据的属性和接收属性值的props。

当父组件向子组件传递数据时，建议使用v-bind来进行属性传递，因为只有这种形式才能将数据和数据真实的类型一起传递给子组件
否则如果使用普通的自定义属性来传递数据，实际上传递的就是字符串。
建议使用绑定属性的方式来传递数据。

回顾：
1.Vue的生命周期：8个声明周期，最常用的是mounted，在mounted生命周期函数中会去发送ajax请求页面需要的数据
2.图书管理：完成图书的展示，图书的新增，图书的修改，图书的删除，图书总数的展示，监听新增图书时不能跟现有的图书重名
3.创建全局组件和局部组件。
Vue.component("组件名称（建议使用横线命名）",{
    data:function(){
        return {
            //数据
        }
    },
    template:"组件模板（组件需要展示的html结构代码）",
    methods:{
        //组件需要使用的方法
    }
})

4.掌握如何从父组件向子组件传递数据
课后完成案例“父组件向子组件传递数据.html”

5.安装Vue Devtools插件