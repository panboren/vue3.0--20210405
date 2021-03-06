注册“全局”组件
const app = Vue.createApp({})
app.component('SearchInput', SearchInputComponent)
app.directive('focus', FocusDirective)
app.use(LocalePlugin)

允许链式：

一个应用需要被挂载到一个 DOM 元素中
const vm = app.mount('#app')
mount 不返回应用本身。相反，它返回的是根组件实例。

将用户定义的 property 添加到组件实例中，
例如 methods，props，computed，inject 和 setup

内置 property，如 $attrs 和 $emit。这些 property 都有一个 $ 前缀，
以避免与用户定义的 property 名冲突。

不要在选项 property 或回调上使用箭头函数，

数据绑定最常见的形式就是使用“Mustache”语法 (双大括号) 的文本插值：

 v-once 指令，你也能执行一次性地插值，这会影响到该节点上的其它数据绑定：

为了输出真正的 HTML，你需要使用v-html 指令：

Mustache 语法不能在 HTML attribute 中使用，然而，可以使用 v-bind 指令：

如果绑定的值是 null 或 undefined，那么该 attribute 将不会被包含在渲染的元素上。

对于所有的数据绑定，Vue.js 都提供了完全的 JavaScript 表达式支持。

每个绑定都只能包含单个表达式

v-bind 指令可以用于响应式地更新 HTML attribute：

v-on 指令，它用于监听 DOM 事件

可以在指令参数中使用 JavaScript 表达式，方法是用方括号括起来：
<a v-bind:[attributeName]="url"> ... </a>


修饰符 (modifier) 是以半角句号 . 指明的特殊后缀，用于指出一个指令应该以特殊方式绑定。

v-bind 缩写 :  <a :[key]="url"> ... </a>

v-on 缩写 @  <a @[event]="doSomething"> ... </a>

动态参数预期会求出一个字符串，值为 null。被显性地用于移除绑定。
非字符串类型的值都将会触发一个警告

<a v-bind:['foo' + bar]="value"> ... </a>
变通的办法是使用没有空格或引号的表达式，或用计算属性替代这种复杂表达式。

DOM 中使用模板时，避免使用大写字符来命名键名，浏览器会把 attribute 名全部强制转为小写


Data Property 和方法

组件的 data 选项是一个函数，返回一个对象。以 $data 的形式存储在组件实例中
修改 vm.count 的值也会更新
提供所需值的 property 使用 null、undefined 或其他占位的值

Vue 使用 $ 前缀通过组件实例暴露自己的内置 API。
它还为内部 property 保留 _ 前缀。
应该避免使用这两个字符开头的的顶级 data property 名称。


methods 选项向组件实例添加方法，它应该是一个包含所需方法的对象：
Vue 自动为 methods 绑定 this，以便于它始终指向组件实例
在定义methods 时应避免使用箭头函数，因为这会阻止 Vue 绑定恰当的 this 指向。
在模板中，它们通常被当做事件监听使用：


Vue 没有内置支持防抖和节流，但可以使用 Lodash 等库来实现
created 里添加该防抖函数:
 // 用 Lodash 的防抖函数
    this.debouncedClick = _.debounce(this.click, 500)

/ 移除组件时，取消定时器
    this.debouncedClick.cancel()



对于任何包含响应式数据的复杂逻辑，你都应该使用计算属性。

一函数定义vs一个计算属性，最终结果确实是完全相同。不同的是计算属性是基于它们的反应依赖关系缓存的

计算属性默认只有 getter，不过在需要时你也可以提供一个 setter：

watch当需要在数据变化时执行异步或开销较大的操作时，这个方式是最有用的。


Class 与 Style 绑定
    表达式结果的类型除了字符串之外，还可以是对象或数组
:class (v-bind:class 的简写) 一个对象，以动态地切换 class
<div :class="{ active: isActive }"></div>

绑定的数据对象不必内联定义在模板里：

这里绑定一个返回对象的计算属性。这是一个常用且强大的模式


一个数组传给 :class，以应用一个 class 列表：
<div :class="[activeClass, errorClass]"></div>
数组语法中也可以使用对象语法：
<div :class="[{ active: isActive }, errorClass]"></div>

在带有单个根元素的自定义组件上使用 class attribute 时，这些 class 将被添加到该元素中。、
此元素上的现有 class 将不会被覆盖。


:style 的对象语法十分直观——看着非常像 CSS
用驼峰式 (camelCase) 或短横线分隔 (kebab-case，记得用引号括起来) 来命名

直接绑定到一个样式对象通常更好，这会让模板更清晰：
在 :style 中使用需要 (浏览器引擎前缀) vendor prefixes 的 CSS property 时，
如 transform，Vue 将自动侦测并添加相应的前缀。

条件渲染

v-if 指令用于条件性地渲染一块内容。
这块内容只会在指令的表达式返回 truthy 值的时候被渲染。

在 <template> 元素上使用 v-if 条件渲染分组
最终的渲染结果将不包含 <template> 元素。
v-else 元素必须紧跟在带 v-if 或者 v-else-if 的元素的后面，否则它将不会被识别。
v-if 是“真正”的条件渲染，因为它会确保在切换过程中，
条件块内的事件监听器和子组件适当地被销毁和重建。

v-if 也是惰性的：如果在初始渲染时条件为假，
则什么也不做——直到条件第一次变为真时，才会开始渲染条件块。
v-if 有更高的切换开销，
如果在运行时条件很少改变，则使用 v-if 较好

v-show 的元素始终会被渲染并保留在 DOM 中。
v-show 只是简单地切换元素的 CSS property display。
注意，v-show 不支持 <template> 元素，也不支持 v-else。
简单地基于 CSS 进行切换。
v-show 有更高的初始渲染开销。
需要非常频繁地切换，则使用 v-show 较好

当 v-if 与 v-for 一起使用时，v-if 具有比 v-for 更高的优先级

列表渲染
v-for 指令需要使用 item in items 形式的特殊语法，其中 items 是源数据数组，
而 item 则是被迭代的数组元素的别名
v-for 块中，我们可以访问所有父作用域的 property。
v-for 还支持一个可选的第二个参数，即当前项的索引。

用 of 替代 in 作为分隔符，因为它更接近 JavaScript 迭代器的语法：

v-for 来遍历一个对象的 property。
提供第二个的参数为 property 名称 (也就是键名 key)：
<li v-for="(value, name) in myObject">
还可以用第三个参数作为索引：

在遍历对象时，会按 Object.keys() 的结果遍历

v-for 渲染的元素列表时，它默认使用“就地更新”的策略
只适用于不依赖子组件状态或临时 DOM 状态 (例如：表单输入值) 的列表渲染输出。

每个节点的身份，从而重用和重新排序现有元素，
你需要为每项提供一个唯一 key attribute：
建议尽可能在使用 v-for 时提供 key attribute，
请用字符串或数值类型的值。

数组更新检测
#变更方法
push() pop() shift() unshift()  splice() sort() reverse()

filter()、concat() 和 slice()不会变更原始数组，而总是返回一个新数组
当使用非变更方法时，可以用新数组替换旧数组：

含有相同元素的数组去替换原来的数组是非常高效的操作

v-for 的 <template> 来循环渲染一段包含多个元素的内容

不推荐在同一元素上使用 v-if 和 v-for

可以把 v-for 移动到 <template> 标签中来修正：

在自定义组件上，你可以像在任何普通元素上一样使用 v-for：

任何数据都不会被自动传递到组件里，因为组件有自己独立的作用域。
为了把迭代数据传递到组件里，我们要使用 props：


事件处理
v-on 指令 (通常缩写为 @ 符号) 来监听 DOM 事件
用法为 v-on:click="methodName" 或使用快捷方式 @click="methodName"

内联语句处理器中访问原始的 DOM 事件。可以用特殊变量 $event 把它传入方法

多事件处理器
事件处理程序中可以有多个方法，这些方法由逗号运算符分隔：
<button @click="one($event), two($event)">

v-on 提供了事件修饰符。之前提过，修饰符是由点开头的指令后缀来表示的。
.stop
.prevent
.capture
.self
.once
.passive

使用修饰符时，顺序很重要；相应的代码会以同样的顺序产生

Vue 还对应 addEventListener 中的 passive 选项提供了 .passive 修饰符。
<div @scroll.passive="onScroll">...</div>
不要把 .passive 和 .prevent 一起使用，因为 .prevent 将会被忽略

按键修饰符
<input @keyup.enter="submit" />

Vue 为最常用的键提供了别名：
按键别名
.enter .tab .delete (捕获“删除”和“退格”键)
.esc .space .up .down .left .right

系统修饰键
.ctrl .alt  .shift .meta
事件触发时修饰键必须处于按下状态。

.exact 修饰符允许你控制由精确的系统修饰符组合触发的事件。

鼠标按钮修饰符
.left  .right  .middle

表单输入绑定
#基础用法

v-model 在内部为不同的输入元素使用不同的 property 并抛出不同的事件：

text 和 textarea 元素使用 value property 和 input 事件；
checkbox 和 radio 使用 checked property 和 change 事件；
select 字段将 value 作为 prop 并将 change 作为事件。

input 事件监听器和 value 绑定，而不是使用 v-model。

v-model 表达式的初始值未能匹配任何选项，<select> 元素将被渲染为“未选中”状态

对于单选按钮，复选框及选择框的选项，
v-model 绑定的值通常是静态字符串 (对于复选框也可以是布尔值)：

.lazy
添加 lazy 修饰符，从而转为在 change 事件_之后_进行同步：

.trim
如果要自动过滤用户输入的首尾空白字符，可以给 v-model 添加 trim 修饰符

.number
如果想自动将用户的输入值转为数值类型，可以给 v-model 添加 number 修饰符：

在组件上使用 v-model

组件基础
#基本实例
组件是带有名称的可复用实例
每用一次组件，就会有一个它的新实例被创建。

通过 component 方法全局注册的：
app.component(）

通过 Prop 向子组件传递数据
Prop 是你可以在组件上注册的一些自定义 attribute。

一个组件默认可以拥有任意数量的 prop，无论任何值都可以传递给 prop。

父级组件可以像处理原生 DOM 事件一样通过 v-on 或 @ 监听子组件实例的任意事件：
<blog-post ... @enlarge-text="postFontSize += 0.1"></blog-post>
子组件可以通过调用内建的 $emit 方法并传入事件名称来触发一个事件：
<button @click="$emit('enlargeText')">

可以在组件的 emits 选项中列出已抛出的事件：
app.component('blog-post', {
  props: ['title'],
  emits: ['enlargeText']
})

$emit 的第二个参数来提供这个值：

然后当在父级组件监听这个事件的时候，我们可以通过 $event 访问到被抛出的这个值：
这个值将会作为第一个参数传入这个方法：


在组件上使用 v-model
<input :value="searchText" @input="searchText = $event.target.value" />

当用在组件上时，v-model 则会这样：
<custom-input
  :model-value="searchText"
  @update:model-value="searchText = $event"
></custom-input>

在该组件中实现 v-model 的另一种方法是使用 computed property 的功能来定义 getter 和 setter。
get 方法应返回 modelValue property，set 方法应该触发相应的事件。
 computed: {
    value: {
      get() {return this.modelValue},
      set(value) { this.$emit('update:modelValue', value)}
    }
  }

动态组件
Vue 的 <component> 元素加一个特殊的 is attribute 来实现：

已注册组件的名字，或
一个组件的选项对象

v-is 指令作为一个变通的办法：
<table>
  <tr v-is="'blog-post-row'"></tr>
</table>
v-is 值应为 JavaScript 字符串文本：

HTML attribute 名不区分大小写，浏览器将所有大写字符解释为小写
如果我们从以下来源使用模板的话，这条限制是不存在的：

字符串模板 (例如：template: '...')
单文件组件
<script type="text/x-template">

组件注册
组件名就是 app.component 的第一个参数，
遵循 W3C 规范中的自定义组件名 (字母全小写且必须包含一个连字符)
全部小写
包含连字符 (及：即有多个单词与连字符符号连接)
使用 kebab-case、

全局注册
app.component 来创建组件：
在所有子组件中也是如此，也就是说这三个组件在各自内部也都可以相互使用。

#局部注册
注意局部注册的组件在其子组件中不可用。
通过 Babel 和 webpack 使用 ES2015 模块，那么代码看起来像这样
import ComponentA from './ComponentA.vue'
用在模板中的自定义元素的名称
包含了这个组件选项的变量名

Prop 类型
     到这里，我们只看到了以字符串数组形式列出的 prop：
props: {
  title: String,
  likes: Number,
  isPublished: Boolean,
  commentIds: Array,
  author: Object,
  callback: Function,
  contactsPromise: Promise // 或任何其他构造函数
}

prop 可以通过 v-bind 或简写 : 动态赋值
所有的 prop 都使得其父子 prop 之间形成了一个单向下行绑定

这个 prop 用来传递一个初始值；
这个子组件接下来希望将其作为一个本地的 prop 数据来使用。
在这种情况下，最好定义一个本地的 data property 并将这个 prop 作为其初始值：

这个 prop 以一种原始的值传入且需要进行转换。在这种情况下，
最好使用这个 prop 的值来定义一个计算属性：

注意在 JavaScript 中对象和数组是通过引用传入的，
所以对于一个数组或对象类型的 prop 来说，
在子组件中改变变更这个对象或数组本身将会影响到父组件的状态。

类型检查
type 可以是下列原生构造函数中的一个：
String Number Boolean Array Object Date Function Symbol

type 还可以是一个自定义的构造函数

Prop 的大小写命名 (camelCase vs kebab-case)
HTML 中的 attribute 名是大小写不敏感的，所以浏览器会把所有大写字符解释为小写字符。

change 事件监听器从父组件传递到子组件，
它将在原生 select 的 change 事件上触发。

禁用 Attribute 继承
inheritAttrs: false

多个根节点上的 Attribute 继承
多个根节点的组件不具有自动 attribute 回退行为。
如果未显式绑定 $attrs，将发出运行时警告。

 <main v-bind="$attrs">...</main>


自定义事件

事件名提供了自动的大小写转换。如果用驼峰命名的子组件中触发一个事件，
你将可以在父组件中添加一个 kebab-case (短横线分隔命名) 的监听器。

定义自定义事件
emits 选项在组件上定义已发出的事件。
 emits: ['inFocus', 'submit']

当在 emits 选项中定义了原生事件 (如 click) 时，
将使用组件中的事件替代原生事件侦听器。

验证抛出的事件
与 prop 类型验证类似，如果使用对象语法而不是数组语法定义发出的事件，
则可以验证它。

组件上的 v-model 使用 modelValue 作为 prop 和 update:modelValue 作为事件。
每个 v-model 将同步到不同的 prop，而不需要在组件中添加额外的选项

 props: {
    modelValue: String,
    modelModifiers: {
      default: () => ({})
    }
  },

对于带参数的 v-model 绑定，生成的 prop 名称将为 arg + "Modifiers"：


插槽

插槽还可以包含任何模板代码，包括 HTML：
或其他组件
template 中没有包含一个 <slot> 元素，
则该组件起始标签和结束标签之间的任何内容都会被抛弃

父级模板里的所有内容都是在父级作用域中编译的；
子模板里的所有内容都是在子作用域中编译的。

备用内容
可以将它放在 <slot> 标签内
<slot> 元素有一个特殊的 attribute：name。
这个 attribute 可以用来定义额外的插槽：
一个不带 name 的 <slot> 出口会带有隐含的名字“default”。

可以在一个 <template> 元素上使用 v-slot 指令，并以 v-slot 的参数的形式提供其名称：
注意，v-slot 只能添加在 <template> 上 (只有一种例外情况)

绑定在 <slot > 元素上的 attribute 被称为插槽 prop。
父级作用域中，我们可以使用带值的 v-slot 来定义我们提供的插槽 prop 的名字：
v-slot:default="slotProps"

独占默认插槽的缩写语法
v-slot 直接用在组件上：
<todo-list v-slot:default="slotProps">

注意默认插槽的缩写语法不能和具名插槽混用，因为它会导致作用域不明确：

只要出现多个插槽，请始终为所有的插槽使用完整的基于 <template> 的语法：

v-slot 的值实际上可以是任何能够作为函数定义中的参数的 JavaScript 表达式。
用 ES2015 解构来传入具体的插槽 prop
v-slot="{ item }"

动态插槽名
<template v-slot:[dynamicSlotName]>
具名插槽的缩写(v-slot:) 替换为字符  #default
该缩写只在其有参数的时候才可用。 
使用缩写的话，你必须始终以明确插槽名取而代之：


Provide / Inject
父组件有一个 provide 选项来提供数据，
子组件有一个 inject 选项来开始使用这些数据。

要访问组件实例 property，我们需要将 provide 转换为返回对象的函数
 provide() {
    return { todoLength: this.todos.length}
  },

provide/inject 绑定并不是响应式的

动态组件 & 异步组件

在动态组件上使用 keep-alive

<!-- 失活的组件将会被缓存！-->
<keep-alive>
  <component :is="currentTabComponent"></component>
</keep-alive>

异步组件
我们可能需要将应用分割成小一些的代码块，
并且只在需要的时候才从服务器加载一个模块。为了简化，
Vue 有一个 defineAsyncComponent 方法：
此方法接受返回 Promise 的工厂函数。

const AsyncComp = defineAsyncComponent( () =>
    new Promise((resolve, reject) => {
      resolve({
        template: '<div>I am async!</div>'
      })
    })
)

动态地导入：
import { defineAsyncComponent } from 'vue'
const AsyncComp = defineAsyncComponent(() =>
  import('./components/AsyncComponent.vue')
)

与 Suspense 一起使用
可以在其选项中指定 suspensible:false

模板引用
ref attribute 为子组件或 HTML 元素指定引用 ID

以编程的方式
 this.$refs.input.focus()

向组件本身添加另一个 ref，并使用它从父组件触发 focusInput 事件
this.$refs.usernameInput.focusInput()

$refs 只会在组件渲染完成之后生效。
这仅作为一个用于直接操作子元素的“逃生舱”
——你应该避免在模板或计算属性中访问 $refs。



处理边界情况
必须手动强制更新，那么你可以使用 $forceUpdate

一个包含很多静态内容的组件，向根元素添加 v-once 指令来确保只对其求值一次，然后进行缓存
不要过度使用这种模式

过渡 & 动画概述

添加一个条件 class 来激活动画，而无需挂载组件。
 <div :class="{ shake: noActivated }">
通过插值的方式来实现，例如在发生交互时将样式绑定到元素上

硬件加速
perspective: 1000px;
backface-visibility: hidden;
transform: translateZ(0);

0.25s 是一个最佳选择
Easing 是在动画中表达深度的一个重要方式。
起始动画节点使用ease-out，结束动画节点使用ease-in


进入过渡 & 离开过渡

单元素/组件的过渡
条件渲染 (使用 v-if)
条件展示 (使用 v-show)
动态组件
组件根节点

v-enter-from：定义进入过渡的开始状态
v-enter-active：定义进入过渡生效时的状态。
v-enter-to：定义进入过渡的结束状态
v-leave-from：定义离开过渡的开始状态。
v-leave-active：定义离开过渡生效时的状态。
v-leave-to：离开过渡的结束状态。

自定义过渡 class 类名
我们可以通过以下 attribute 来自定义过渡类名：
enter-from-class
enter-active-class
enter-to-class
leave-from-class
leave-active-class
leave-to-class
优先级高于普通的类名


同时使用过渡和动画
事件监听器：transitionend 或 animationend
两种过渡动效
需要使用 type attribute 
并设置 animation 或 transition 来明确声明你需要 Vue 监听的类型。


显性的过渡持续时间
<transition :duration="1000">...</transition>
定制进入和移出的持续时间：
<transition :duration="{ enter: 500, leave: 800 }">...</transition>

JavaScript 过渡的时候，在 enter 和 leave 钩中必须使用 done 进行回调

添加 :css="false"，也会让 Vue 会跳过 CSS 的检测，除了性能略高之外，
这可以避免过渡过程中 CSS 规则的影响。

多个元素的过渡
v-if/v-else-if/v-else 或将单个元素绑定到一个动态 property，
可以在任意数量的元素之间进行过渡

Vue 提供了过渡模式
in-out: 新元素先进行过渡，完成之后当前元素过渡离开。
out-in: 当前元素先进行过渡，完成之后新元素过渡进入
out-in 是你大多数时候想要的状态 😃
<transition name="fade" mode="out-in">


多个组件之间过渡
不需要 key 属性。相反，我们包裹了一个动态组件 ：
 <transition name="component-fade" mode="out-in">
    <component :is="view"></component>
  </transition>


列表过渡
单个节点
同一时间渲染多个节点中的一个


列表的移动过渡
<transition-group> 组件还有一个特殊之处。
新增的 v-move class
通过 name attribute 来自定义前缀，
也可以通过 move-class attribute 手动设置。

需要注意的是使用 FLIP 过渡的元素不能设置为 display: inline。
作为替代方案，可以设置为 display: inline-block 或者放置于 flex 中


可复用的过渡
将 <transition> 或者 <transition-group> 作为根组件，
然后将任何子组件放置在其中就可以了。


状态过渡

状态动画与侦听器
 watch: {
    number(newValue) {
      gsap.to(this.$data, { duration: 0.5, tweenedNumber: newValue })
    }
  }

动态状态过渡

把过渡放到组件里


介绍
组合式 API 基础
Vue 组件中，我们将此位置称为 setup。
新的 setup 组件选项在创建组件之前执行
一旦 props 被解析，就作为组合式 API 的入口点。

由于在执行 setup 时，组件实例尚未被创建，因此在 setup 选项中没有 this。这意味着，
除了 props 之外，你将无法访问组件中声明的任何属性——本地状态、计算属性或方法。

setup 选项应该是一个接受 props 和 context 的函数

setup(props) {
    console.log(props) // { user: '' }
    return {} // 这里返回的任何内容都可以用于组件的其余部分
  }

带 ref 的响应式变量
ref 函数使任何响应式变量在任何地方起作用
import { ref } from 'vue'
const counter = ref(0)
console.log(counter) // { value: 0 }

ref 为我们的值创建了一个响应式引用。在整个组合式 API 中会经常使用引用的概念。

创建一个响应式的 repositories 变量：
import { fetchUserRepositories } from '@/api/repositories'
const getUserRepositories = async () => {
    repositories.value = await fetchUserRepositories(props.user)
  }

生命周期钩子注册内部 setup
前缀为 on：即 mounted 会看起来像 onMounted。
import { ref, onMounted } from 'vue'
 onMounted(getUserRepositories) // 在 `mounted` 时调用 `getUserRepositories`

watch 响应式更改
接受 3 个参数：
一个我们想要侦听的响应式引用或 getter 函数
一个回调
可选的配置选项

  // 使用 `toRefs` 创建对prop的 `user` property 的响应式引用


独立的 computed 属性
import { ref, computed } from 'vue'
const twiceTheCounter = computed(() => counter.value * 2)
computed 的第一个参数传递的 getter 类回调的输出的一个只读的响应式引用
为了访问新创建的计算变量的 value，我们需要像使用 ref 一样使用 .value property。

我们将首先将上述代码提取到一个独立的组合式函数。

Setup
使用 setup 函数时，它将接受两个参数：
props
context

setup 函数中的 props 是响应式的，当传入新的 prop 时，它将被更新。
因为 props 是响应式的，你不能使用 ES6 解构，因为它会消除 prop 的响应性。

如果需要解构 prop，可以通过使用 setup 函数中的 toRefs 来完成此操作
const { title } = toRefs(props)

传入的 props 中可能没有 title
toRefs 将不会为 title 创建一个 ref 
const title = toRef(props, 'title')

Context
 // Attribute (非响应式对象)
    console.log(context.attrs)
// 插槽 (非响应式对象)
    console.log(context.slots)
 // 触发事件 (方法)
    console.log(context.emit)

ontext
不是响应式的，可以使用 ES6 解构。
 setup(props, { attrs, slots, emit }) { }

attrs 和 slots 是有状态的对象，它们总是会随组件本身的更新而更新
始终以 attrs.x 或 slots.x 的方式引用 property
如果你打算根据 attrs 或 slots 更改应用副作用，
那么应该在 onUpdated 生命周期钩子中执行此操作。

访问组件的 property
props attrs slots emit

从 setup 返回的 refs 在模板中访问时是被自动解开的，因此不应在模板中使用 .value。

setup 还可以返回一个渲染函数，该函数可以直接使用在同一作用域中声明的响应式状态：

在 setup() 内部，this 不会是该活跃实例的引用


生命周期钩子
 setup () 内部调用生命周期钩子

选项式 API	Hook inside setup
beforeCreate	Not needed 
created	                Not needed*
beforeMount	onBeforeMount
mounted	                onMounted
beforeUpdate	onBeforeUpdate
updated	                onUpdated
beforeUnmount	onBeforeUnmount
unmounted	onUnmounted
errorCaptured	onErrorCaptured
renderTracked	onRenderTracked
renderTriggered	onRenderTriggered

setup 是围绕 beforeCreate 和 created 生命周期钩子运行的，所以不需要显式地定义它们。

这些钩子中编写的任何代码都应该直接在 setup 函数中编写。


Provide / Inject

使用 Provide
vue 显式导入 provide 方法

provide 函数允许你通过两个参数定义 property：

property 的 name (<String> 类型)
property 的 value

setup() { provide('name ', 'value') }

使用 inject
setup() 中使用 inject 时，要从 vue 显式导入它

inject 函数有两个参数：
要 inject 的 property 的名称
一个默认的值 (可选)

import { inject } from 'vue'                  //  (可选)
const userLocation = inject('location', 'The Universe') 

添加响应性
可以在 provide 值时使用 ref 或 reactive。

 const location = ref('North Pole')
 provide('location', location)

修改响应式 property
当使用响应式 provide / inject 值时，建议尽可能，
在提供者内保持响应式 property 的任何更改。

在注入数据的组件内部更新 inject 的数据。
建议 provide 一个方法来负责改变响应式 property。

确保通过 provide 传递的数据不会被 inject 的组件更改,使用 readonly。



模板引用
 <div ref="root">This is a root element</div>
 const root = ref(null)
 return { root }
虚拟 DOM 挂载/打补丁过程中执行的.
因此模板引用只会在初始渲染之后获得赋值。
ref 的行为与任何其他 ref 一样：它们是响应式的，

JSX 中的用法
 return () =>  h('div', { ref: root })
 return () => <div ref={root} />

v-for 中的用法
v-for 内部使用时没有特殊处理
使用函数引用执行自定义处理：
onBeforeUpdate(() => {  divs.value = [] })

watch() 和 watchEffect() 副作用是在 DOM 被挂载或更新之前运行的
当侦听器运行副作用时，模板引用还没有被更新。

const root = ref(null)
watchEffect(() => {
        // 这个副作用在 DOM 更新之前运行，因此，模板引用还没有持有对元素的引用。
        console.log(root.value) // => null
      })

因此，使用模板引用的侦听器应该用 flush: 'post' 选项来定义
将在 DOM 更新后运行副作用

  watchEffect(() => {
        console.log(root.value) // => <div></div>
      },  
      {flush: 'post'})


Mixin
当组件和 mixin 对象含有同名选项时，这些选项将以恰当的方式进行“合并”。
数据对象在内部会进行递归合并，并在发生冲突时以组件数据优先。

同名钩子函数将合并为一个数组，因此都将被调用
mixin 对象的钩子将在组件自身钩子之前调用。

值为对象的选项
methods、components 和 directives，将被合并为同一个对象。
两个对象键名冲突时，取组件对象的键值对。

自定义选项合并策略
app.config.optionMergeStrategies 中添加一个函数


自定义指令

app.directive('focus', {

注册局部指令，组件中也接受一个 directives 的选项：

directives: {
  focus: {
    // 指令的定义
    mounted(el) {
      el.focus()
    }
  }

钩子函数
一个指令定义对象可以提供如下几个钩子函数 (均为可选)：

created：在绑定元素的 attribute 或事件监听器被应用之前调用。在指令需要附加须要在普通的 v-on 事件监听器前调用的事件监听器时，这很有用。

beforeMount：当指令第一次绑定到元素并且在挂载父组件之前调用。

mounted：在绑定元素的父组件被挂载后调用。

beforeUpdate：在更新包含组件的 VNode 之前调用。

updated：在包含组件的 VNode 及其子组件的 VNode 更新后调用。

beforeUnmount：在卸载绑定元素的父组件之前调用

unmounted：当指令与元素解除绑定且父组件已卸载时，只调用一次。


动态指令参数
参数可以根据组件实例数据进行更新
v-mydirective:[argument]="value"

了使其更具动态性，我们还可以允许修改绑定值。
app.directive('pin', {
  mounted(el, binding) {
    el.style.position = 'fixed'
    const s = binding.arg || 'top'
    el.style[s] = binding.value + 'px'
  },
  updated(el, binding) {
    const s = binding.arg || 'top'
    el.style[s] = binding.value + 'px'
  }
})


#函数简写
app.directive('pin', (el, binding) => {

对象字面量
<div v-demo="{ color: 'white', text: 'hello!' }"></div>

在组件中使用
自定义指令总是会被应用在组件的根节点上。

当被应用在一个多根节点的组件上时，指令会被忽略，并且会抛出一个警告。


Teleport

<teleport to="body">
使用 <teleport>，并告诉 Vue “Teleport 这个 HTML 到该‘body’标签”。


与 Vue components 一起使用
如果 <teleport> 包含 Vue 组件，则它仍将是 <teleport> 父组件的逻辑子组件：

在同一目标上使用多个 teleport
多个 <teleport> 组件可以将其内容挂载到同一个目标元素。
顺序将是一个简单的追加——稍后挂载将位于目标元素中较早的挂载之后。



渲染函数

render() {
    return h(
      'h' + this.level, // tag name
      {}, // props/attributes
      this.$slots.default() // array of children
    )
  },

h() 参数
接受三个参数：

// @returns {VNode}
h(
  // {String | Object | Function | null} tag
  // 一个 HTML 标签名、一个组件、一个异步组件，或者 null。
  // 使用 null 将会渲染一个注释。
  //
  // 必需的。
  'div',

  // {Object} props
  // 与 attribute、prop 和事件相对应的对象。
  // 我们会在模板中使用。
  //
  // 可选的。
  {},

  // {String | Array | Object} children
  // 子 VNodes, 使用 `h()` 构建,
  // 或使用字符串获取 "文本 Vnode" 或者
  // 有插槽的对象。
  //
  // 可选的。
  [
    'Some text comes first.',
    h('h1', 'A headline'),
    h(MyComponent, {
      someProp: 'foobar'
    })
  ]
)

VNodes 必须唯一

需要重复很多次的元素/组件，你可以使用工厂函数来实现

v-model 指令扩展为 modelValue 和 onUpdate:modelValue 在模板编译过程中
props: ['modelValue'],
emits: ['update:modelValue'],
render() {
  return h(SomeComponent, {
    modelValue: this.modelValue,
    'onUpdate:modelValue': value => this.$emit('update:modelValue', value)
  })
}

v-on
render() {
  return h('div', {
    onClick: $event => console.log('clicked', $event.target)
  })
}

事件修饰符
.passive 、 .capture和 .once 事件修饰符，可以使用驼峰写法将他们拼接在事件名后面

你可以通过 this.$slots 访问静态插槽的内容，每个插槽都是一个 VNode 数组：
 return h('div', {}, this.$slots.default())


这就是为什么会有一个 Babel 插件，用于在 Vue 中使用 JSX 语法，
它可以让我们回到更接近于模板的语法上。


插件

插件是自包含的代码，通常向 Vue 添加全局级功能。
它可以是公开 install() 方法的 object，也可以是 function
插件的功能范围没有严格的限制——一般有下面几种：

添加全局方法或者 property。如：vue-custom-element

添加全局资源：指令/过滤器/过渡等。如：vue-touch）

通过全局 mixin 来添加一些组件选项。(如vue-router)

添加全局实例方法，通过把它们添加到 config.globalProperties 上实现。

一个库，提供自己的 API，同时提供上面提到的一个或多个功能。如 vue-router

两个参数：由 Vue 的 createApp 生成的 app 对象和用户传入的选项。
export default {
  install: (app, options) => {
    // Plugin code goes here
  }
}

应用程序可用的键，因此我们将使用 app.config.globalProperties 暴露它。

使用插件
createApp() 初始化 Vue 应用程序后，
你可以通过调用 use() 方法将插件添加到你的应用程序中。
它还会自动阻止你多次使用同一插件，因此在同一插件上多次调用只会安装一次该插件
第二个参数是可选的，并且取决于每个特定的插件


深入响应性原理
Vue 3 版本也使用了 Object.defineProperty 来支持 IE 浏览器。
两者具有相同的 Surface API，但是 Proxy 版本更精简，同时提升了性能。

 Proxy 是一个包含另一个对象或函数并允许你对其进行拦截的对象。
把对象包裹在 Proxy 里的同时可以对其进行拦截。这种拦截被称为陷阱。

跟踪更改它的函数：我们在 proxy 中的 getter 中执行此操作，称为 effect
触发函数以便它可以更新最终值：我们在 proxy 中的 setter 中进行该操作，名为 trigger

当使用合成 API 显式创建响应式对象时，最佳做法是不要保留对原始对象的引用，而只使用响应式版本：


响应性基础
对象创建响应式状态，可以使用 reactive 方法：

reactive 相当于 Vue 2.x 中的 Vue.observable()
响应式转换是“深度转换”——它会影响嵌套对象传递的所有 property。

创建独立的响应式值作为 refs
const count = ref(0)
ref 会返回一个可变的响应式对象，该对象作为它的内部值——一个响应式的引用，
此对象只包含一个名为 value 的 property ：

Ref 展开
当 ref 作为渲染上下文 (从 setup() 中返回的对象) 上的 property 返回并可以在模板中被访问时，
它将自动展开为内部值。不需要在模板中追加 .value：

访问响应式对象
当 ref 作为响应式对象的 property 被访问或更改时，
为使其行为类似于普通 property，它会自动展开内部值：

将新的 ref 赋值给现有 ref 的 property，将会替换旧的 ref：

Ref 展开仅发生在被响应式 Object 嵌套的时候。
当从 Array 或原生集合类型如 Map访问 ref 时，不会进行展开：

遗憾的是，使用解构的两个 property 的响应性都会丢失。
对于这种情况，我们需要将我们的响应式对象转换为一组 ref。
这些 ref 将保留与源对象的响应式关联：

let { author, title } = toRefs(book)
我们需要使用 .value 作为标题，现在是 ref

使用 readonly 防止更改响应式对象


响应式计算和侦听

computed 方法：它接受 getter 函数并为 getter 返回的值返回一个不可变的响应式 ref 对象。
它可以使用一个带有 get 和 set 函数的对象来创建一个可写的 ref 对象。

watchEffect 方法。它立即执行传入的一个函数，
同时响应式追踪其依赖，并在其依赖变更时重新运行该函数。

watchEffect(() => console.log(count.value))
在组件卸载时自动停止。

显式调用返回值以停止侦听
const stop = watchEffect(() => {})
stop()

onInvalidate 函数作入参，用来注册清理失效时的回调。
副作用即将重新执行时
侦听器被停止 (如果在 setup() 或生命周期钩子函数中使用了 watchEffect，则在组件卸载时)


副作用刷新时机
默认情况下，会在所有的组件 update 前执行：
将在组件更新前执行副作用。

如果需要在组件更新 (例如：当与模板引用一起) 后重新运行侦听器副作用，
我们可以传递带有 flush 选项的附加 options 对象 (默认为 'pre')： // flush: 'post'
flush 选项还接受 sync，这将强制效果始终同步触发。然而，这是低效的，应该很少需要。

侦听器调试
onTrack 和 onTrigger 选项可用于调试侦听器的行为。
onTrack 将在响应式 property 或 ref 作为依赖项被追踪时被调用。
onTrigger 将在依赖项变更导致副作用被触发时被调用。


onTrack 和 onTrigger 只能在开发模式下工作。


watch 需要侦听特定的数据源，并在回调函数中执行副作用。
它也是惰性的，即只有当被侦听的源发生变化时才执行回调。

与 watchEffect 比较，watch 允许我们：
懒执行副作用；
更具体地说明什么状态应该触发侦听器重新运行；
访问侦听状态变化前后的值。

侦听单个数据源
// 侦听一个 getter
watch(
  () => state.count,
  (count, prevCount) => { }
)

// 直接侦听ref
const count = ref(0)
watch(count, (count, prevCount) => { })

侦听多个数据源
watch([firstName, lastName], (newValues, prevValues) => { })

侦听响应式对象
使用侦听器来比较一个数组或对象的值，这些值是响应式的，
要求它有一个由值构成的副本。

检查深度嵌套对象或数组中的 property 变化时，
仍然需要 deep 选项设置为 true。

侦听一个响应式对象或数组将始终返回该对象的当前值和上一个状态值的引用
需要对值进行深拷贝: lodash.cloneDeep

与 watchEffect 共享的行为
watch 与 watchEffect共享停止侦听，
清除副作用 (相应地 onInvalidate 会作为回调的第三个参数传入)、
副作用刷新时机和侦听器调试行为。

渲染机制和优化
虚拟 DOM 是轻量级的 JavaScript 对象，由渲染函数创建。
它包含三个参数：元素，具有数据、prop、attr 等的对象，
以及一个数组。数组是我们传递子级的地方，子级也具有所有这些参数，
然后它们也可以具有子级，依此类推，直到我们构建完整的元素树为止。

更改应用至 JavaScript 副本、虚拟 DOM 中，
然后在它们和实际 DOM 之间执行 diff
虚拟 DOM 允许我们对 UI 进行高效的更新！
