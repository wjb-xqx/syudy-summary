# Vue.js是一套构建用户界面的框架
能够帮助我们减少不必要的DOM操作；提高渲染效率；双向数据绑定的概念，通过
框架提供的指令，我们前端程序员只需要关心数据的业务逻辑，不再关心DOM是如何渲染的了
在Vue中，一个核心的概念，就是让用户不再操作DOM元素，解放了用户的双手，让程序员可以更多的时间去关注业务逻辑；
# vue基本
  vue基本指令
插值表达式{{}}，v-cloak，v-text，v-html
v-bind 绑定数据
 条件渲染
v-if
v-else
v-else-if
v-show 
 列表渲染
v-for
数组更新检测
data中的数据改变会引起视图改变MVVM
变异方法--改变原数组，引起视图更新
1.push（）
例如：
data （） {
    names：['li','bai','jing']
}
this.names.push（'ti'）
2.pop（）
3.shift（）
4.unshift（）
5.sort（）
6.reverse（）

替换数组--不改变数组，重新生成新的数组，所以不会改变视图变化
1.filter(function() {
    return  numbers /2 === 0;
})
2.concat()
3.slice()

显示过滤/排序结果
<li v-for="n in evenNumbers">{{ n }}</li>
方法一 创建返回过滤或排序数组的计算属性
data: {
  numbers: [ 1, 2, 3, 4, 5 ]
},
computed: {
  evenNumbers: function () {
    return this.numbers.filter(function (number) {
      return number % 2 === 0
    })
  }
}
方法二 在计算属性不适用的情况下 (例如，在嵌套 v-for 循环中) 你可以使用一个 method 方法：
<li v-for="n in even(numbers)">{{ n }}</li>
data: {
  numbers: [ 1, 2, 3, 4, 5 ]
},
methods: {
  even: function (numbers) {
    return numbers.filter(function (number) {
      return number % 2 === 0
    })
  }
}


 事件监听
v-on  
methods 
事件参数
 事件修饰符：
@click.stop  阻止冒泡
@click.prevent    阻止默认行为
@click.capture     实现捕获触发事件的机制
@click.self          使用 .self 实现只有点击当前元素时候，才会触发事件处理函数 .self 只会阻止
自己身上冒泡行为的触发，并不会真正阻止 冒泡的行为
@click.once 只触发一次事件处理函数 
<a href="http://www.baidu.com" @click.prevent.once="linkClick">

 按键修饰符
@keyup
常用按键有
.enter 按enter键触发
<input type="text" @keyup.enter="linkClick">                                      .delete (捕获删除和退格键)
.esc
.space
.up
.down
.left
.right
可以通过全局config.keyCodes对象自定义按键修饰符别名
//可以使用 v-on：keyup.f1
Vue.config.keyCodes.f1 = 112 

# 计算属性和侦听器
computed ： {
    很多方法
    mesge() {

    }
}

基础例子
<div id="example">
  <p>Original message: "{{ message }}"</p>
  <p>Computed reversed message: "{{ reversedMessage }}"</p>
</div>
var vm = new Vue({
  el: '#example',
  data: {
    message: 'Hello'
  },
  computed: {
    // 计算属性的 getter
    reversedMessage: function () {
      // `this` 指向 vm 实例
      return this.message.split('').reverse().join('')
    }
  }
})
结果：
Original message: "Hello"
Computed reversed message: "olleH"

计算属性与methods区别
我们可以将同一函数定义为一个方法而不是一个计算属性。两种方式的最终结果确实是完全相同的。然而，不同的是计算属性是基于它们的依赖进行缓存的。只在相关依赖发生改变时它们才会重新求值。这就意味着只要 message 还没有发生改变，多次访问 reversedMessage 计算属性会立即返回之前的计算结果，而不必再次执行函数。


侦听器：实时获取数据watch方法---避免使用watch，放在输入框中使用比较合适
 watch: {
    // 如果 `question` 发生改变，这个函数就会运行
    question: function (newQuestion, oldQuestion) {
      this.answer = 'Waiting for you to stop typing...'
      this.debouncedGetAnswer()
    }
  }


# 表单输入绑定 v-model
v-model 指令在表单 <input>、<textarea> 及 <select> 元素上创建双向数据绑定
多行文本
<span>Multiline message is:</span>
<p style="white-space: pre-line;">{{ message }}</p>
<br>
<textarea v-model="message" placeholder="add multiple lines"></textarea>
修饰符
.lazy
在默认情况下，v-model 在每次 input 事件触发后将输入框的值与数据进行同步 (除了上述输入法组合文字时)。你可以添加 lazy 修饰符，从而转变为使用 change 事件进行同步：
<!-- 在“change”时而非“input”时更新 -->
<input v-model.lazy="msg" >
.number
如果想自动将用户的输入值转为数值类型，可以给 v-model 添加 number 修饰符：
<input v-model.number="age" type="number">
.trim
如果要自动过滤用户输入的首尾空白字符，可以给 v-model 添加 trim 修饰符：
<input v-model.trim="msg">

限制操作频率---比如搜索框中如果输入什么就请求什么，减去服务器负担


<div id="watch-example">
  <p>
    Ask a yes/no question:
    <input v-model="question">
  </p>
  <p>{{ answer }}</p>
</div>
<!-- 因为 AJAX 库和通用工具的生态已经相当丰富，Vue 核心代码没有重复 -->
<!-- 提供这些功能以保持精简。这也可以让你自由选择自己更熟悉的工具。 -->
<script src="https://cdn.jsdelivr.net/npm/axios@0.12.0/dist/axios.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/lodash@4.13.1/lodash.min.js"></script>
<script>
var watchExampleVM = new Vue({
  el: '#watch-example',
  data: {
    question: '',
    answer: 'I cannot give you an answer until you ask a question!'
  },
  watch: {
    // 如果 `question` 发生改变，这个函数就会运行
    question: function (newQuestion, oldQuestion) {
      this.answer = 'Waiting for you to stop typing...'
      this.debouncedGetAnswer()
    }
  },
  created: function () {
    // `_.debounce` 是一个通过 Lodash 限制操作频率的函数。
    // 在这个例子中，我们希望限制访问 yesno.wtf/api 的频率
    // AJAX 请求直到用户输入完毕才会发出。想要了解更多关于
    // `_.debounce` 函数 (及其近亲 `_.throttle`) 的知识，
    // 请参考：https://lodash.com/docs#debounce
    this.debouncedGetAnswer = _.debounce(this.getAnswer, 500)
  },
  methods: {
    getAnswer: function () {
      if (this.question.indexOf('?') === -1) {
        this.answer = 'Questions usually contain a question mark. ;-)'
        return
      }
      this.answer = 'Thinking...'
      var vm = this
      axios.get('https://yesno.wtf/api')
        .then(function (response) {
          vm.answer = _.capitalize(response.data.answer)
        })
        .catch(function (error) {
          vm.answer = 'Error! Could not reach the API. ' + error
        })
    }
  }
})
</script>

