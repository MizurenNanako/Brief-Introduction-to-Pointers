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

> ref ver 5.1

<h1 id="h1">C语言指针指北</h1>
<table><tr><td width="80"> <a href="https://gitee.com/Tomnycui"><img id="i0" src="./Knock.png"></a> </td> <td style="word-break:break-all"> <i>阅读本文，你将得到： <bk/> 对<b>内存</b>、<b>指针</b>、<b>字符串</b>、<b>函数调用和参数传递</b>、<b>文件操作</b> 的深刻理解。</i> </td> </tr></table>

<table style="max-width:800px">
<tr>
<td width="">
<table>
<tr>
    <td colspan="2"><b><div align="center">C语言指针指北：目录</div></b></td>
</tr>
<tr><td width="50"><a href="#h1"><img src="./Knock.png"></a></td><td>启：启之秘术之启</td></tr>
<tr><td><a href="#cp1"><img src="./The_Byzantine_Tinct.png"></a></td><td style="word-break:break-all">第一节：<br/>(静态)内存模型的引入</td></tr>
<tr><td><a href="#cp2"><img src="./Geminiadfucine.png"></a></td><td>第二节(上)：<br/>指针！指针！</td></tr>
<tr><td><a href="#cp2s1"><img src="./Influenceknockg.png"></a></td><td>第二节(下)：<br/>指针：觉醒</td></tr>
<tr><td><a href="#cp3"><img src="./Manualdeparturevak.png"></a></td><td>第三节：<br/>指针、数组、字符串</td></tr>
<tr><td><a href="#cp4"><img src="。/../Azoth.png"></a></td><td>第四节：<br/>动态内存管理</td></tr>
<tr><td><a href="#cp5"><img src="./Consecrated_Lintel.png"></a></td><td>第五节：<br/>地址的传递</td></tr>
</table>
</td>
<td>
<img src="./The_Wreck_of_the_Christabel.png">
<br/>
<blockquote><p>June the 28th, once again.</p></blockquote>
</td>
</table>

<h2 id="cp1">第一节：(静态)内存模型的引入</h2>
<table><tr><td width="80px"><a href="#h1"><img id="i1" src="./The_Byzantine_Tinct.png"></a> </td> <td style="word-break:break-all"> <i>C是<b>直接操作机器</b>的语言之一。因此，在讨论主要用于直接操作内存的指针前，需要先介绍静态的内存模型。</i> </td> </tr> </table>

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
- 字节的<ruby>地址<rt>address</rt></ruby>具有<ruby>后继性<rt>在整数集内的</rt></ruby>；<span class="heimu"><details><summary>解释：「后继性」</summary><blockquote><p>「后继性」指一个元素有其「后继者」的性质。<br/>例如：`0` 的后继是 `1`，`1` 的后继是 `2`，……<br/><i>由于这条性质来自于自然数，因此不妨看看其来源：<a href="https://encyclopediaofmath.org/wiki/Peano_axioms">Encyclopedia of Math: Peano_axioms</a></i></p></blockquote></details></span>

<font color="#71AEE2"><b>就这？</b></font>
<font color="#AC6E46"><b>就这。这已经是完整的（静态）内存模型的概念了。</b></font>
<font color="#71AEE2"><b>为什么强调“静态”？</b></font>
<font color="#AC6E46"><b>因为只介绍了<ruby>内存位置<rt>memory location</rt></ruby>而没有提及<ruby>内存顺序<rt>memory order</rt></ruby>，后者是多线程下的问题，希望你永远不会遇到。</b></font>
<font color="71AEE2"><b>所以介绍这个到底有什么用呢？<ruby>（半恼）<rt><i>赫尔墨特有急躁</i></rt></ruby></b></font>

<h2 id="cp2">第二节：指针！指针！</h2>
<table><tr><td width="80"><a href="#h1"><img id="i2" src="./Geminiadfucine.png"></a> </td><td> <i>The Pointers permits no seal and no isolation. It thrusts us gleefully out of the safety of ignorance.</i> </td></tr></table>

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
<table><tr><td width="80"><a href="#h1"><img id="i1" src="./Influenceknockg.png"></a> </td> <td> <i>There is a Door that should not open, and even now its hinges are roused...... </i> </td> </tr> </table>

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

<details>
  <summary>知识扩充：「解除引用算符」</summary>
  <blockquote><ruby>解除引用算符<rt>Dereference Operator</rt></ruby>实际上就是<ruby>间接访问算符<rt>Indirection Operator</rt></ruby>，这实际上是一个历史问题。国内对「解除引用」的翻译往往各有千秋，甚至违反直觉，阻碍理解<span class="heimu">（因为C++中「引用」有特殊含义）</span>，因此这里不采用这个名称。</blockquote>
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

<table><tr><td width="80"> <a href="#h1"><img id="i0" src="./Manualdeparturevak.png"></a> </td> <td> <i> The Wound and the Threshold and the Revelation are all the Gate's aspects, and here is their secret doctrine...</i> </td> </tr></table>

> 指针出现的初衷是为了操作任意内存上的数据，因此，用尽量少的指针变量控制和管理尽量大的内存区域，就成为了一个必须解决的问题。
> 不妨回顾一下内存模型：
>  
> 1. 字节是可访问的最小内存单位；
> 2. 字节的地址具有唯一性；
> 3. 字节的地址具有在整数集内的后继性；
>  
> 其中，条件 (1) 已用于推出变量的本质；条件 (2) 建立了从地址到变量的映射关系<small><span class="heimu">（「寻址」和「间接访问」）</span></small>，而条件 (3) 还没有被使用。

从条件 (3) 我们可以推出：
> **3.1. 在已知一个地址 p 的条件下，p 减去一个不大于自身的正整数或加上一个任意大小的正整数后，得到的仍为合法的地址。**

这个性质用在指针上，就得到了指针的一个功能（性质）：**指针偏移**。

<font color="#71AEE2"><b>我明白了，前面的递增和递减运算符是用来计算指针偏移的！</b></font>
<font color="#AC6E46"><b>是的。不仅如此，数加和数减也是用于计算指针偏移的。</b></font>

**指针偏移本质上是地址的计算。**
为了说明这类操作可以怎么用，我们得先找一片连续的内存区域。在这里，选用数组的方式来实现。<details><summary>解释：「数组」</summary><blockquote><p><small>对于数组的认识，本来假定读者已经熟悉，但此处不妨作为一个复习。</small><br/><b><ruby>数组<rt>Array</rt></ruby></b>是一种在内存中连续分配<span class="heimu">（占有连续的内存区域）</span>的、固定长度的数据结构。显然：一个储存 `<type>` 类型变量的长度为 `N` 的数组占有内存大小为：<br/>`N * sizeof(<type>)`</p></blockquote></details>

先整一个数组：

```cpp
int array[114514] = {1, 9, 1, 9, 8, 1, 0};
```

就这样我们获得了一个名为 `array` 的，长度为 `114514`，第 `0~6` 个元素已经指定<small><span class="heimu">（依次为1、9、1、9、8、1、0）</span></small>的数组。

显然，我们可以取其中一个元素的地址出来看看：

```cpp
// 0 1 2 3 4 5 6 ......
// 1 9 1 9 8 1 0 ......
//     ↑Here
int *needle = &(array[2]);
```

显然现在 `*needle` 是 `1`，
同时有：

|表达式|值|表达式|值|
|---|---|---|---|
|`*(needle - 2)`|`1`|`*(needle + 2)`|`8`|
|`*(needle - 1)`|`9`|`*(needle + 3)`|`1`|
|`*(needle + 1)`|`9`|`*(needle + 4)`|`0`|

<font color="#71AEE2"><b>哇哦，这不是和那个方括号一样吗？</b></font>
<font color="#AC6E46"><b>实际上，`operator[]` 本身就会展开，例如 `array[5]` 会展开成 `*(array + 5)` 这种形式。</b></font>
<font color="#71AEE2"><b>我开始理解为什么C的数组下标是从 `0` 开始的了……</b></font>
<font color="#71AEE2"><b>慢着，你刚刚写的是 `*(array + 5)` 是吧？！！也就是说——`array` 也是指针？？？</b></font>
<font color="#AC6E46"><b>是的！`array` 指向的，就是数组的第一个元素。</b></font>
<font color="#AC6E46"><b>不仅如此，指针加整数的运算，是具有交换性的——也就是说，`array + 2` 等价于 `2 + array`。</b></font>
<font color="#71AEE2"><b>你的意思是说……我可以将 `array[2]` 写成 `2[array]` 吗？！</b></font>
<font color="#AC6E46"><b>完全可以。</b></font>

<font color="Gold"><b>[WARNING: ]</font> 接下来开始提及的内容请务必认真注意。</b>

注意到刚刚我们使用的数组下标是在定义范围内的。如果使用范围外的内存会发生什么？<small><span class="heimu">例如：数组下标越界、指针指向的地址是你不该动的……</span></small>
<blockquote><div align="center">我很想这么说：「什么都不会发生。」<br/>但我的运气实在是太坏了。</div><div align="right">——沃·兹基硕德</div></blockquote>

实际上，一切皆有可能，包括系统崩溃。
这是因为：内存是连续的，理论上合法的内存地址是 <code>0~2<sup>64</sup> - 1</code> ，因此只要是这个范围内的地址指向的 `byte`，都是可以访问的——唯一问题是这些 `byte` 是不是你能用的——例如别的程序（包括系统）的内存区域。<b>一旦动了你不该动的数据，很可能就是你的程序<font color="Orange" size="5">寄</font>或被你攻击的程序<font color="Orange" size="5">寄</font>。</b>

但是也有一个好消息：万一崩系统了，重启就好了。

<font color="#71AEE2"><b>不是，为什么我的程序会<font size="5">寄</font>？<font>¯\\\_(ツ)\_\/¯</font></b></font>
<font color="#AC6E46"><b>因为如果你尝试动操作系统的内存，对面有（很大）几率先下手为强。（笑）</b></font>
<font color="#71AEE2"><b>那我的内存不够用怎么办？ (┙>∧<)┙へ┻┻</b></font>

<details><summary>知识扩充：「慢着慢着，你还没讲字符串呢！」</summary><blockquote><s>出现在标题不代表会讲。</s><br/>字符串实际上是一种特殊的数组：

1. 所有元素都是 `char`；
2. 至少有一个元素为 `'\0'`。
<span class="heimu"><code class="heimu">'\0'</code> 是一个特殊字符，ascii值是 <code class="heimu">0</code>，又称0字符，用于标记字符串的结尾。</span>

满足以上两条的数组才能叫字符串。例如数组：

```cpp
char str_a[] = {'H', 'e', 'l', 'l', 'o', '\0'};
```

等价于 `char str_b[] = "Hello";`
</blockquote></details>

<h2 id="cp4"> 第四节：动态内存 </h2>
<table><tr><td width="80"><a href="#h1"><img src="。/../Azoth.png"></a> </td><td> <i>Perhaps it isn't the final solvent that the alchemists sought. But it will do, for our purposes.</i> </td></tr></table>

在第二节的时候，我们曾经提到过一种**运行期产生**的、**没有名字**的变量：
<blockquote>
<font color="#71AEE2"><b>难道还有没有名字的变量吗？</b></font><br/>
<font color="#AC6E46"><b>正是如此。</b></font><br/>
有一些变量本身，确实存在于内存之中，但是没有名字——因为它们并不是在编译期产生的，而是在<b>运行期产生的</b>——因此不能通过名字被直接使用。<br/>
</blockquote>

<font color="#71AEE2"><b>我想起来了——可是这又有什么用呢？</b></font>
<font color="#AC6E46"><b>这种动态获取的内存是很多数据结构<small><span class="heimu">（如：链表、树、图）</span></small>的重要实现方式，在这里我暂时不会深入讲解——但是总而言之，这大有用武之地。</b></font>
<font color="#71AEE2"><b>听起来有点意思，怎样获取这种动态的内存呢？</b></font>

操作系统是内存的管理者，所有内存理论上都是操作系统进行管理的——所以要获取一块内存，得向操作系统申请——这就要用到函数 `malloc`。
它是这样用的：`malloc(<N>);`其中 `<N>` 是要申请的 `byte` 的数目。
例如要申请`40`个`byte`：`malloc(40);`

<font color="#71AEE2"><b>好极啦！现在我们拿到了40个byte的使用权——诶，等等，怎么用呢？<br/>让我想想……没有变量名，有地址吗？</b></font>
<font color="#AC6E46"><b>有。</b></font>
<font color="#71AEE2"><b>在哪？</b></font>
<font color="#AC6E46"><b>刚刚丢了。</b></font>

实际上 `malloc` 申请完内存之后会返回一个地址，指向被申请区域的第一个`byte`，而我们为了接下来的使用，必须用一个指针去承接这个地址值。
考虑到`40`个`byte`可以看成是`10`个`int`，我们不妨用`int *`来承接这个地址。

```cpp
int *pointer; // 先整一个指针
pointer = malloc(40); // 申请，并保存得到的地址。
```

现在这个 `pointer` 可以当数组用，下标范围是 `[0, 10)` 内的整数。
也就是说 `pointer[0]`、`pointer[1]`、……、`pointer[9]` 都能用。
当然，写成指针偏移的形式更清晰：
`*pointer`、`*(pointer + 1)`、……、`*(pointer + 9)`

> 所有标准库里面的函数描述区间的时候都是左闭右开区间。

<font color="Gold"><b>[WARNING: ]</font> 接下来开始提及的内容请务必认真注意。</b>

无论什么时候使用动态内存，即使用 `malloc` 开出了内存空间，**一定要记得释放**。

释放的方法很简单：以刚刚开的内存空间为例，只需要知道那一整段空间的第一个`byte`的地址就可以完成释放——因为操作系统是用首地址来记录内存申请记录的。
刚刚我们把开出来的内存的首地址放在了 `pointer` 里，因此现在要：

```cpp
free(pointer); // 归还（释放）内存。
pointer = NULL; // 移开这个指针的指向。
```

这样就够了。

<details><summary>知识拓展：「归还后置<code>NULL</code>」</summary><blockquote>
因为<code>free</code>完之后，<code>pointer</code> 仍然指向原来的旧地址——一个已经失去所有权的地址——继续指向这个地址有误用的风险，这是要避免的。
使用已归还的和使用不属于自己的内存是一个性质的。
</blockquote></details>

<h2 id="cp5"> 第五节：地址的传递 </h2>
<table><tr><td width="80"><a href="#h1"><img src="./Consecrated_Lintel.png"></a> </td><td> <i>This is the skull of a door through which power has passed.</i> </td></tr></table>

<font color="DarkGreen"><b>[ATTENTION: ]</font> 本节内容将成为重要技巧。</b>

在开始阐述本节内容之前，我们需要先回顾一下函数传参的机制。

<details><summary>知识拓展：「函数传参机制」</summary><blockquote>
在有参数的函数被调用的时候，往往需要考虑传参。
例如对于函数 <code>z = f(x, y);</code>，其中 `z` 是返回值，`x` `y` 是本地的变量。
以一个具体的函数为例子：

```c
int f(int x, int y)
{
    return x + y;
}
```

当我们使用这个函数的时候：

```c
int a = 0, b = 2;
int c = f(x, y);
```

实际上过程是这样的：

1. 根据参数进行一一对应：`int x = a;`，`int y = b;` （注意这里 `x` 不是 `a`，是一个新变量，只不过获得了 a 的值——而 a 在 f 外面。），这一步之后，`x` 的值是 `0`，`y` 的值是 `2`。
2. 在 `f` 的内部，进行运算 `int unnamed = x + y;`，这里，运算完成后，隐藏变量 `unnamed` 的值是 `2`。
3. 结束 `f` 的存在，将返回值返回到原本的位置，于是 `c = f(0, 2);` 变成了 `c = 2`。

</blockquote></details><br/>

**函数传参永远只会进行值的传递**：你永远无法直接把「变量」的整个概念原封不动地传递进去。
实际上，传递进去的只是该变量在那一时刻的值——并用函数中的其中一个形式参数来承接这个值。
那么很显然，函数中被用于承载该值的形式参数，和原本的变量，在完成值传递之后就已经没有关系了。
**形式参数是一个独立的变量，和外部赋予它值的变量毫不相关**，任何对于形式参数的改动都不会对外部产生分毫的影响——这就是数学意义上的函数——除了计算之外没有副作用的函数。

这很好——除非你的函数的实现依赖于这种副作用。
例如，考虑函数，用于交换两个变量的值：

```c
void swap(int a, int b)
{
    int c = a; a = b; b = c;
}
```

但是这种作用无法传递到外面去，例如，使用 `swap(x, y);`，`x`、`y` 的值不会受到丝毫的影响。

<font color="#71AEE2"><b>啊淦，不会吧不会吧，参数传递全是按值传递？</b></font>
<font color="#AC6E46"><b>确实如此。（摊手）</b></font>
<font color="#71AEE2"><b>那不就没法修改外面的变量了吗？</b></font>
<font color="#AC6E46"><b>我们可以往别的方向想想——我们可不可以绕开这个机制？</b></font>

既然我们最终要操作的是变量，我们不妨想想它们的本质是什么——是一段内存区域——那么它们就肯定在内存上；更进一步地，很显然，所有变量自从开始存在，就会在内存上占有一段固定的区域——因此地址也是固定的。

**所以我们要传递的，是地址。**

刚刚的代码可以实现成这样：

```c
void swap(int *p_a, int *p_b)
{
    int c = *p_a; *p_a = *p_b; *p_b = c;
}
```

使用 `swap(&x, &y);` 来完成交换。

<font color="#71AEE2"><b>这也太作弊了吧？这是怎么做到修改了外面的变量的？`p_a` 和 `p_b` 不会影响到外面的 `&x` 和 `&y` 啊？</b></font>
<font color="#AC6E46"><b>确实没有，但我们本来想动的就是地址背后的东西——`x` 和 `y` 本身。操作 `p_a` 和 `p_b` <span class="heimu">像嘉心糖一样</span>什么用都没有，但是 `*p_a` 和 `*p_b` 真的就等价于外面的 `x` 和 `y` 啊。</b></font>

这种技巧更多用于数组的传递。
数组作为一种连续的内存区域，完成传递只需要三种信息：

1. 其中一个元素的地址
2. 基于该元素确定的下标范围
3. 所有元素的类型

完成这三种信息的传递，其中一种组合就是：

- 数组第一个元素的地址以及类型（地址+类型 也就是一个指针相当的信息）
- 从该元素开始计算的，数组长度。（由于是首地址，因此只需要记录总长，首地址前不再有元素。）

——实际上这就是数组本身的组织方式。

对于某些特殊的数组，如果已知用固定的元素作为结尾，例如 `0`，并且保证此元素在整个数组中必定存在且出现在结尾，那么就不需要额外传递数组长度——因为数组长度可以由数据本身给出——这类数组的典型例子就是字符串。<span class="heimu"><s>忘了就翻前面的知识扩充。</s></span>

未完待续

## 第六节：指针的第二种形态

未完待续

</div>
</body>
