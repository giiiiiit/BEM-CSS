> BEM 是对 CSS 命名的一种规范，推崇将 WEB 页面模块化，从而提高代码的重用度，减少后期维护的成本。[原文](https://en.bem.info/methodology/key-concepts/)
# BEM CSS命名规范一 Quick start
### 简介
BEM（Block，Element，Modifier）是一个基于组件方式的 web 开发方法。

他的基本思想是将用户界面分为独立的模块。
    
即使是一个复杂的 UI 界面，也会使得用户界面开发简单而迅速，，并且允许重用现有的代码，而不是复制粘贴。

### 内容
- 模块
- 元素
- 我应该创建一个 Block 还是一个 Element
- 修饰符
- 混合模式
- 文件系统
### 模块
一个可重复使用的功能独立的页面组件。在 HTML 中，blocks 通过 class 属性表示。

特征：

- [模块名称](https://en.bem.info/methodology/naming-convention/#block-name)描述了它的目的（“它是什么？” —— 菜单或者按钮），而不是它的状态（“它看起来是什么样子？” —— 红色或者大的）。

例如：
```html
<!-- 正确的，这个 'error' 模块是具有语义上的意义的 -->
<div class="error"></div>
<!-- 不正确的，它描述了模块的外观 -->
<div class="red-text"></div>
```
- 模块不应该影响它所在的环境，这意味着你不应该为模块设置会影响到外部的形状（影响大小的 ` padding` 或边框）和定位
- 你也不应该在使用 BEM 的时候使用 CSS 标签选择器和 ID 选择器

这些保证了模块必要的独立性，可以更好地重用模块或者将他们从一个地方移动到另一个地方。

### 模块使用指南
#### 嵌套关系
- 模块与模块之间可以彼此嵌套
- 你可以有任意级别的嵌套层次

例子：
```html
<!-- 'head' 模块 -->
<header class="header">
    <!-- 嵌套 'logo' 模块 -->
    <div class="logo"></div>

    <!-- 嵌套 'search-form' 模块 -->
    <form class="search-form"></form>
</header>
```
### 元素
是一个模块的组成部分，且不能脱离模块单独地被使用。

特征：

- [元素名称](https://en.bem.info/methodology/naming-convention/#element-name)描述了它的目的（用处）（“这是什么？” —— item，text，等等。），而不是它的状态（“什么类型的，或者它看起来是什么样的？” —— 红色，大的，等等。）
- 完整的元素名的结构是 `block-name__element-name`。元素的名字与模块的名字使用双下划线分隔（__）

例子：
```html
<!-- 'search-form' 模块 -->
<form class="search-form">
    <!-- 在 'search-form' 模块内的 'input' 元素 -->
    <input class="search-form__input"/>
    <!-- 在 'search-form' 模块内的 'button' 元素 -->
    <button class="search-form__button"></button>
</form>
```
### 元素使用指南
- 嵌套关系
- 组成部分
- 可选性
### 嵌套关系
- 元素之间可以彼此嵌套
- 你可以拥有任意层次的嵌套级别
- 一个元素总是一个模块的一部分，而不是另一个元素的一部分，这意味着元素的名称不能被定义为 block__elem1__elem2 这样的层次结构。

例子：
```html
<!--
     正确的。完整的元素名的结构符合如下模式：
     'block-name__element-name'
 -->
<form class="search-form">
    <div class="search-form__content">
        <input class="search-form__input"/>
        <button class="search-form__button"></button>
    </div>
</form>

 <!--
     不正确的。完整的元素名的结构不符合如下模式：
     'block-name__element-name'
 -->
<form class="search-form">
    <div class="search-form__content">
        <!-- 推荐：'search-form__input' 或者 'search-form__content-input' -->
        <input class="search-form__content__input"/>
        <!-- 推荐：'search-form__button' 或者 'search-form__content-button' -->
        <button class="search-form__content__button"></button>
    </div>
</form>
```
如果在模块名称上定义了命名空间，也要保证元素名称是依赖于模块的（block_elem）。
  在 DOM 树中，一个模块可以有元素嵌套结构
  
例子:
```html
<div class="block">
    <div class="block_elem1">
        <div class="block_elem2">
            <div class="block_elem3"></div>
        </div>
    </div>
</div>
```
然而，在 BEM 的方法论中，这样的模块结构通常表示为一个并列的元素列表

例子:
```html
.block {}
.block_elem1 {}
.block_elem2 {}
.block_elem3 {}
```
这样在代码中，你可以在不改变每个单独的元素的情况下改变一个模块的 DOM 结构

例子:
```html
<div class="block">
    <div class="block_elem1">
        <div class="block_elem1"></div>
    </div>
    <div class="block_elem3"></div>
</div>
```
模块的结构改变了，但是元素的规则和他们的名字仍然保持不变。

### 组成部分
一个元素总是一个模块的一部分，你不应该单独使用它。

例子:
```html
<!-- 正确的。元素都位于 'search-form' 模块内 -->
<!-- 'search-form' 模块 -->
<form class="search-form">
    <!-- 在 'search-form' 模块内的 'input' 元素 -->
    <input class="search-form__input" />
    <!-- 在 'search-form' 模块内的 'button' 元素 -->
    <button class="search-form__button"></button>
</form>

<!-- 不正确的。元素位于 'search-form' 模块的上下文之外 -->
<!-- 'search-form' 模块 -->
<form class=""search-block>
</form>

<!-- 在 'search-form' 模块内的 'input' 元素 -->
<input class="search-form__input"/>

<!-- 在 'search-form' 模块内的 'button' 元素 -->
<button class="search-form__button"></button>
```
### 可选性
一个元素是一个可选的模块组件。并不是所有的模块都必须有元素。

例子：
```html
<!-- 'search-form' 模块 -->
<div class="search-form">
    <!-- 'input' 模块 -->
    <input class="input"/>

    <!-- 'button' 模块 -->
    <button class="button"></button>
</div>
```
### 我应该创建一个模块还是一个元素？
1. 如果这段代码可能被重用，并且它不依赖于页面上的其他组件，那你应该创建一个模块。
2. 如果这段代码在没有父实体（模块）的情况下不能使用，那你应该创建一个元素。

为了简化开发，元素应该被分割成一小部分-子元素。在 BEM 方法论中，[你不能创建元素的元素](https://en.bem.info/methodology/quick-start/#nesting-1)，在这种情况下，你需要创建一个服务模块，而不是创建一个元素。

### 修饰符
一种用于定义模块和元素的外观，状态和行为的实体。

特征：

- [修饰符的名称](https://en.bem.info/methodology/naming-convention/#modifier-name)描述了它的外观（“多大？”或者“它的主题是什么？”等等—— `size_`s 或者 `theme_islands`），它的状态（“它与其他有什么不同？” —— `disabled`，`focused`，等等）以及他的行为（“它的行为什么？”或者“它如何响应用户？”——比如 `directions_left-top`）。
- 修饰符的名字与模块或者元素的名字使用单下划线分隔（_）
### 修饰符的类型
#### Boolean
- 当修饰符的存在或不存在是重要的，与它的值无关时使用这种类型的修饰符。比如：`disabled`。如果一个布尔类型的修饰符是可见的，它的值被假定为 `true`。
- 修饰符的全名的结构遵循如下模式：
- `block-name_modifier_name`
- `block-name__element-name_modifier-name`

例子:
```html
<!-- 'search-form' 模块有一个 ‘focused’ 的布尔类型的修饰符 -->
<form class="search-form search-form_focused">
    <input class="search-form__input"/>

    <!-- 'button' 元素有一个 'disabled' 的布尔类型修饰符 -->
    <button class="search-form__button search-form__button_disabled">Search</button>
</form>
```
#### 键-值
- 当修饰符的值是重要的使用键值对类型。例如：“一个 `islands` 设计主题的按钮”：`menu_theme_islands`。
- 这种类型的修饰符的全名的结构遵循如下模式：
- `block-name_modifier-name_modifier-value`
- `block-name__element-name_modifier-name_modifier-value`

例子:
```html
<!-- The `search-form` 模块有值为 'islands' 的 `theme` 修饰 -->
<form class="search-form search-form_theme_islands">
    <input class="search-form__input">

    <!-- The `button` 元素有值为 'm' 的 `size` 修饰 -->
    <button class="search-form__button search-form__button_size_m">Search</button>
</form>

<!-- 你不能同时使用两个具有不同值的的相同修饰符 -->
<form class="search-form
             search-form_theme_islands
             search-form_theme_lite">

    <input class="search-form__input">

    <button class="search-form__button
                   search-form__button_size_s
                   search-form__button_size_m">
        Search
    </button>
</form>
```
### 修饰符使用指南
### 一个修饰符不能被单独使用。
从 BEM 的角度，一个修饰符不能脱离模块或元素而被使用。一个修饰符应该改变实体的外观，行为或者状态，而不是替换它。

例子:
```html
<!-- 正确的。'search-form' 模块有值为 'islands' 的 'theme' 修饰符 -->
<form class="search-form search-form_theme_islands">
    <input class="search-form__input">

    <button class="search-form__button">Search</button>
</form>

<!-- 不正确的。'search-form' 丢失了 -->
<form class="search-form_theme_islands">
    <input class="search-form__input">

    <button class="search-form__button">Search</button>
</form>
```
> [为什么需要在元素或修饰符的名称上添写上模块的名称？](Why write the block name in the names of modifiers and elements?)

### 混合模式
一种在单一的 DOM 节点上使用不同 BEM 实体的技术。

混合模式允许你：

结合多个实体的行为和样式，而不是重复编写代码
在现有代码的基础上创建具有新语义的 UI 组件

例子：
```html
<!-- 'header' 模块 -->
<div class="header">
    <!--
        将 'header' 模块的 'search-form' 元素与 'search-form' 模块混合在一起使用
    -->
    <div class="search-form header__search-form"></div>
</div>
```
在这个例子中，我们将 `header` 模块的 `search-form` 元素与 `search-form` 模块的行为和样式结合在一起。这种方式允许我们在 `header__search-form` 元素上设置额外的形状和定位，而 `search-form` 模块仍然是通用的。因此，我们可以在任何环境中使用模块，因为模块没有指定任何填充。这正是我们可以独立调用模块的原因。

### 文件系统
在 BEM 方法论中采用的组件概念同样适用于项目的文件结构中。模块、元素和修饰符的实现可以被分在独立的文件中，这意味着，我们单独地使用它们。

特征：

- 一个单独的模块对应一个单独地目录
- 模块和其对应的目录拥有相同的名字。比如， `header` 模块放置在 `header/` 目录中，`menu` 模块放置在 `menu/` 目录中。
- 一个模块的实现分为单独的文件。比如， `header.css` 和 `header.js`。
- 模块目录是其元素和修饰所在目录的根目录。
- 元素目录的名称以双下划线（__）开始。比如，`header/__logo/` 和 `menu/__item`。
- 修饰目录的名称以单下划线（_）开始。比如，`header_fixed` 和 `menu/_theme_islands/`。
- 元素和修饰的实现分为不同的文件。比如，`header__input.js` 和 `header_theme_islands.css`。

例子：
```css
search-form/                           # Directory of the search-form

    __input/                           # Subdirectory of the search-form__input
        search-form__input.css         # CSS implementation of the
                                       # search-form__input element
        search-form__input.js          # JavaScript implementation of the
                                       # search-form__input element

    __button/                          # Subdirectory of the search-form__button element
        search-form__button.css
        search-form__button.js

    _theme/                            # Subdirectory of the search-form_theme modifier
        search-form_theme_islands.css  # CSS implementation of the search-form block
                                       # that has the theme modifier with the value
                                       # islands
        search-form_theme_lite.css     # CSS implementation of the search-form block
                                       # that has the theme modifier with the value
                                       # lite

search-form.css                        # CSS implementation of the search-form block
search-form.js                         # JavaScript implementation of the
                                       # search-form block
```
这样的文件结构可以很好地支持我们重用代码。

> 在生产环境中，这些分支的文件结构将会被组合成共享的文件。
    
遵循这样的文件结构并不是必须的。你可以使用任何可替代的项目结构，根据 BEM 原则来组织你的文件结构，比如：
- [Flat](https://en.bem.info/methodology/filestructure/#flat)
- [Flex](https://en.bem.info/methodology/filestructure/#flex)


