# 每周总结可以写在这里

ECMA 标准 + Unicode 组成了 JavaScript 语言的基石。

# 一般命令式编程语言构成

1. **Atom**
   - Identifier
   - Literal
2. **Expression**
   - Atom
   - Operator
   - Punctuator
3. **Statement**
   - Expression
   - Keyword
   - Punctuator
4. **Structure**
   - Function
   - Class
   - Process
   - Namespace
5. **Program**
   - Program
   - Mould
   - Package
   - Library

# Unicode

*ECMA-262.pdf*

># A.1 Lexical Grammar
>
>​	SourceCharacter ::
>
>​		any Unicode code point

Unicode官网：https://home.unicode.org/

FileFormat.Info：https://www.fileformat.info/

- Unicode：https://www.fileformat.info/info/unicode/index.htm

  - Unicode Blocks：https://www.fileformat.info/info/unicode/block/index.htm

    - Unicode Block 'Basic Latin' / List without images (fast) ：

      https://www.fileformat.info/info/unicode/block/basic_latin/list.htm

    - Unicode Block 'CJK Unified Ideographs' / List without images (fast)（中文）：

      https://www.fileformat.info/info/unicode/block/cjk_unified_ideographs/list.htm

  - Unicode Character Categories：https://www.fileformat.info/info/unicode/category/index.htm

# JavaScript 语法分析

```
InputElement
  WhiteSpace      空白符（空格、Tab）    无效的东西
  LineTerminator  换行符                无效的东西
  Comment                              无效的东西
  Token           JavaScript中一切有效的东西
    Identifier      标识符（变量、属性、方法等）  实际意义
    Literal         直接量                      实际意义
    Punctuator      符号                        程序结构
    keywords        关键字                      程序结构
```

InputElement

- WhiteSpace

  - \<TAB>：制表符 `CHARACTER TABULATION (U+0009)`

  - \<VT>：纵向制表符 `LINE TABULATION (U+000B)`

  - \<FF>：分页符 `FORM FEED (FF) (U+000C)`

  - \<SP>：普通空格 `SPACE`

  - \<NBSP>：非断行空格 `NO-BREAK SPACE (U+00A0)`

    >它是SP的一个变体，在文字排版中，可以避免因为空格在此处发生断行，其它方面和普通空格完全一样。多数的JS编辑环境都会把它当做普通空格（因为一般源代码编辑环境根本就不会自动折行……）

  - \<ZWNBSP>：旧称 \<BOM>，是 `ESC` 新加入的空白符 `ZERO WIDTH NO-BREAK SPACE (U+FEFF)`

- LineTerminator

  - \<LF>：`\n`
    - `U+000A` LINE FEED (LF)
  - \<CR>：回车 `\r`
    - `U+000D` CARRIAGE RETURN (CR)
  - \<LS>：分行符
    - `U+2028` LINE SEPARATOR
  - \<PS>：分段符
    - `U+2029` PARAGRAPH SEPARATOR

- Comment

  ```js
  //
  /**/
  
  // 注释里不能使用 \u 替换 *
  /\u002A*/
  
  // 也不可以嵌套
  /* 
  /* 
   */
   */
  ```

- Token

  - Identifier
  - Punctuator
  - Literal
  - keywords

  ```
  Punctuator      符号                        程序结构
  keywords        关键字                      程序结构
  
  Identifier      标识符（变量、属性）         实际意义
  Literal         直接量                      实际意义
  ```

  举个例子：

  ```js
  for (let i = 0; i < 129; i++) {
      document.write(
          `${i} <span style="background: lightgreen">${String.fromCharCode(i)}</span><br>`
      );
  }
  
  Punctuator：()、=、+、<、;、{}
  keywords  ：for、let
  Identifier：i、document、document.write
  Literal   ：0、129
  ```

# 关于直接量（Literal）

- Number
  - 存储 Uint8Array、Float64Array
  - 各种进制的写法
    - 二进制 0b
    - 八进制 0o
    - 十进制 0x
- String
  - Character
  - Code Point
  - Encoding
    - unicode 编码 - utf
      - utf-8 可变长度 （控制位的用处）
  - Grammar
    - `''`、`""`、``` `
- Boolean
  - `true`
  - `false`
- Null
- Undefind

# 本周作业

1.写一个正则表达式 匹配所有 Number 直接量

ECMA 的标准如下：

```
NumericLiteral ::
  DecimalLiteral
  BinaryIntegerLiteral
  OctalIntegerLiteral
  HexIntegerLiteral
```

- 整数：`/^-?\\d+$/`

- 浮点数：`/^(-?\\d+)(\\.\\d+)?$/`

- 二进制：`/^[01]+$/`

- 八进制：`/^[0-7]+$/`

- 十六进制：

  ```
  /(^0x[a-f0-9]{1,2}$)|(^0X[A-F0-9]{1,2}$)|(^[A-F0-9]{1,2}$)|(^[a-f0-9]{1,2}$)/
  ```

2.写一个 UTF-8 Encoding 的函数

```js
function encodeUtf8(text) {
    const code = encodeURIComponent(text);
    const bytes = [];
    for (var i = 0; i < code.length; i++) {
        const c = code.charAt(i);
        if (c === '%') {
            const hex = code.charAt(i + 1) + code.charAt(i + 2);
            const hexVal = parseInt(hex, 16);
            bytes.push(hexVal);
            i += 2;
        } else bytes.push(c.charCodeAt(0));
    }
    return bytes;
}
```

3.写一个正则表达式，匹配所有的字符串直接量，单引号和双引号

ECMA 的标准如下：

```
StringLiteral ::
  " DoubleStringCharacters opt "
  ' SingleStringCharacters opt '
```

