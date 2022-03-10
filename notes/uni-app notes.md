# HBuilderX使用hints

Ctrl + Alt + 方向键    多光标

Ctrl + ]    在选区前后添加tag

Ctrl + =    扩大选区

Alt + 左键单机    转到定义

双击自动选择智能选区

Emmet 语法： div.className>ul>li * 3>span ，按下**tab**键即生成

```html
<div class="className">
    <ul>
        <li><span></span></li>
        <li><span></span></li>
        <li><span></span></li>
    </ul>
</div>
```

调试糖：使用"condition"可以方便地直接调试到某一个页面，避免反复的路径索引。





# uni-app 学习笔记

uni-app**条件编译**，发挥不同平台的特色能力

:data-变量名称 给当前标签一个数据，**绑定**事件的时候，用currentTarget的dataset可以获取这个数据

MVVM模式：双向数据绑定，不用DOM选择器，不用通过事件监听去修改数据，也不用吧setData去修改数据。 差量渲染性能更好。

`<text :href="url" @click="myMethod()">` 

:是v-bind:的缩写

@是v-on:的缩写



#### 数据绑定

属性值里面进行运算的example:

```vue
<template>
  <view>
      <view v-for="(item,index) in 10">
      <!-- 通过%运算符求余数，实现隔行换色的效果 -->
      <view :class="'list-' + index%2">{{index%2}}</view>
    </view>
  </view>
</template>
<script>
  export default {
    data() {
      return { }
    }
  }
</script>
<style>
  .list-0{
    background-color: #aaaaff;
  }
  .list-1{
    background-color: #ffaa7f;
  }
</style>
```

数据绑定只能使用表达式，而不能使用语句。



data必须写成return的形式。

动态地切换 class：

```vue
<view class="static" :class="{ active: isActive, 'text-danger': hasError }">222</view>
```

#### 条件渲染

`v-if` `v-else` `v-else-if`

使用`<template>`充当`<block>`

`v-show`是勤奋的，会提前渲染好，但是不会显示。

#### 列表渲染

`v-for`优先级大于`v-if`，但是不推荐同时使用二者。

##### 遍历数组

```vue
 <template>
        <view>
            <view v-for="(item, index) in items">
                {{ index }} - {{ item.message }}
            </view>
        </view>
    </template>
    <script>
        export default {
            data() {
                return {
                    items: [
                        { message: 'Foo' },
                        { message: 'Bar' }
                    ]
                }
            }
        }
    </script>
```

##### 遍历对象的属性

```vue
    <template>
        <view>
            <view v-for="(value, name, index) in object">
                 {{ index }}. {{ name }}: {{ value }}
            </view>
        </view>
    </template>
    <script>
        export default {
            data() {
                return {
                    object: {
                        title: 'How to do lists in Vue',
                        author: 'Jane Doe',
                        publishedAt: '2020-04-10'
                    }
                }
            }
        }
    </script>
```

#### 事件处理器

@绑定事件处理器，里面可以直接摆放js语句，如`@click="count += 1"` 或者`@click="function('hello')"`

#### 表单控件绑定

一些名词的改换。

#### 计算属性和监听器

```vue
    <script>
        export default {
            data() {
                return {
                    firstName: 'Foo',
                    lastName: 'Bar'
                }
            },
            computed: {
                fullName: {
                    // getter
                    get(){
                        return this.firstName + ' ' + this.lastName
                    },
                    // setter
                    set(newValue){
                        var names = newValue.split(' ')
                        this.firstName = names[0]
                        this.lastName = names[names.length - 1]
                    }
                }
            }
        }
    </script>
```

使用数据绑定可以当做普通data的语法使用。复杂的计算逻辑推荐这个。

（getter, setter）

和直接绑定一个方法相比：使用**计算属性**意味着放在缓存里。只有依赖的数据发生变化，才会重新求值。



# 遇到的神坑

* 引用SVG, 不能只调width，否则高度方向会有margin。
* 微信小程序background-image 不能在CSS样式里面调用图片，只能写在style里面，路径一开始还不能带/，否则手机端看不见。但是如果不带，电脑调试又看不到...
* tag的properties里面如果要用变量，前面一定要加`:` ，比如 `:class="classVariable"`
* 用flex布局，用margin控制边距。遇到左边一个对右边n个的情况，采用右边封装成一个大的view的方式
* 字体描边采用4个text-shadow
* flex布局，父级元素要指定大小，才方便置底
* style 里面需使用 kekbeb-case 命名方式：即 --dist-menu 而不是 --distMenu 方式，否则在微信小程序模式运行无效。
* CSS中使用变量必须"--asfasdf--ff"不能大写
* 自定义组件要有插槽`<slot></slot>`，一个插槽插不了多个东西
* 自定义组件不要用可能重名的CSS！！

# CSS

* flex弹性盒布局：justify-content是沿着main-axis方向的对齐，align-items是沿着cross-axis方向的对齐。

* CSS中使用`--variable-name`定义变量，不允许大写，必须是这种命名。使用的时候：`var(--variable-name)`

* 如果要计算，就是`width: calc(100% - 30px)` ==我的程序的样式还可以优化==

* `:hover`鼠标停留在上面的样式。

* 动画：`transition: <property> <duration>` 平滑地实现样式的过渡

  * `transition-timing-function: <linear|ease|ease-in|ease-out|ease-in-out>`不同的动画曲线
  * 特殊地：使用`transform`样式。 e.g. div:hover里面写`transform: rotate(180deg);`然后div里面写：`transition: ..., transform 2s;`
  * `transtition-delay: 2s` 设置延迟启动时间。
  * **transition 对 background-image无效， 我猜想是因为其很难用线性性质去描述。**
  * `transition: background 400ms;` 动感地切换背景。 

* button天生带有hover的class

* `animation`动画： 

* ```CSS
  @keyframes ripple {
      to {
          transform: scale(4);
          opacity: 0;
      }
  }
  ```

  只有当里面的Prop. 都有默认值，或者在样式里面已经事先指定，才能省略`from` 。

### SCSS

```scss
@use "sass:color";

.button {
  $primary-color: #6b717f;
  color: $primary-color;
  border: 1px solid color.scale($primary-color, $lightness: 20%);
}

```

变量赋值： !default作为缺省值

```SCSS
$content: "Second content?" !default;
```

用 @extend 去继承样式

## Rippling Effects

* button的样式

  * `position: relative` 让后面的高亮部分得以使用`position:absolute`去控制位置

  * `overflow: hidden` 让高光部分不会超出边界。

# Git

##### 冲突时，手动解决

​	`>>>>>`和`======`之间是一个版本，`=======`和`<<<<<<<`是另一个版本。删去之后用`git add . `和`commit`

`$ git reset --hard <commmit id | HEAD^>`

用`$ git reflog`查看所有历史的操作，可以看到commit的id

`$ git stash`存在栈里面，`$ git stash apply`把栈的东西应用到当前节点上。

==push完了记得挪到`dev`分支上。==

`$ git reset --hard HEAD`删除已跟踪的文件

`$ git clean -xdf`删除所有未追踪文件

