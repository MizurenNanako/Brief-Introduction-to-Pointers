<head>
<!--Heimu from moegirlpedia-->
<style>
.heimu,
  .heimu a,
  a .heimu,
  .heimu a.new {
    background-color: #252525;
    color: #252525;
    text-shadow: none;
  }
  .heimu:hover,
  .heimu:active,
  .heimu:hover .heimu,
  .heimu:active .heimu {
    color: rgba(230, 231, 234, 0.848) !important;
  }
  .heimu:hover a,
  a:hover .heimu,
  .heimu:active a,
  a:active .heimu {
    color: rgba(173, 216, 230, 0.887) !important;
  }
  .heimu:hover .new,
  .heimu .new:hover,
  .new:hover .heimu,
  .heimu:active .new,
  .heimu .new:active,
  .new:active .heimu {
    color: #BA0000 !important;
  }
  table {
    display: block;
    overflow-x: scroll;
}
</style>
</head>
<body>
<div style="width: 100%; min-width: 450px; max-width:800px">

> ref ver 2.4

<h1 id="h1">C语言指针指北</h1>
<table><tr><td width="80"> <a href="https://gitee.com/Tomnycui"><img id="i0" src="https://static.wikia.nocookie.net/cultistsimulator_gamepedia_en/images/e/e4/Knock.png/revision/latest/scale-to-width-down/100?cb=20180927063719"></a> </td> <td style="word-break:break-all"> <i>阅读本文，你将得到： <bk/> 对<b>内存</b>、<b>指针</b>、<b>字符串</b>、<b>函数调用和参数传递</b>、<b>文件操作</b> 的深刻理解。</i> </td> </tr></table>

<table style="max-width:800px">
<tr>
<td width="320">
<table>
<tr>
    <td colspan="2"><b><div align="center">C语言指针指北：目录</div></b></td>
</tr>
<tr><td width="50"><a href="#h1"><img src="https://static.wikia.nocookie.net/cultistsimulator_gamepedia_en/images/e/e4/Knock.png/revision/latest/scale-to-width-down/100?cb=20180927063719"></a></td><td>启：启之秘术之启</td></tr>
<tr><td><a href="#cp1"><img src="https://static.wikia.nocookie.net/cultistsimulator_gamepedia_en/images/e/ed/The_Byzantine_Tinct.png/revision/latest?cb=20190228222246"></a></td><td>第一节：(静态)内存模型的引入</td></tr>
<tr><td><a href="#cp2"><img src="https://static.wikia.nocookie.net/cultistsimulator_gamepedia_en/images/5/5a/Geminiadfucine.png/revision/latest?cb=20190311225032"></a></td><td>第二节(上)：指针！指针！</td></tr>
<tr><td><a href="#cp2s1"><img src="https://static.wikia.nocookie.net/cultistsimulator_gamepedia_en/images/e/ec/Influenceknockg.png/revision/latest/scale-to-width-down/260?cb=20190424212946"></a></td><td>第二节(下)：指针：觉醒</td></tr>
<tr><td><a href="#cp3"><img src="https://static.wikia.nocookie.net/cultistsimulator_gamepedia_en/images/2/2d/Manualdeparturevak.png/revision/latest/scale-to-width-down/260?cb=20190311225056"></a></td><td>第三节：指针、数组、字符串</td></tr>
</table>
</td>
<td>
<img src="https://static.wikia.nocookie.net/cultistsimulator_gamepedia_en/images/2/25/The_Wreck_of_the_Christabel.png/revision/latest/scale-to-width-down/260?cb=20190219094258">
<br/>
<blockquote><p>June the 28th, once again.</p></blockquote>
</td>
</table>

<h2 id="cp1">第一节：(静态)内存模型的引入</h2>
<table><tr><td width="80px"><a href="#h1"><img id="i1" src="https://static.wikia.nocookie.net/cultistsimulator_gamepedia_en/images/e/ed/The_Byzantine_Tinct.png/revision/latest?cb=20190228222246"></a> </td> <td style="word-break:break-all"> <i>C是<b>直接操作机器</b>的语言之一。因此，在讨论主要用于直接操作内存的指针前，需要先介绍静态的内存模型。</i> </td> </tr> </table>

### 什么是内存模型？

内存模型是一个抽象概念，用于描述特定的内存组织方式。C作为一种成熟的底层语言，具有最接近于物理内存组织方式的内存模型。

### 什么是 C 的（静态）内存模型？

引用一段 `cppreference.com` 的解释：
> **Memory model**
The data storage (memory) available to a C program is *one or more contiguous sequences of bytes*. Each byte in memory has a unique address.
**Bytes**
A byte is the smallest addressable unit of memory. It is defined as a contiguous sequence of bits. C supports bytes of  (normally) sizes 8 bits and greater.

<p align="right"><a href="https://en.cppreference.com/w/c/language/memory_model">from C Memory Model</a></p>

<font color="DeepPink"><b>TL;DR</b></font>
简单来说，C语境下的<ruby>内存<rt>memory</rt></ruby>被看成是一段连续的由<ruby>字节<rt>byte(s)</rt></ruby>组成的序列。其中 一般情况下一个字节相当于八个<ruby>比特<rt>bit(s)</rt></ruby>（`bit(s)`又译作“位”），每个字节都具有独一无二的<ruby>地址<rt>address</rt></ruby>，且是<ruby>连续<rt>正整数集内</rt></ruby>的。
抽象一下，我们得到了<b><ruby>字节<rt>byte</rt></ruby>的概念</b>：

- 字节是可访问的最小<ruby>内存单位<rt>memory unit</rt></ruby>；
- 字节的<ruby>地址<rt>address</rt></ruby>具有唯一性；
- 字节的<ruby>地址<rt>address</rt></ruby>具有<ruby>连续性<rt>在正整数集内的</rt></ruby>；

<font color="#71AEE2"><b>就这？</b></font>
<font color="#AC6E46"><b>就这。这已经是完整的（静态）内存模型的概念了。</b></font>
<font color="#71AEE2"><b>为什么强调“静态”？</b></font>
<font color="#AC6E46"><b>因为只介绍了<ruby>内存位置<rt>memory location</rt></ruby>而没有提及<ruby>内存顺序<rt>memory order</rt></ruby>，后者是多线程下的问题，希望你永远不会遇到。</b></font>
<font color="71AEE2"><b>所以介绍这个到底有什么用呢？<ruby>（半恼）<rt><i>赫尔墨特有急躁</i></rt></ruby></b></font>

<h2 id="cp2">第二节：指针！指针！</h2>
<table><tr><td width="80"><a href="#h1"><img id="i2" src="https://static.wikia.nocookie.net/cultistsimulator_gamepedia_en/images/5/5a/Geminiadfucine.png/revision/latest?cb=20190311225032"></a> </td><td> <i>The Pointers permits no seal and no isolation. It thrusts us gleefully out of the safety of ignorance.</i> </td></tr></table>

如上文所说，C作为强大的底层语言，具有直接操作硬件<small><span class="heimu">（尤其是内存）</span></small>的能力，这种能力的体现就是指针。
一切在内存上的数据<small><span class="heimu">（包括系统和其他程序的数据和本体）</span></small>都在指针的操作范围内。<small><span class="heimu">（一个著名的例子是 Cheat Engine 的内存修改）</span></small>

<font color="Gold"><b>[WARNING: ]</font> 接下来所提到的所有指针均指「<ruby>变量指针<rt>Pointer of Varible</rt></ruby>」并且简称为「指针」。对于「<ruby>函数指针<rt>Function Pointer</rt></ruby>」，我们将在最后一节提及。</b>
> 现在介绍函数指针还为时过早，我们不妨放到最后谈，在此之前如无特别注明，所有提及的指针都是变量指针，下面不引起混淆时简称指针。

首先先给出<ruby>变量指针<rt>Pointer of Varible</rt></ruby>的完整定义：

- 指针是一种整型变量，在64位系统上，指针等同于 `unsigned long long` ，大小为8 `byte`。
- 因为所有的变量指针都是 `unsigned long long`， 因此指针没有严格意义上的类型。
- <small>（接上条）</small>**通常意义下指针的所谓 “类型” 实际上是一个编译器标签。我们沿用 “类型” 这个称呼。**
- <small>（接上条）</small>因为所有<ruby>指针<rt>变量指针</rt></ruby>都是 `unsigned long long`，因此它们之间可以相互转换。
- 一般情况下，指针储存的值以“<ruby>地址<rt>address</rt></ruby>”的语义进行解释。

<font color="DeepPink"><b>THUS</b></font>
也就是说，指针不过是一种特殊变量，内部储存的还是整数，因此本质上指针也是一种整数变量——只不过它们所储存的整数具有「地址」的意义。

指针的工作原理源于内存模型。内存上的每个字节都有且仅有唯一确定的地址，因此要确定一个变量，只需要三个信息：

1. 该变量在内存上的位置；
2. 该变量所占据内存的长度（占了多少字节）；
3. 如何解释该变量。

其中第一条指的就是该变量的地址，而第二条和第三条指的就是该变量的类型——因为类型同时提供了「长度」和「如何处理」两个信息。

> 举个简单的例子，对于 `int` 类型，长度是 `4bytes`，处理方式是「按四字节长度的整数的处理方式处理」；对于 `double` 类型，长度是 `8bytes`，处理方式是「按双精度浮点数的处理方式处理」。

因此指针被设计成同时处理这两种信息的载体：

- 对于内存位置，根据字节地址的唯一确定性，**储存「变量所占据的第一个字节的地址」**。
- 对于长度和解释方式，在编译期处理——也就是前面提到的，使用形式上的“类型”来进行处理。
指针的“类型”和指针变量本身无关，而是用于解释被指向的内存区域——至于那块区域是否真的存在这样同类型的一个变量，实际上无法检查。<span class="heimu">因为「类型」这个概念本身就只存在于编译期。</span>

这三个信息，也就是一个指针，足以用于控制任意内存区域——以任意方式。
我们可以这样声明指针：

```cpp
<type> *<pointer_name> = <init_value>;
```

请原谅我使用这样严肃死板的写法，接下来是一些解释：

- `<type>` 是任意类型——你可以使用任何类型替代它，例如用 `int`；
- `<pointer_name>` 是将要被声明的指针的名字——这里用 `p` 来替代它；
- `<init_value>` 是初始化这个指针用的值。这不是必需的，但是建议使用 `NULL` 来进行初始化。

于是我们得到了这样的一行代码：`int *p = NULL;` 声明了名为 `p` 的指针，将以 `int` 的解释方式解释它所指向的内存区域，并且初始化——将它指向一个安全的、没有任何内容的、保留的地址：`NULL`。

> `NULL` 是一个常量，代表一个安全的内存地址，以避免出现「野指针」——一种没有进行初始化的指针，指向随机的位置。
用 `0` 或者 `nullptr` 替代 `NULL` 一般情况下都是允许的，但不提倡使用前者<span class="heimu">（尽管一般情况下 <code class="heimu">NULL</code> 的值实际上就是 <code class="heimu">0</code>，但这是由操作系统决定的）</span>，而后者是C++的关键字，不能在纯C下使用。

现在我们有指针了。

<h3 id="cp2s1">指针：觉醒</h3>
<table><tr><td width="80"><a href="#h1"><img id="i1" src="https://static.wikia.nocookie.net/cultistsimulator_gamepedia_en/images/e/ec/Influenceknockg.png/revision/latest/scale-to-width-down/260?cb=20190424212946"></a> </td> <td> <i>There is a Door that should not open, and even now its hinges are roused...... </i> </td> </tr> </table>

<h4>指针最基础的运用：储存地址</h4>

很显然，一个只储存了 `NULL` 这个啥用没有只能占位的地址的指针是干不出什么有用的事的，所以我们还得想办法搞几个变量的地址——十分方便地，C给我们提供了这样一个运算符：`&`，它的名字就叫<ruby>寻址算符<rt>Addressing Operator</rt></ruby>。
用法是这样的：`&<varible>;` 假设对变量 `int a = 123;` 使用这个算符：`&a;` 然后我们将得到一个值：`(&a)`，这是一个整数值：`a`在内存中的「绝对地址」。
很自然的，可以把这个地址存在指针里：

```cpp
int *p_int = &a;
float *p_float = &a;
short *p_short = &a;
```

<font color="#71AEE2"><b>等等，你怎么整了三个指针，还是不同类型的？</b></font>
<font color="#AC6E46"><b>因为类型对指针储存的地址没有意义，所以都一样，都可以存。</b></font>
<font color="#71AEE2"><b>但是指针的“类型”，不是会影响什么“解释方式”吗？那到底是什么？</b></font>
<font color="#AC6E46"><b>好问题！这就是接下来会谈到的部分。</b></font>

现在我们不但有指针，也获得了 `int` 类型的变量 `a` 的地址，已经具有充足的条件来操作 `a` 了。

<h4>指针最基础的运用：间接访问</h4>

因此我们引入第二个要介绍的新算符：`*`， 名字叫<b><ruby>间接访问算符<rt>Indirection Operator</rt></ruby></b>，简称「**间接算符**」。
这个算符只能用于指针，用法是这样的：`*<pointer>;` 。以上面的 `p_int` 为例子，使用这个算符：`*p_int;`，我们可以得到一个**引用**：`(*p_int)`，这个引用与 `a` 完全等价。

<font color="#71AEE2"><b>等等，等等，等等……<span style="font-size: 0.6cm">引用</span>？什么鬼什么鬼什么鬼<span style="font-size: 0.5cm">？<span style="font-size: 0.6cm">？<span style="font-size: 0.7cm">？</span></span></span></b></font>
<font color="#AC6E46"><b>别急，别急，我知道这听起来有点突兀，但是我们不妨看看下面的例子。</b></font>

<table><tr><td colspan=2><span style="font-size: 0.6cm">两种表达完全等价的例子：</span></td></tr><tr><td>

|表达1|表达2|
|---|---|
|<div style="width: 2cm">`a = 5;`</div>|<div style="width: 3.6cm">`(*p_int) = 5;`</div>|
|`a += 3;`|`(*p_int) += 3;`|
|`++a;`|`++(*p_int);`|

</td><td>
<table><tr><td>
<span style="font-size: 0.45cm">注：</span>
<div style="width: 4cm">在<br/><span><code>int a = 123;</code><br/><code>int *p_int = &a;</code></span><br/>的前提下。</div>
</td></tr></table>
</td></tr></table>

<font color="#71AEE2"><b>我想我理解「完全等价」了：<code>(*p_int)</code> 成为了 <code>a</code> 的一个功能完整的「替身」，所有对 <code>(*p_int)</code> 的操作实际上都会落到 <code>a</code> 上——这仅仅是因为 <code>p_int</code> 持有 <code>a</code> 的地址吗？<code>p_int</code> 是怎么知道 <code>a</code> 的类型的呢？<small><span class="heimu">（换言之，编译器怎么知道要用处理<code class="heimu">int</code>的方式处理<code class="heimu">a</code>呢？）</span></small></b></font>
<font color="#AC6E46"><b>事实上，它根本就不知道。</b></font>

当我们获取 `a` 的地址的时候，实际上我们只获取了 `a` 的地址，而丢失了所有其他能使它成为变量的信息：包括它的**名字**和**类型**，<small><span class="heimu">（「类型」包括了「大小（长度）」和「解释方式」两种信息）</span></small> 在只知道地址的情况下，我们是无法正确使用这块属于 `a` 的内存区域的。
但是我们可以指定如何解释它——使用指针的「类型」。<span class="heimu"><s>如果忘了，不妨翻翻前面的内容先复习一下。</s></span>
上面的例子中，使用的是定义为：`int *p;` 的指针，因此使用 `*p` 时，其整体的类型是 `int`。
同理，定义为：`float *p;` 的指针，使用 `*p` 时，其整体的类型是 `float`；
定义为：`short *p;` 的指针，使用 `*p` 时，其整体的类型是 `short`。

<font color="#71AEE2"><b>但是 `float` 和 `int` 根本就是两套解释方法啊？</b></font>
<font color="#AC6E46"><b>不管，只要是对 `float` 指针使用 `&` 得到的，就按 `float` 来解释。</b></font>

<font color="#71AEE2"><b>但是 `short` 长度上比 `int` 短了一半啊？</b></font>
<font color="#AC6E46"><b>因此被以 `short` 的形式使用的也只有 `int` 中最低位的两个字节。</b></font>

<font color="#71AEE2"><b>那如果使用的是比 `int` 还长的 `long long` 呢？</b></font>
<font color="#AC6E46"><b>不管，直接从地址确定的那个 `byte` 开始数，数够8个<small><span class="heimu">（<code class="heimu">long long</code> 长度为 <code class="heimu">8bytes</code>）</span></small>，当 `long long` 用。</b></font>

<font color="#71AEE2"><b>所以无论是什么类型的指针，间接访问的时候都是直接按地址找第一个字节，然后数够足够的字节吗？</b></font>
<font color="#AC6E46"><b>正是如此。</b></font>

<details>
  <summary>知识扩充：「为什么要用 <code>NULL</code> 初始化指针？」</summary>
  <blockquote>由于指针储存的值被当作地址解释，因此对指针「间接访问」时，操作会落实到对应的内存上。在没有初始化的时候，指针一开始指向的地址是不可预知的——可能是操作系统中的某个关键数据中的一部分——在这个位置写入新的值会导致操作系统崩溃。而 <code>NULL</code> 是一个约定俗成的位置，操作系统会保证对这个位置的任何操作都是无效的——例如间接访问 <code>NULL</code> 指向的内存<span class="heimu">（0号位置）</span>不会崩溃系统，但会崩溃你的程序。 </blockquote>
</details>

<details>
  <summary>知识扩充：「解除引用算符」</summary>
  <blockquote><ruby>解除引用算符<rt>Dereference Operator</rt></ruby>实际上就是<ruby>间接访问算符<rt>Indirection Operator</rt></ruby>，这实际上是一个历史问题。国内对「解除引用」的翻译往往各有千秋，甚至违反直觉，阻碍理解<span class="heimu">（因为C++中「引用」有特殊含义）</span>，因此这里不采用这个名称。</blockquote>
</details>

<details>
  <summary>知识扩充：所以到底什么是引用？</summary>
  <blockquote>
  <font color="#71AEE2"><b>可是即便如此，我还是不知道「引用」是什么意思。</b></font><br/>
  <font color="#AC6E46"><b>这得回归到一个基本问题：你想想，我们是怎么使用变量`a`的？</b></font><br/>
  <font color="#71AEE2"><b>蛤？这不是<span style="font-size: 0.5cm">直接使用</span>吗？</b></font><br/>
  <font color="#AC6E46"><b>没错，这是因为 `a` 有名字：`a`，这是它的符号，它的名字，我们用 `a` 来表示它，来表示内存上的一段区域，来表达这个概念：一个变量，并且是 `int` 类型的。</b></font><br/>
  <font color="#71AEE2"><b>难道还有没有名字的变量吗？</b></font><br/>
  <font color="#AC6E46"><b>正是如此。</b></font><br/>
  有一些变量本身，确实存在于内存之中，但是没有名字——因为它们并不是在编译期产生的，而是在运行期产生的——因此不能通过名字被直接使用。<br/>
  但他们是可以被使用的！<br/>
  只要知道它们的类型和地址，只需要声明一个对应的指针来指向这个地址，就可以间接访问它们的本体——这也是「间接访问运算符」的名称来源。<br/>
  <font color="#71AEE2"><b>所以变量名本身，也是一个引用？</b></font><br/>
  <font color="#AC6E46"><b>这不是严谨的，但是确实可以这样理解。</b></font><br/>
  <font color="#71AEE2"><b>但是既然运算结果是引用，为什么这个算符还叫「解除引用」？</b></font><br/>
  <font color="#AC6E46"><b>额……这是历史问题，事实上，叫<span style="font-size: 0.6cm">「间接访问运算符」</span>更准确。</b></font><br/>
  <font color="#71AEE2"><b>原来如此……</b></font>
  </blockquote>
</details>

#### 总结：

对于任意变量（包括指针），都具有以下操作：

|操作|语法|例子的返回值|例子|说明|
|---|---|---|---|---|
|寻址|<div style="width: 3cm">`&<varible>`</div>|<div style="width: 4.8cm">`address of <varible>`</div>|<div style="width: 5.5cm">`&a;`<br/> where `a` is an `int`</div>|<div style="width: 4.3cm">获得 `a` 的内存地址，一般是一个`unsigned long long`类型的值</div>|

对于指针，有如下操作：

|操作|语法|例子|说明|
|---|---|---|---|
|声明|<div style="width: 9cm">`<type> *<pointer_name> = <init_value>;`</div>|<div style="width: 5cm">`int *p = 0x26fa7139;`</div>|<div style="width: 10cm">声明变量 `p` 为对 `int` 型的指针，以十六进制值 `0x26fa7139` 初始化</div>|
|赋值|`<pointer> = <address>;`|`p = nullptr;`|<div style="width: 10cm">给 `p` 赋常量 `nullptr`</div>|
|<div style="width: 1cm">间接访问</div>|`*<pointer>;`|`*p`|间接访问 `p` 指向的内存区域 `(*p)`整体恢复变量名同等地位。|
|前递增|`++<pointer>;`|`++p`|<div style="width: 10cm">`p` 所指位置后移 `sizeof(<type>)` 个 `byte`</div>|
|后递增|`<pointer>++;`|`p++`|同上|
|前递减|`--<pointer>;`|`--p`|<div style="width: 10cm">`p` 所指位置前移 `sizeof(<type>)` 个 `byte`</div>|
|后递减|`<pointer>--;`|`p--`|同上|
|数加法|`<pointer> + <整数类型>;`|`p + 13`|<div style="width: 8cm">得到一个新指针，指向 `p` 所指位置后移 `13 * sizeof(<type>)` 个 `byte`</div>|
|数减法|`<pointer> - <整数类型>;`|`p - 42`|<div style="width: 8cm">得到一个新指针，指向 `p` 所指位置前移 `45 * sizeof(<type>)` 个 `byte`</div>|
|减法|`<pointer1> - <pointer2>;`|`p1 - p2` <br/>where `p1` and `p2` are pointers of same type. |<div style="width: 10cm">`p` 所指位置前移 `45 * sizeof(<type>)` 个 `byte`</div>|
|赋值加法|`<pointer> += <整数类型>;`|`p += 23`|<div style="width: 10cm">`p` 所指位置后移 `23 * sizeof(<type>)` 个 `byte`</div>|
|赋值减法|`<pointer> -= <整数类型>;`|`p -= 45`|<div style="width: 10cm">`p` 所指位置前移 `45 * sizeof(<type>)` 个 `byte`</div>|

注：`<type>` 为指针的“类型”。

<font color="#71AEE2"><b>好多运算符……但是除了前三个，其他的是什么意思呢？</b></font>
<font color="#AC6E46"><b>这就是我们下一节将要涉及的内容了。</b></font>

<h2 id="cp3"> 第三节：指针、数组、字符串 </h2>

<table><tr><td width="80"> <a href="#h1"><img id="i0" src="https://static.wikia.nocookie.net/cultistsimulator_gamepedia_en/images/2/2d/Manualdeparturevak.png/revision/latest/scale-to-width-down/260?cb=20190311225056"></a> </td> <td> <i> The Wound and the Threshold and the Revelation are all the Gate's aspects, and here is their secret doctrine...</i> </td> </tr></table>

> 指针出现的初衷是为了操作任意内存上的数据，因此，用尽量少的指针变量控制和管理尽量大的内存区域，就成为了一个必须解决的问题。
> 不妨回顾一下内存模型：
> 1. 字节是可访问的最小内存单位；
> 2. 字节的地址具有唯一性；
> 3. 字节的地址具有在正整数集内的连续性；
> 
> 其中，条件 (1) 已用于推出变量的本质；条件 (2) 建立了从地址到变量的映射关系<small><span class="heimu">（「寻址」和「间接访问」）</span></small>，而条件 (3) 还没有被使用。

从条件 (3) 我们可以推出：
> **3.1. 在已知一个地址 p 的条件下，p 减去一个不大于自身的正整数或加上一个任意大小的正整数后，得到的仍为合法的地址。**

这个性质用在指针上，就得到了指针的一个功能（性质）：**指针偏移**。

（未完待续）

## 第四节：地址的传递

未完待续

## 第五节：指针的第二种形态

未完待续

</div>
</body>