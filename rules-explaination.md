### 一. 规则解释

#### 1. 规则的种类

大多数的规则是让你选择你想要什么、禁止什么，如`number-leading-zero: string - "always"|"never"`

- No rules

  但是，有少部分包含`*-no-*`的规则只能禁止。比`block-no-empty`, 这种规则没值，也没必要有值。

- Max-min rules

  还有一部分包含`*-max/min-*`的规则，用于设定一个限制。它的值是`int`。如：`number-max-precision`

- Whitespace rules

  规定用empty line，还是 a single space，还是a newline，还是 no space。

  这类规则的key的关键字是

  - 指定插入位置：`before`,`after`,`inside`

  - 指定插入什么：

    - `empty-line`, `space`,`newline`

  - 指定被插入的对象

    例如function、comment。也可以具体指定他们的更精细位置，如`function-comma`。

    标点符号还有：`comma（逗号）`,`colon(冒号)`,`semiconlon（分号）`,`open-brace（左大括号）`,`closing-brace（右大括号）`,`opening-parenthesis（左圆括号）`,`closing-parenthesis（右圆括号）`,`operator`,`orange-operator`，`parentheses(圆括号)`

  这类规则的key的组成是：被插入的对象+被插入对象再精细+空白符号类型+插入位置。

  如：`function-comma-space-after`

#### 2. 规则的组成

- key的组成

​	the thing + what the rule is checking -> number（数字）-leading-zero（要检验数字的什么）

- 值的组成

  - 一个option：primary option。

    allow，never，disallow

  - 多个option：

    [primary option, second option(obj)] -> "indentation": ["tab", { "except": ["value"] }]

    awlays-multi-line指的是css多行的时候，才校验。

    - second option
      - except：不包括。
        - after-custom-property
        - first-nested
        - after-declaration
        - after-comment

#### 3. 有趣的

stylelint里面只有empty-line-before，没有empty-line-after，这是因为每个元素都只负责和它前一个元素的距离。



### 二. 规则罗列

#### color 颜色规则

- [`color-no-invalid-hex`](https://stylelint.io/user-guide/rules/color-no-invalid-hex/) 无效的颜色


- color-named 是否允许用单词作为color的值。比如：red, black等

- color-no-hex 不允许16进制的颜色

  - error `a { color: #000 }`


  - right: `a { color: black }`

#### 字体 font family

- [`font-family-no-duplicate-names`](https://stylelint.io/user-guide/rules/font-family-no-duplicate-names/) 不允许重复定义名字
- [`font-family-no-missing-generic-family-keyword`](https://stylelint.io/user-guide/rules/font-family-no-missing-generic-family-keyword/) 不允许未设置默认的字体

#### 方法 function

- `function-calc-no-unspaced-operator` calc方法的参数中，操作符前后必须有空白字符

  - error

    `a { top: calc(1px+2px); }`


  - right

    `a { top: calc(1px + 2px); }`

-  `function-linear-gradient-no-nonstandard-direction`

  *Standard direction* 是以下其中一条：

  - 一个角度

  - to + 一个角落(top right) 或者一条边(top、right)

    如：to top、to top right

  常见的错误用法是只有角落/边，却没有加to; 或者没有加deg

  - error

    `.foo { background: linear-gradient(top, #fff, #000); }`

    `.foo { background: linear-gradient(to top top, #fff, #000); }`

    `.foo { background: linear-gradient(45, #fff, #000); }`

  - right

    `.foo { background: linear-gradient(to top, #fff, #000); }`

    `.foo { background: linear-gradient(45deg, #fff, #000); }`

- function-blacklist 可以使用的函数黑名单

- function-whitelist 可以使用的函数白名单

- function-url-no-scheme-relative 不允许**//**开头的地址

   url scheme - 网络协议，如ftp，http

  > A [scheme-relative url](https://url.spec.whatwg.org/#syntax-url-scheme-relative) is a url that begins with `//` followed by a host.

- functiton-url-scheme-blacklist 协议黑名单

- function-url-scheme-whitelist 协议白名单

#### String

- String-no-newline 在字符串中不允许空行

#### Unit

- unit-no-unknow 不允许未知的单位

  第一选项：true

  第二选项：`ignoreUnits: ["/regex/", "string"]`


- unit-blacklist 单位白名单
- unit-whitelist 单位黑名单

#### Keyframe

- [`keyframe-declaration-no-important`](https://stylelint.io/user-guide/rules/keyframe-declaration-no-important/): 在keyframe中不允许使用·important

  - error

    ```js
    @keyframes important1 {
      from {
        margin-top: 50px;
      }
      to {
        margin-top: 100px ! important;
      }
    }
    ```

#### Declaration 声明

- [declaration-bang-space-after](https://stylelint.io/user-guide/rules/declaration-bang-space-after/):在高优先级声明符号（!important中的!）后面指定一个空格或禁止留有空格

- [declaration-bang-space-before](https://stylelint.io/user-guide/rules/declaration-bang-space-before/): 在高优先级声明符号（!important中的!）前面指定一个空格或禁止留有空格

- [declaration-colon-newline-after](https://stylelint.io/user-guide/rules/declaration-colon-newline-after/): 在声明冒号后要求一个换行符或不允许空白块，**建议：**["always-multi-line"](https://stylelint.io/user-guide/rules/declaration-colon-newline-after/#always-multi-line)

  - ["always"](https://stylelint.io/user-guide/rules/declaration-colon-newline-after/#always)

    ```
    a { 
      color: 
        pink; 
    }
    ```

  - ["always-multi-line"](https://stylelint.io/user-guide/rules/declaration-colon-newline-after/#always-multi-line)

  ```
  a { 
    box-shadow: 
      0 0 0 1px #5b9dd9, 
      0 0 2px 1px rgba(30, 140, 190, 0.8); 
  } 
  a { 
    color: pink; 
  }
  ```

- [declaration-colon-space-after](https://stylelint.io/user-guide/rules/declaration-colon-space-after/): 在声明冒号后要求一个空格或禁止空格

- [declaration-colon-space-before](https://stylelint.io/user-guide/rules/declaration-colon-space-before/):在声明冒号前面要求一个空格或禁止空格

- [declaration-empty-line-before](https://stylelint.io/user-guide/rules/declaration-empty-line-before/): 在属性声明之前要求或禁止空行



#### Declaration block

- [`declaration-block-no-duplicate-properties`](https://stylelint.io/user-guide/rules/declaration-block-no-duplicate-properties/) 在样式块中不允许重复的属性

  - error

    `a { color: pink; color: orange; }`

- [`declaration-block-no-shorthand-property-overrides`](https://stylelint.io/user-guide/rules/declaration-block-no-shorthand-property-overrides/) 样式块中不允许使用**属性值缩写**来覆盖属性

  - error

    ```css
    a {
      border-top-width: 1px;
      top: 0;
      bottom: 3px;
      border: 2px solid blue;
    }
    ```

  - right

    ```css
    a {
        padding: 10px;
        padding-left: 20px;
    }
    ```

- [declaration-block-semicolon-newline-after](https://stylelint.io/user-guide/rules/declaration-block-semicolon-newline-after/): 声明之间的分号后指定或禁止一个换行符

- [declaration-block-semicolon-newline-before](https://stylelint.io/user-guide/rules/declaration-block-semicolon-newline-before/): 声明之间的分号前面指定或禁止一个换行符

- [declaration-block-semicolon-space-after](https://stylelint.io/user-guide/rules/declaration-block-semicolon-space-after/):  声明之间的分号后面指定或禁止一个空格

- [declaration-block-semicolon-space-before](https://stylelint.io/user-guide/rules/declaration-block-semicolon-space-before/): 声明之间的分号前面指定或禁止一个空格

- [declaration-block-trailing-semicolon](https://stylelint.io/user-guide/rules/declaration-block-trailing-semicolon/): 在声明块最后一个声明指定或禁止分号

#### Block

- block-no-empty 不允许空白的块
- [block-closing-brace-empty-line-before](https://stylelint.io/user-guide/rules/block-closing-brace-empty-line-before/): 在块结束符号( '}' )前面要求或者禁止空行
- [block-closing-brace-newline-after](https://stylelint.io/user-guide/rules/block-closing-brace-newline-after/): 在块结束符号( '}' )后面要求或者禁止空行
- [block-closing-brace-newline-before](https://stylelint.io/user-guide/rules/block-closing-brace-newline-before/): 在块结束符号( '}' )前面规定一个换行或者禁止换行
- [block-closing-brace-space-after](https://stylelint.io/user-guide/rules/block-closing-brace-space-after/): 在块结束符号( '}' )前面规定一个换行或者禁止换行
- [block-closing-brace-space-before](https://stylelint.io/user-guide/rules/block-closing-brace-space-before/): 在块结束符号( '}' )前面规定一个空格或者禁止空格
- [block-opening-brace-newline-after](https://stylelint.io/user-guide/rules/block-opening-brace-newline-after/): 在块开始符号( '{' )后面规定一个换行或者禁止换行，option自行查阅
- [block-opening-brace-newline-before](https://stylelint.io/user-guide/rules/block-opening-brace-newline-before/): 在块开始符号( '{' )前面规定一个换行或者禁止换行，option自行查阅
- [block-opening-brace-space-after](https://stylelint.io/user-guide/rules/block-opening-brace-space-after/): 在块开始符号( '{' )后面规定一个空格或者禁止空格，option自行查阅
- [block-opening-brace-space-before](https://stylelint.io/user-guide/rules/block-opening-brace-space-before/): 在块开始符号( '{' )前面面规定一个空格或者禁止空格

#### Selector

- [`selector-pseudo-class-no-unknown`](https://stylelint.io/user-guide/rules/selector-pseudo-class-no-unknown/) 不允许使用未知的伪类
- [`selector-pseudo-element-no-unknown`](https://stylelint.io/user-guide/rules/selector-pseudo-element-no-unknown/) 不允许使用未知的伪元素
- [`selector-type-no-unknown`](https://stylelint.io/user-guide/rules/selector-type-no-unknown/) 不允许使用未知的元素类型
- [selector-attribute-brackets-space-inside](https://stylelint.io/user-guide/rules/selector-attribute-brackets-space-inside/): 在属性选择器的括号之间要求空格或者禁止空格
- [selector-attribute-operator-space-after](https://stylelint.io/user-guide/rules/selector-attribute-operator-space-after/): 属性选择器中在操作符（‘=’）后面要求或者禁止空格
- [selector-attribute-operator-space-before](https://stylelint.io/user-guide/rules/selector-attribute-operator-space-before/): 属性选择器中在操作符（‘=’）前面要求或者禁止空格
- [selector-attribute-quotes](https://stylelint.io/user-guide/rules/selector-attribute-quotes/): 属性选择器中属性值要求或者禁止加单引号或双引号
- [selector-combinator-space-after](https://stylelint.io/user-guide/rules/selector-combinator-space-after/): 在组合选择器之间的符号后要求空格或者禁止空格
- [selector-combinator-space-before](https://stylelint.io/user-guide/rules/selector-combinator-space-before/): 在组合选择器之间的符号前面要求空格或者禁止空格
- [selector-descendant-combinator-no-non-space](https://stylelint.io/user-guide/rules/selector-descendant-combinator-no-non-space/): 组合选择器之前禁止多余的空格或空行
- [selector-pseudo-class-case](https://stylelint.io/user-guide/rules/selector-pseudo-class-case/): 伪类选择器小写或大写
- [selector-pseudo-class-parentheses-space-inside](https://stylelint.io/user-guide/rules/selector-pseudo-class-parentheses-space-inside/): 伪类选择器的括号之间需要或者禁止空格
- [selector-pseudo-element-case](https://stylelint.io/user-guide/rules/selector-pseudo-element-case/): 伪元素选择器小写或大写
- [selector-pseudo-element-colon-notation](https://stylelint.io/user-guide/rules/selector-pseudo-element-colon-notation/): 伪元素单或双冒号
- [selector-type-case](https://stylelint.io/user-guide/rules/selector-type-case/): 类型选择器小写或大写

#### Selector list

- [selector-list-comma-newline-after](https://stylelint.io/user-guide/rules/selector-list-comma-newline-after/): .在选择器列表的逗号后指定一个换行符或禁止留有空格
- [selector-list-comma-newline-before](https://stylelint.io/user-guide/rules/selector-list-comma-newline-before/): 在选择器列表的逗号前面指定一个换行符或禁止留有空格，option自行查阅
- [selector-list-comma-space-after](https://stylelint.io/user-guide/rules/selector-list-comma-space-after/): 在选择器列表的逗号后指定一个空格或禁止留有空格，option自行查阅
- [selector-list-comma-space-before](https://stylelint.io/user-guide/rules/selector-list-comma-space-before/): 在选择器列表的逗号前指定一个空格或禁止留有空格，option自行查阅

#### Media feature

- media-feature-name-no-unknown 不允许使用未知的媒体查询

  - error `@media screen and (unknown: 10px) {}`
  - right `@media (min-width: 700px) {}`

- [media-feature-colon-space-after](https://stylelint.io/user-guide/rules/media-feature-colon-space-after/): 在媒体特性的冒号（@media (max-width:600px) {} 中的：）后指定一个空格或禁止留有空格

- [media-feature-colon-space-before](https://stylelint.io/user-guide/rules/media-feature-colon-space-before/): 在媒体特性的冒号（@media (max-width:600px) {} 中的：）前面指定一个空格或禁止留有空格

- [media-feature-name-case](https://stylelint.io/user-guide/rules/media-feature-name-case/): 媒体特性名称小写或大写

- [media-feature-parentheses-space-inside](https://stylelint.io/user-guide/rules/media-feature-parentheses-space-inside/): 媒体特征的括号之间需要一个空格或者禁止空格。

  [Options](https://stylelint.io/user-guide/rules/media-feature-parentheses-space-inside/#options)

  - ["always"](https://stylelint.io/user-guide/rules/media-feature-parentheses-space-inside/#always)

    ```
    @media ( max-width: 300px ) {}  
    ```

  - ["never"](https://stylelint.io/user-guide/rules/media-feature-parentheses-space-inside/#never)

    ```css
    @media (max-width: 300px) {}
    ```

- [media-feature-range-operator-space-after](https://stylelint.io/user-guide/rules/media-feature-range-operator-space-after/): 在媒体特性的运算符（@media (width>=600px) {} 中的>=）后指定一个空格或禁止留有空格

- [media-feature-range-operator-space-before](https://stylelint.io/user-guide/rules/media-feature-range-operator-space-before/): 在媒体特性的运算符（@media (width>=600px) {} 中的>=）前面指定一个空格或禁止留有空格

#### Media query list

- [media-query-list-comma-newline-after](https://stylelint.io/user-guide/rules/media-query-list-comma-newline-after/): 在媒体查询列表的逗号（@media screen and (color), projection and (color) {}中的，）后指定一个换行符或禁止留有空格
- [media-query-list-comma-newline-before](https://stylelint.io/user-guide/rules/media-query-list-comma-newline-before/): 在媒体查询列表的逗号（@media screen and (color), projection and (color) {}中的，）前面指定一个换行符或禁止留有空格
- [media-query-list-comma-space-after](https://stylelint.io/user-guide/rules/media-query-list-comma-space-after/):在媒体查询列表的逗号后指定一个空格或禁止留有空格
- [media-query-list-comma-space-before](https://stylelint.io/user-guide/rules/media-query-list-comma-space-before/): 在媒体查询列表的逗号前指定一个空格或禁止留有空格

#### Rule

- [rule-empty-line-before](https://stylelint.io/user-guide/rules/rule-empty-line-before/): 声明块之间前面要求或者禁止空行

#### At-rule

- At-rule-no-unknow 不允许未知的@使用
  - error `@unknown {}`
  - right `@charset "UTF-8"`
- [at-rule-empty-line-before](https://stylelint.io/user-guide/rules/at-rule-empty-line-before/): 在@规则前要求或禁止一个空行
- [at-rule-name-case](https://stylelint.io/user-guide/rules/at-rule-name-case/): 规定@规则名大写或者小写
- [at-rule-name-newline-after](https://stylelint.io/user-guide/rules/at-rule-name-newline-after/): 在@规则名后面要求一个换行 "always"|"always-single-line"
- [at-rule-name-space-after](https://stylelint.io/user-guide/rules/at-rule-name-space-after/): 在@规则名后面要求一个空格 
- [at-rule-semicolon-newline-after](https://stylelint.io/user-guide/rules/at-rule-semicolon-newline-after/): 在@规则分号后面要求一个换行 "always"
- [at-rule-semicolon-space-before](https://stylelint.io/user-guide/rules/at-rule-semicolon-space-before/): 在@规则分号前面要求或者禁止空格

#### Comment

- comment-no-empty 不允许空白的注释
- [comment-empty-line-before](https://stylelint.io/user-guide/rules/comment-empty-line-before/): 在注释后面要求或禁止空行
- [comment-whitespace-inside](https://stylelint.io/user-guide/rules/comment-whitespace-inside/): 在注释里面要求或者禁止空格

#### General/Sheet

- no-descending-specificity 不允许选择器权重递减书写

  - error 

    ```css
    .container a {
      top: 10px;
    }
    a {
      top: 0;
    }
    ```

  - right 

    ```css
    a {
      top: 0;
    }
    .container a {
      top: 10px;
    }
    ```

- no-duplicate-at-import-rules 不允许重复的导入（@import）

  - error

    ```css
    @import "a.css";
    @import 'a.css';
    ```

- no-duplicate-selectors 不允许重复的选择器

  - error

    ```css
    a { color: red; }
    a { font-size: 12px; }
    ```

  - right

    ```css
    a {
     color: red;
     font-size: 12px;
    }
    ```

- no-empty-source  不允许空白字符

  - error `\n`, `\t\t`

- no-extra-semicolons 不允许额外的封号（一个就够了）

- no-invalid-double-slash-comments 不允许双斜杠注释（因为css中没有双斜杠注释）

#### Number

- Number-max-precision 限制小数点的最大精度（小数点后几位）

  若设置为2

  - error `a { top: 3.2544px }`
  - right `a { top: 3.22px }`

#### Time

- Time-min-milliseconds 能设置的最小毫秒数

#### Shorthand property

- shorthand-property-no-redundant-values 缩写的属性值中不允许有冗余的值(能缩写就缩写)。
  - error `a { padding: 10px 10px }`

#### Value

- value-no-vendor-prefix 属性值不允许带有浏览器前缀
  - error `a { display: -webkit-flex; }`
  - right `a { dispaly: flex; }`
- [value-keyword-case](https://stylelint.io/user-guide/rules/value-keyword-case/):  对属性值规定大写或者小写  string: "lower"|"upper"，  **建议： lower**

#### Value list

- [value-list-comma-newline-after](https://stylelint.io/user-guide/rules/value-list-comma-newline-after/): 在值列表的逗号后面指定一个换行符或禁止留有空格， 

  **options：**["always"](https://stylelint.io/user-guide/rules/value-list-comma-newline-after/#always), ["always-multi-line"](https://stylelint.io/user-guide/rules/value-list-comma-newline-after/#always-multi-line), ["never-multi-line"](https://stylelint.io/user-guide/rules/value-list-comma-newline-after/#never-multi-line)

- [value-list-comma-newline-before](https://stylelint.io/user-guide/rules/value-list-comma-newline-before/): 在值列表的逗号前面指定一个换行符或禁止留有空格

  **options**：同上

- [value-list-comma-space-after](https://stylelint.io/user-guide/rules/value-list-comma-space-after/): 在值列表的逗号后指定一个空格或禁止留有空格

  [**Options**](https://stylelint.io/user-guide/rules/value-list-comma-space-after/#options):["always"](https://stylelint.io/user-guide/rules/value-list-comma-space-after/#always), ["never"](https://stylelint.io/user-guide/rules/value-list-comma-space-after/#never), ["always-single-line"](https://stylelint.io/user-guide/rules/value-list-comma-space-after/#always-single-line), ["never-single-line"](https://stylelint.io/user-guide/rules/value-list-comma-space-after/#never-single-line)

- [value-list-comma-space-before](https://stylelint.io/user-guide/rules/value-list-comma-space-before/): 在值列表的逗号前指定一个空格或禁止留有空格

  [**Options**](https://stylelint.io/user-guide/rules/value-list-comma-space-after/#options):同上

- [value-list-max-empty-lines](https://stylelint.io/user-guide/rules/value-list-max-empty-lines/): Limit the number of adjacent empty lines within value lists.限制相邻的数量值列表内空行最大数量

  [**Options**](https://stylelint.io/user-guide/rules/value-list-comma-space-after/#options): int型（0，1.....）

#### Custom property

- custom-property-pattern  为自定义属性指定一个格式。

  若设置为`"foo-.+"`

  - error： `root { --boo-bar: 0; }`
  - right：`:root { --foo-bar: 0; }`

- [custom-property-empty-line-before](https://stylelint.io/user-guide/rules/custom-property-empty-line-before/): 在用户自定义属性前面不允许或要求空行

#### Property

- property-blacklist 属性key白名单

- property-whitelist 属性key白名单

- Property-no-vendor-prefix 属性key不允许有浏览器前缀

- property-no-unknown 不允许未知的属性

- [property-case](https://stylelint.io/user-guide/rules/property-case/): Specify lowercase or uppercase for properties. 规定属性要么大写要么小写，  **建议： lower**

  [**Options**](https://stylelint.io/user-guide/rules/property-case/#options)**：**["lower"](https://stylelint.io/user-guide/rules/property-case/#lower)，["upper"](https://stylelint.io/user-guide/rules/property-case/#upper)

#### General / Sheet

- [indentation](https://stylelint.io/user-guide/rules/indentation/): 指定缩进
  - [Options](https://stylelint.io/user-guide/rules/indentation/#options) int型，2，4，6...
  - [Optional secondary options](https://stylelint.io/user-guide/rules/indentation/#optional-secondary-options)
    - [indentInsideParens: "twice"|"once-at-root-twice-in-block"](https://stylelint.io/user-guide/rules/indentation/#indentinsideparens-twiceonce-at-root-twice-in-block)
    - [indentClosingBrace: true|false](https://stylelint.io/user-guide/rules/indentation/#indentclosingbrace-truefalse)
    - [except: ["block", "param", "value"\]](https://stylelint.io/user-guide/rules/indentation/#except-block-param-value)
    - [ignore: ["inside-parens", "param", "value"\]](https://stylelint.io/user-guide/rules/indentation/#ignore-inside-parens-param-value)
      - ["inside-parens"](https://stylelint.io/user-guide/rules/indentation/#inside-parens)
      - ["param"](https://stylelint.io/user-guide/rules/indentation/#param)
      - ["value"](https://stylelint.io/user-guide/rules/indentation/#value)
- [max-empty-lines](https://stylelint.io/user-guide/rules/max-empty-lines/): 限制相邻的空行数
  - [Options](https://stylelint.io/user-guide/rules/max-empty-lines/#options) int型
  - [Optional secondary options](https://stylelint.io/user-guide/rules/max-empty-lines/#optional-secondary-options)
    - [ignore: ["comments"\]](https://stylelint.io/user-guide/rules/max-empty-lines/#ignore-comments)
- [max-line-length](https://stylelint.io/user-guide/rules/max-line-length/):限制每行的长度
  - [Options](https://stylelint.io/user-guide/rules/max-empty-lines/#options) int型
  - [Optional secondary options](https://stylelint.io/user-guide/rules/max-line-length/#optional-secondary-options)
    - [ignore: ["non-comments"\]](https://stylelint.io/user-guide/rules/max-line-length/#ignore-non-comments)
    - [ignore: ["comments"\]](https://stylelint.io/user-guide/rules/max-line-length/#ignore-comments)
    - [ignorePattern: "/regex/"](https://stylelint.io/user-guide/rules/max-line-length/#ignorepattern-regex)
- [no-eol-whitespace](https://stylelint.io/user-guide/rules/no-eol-whitespace/): 禁止行尾留有空白
  - [Options](https://stylelint.io/user-guide/rules/no-eol-whitespace/#options)
    - [true](https://stylelint.io/user-guide/rules/no-eol-whitespace/#true)
  - [Optional secondary options](https://stylelint.io/user-guide/rules/no-eol-whitespace/#optional-secondary-options)
    - [ignore: ["empty-lines"\]](https://stylelint.io/user-guide/rules/no-eol-whitespace/#ignore-empty-lines)
      - ["empty-lines"](https://stylelint.io/user-guide/rules/no-eol-whitespace/#empty-lines)
- [no-missing-end-of-source-newline](https://stylelint.io/user-guide/rules/no-missing-end-of-source-newline/): 要求样式表后面一个空行
