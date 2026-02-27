<!DOCTYPE html>
<html lang="zh">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>C语言格式化输出（printf）完全指南</title>
    <style>
        body {
            font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif;
            line-height: 1.6;
            color: #333;
            max-width: 1200px;
            margin: 0 auto;
            padding: 20px;
            background-color: #f9f9f9;
        }
        h1, h2, h3 {
            color: #2c3e50;
            border-bottom: 2px solid #3498db;
            padding-bottom: 8px;
            margin-top: 30px;
        }
        table {
            width: 100%;
            border-collapse: collapse;
            margin: 20px 0;
            background: white;
            box-shadow: 0 1px 3px rgba(0,0,0,0.1);
        }
        th {
            background-color: #3498db;
            color: white;
            padding: 10px;
            font-weight: 600;
        }
        td {
            padding: 10px;
            border: 1px solid #ddd;
            vertical-align: top;
        }
        tr:nth-child(even) {
            background-color: #f2f2f2;
        }
        code {
            background-color: #ecf0f1;
            padding: 2px 6px;
            border-radius: 4px;
            font-family: "SF Mono", Monaco, "Cascadia Code", "Roboto Mono", monospace;
            font-size: 0.95em;
            color: #c7254e;
        }
        pre {
            background-color: #2d2d2d;
            color: #f8f8f2;
            padding: 15px;
            border-radius: 5px;
            overflow-x: auto;
            font-family: "SF Mono", Monaco, "Cascadia Code", "Roboto Mono", monospace;
            font-size: 0.95em;
            line-height: 1.4;
        }
        pre code {
            background-color: transparent;
            color: inherit;
            padding: 0;
        }
        .note {
            background-color: #fff3cd;
            border-left: 4px solid #ffc107;
            padding: 12px 20px;
            margin: 20px 0;
            border-radius: 4px;
        }
        .example-output {
            color: #27ae60;
            font-weight: 500;
        }
        hr {
            border: 0;
            height: 1px;
            background: linear-gradient(to right, #ccc, #333, #ccc);
            margin: 30px 0;
        }
    </style>
</head>
<body>

<h1>C语言格式化输出（<code>printf</code>）完全指南</h1>

<h2>一、基本格式</h2>
<pre><code>%[flags][width][.precision][length]specifier</code></pre>
<p>每个部分均为可选，但<strong>类型说明符（specifier）必须存在</strong>。下面详细解释每个组成部分。</p>

<hr>

<h2>二、类型说明符（specifier）</h2>
<p>指定输出数据的类型，必须放在格式字符串的最后。</p>
<table>
    <thead>
        <tr>
            <th>说明符</th>
            <th>输出类型</th>
            <th>示例</th>
        </tr>
    </thead>
    <tbody>
        <tr><td><code>d</code> 或 <code>i</code></td><td>有符号十进制整数</td><td><code>printf("%d", 123);</code> → <span class="example-output">123</span></td></tr>
        <tr><td><code>u</code></td><td>无符号十进制整数</td><td><code>printf("%u", 456);</code> → <span class="example-output">456</span></td></tr>
        <tr><td><code>o</code></td><td>无符号八进制整数（无前导0）</td><td><code>printf("%o", 10);</code> → <span class="example-output">12</span></td></tr>
        <tr><td><code>x</code></td><td>无符号十六进制整数（小写字母）</td><td><code>printf("%x", 255);</code> → <span class="example-output">ff</span></td></tr>
        <tr><td><code>X</code></td><td>无符号十六进制整数（大写字母）</td><td><code>printf("%X", 255);</code> → <span class="example-output">FF</span></td></tr>
        <tr><td><code>f</code></td><td>十进制浮点数（小写），默认6位小数</td><td><code>printf("%f", 3.14);</code> → <span class="example-output">3.140000</span></td></tr>
        <tr><td><code>F</code></td><td>同 <code>%f</code>，但无穷大与NaN用大写</td><td><code>printf("%F", INFINITY);</code> → <span class="example-output">INF</span></td></tr>
        <tr><td><code>e</code></td><td>科学计数法（小写e）</td><td><code>printf("%e", 123.45);</code> → <span class="example-output">1.234500e+02</span></td></tr>
        <tr><td><code>E</code></td><td>科学计数法（大写E）</td><td><code>printf("%E", 123.45);</code> → <span class="example-output">1.234500E+02</span></td></tr>
        <tr><td><code>g</code></td><td>自动选择 <code>%f</code> 或 <code>%e</code>，不输出无意义零</td><td><code>printf("%g", 123.45);</code> → <span class="example-output">123.45</span></td></tr>
        <tr><td><code>G</code></td><td>自动选择 <code>%F</code> 或 <code>%E</code></td><td><code>printf("%G", 123.45);</code> → <span class="example-output">123.45</span></td></tr>
        <tr><td><code>a</code></td><td>十六进制浮点数（小写，C99）</td><td><code>printf("%a", 3.14);</code> → <span class="example-output">0x1.91eb86p+1</span></td></tr>
        <tr><td><code>A</code></td><td>十六进制浮点数（大写，C99）</td><td><code>printf("%A", 3.14);</code> → <span class="example-output">0X1.91EB86P+1</span></td></tr>
        <tr><td><code>c</code></td><td>单个字符</td><td><code>printf("%c", 'A');</code> → <span class="example-output">A</span></td></tr>
        <tr><td><code>s</code></td><td>字符串（以'\0'结尾）</td><td><code>printf("%s", "hello");</code> → <span class="example-output">hello</span></td></tr>
        <tr><td><code>p</code></td><td>指针地址（实现定义，通常十六进制）</td><td><code>printf("%p", &a);</code> → <span class="example-output">0x7ffd1234</span></td></tr>
        <tr><td><code>n</code></td><td>特殊：将已输出字符数存入对应整型指针</td><td><code>int count; printf("abc%n", &count);</code> → <code>count=3</code></td></tr>
        <tr><td><code>%</code></td><td>输出一个百分号</td><td><code>printf("%%");</code> → <span class="example-output">%</span></td></tr>
    </tbody>
</table>

<h2>三、标志（flags）</h2>
<p>可选，多个可同时使用，用于调整对齐、符号、填充等。</p>
<table>
    <thead>
        <tr>
            <th>标志</th>
            <th>作用</th>
            <th>示例（以整数123，宽度8为例）</th>
        </tr>
    </thead>
    <tbody>
        <tr><td><code>-</code></td><td><strong>左对齐</strong>（默认右对齐）</td><td><code>%-8d</code> → <span class="example-output">123     </span></td></tr>
        <tr><td><code>+</code></td><td>强制显示正负号（正数显示<code>+</code>，负数显示<code>-</code>）</td><td><code>%+d</code> → <span class="example-output">+123</span><br>（若宽度8：<code>%+8d</code> → <span class="example-output">    +123</span>）</td></tr>
        <tr><td>空格</td><td>正数前显示一个空格，负数前显示<code>-</code></td><td><code>% d</code> → <span class="example-output"> 123</span><br>（若宽度8：<code>% 8d</code> → <span class="example-output">     123</span>，注意空格占位）</td></tr>
        <tr><td><code>#</code></td><td>与<code>o</code>、<code>x</code>、<code>X</code>配合：显示进制前缀（<code>0</code>、<code>0x</code>、<code>0X</code>）<br>与浮点类型（<code>f</code>、<code>e</code>、<code>g</code>等）配合：即使小数部分为0也显示小数点</td><td><code>%#o</code>（255）→ <span class="example-output">0377</span><br><code>%#x</code>（255）→ <span class="example-output">0xff</span><br><code>%#f</code>（123.0）→ <span class="example-output">123.</span></td></tr>
        <tr><td><code>0</code></td><td>用<strong>零填充</strong>宽度（当指定宽度且未使用<code>-</code>时）</td><td><code>%08d</code> → <span class="example-output">00000123</span></td></tr>
    </tbody>
</table>
<div class="note">
    <strong>注意：</strong>
    <ul>
        <li><code>-</code> 和 <code>0</code> 同时出现时，<code>-</code> 优先，<code>0</code> 失效。</li>
        <li><code>+</code> 和 空格 同时出现时，<code>+</code> 优先。</li>
    </ul>
</div>

<h2>四、最小字段宽度（width）</h2>
<p>指定输出占用的最少字符数。若实际宽度不足，则用填充字符补齐（默认为空格，可用<code>0</code>标志改为零）。</p>
<table>
    <thead>
        <tr>
            <th>形式</th>
            <th>说明</th>
            <th>示例（整数123）</th>
        </tr>
    </thead>
    <tbody>
        <tr><td>数字（n）</td><td>固定宽度n</td><td><code>%5d</code> → <span class="example-output">  123</span></td></tr>
        <tr><td><code>*</code></td><td>宽度由参数列表中的整数指定（动态宽度）</td><td><code>printf("%*d", 5, 123);</code> → <span class="example-output">  123</span></td></tr>
    </tbody>
</table>

<h2>五、精度（.precision）</h2>
<p>以点 <code>.</code> 开头，后跟数字或 <code>*</code>。含义依类型而异。</p>
<table>
    <thead>
        <tr>
            <th>类型</th>
            <th>精度含义</th>
            <th>示例</th>
        </tr>
    </thead>
    <tbody>
        <tr><td><strong>整数</strong>（d,i,u,o,x,X）</td><td>数字的<strong>最小位数</strong>（不足时左侧补零）</td><td><code>%.5d</code>（123）→ <span class="example-output">00123</span><br><code>%.5x</code>（255）→ <span class="example-output">000ff</span></td></tr>
        <tr><td><strong>浮点数</strong>（f,F,e,E）</td><td>小数点后的位数</td><td><code>%.2f</code>（3.1415）→ <span class="example-output">3.14</span><br><code>%.0f</code>（3.1415）→ <span class="example-output">3</span></td></tr>
        <tr><td><strong>字符串</strong>（s）</td><td>最多输出的字符数</td><td><code>%.3s</code>（"hello"）→ <span class="example-output">hel</span></td></tr>
        <tr><td><strong>指针</strong>（p）</td><td>通常忽略精度</td><td></td></tr>
        <tr><td>使用 <code>*</code></td><td>精度由参数列表中的整数指定</td><td><code>printf("%.*s", 3, "hello");</code> → <span class="example-output">hel</span></td></tr>
    </tbody>
</table>
<div class="note">
    <strong>注意：</strong>
    <ul>
        <li>若精度与宽度同时指定，先应用精度截取/补零，再考虑宽度填充。</li>
        <li>精度为0且数值为0时，对于整数不输出任何数字（但可能受标志影响）。</li>
    </ul>
</div>

<h2>六、长度修饰符（length）</h2>
<p>用于指示参数的实际长度，与类型说明符搭配使用。</p>
<table>
    <thead>
        <tr>
            <th>修饰符</th>
            <th>作用</th>
            <th>示例</th>
        </tr>
    </thead>
    <tbody>
        <tr><td><code>hh</code></td><td>将整数参数视为 <code>signed char</code> 或 <code>unsigned char</code></td><td><code>printf("%hhd", (char)65);</code> → <span class="example-output">65</span></td></tr>
        <tr><td><code>h</code></td><td>视为 <code>short</code> 或 <code>unsigned short</code></td><td><code>printf("%hd", (short)123);</code> → <span class="example-output">123</span></td></tr>
        <tr><td><code>l</code></td><td>对于整数：视为 <code>long</code> 或 <code>unsigned long</code><br>对于浮点（C99）：视为 <code>double</code>（但<code>%lf</code>通常用于<code>double</code>）</td><td><code>printf("%ld", 123L);</code> → <span class="example-output">123</span><br><code>printf("%lf", 3.14);</code> → <span class="example-output">3.140000</span></td></tr>
        <tr><td><code>ll</code></td><td>视为 <code>long long</code> 或 <code>unsigned long long</code></td><td><code>printf("%lld", 123LL);</code> → <span class="example-output">123</span></td></tr>
        <tr><td><code>j</code></td><td>视为 <code>intmax_t</code> 或 <code>uintmax_t</code>（C99）</td><td><code>printf("%jd", (intmax_t)123);</code> → <span class="example-output">123</span></td></tr>
        <tr><td><code>z</code></td><td>视为 <code>size_t</code></td><td><code>printf("%zu", sizeof(int));</code> → <span class="example-output">4</span>（假设）</td></tr>
        <tr><td><code>t</code></td><td>视为 <code>ptrdiff_t</code></td><td><code>printf("%td", ptr1 - ptr2);</code> → 差值</td></tr>
        <tr><td><code>L</code></td><td>用于浮点，视为 <code>long double</code></td><td><code>printf("%Lf", 3.14L);</code> → <span class="example-output">3.140000</span></td></tr>
    </tbody>
</table>
<div class="note">
    <strong>注意：</strong>
    <ul>
        <li>长度修饰符必须与对应类型的数据匹配，否则行为未定义。</li>
        <li>对于浮点数，<code>%lf</code> 在C99之后合法，但<code>%f</code> 在<code>printf</code>中已可用于<code>double</code>（因为<code>float</code>会被自动提升为<code>double</code>）。<code>%Lf</code> 用于<code>long double</code>。</li>
    </ul>
</div>

<h2>七、动态宽度/精度</h2>
<p>使用 <code>*</code> 代替宽度或精度，实际值从参数列表中获取（顺序对应）。</p>
<pre><code>printf("%*.*f", 8, 2, 3.14159);  // 宽度8，精度2 → "    3.14"</code></pre>
<p>参数列表中先提供宽度，再提供精度，最后提供要输出的数据。</p>

<h2>八、返回值</h2>
<p><code>printf</code> 函数返回<strong>成功输出的字符总数</strong>（不包括字符串结尾的 <code>\0</code>）。若出错，返回一个负数。</p>
<pre><code>int len = printf("Hello, %d", 123);  // len = 10（"Hello, 123"共10个字符）</code></pre>

<h2>九、常见组合示例</h2>
<pre><code>#include &lt;stdio.h&gt;

int main() {
    int n = 255;
    float pi = 3.1415926;
    char *str = "Hello";

    printf("|%6d|\n", n);          // |   255|
    printf("|%-6d|\n", n);         // |255   |
    printf("|%06d|\n", n);         // |000255|
    printf("|%#x|\n", n);          // |0xff|
    printf("|%#X|\n", n);          // |0XFF|
    printf("|%6.2f|\n", pi);       // |  3.14|
    printf("|%6.3s|\n", str);      // |   Hel|
    printf("|%*.*s|\n", 8, 2, str);// |      He|
    printf("|%+d % d|\n", 123, 123); // |+123  123|
    printf("|%p|\n", &n);          // 输出地址，如0x7ffd1234
    printf("|%08.3d|\n", 42);      // |000042|（先精度补足3位→042，再宽度8零填充→000042）
    return 0;
}</code></pre>

<h2>十、注意事项</h2>
<ol>
    <li><strong>类型必须匹配</strong>：格式说明符与参数类型不匹配会导致未定义行为（可能输出错误值或崩溃）。</li>
    <li><strong><code>%s</code>要求字符串以'\0'结尾</strong>：否则会一直读取直到遇到零，可能导致越界。</li>
    <li><strong><code>%n</code>是危险的</strong>：它写入内存，若使用不当可能引发安全问题，一般不推荐。</li>
    <li><strong>浮点精度</strong>：默认精度为6（<code>%f</code>），若想不显示小数部分，可用<code>%.0f</code>。</li>
    <li><strong>宽度与填充</strong>：若输出内容宽度超过指定宽度，则宽度设置失效，输出完整内容。</li>
    <li><strong>位置参数</strong>：C语言不支持按位置指定参数（如<code>%2$d</code>），但POSIX扩展提供了此功能，不是标准C。</li>
</ol>

<hr>
<p style="text-align: right; color: #7f8c8d;">—— 完 ——</p>

</body>
</html>
