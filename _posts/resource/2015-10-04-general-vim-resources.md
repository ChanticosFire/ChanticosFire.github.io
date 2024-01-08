---
layout: post
title: Vim 常用资源
category: 资源
tags: Vim
keywords: Vim
---

[Vim](http://www.vim.org/) 8.2 快速参考备忘单的有用集合，可帮助您更快地学习 vim 编辑器。

## [](https://ref.15926.tech/docs/vim#%E5%85%A5%E9%97%A8)入门

### [](https://ref.15926.tech/docs/vim#%E8%BF%90%E5%8A%A8%E5%9B%BE)运动图

```bash
▼/▶ 光标   ▽/▷ 目标
```

#### [](https://ref.15926.tech/docs/vim#%E5%B7%A6%E5%8F%B3%E5%8A%A8%E4%BD%9C)左右动作

```bash
╭┈┈┈┈┈┈┈┈┈┈┈┈┈ |      
├┈┈┈┈┈┈┈┈┈┈┈┈┈ 0      $ ┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈╮
┆  ╭┈┈┈┈┈┈┈┈┈┈ ^      fe ┈┈┈┈┈┈┈┈╮       ┆
┆  ┆  ╭┈┈┈┈┈┈┈ Fo     te ┈┈┈┈┈┈┈╮┆       ┆
┆  ┆  ┆╭┈┈┈┈┈┈ To     30| ┈┈┈╮  ┆┆       ┆
┆  ┆  ┆┆ ╭┈┈┈┈ ge     w ┈┈┈╮ ┆  ┆┆       ┆
┆  ┆  ┆┆ ┆ ╭┈┈ b      e ┈╮ ┆ ┆  ┆┆       ┆
┆  ┆  ┆┆ ┆ ┆  ╭h      l╮ ┆ ┆ ┆  ┆┆       ┆
▽  ▽  ▽▽ ▽ ▽  ▽▼      ▼▽ ▽ ▽ ▽  ▽▽       ▽
   echo "A cheatsheet from quickReference"
```

#### [](https://ref.15926.tech/docs/vim#%E4%B8%8A%E4%B8%8B%E5%8A%A8%E4%BD%9C)上下动作

```bash
                  - SCREEN 1 START
   ╭┈┬┈┈┈┈┈┈┈┈┈▷  #!/usr/bin/python
   ┆ ┆     ╭┈┈┈▷      
   ┆ ┆     ┆      print("Hello")
   ┆ ┆     { } ▶  print("Vim")
   ┆ ┆       ┆    print("!")
   ┆ ┆       └┈▷     
   ┆ ┆ ╭┈┈┈┬┈┈┈▷  print("Welcome")
G gg H M L k j ▶  print("to")
┆        ┆   └┈▷  print("Quick Reference")
┆        ┆        print("/vim")
┆        ┆     
┆        ╰┈┈┈┈┈▷ 
┆                 - SCREEN 1 END
╰┈┈┈┈┈┈┈┈┈┈┈┈┈┈▷  print("SCREEN 2")
```

### [](https://ref.15926.tech/docs/vim#%E5%8A%A8%E4%BD%9C)动作

|   |   |
|---|---|
|`h` \| `j` \| `k` \| `l`|方向键|
|`<C-u>` _/_ `<C-d>`|上/下半页|
|`<C-b>` _/_ `<C-f>`|向上/向下翻页|

#### [](https://ref.15926.tech/docs/vim#%E5%AD%97%E8%AF%8D)字(词)

|   |   |
|---|---|
|`b` _/_ `w`|上一个/下一个单词|
|`ge` _/_ `e`|上一个/下一个词尾|

#### [](https://ref.15926.tech/docs/vim#%E8%A1%8C)行

|   |   |
|---|---|
|`0` _(zero)_ _/_ `$`|行的开始/结束|
|`^`|行开头 _(非空白)_|

#### [](https://ref.15926.tech/docs/vim#%E5%AD%97%E7%AC%A6%E4%B8%B2)字符串

|   |   |
|---|---|
|`Fe` _/_ `fe`|移动到上一个/下一个`e`|
|`To` _/_ `to`|在上一个/下一个`o`之前/之后移动|
|`\|` _/_ `n\|`|转到第一个/`n`列|

#### [](https://ref.15926.tech/docs/vim#%E6%96%87%E6%A1%A3)文档

|   |   |
|---|---|
|`gg` _/_ `G`|第一行/最后一行|
|`:n` \| `nG`|转到第 `n` 行|
|`}` _/_ `{`|下一个/上一个空行|

#### [](https://ref.15926.tech/docs/vim#%E7%AA%97%E5%8F%A3)窗口

|   |   |
|---|---|
|`H` _/_ `M` _/_ `L`|上/中/下屏幕|
|`zt` _/_ `zz` _/_ `zb`|上/中/下这条线|

### [](https://ref.15926.tech/docs/vim#%E6%8F%92%E5%85%A5%E6%A8%A1%E5%BC%8F)插入模式

|   |   |
|---|---|
|`i` _/_ `a`|在光标之前/之后插入|
|`I` _/_ `A`|插入行的开始/结束|
|`o` _/_ `O` _(letter)_|在下方/上方插入新行|
|`s` _/_ `S`|删除字符/行并插入|
|`C` _/_ `cc`|更改到/当前行的结尾|
|`gi`|在最后一个插入点插入|
|`Esc` \| `<C-[>`|退出插入模式|

### [](https://ref.15926.tech/docs/vim#%E4%BF%9D%E5%AD%98%E5%92%8C%E9%80%80%E5%87%BA)保存和退出

|   |   |
|---|---|
|`:w`|保存|
|`:q`|关闭文件|
|`:wq` \| `:x` \| `ZZ`|保存并退出|
|`:wqa`|保存并退出所有文件|
|`:q!` \| `ZQ`|强制退出|
|`:qa`|关闭所有文件|
|`:qa!`|强制退出所有文件|
|`:w` new.txt|写入`new.txt`|
|`:sav` new.txt|保存并编辑`new.txt`|
|`:w` !sudo tee %|写入只读文件|

### [](https://ref.15926.tech/docs/vim#%E6%AD%A3%E5%B8%B8%E6%A8%A1%E5%BC%8F)正常模式

|   |   |
|---|---|
|`r`|替换一个字符|
|`R`|进入替换模式|
|`u` _/_ `3u`|撤消更改`1`/`3`次|
|`U`|在一行上撤消更改|
|`J`|加入下一行|
|`<C-r>` _/_ 5 `<C-r>`|重做更改`1`/`5`次|

### [](https://ref.15926.tech/docs/vim#%E5%89%AA%E5%88%87%E5%92%8C%E7%B2%98%E8%B4%B4)剪切和粘贴

|   |   |
|---|---|
|`x`|删除字符 _(剪切)_|
|`p` _/_ `P`|在之后/之前粘贴|
|`xp`|交换两个字符|
|`D`|删除到行尾 _(剪切)_|
|`dw`|删除单词 _(剪切)_|
|`dd`|删除线 _(剪切)_|
|`ddp`|交换两条线|
|`yy`|拉线 _(复制)_|
|`"*p` \| `"+p`|从系统剪贴板粘贴|
|`"*y` \| `"+y`|粘贴到系统剪贴板|

#### [](https://ref.15926.tech/docs/vim#%E5%9C%A8%E5%8F%AF%E8%A7%86%E5%8C%96%E6%A8%A1%E5%BC%8F%E4%B8%8B)在可视化模式下

|   |   |
|---|---|
|`d` _\|_ `x`|删除选择 _(剪切)_|
|`s`|替换选择|
|`y`|Yank 选择 _(复制)_|

### [](https://ref.15926.tech/docs/vim#%E9%87%8D%E5%A4%8D)重复

|   |   |
|---|---|
|`.`|重复上一个命令|
|`;`|重复最新的 `f`、`t`、`F` 或 `T`|
|`,`|重复最新的`f`、`t`、`F`或`T`颠倒|
|`&`|重复最后一个`:s`|
|`@:`|重复命令行命令|

### [](https://ref.15926.tech/docs/vim#%E5%8F%AF%E8%A7%86%E5%8C%96%E6%A8%A1%E5%BC%8F)可视化模式

|   |   |
|---|---|
|`v`|进入可视化模式|
|`V`|进入视线模式|
|`<C-v>`|进入可视块模式|
|`ggVG`|选择所有文本|
|`>` _/_ `<`|向右/向左移动文本|

### [](https://ref.15926.tech/docs/vim#%E5%AE%8F)宏

|   |   |
|---|---|
|`qi`|录制宏 `i`|
|`q`|停止录制宏|
|`@i`|运行宏`i`|
|`7@i`|运行宏 `i` 7 次|
|`@@`|重复上一个宏|

您可以为任何字母保存宏，而不仅仅是 `i`

## [](https://ref.15926.tech/docs/vim#vim-%E8%BF%90%E7%AE%97%E7%AC%A6)Vim 运算符

### [](https://ref.15926.tech/docs/vim#%E7%94%A8%E6%B3%95)用法

|   |   |
|---|---|
|`d`|w|
|运算符|动作|

将 [可用运算符](https://ref.15926.tech/docs/vim#%E5%8F%AF%E7%94%A8%E8%BF%90%E7%AE%97%E7%AC%A6) 与 [动作](https://ref.15926.tech/docs/vim#%E5%8A%A8%E4%BD%9C) 结合使用以使用它们

### [](https://ref.15926.tech/docs/vim#%E5%8F%AF%E7%94%A8%E8%BF%90%E7%AE%97%E7%AC%A6)可用运算符

|   |   |
|---|---|
|`d`|删除|
|`y`|Yank _(复制)_|
|`c`|更改 _(删除然后插入)_|
|`p`|粘贴|
|`=`|格式代码|
|`g~`|切换案例|
|`gU`|大写|
|`gu`|小写|
|`>`|右缩进|
|`<`|左缩进|
|`!`|通过外部程序过滤|

### [](https://ref.15926.tech/docs/vim#%E4%BE%8B%E5%AD%90)例子

|组合|说明|
|:--|---|
|`d`d|删除当前行|
|`d`j|删除两行|
|`d`w|删除到下一个单词|
|`d`b|删除到单词的开头|
|`d`fa|删除直到 `a` 字符|
|`d`/hello|删除直到 `hello`|
|`c`c|更改当前行，与 `S` 同义|
|`y`y|复制当前行|
|`>`j|缩进 2 行|
|gg`d`G|删除完整的文档|
|gg`=`G|缩进一个完整的文档|
|gg`y`G|复制整个文档|

### [](https://ref.15926.tech/docs/vim#%E8%AE%A1%E6%95%B0)计数

```shell
[数字] <运算符> <动作>
<运算符> [数字] <动作>
```

---

|   |   |
|---|---|
|2`d`d|删除 `2` 行|
|6`y`y|复制 `6` 行|
|`d`3w|删除 `3` 字|
|`d`5j|向下删除 `5` 行|
|`>`4k|向上缩进 `4` 行|

## [](https://ref.15926.tech/docs/vim#vim-%E6%96%87%E6%9C%AC%E5%AF%B9%E8%B1%A1)Vim 文本对象

### [](https://ref.15926.tech/docs/vim#%E7%94%A8%E6%B3%95-1)用法

|   |   |
|---|---|
|`v`|i _/_ a|
|Operator|inner(内部) _/_ around(周围)|

文本块内部或周围使用 [operator](https://ref.15926.tech/docs/vim#%E5%8F%AF%E7%94%A8%E8%BF%90%E7%AE%97%E7%AC%A6) 进行操作

### [](https://ref.15926.tech/docs/vim#%E6%96%87%E6%9C%AC%E5%AF%B9%E8%B1%A1)文本对象

|   |   |
|---|---|
|p|段落|
|w|单词|
|W|WORD  <br>_(被空格包围)_|
|s|句子|
|[ ( { <|[]、() 或 {} 块|
|] ) } >|[]、() 或 {} 块|
|' " `|带引号的字符串|
|b|一个块 [(|
|B|[{中的一个块|
|t|一个 HTML 标签块|

查看 `:help text-objects`

### [](https://ref.15926.tech/docs/vim#%E5%88%A0%E9%99%A4)删除

|   |   |
|---|---|
|`d`iw|删除内词|
|`d`is|删除内句|
|`d`i"|引号中删除|
|`d`a"|删除引号 _(包括引号)_|
|`d`ip|删除段落|

### [](https://ref.15926.tech/docs/vim#%E9%80%89%E6%8B%A9)选择

|   |   |
|---|---|
|`v`i"|选择内引号“`...`{.underline}”|
|`v`a"|选择引号`"..."`{.underline}|
|`v`i[|选择内括号 [`...`{.underline}]|
|`v`a[|选择括号`[...]`{.underline}|
|`v`iw|选择内词|
|`v`ip|选择内部段落|
|`v`ipip|选择更多段落|

### [](https://ref.15926.tech/docs/vim#%E6%9D%82%E9%A1%B9)杂项

|   |   |
|---|---|
|`c`iw|换内字|
|`c`i"|更改内部引号|
|`c`it|更改内部标签 (HTML)|
|`c`ip|更改内部段落|
|`y`ip|Yank 段落|
|`y`ap|Yank 段落 _(包括换行符)_|

## [](https://ref.15926.tech/docs/vim#vim-%E5%A4%9A%E4%B8%AA%E6%96%87%E4%BB%B6)Vim 多个文件

### [](https://ref.15926.tech/docs/vim#buffers-%E7%BC%93%E5%86%B2%E5%99%A8)Buffers (缓冲器)

|   |   |
|---|---|
|`:e file`|在新缓冲区中编辑文件|
|`:bn`|转到下一个缓冲区|
|`:bp`|转到上一个缓冲区|
|`:bd`|从缓冲区列表中删除文件|
|`:b 5`|打开缓冲区 #5|
|`:b file`|按文件转到缓冲区|
|`:ls`|列出所有打开的缓冲区|
|`:sp file`|打开和拆分窗口|
|`:vs file`|打开和垂直拆分窗口|
|`:hid`|隐藏此缓冲区|
|`:wn`|写入文件并移至下一个|
|`:tab ba`|将所有缓冲区编辑为选项卡|

### [](https://ref.15926.tech/docs/vim#%E7%AA%97%E5%8F%A3-1)窗口

|   |   |
|---|---|
|`<C-w>` `s`|拆分窗口|
|`<C-w>` `v`|垂直拆分窗口|
|`<C-w>` `w`|切换窗口|
|`<C-w>` `q`|退出一个窗口|
|`<C-w>` `T`|拆分成一个新标签|
|`<C-w>` `x`|用下一个交换当前|
|`<C-w>` `-` _/_ `+`|减少/增加高度|
|`<C-w>` `<` _/_ `>`|减少/增加宽度|
|`<C-w>` `\|`|最大宽度|
|`<C-w>` `=`|同样高和宽|
|`<C-w>` `h` _/_ `l`|转到左/右窗口|
|`<C-w>` `j` _/_ `k`|转到上/下窗口|

### [](https://ref.15926.tech/docs/vim#%E9%80%89%E9%A1%B9%E5%8D%A1)选项卡

|   |   |
|---|---|
|`:tabe [file]`|E在新选项卡中编辑文件|
|`:tabf [file]`|如果在新选项卡中存在则打开|
|`:tabc`|C失去当前选项卡|
|`:tabo`|关闭o其他选项卡|
|`:tabs`|列出所有标签|
|`:tabr`|转到第一个r标签|
|`:tabl`|转到 last 选项卡|
|`:tabm 0`|我转到位置 `0`|
|`:tabn`|转到 next 选项卡|
|`:tabp`|转到p上一个标签|

#### [](https://ref.15926.tech/docs/vim#%E6%AD%A3%E5%B8%B8%E6%A8%A1%E5%BC%8F-1)正常模式

|   |   |
|---|---|
|`gt`|转到 next 选项卡|
|`gT`|转到p上一个标签|
|`2gt`|转到标签编号 `2`|

## [](https://ref.15926.tech/docs/vim#vim-%E6%90%9C%E7%B4%A2%E5%92%8C%E6%9B%BF%E6%8D%A2)Vim 搜索和替换

### [](https://ref.15926.tech/docs/vim#%E6%90%9C%E7%B4%A2)搜索

|   |   |
|---|---|
|`/foo`|向前搜索|
|`/foo\c`|向前搜索 _(不区分大小写)_|
|`?foo`|向后搜索|
|`/\v\d+`|使用 [regex](https://ref.15926.tech/docs/regex.html) 搜索|
|`n`|下一个匹配的搜索模式|
|`N`|上一个匹配的搜索|
|`*`|向前搜索当前单词|
|`#`|向后搜索当前单词|

### [](https://ref.15926.tech/docs/vim#%E6%9B%B4%E6%8D%A2%E8%A1%8C)更换行

```vim
:[range]s/{pattern}/{str}/[flags]
```

---

|   |   |
|---|---|
|`:s/old/new`|先更换|
|`:s/old/new/g`|全部替换|
|`:s/\vold/new/g`|全部替换为 [regex](https://ref.15926.tech/docs/regex.html)|
|`:s/old/new/gc`|全部替换_(确认)_|
|`:s/old/new/i`|先忽略大小写替换|
|`:2,6s/old/new/g`|在 `2`-`6` 行之间替换|

### [](https://ref.15926.tech/docs/vim#%E6%9B%BF%E6%8D%A2%E6%96%87%E4%BB%B6)替换文件

```vim
:%s/{pattern}/{str}/[flags]
```

---

|   |   |
|---|---|
|`:%s/old/new`|先更换|
|`:%s/old/new/g`|全部替换|
|`:%s/old/new/gc`|全部替换 _(确认)_|
|`:%s/old/new/gi`|全部替换 _(忽略大小写)_|
|`:%s/\vold/new/g`|全部替换为 [regex](https://ref.15926.tech/docs/regex.html)|

### [](https://ref.15926.tech/docs/vim#%E8%8C%83%E5%9B%B4)范围

|   |   |
|---|---|
|`%`|整个文件|
|`’<,’>`|当前选择|
|`5`|第 `5` 行|
|`5,10`|第 `5` 行到第 `10` 行|
|`$`|最后一行|
|`2,$`|第 `2` 行到最后|
|`.`|当前行|
|`,3`|接下来的 `3` 行|
|`-3,`|转发 `3` 行|

### [](https://ref.15926.tech/docs/vim#%E5%85%A8%E5%B1%80%E5%91%BD%E4%BB%A4)全局命令

```vim
:[range]g/{pattern}/[command]
```

---

|   |   |
|---|---|
|`:g/foo/d`|删除包含 `foo` 的行|
|`:g!/foo/d`|删除不包含 `foo` 的行|
|`:g/^\s*$/d`|删除所有空行|
|`:g/foo/t$`|将包含 `foo` 的行复制到 EOF|
|`:g/foo/m$`|将包含 `foo` 的行移动到 EOF|
|`:g/^/m0`|反转文件|
|`:g/^/t.`|复制每一行|

### [](https://ref.15926.tech/docs/vim#inverse%E9%80%86-g)Inverse(逆) :g

```vim
:[range]v/{pattern}/[command]
```

---

|   |   |
|---|---|
|`:v/foo/d`|删除不包含`foo`的行  <br>_(还有`:g!/foo/d`)_|

### [](https://ref.15926.tech/docs/vim#flags%E6%A0%87%E5%BF%97)Flags(标志)

|   |   |
|---|---|
|`g`|替换所有出现|
|`i`|忽略大小写|
|`I`|不要忽略大小写|
|`c`|确认每个替换|

### [](https://ref.15926.tech/docs/vim#%E6%9B%BF%E6%8D%A2%E8%A1%A8%E8%BE%BE%E5%BC%8F%E9%AD%94%E6%9C%AF)替换表达式（魔术）

|   |   |
|---|---|
|`&` _\|_ `\0`|替换为整个匹配的|
|`\1`...`\9`|替换为 0-9 组|
|`\u`|大写下一个字母|
|`\U`|后面的大写字符|
|`\l`|小写下一个字母|
|`\L`|后面的字符小写|
|`\e`|`\u`、`\U`、`\l` 和 `\L` 的结尾|
|`\E`|`\u`、`\U`、`\l` 和 `\L` 的结尾|

### [](https://ref.15926.tech/docs/vim#%E4%BE%8B%E5%AD%90-1)例子

```shell
:s/a\|b/xxx\0xxx/g               # 将 "a b"      修改为 "xxxaxxx xxxbxxx"
:s/test/\U& file/                # 将 "test"     修改为 "TEST FILE"
:s/\(test\)/\U\1\e file/         # 将 "test"     修改为 "TEST file"
:s/\v([abc])([efg])/\2\1/g       # 将 "af fa bg" 修改为 "fa fa gb"
:s/\v\w+/\u\0/g                  # 将 "bla bla"  修改为 "Bla Bla"
:s/\v([ab])|([cd])/\1x/g         # 将 "a b c d"  修改为 "ax bx x x"
:%s/.*/\L&/                      # 将 "HTML"     修改为 "html"
:s/\v<(.)(\w*)/\u\1\L\2/g        # 将单词的每个首字母大写
:%s/^\(.*\)\n\1/\1/              # 删除重复行
:%s/<\/\=\(\w\+\)\>/\U&/g        # 将 HTML 标记转换为大写
:g/^pattern/s/$/mytext           # 查找文本并将其附加到末尾
:g/pattern/norm! @i              # 在匹配行上运行宏
/^\(.*\)\(\r\?\n\1\)\+$          # 查看重复行
/\v^(.*)(\r?\n\1)+$              # 查看重复行（非常神奇）
:v/./,/./-j                      # 将空行压缩成空行
:g/<p1>/,/<p2>/d                 # 从 <p1> 到 <p2> 包含删除
```

## [](https://ref.15926.tech/docs/vim#vimdiff)Vimdiff

### [](https://ref.15926.tech/docs/vim#%E7%94%A8%E6%B3%95-2)用法

```shell
$ vimdiff file1 file2 [file3]
$ vim -d file1 file2 [file3]
```

### [](https://ref.15926.tech/docs/vim#%E7%BC%96%E8%BE%91)编辑

```
:[range]diffget [bufspec]
:[range]diffput [bufspec]
```

---

|   |   |
|---|---|
|`do` _/_ `:diffget`|获取（get）差异|
|`dp` _/_ `:diffput`|放差价|
|`:dif`|重新扫描差异|
|`:diffo`|关闭差异模式|
|`:1,$+1diffget`|获取所有差异|
|`ZQ`|不做改动就退出|

请参阅：[范围](https://ref.15926.tech/docs/vim#%E8%8C%83%E5%9B%B4)

### [](https://ref.15926.tech/docs/vim#%E6%8A%98%E5%8F%A0)折叠

|   |   |
|---|---|
|`zo` _/_ `zO`|打开|
|`zc` _/_ `zC`|关|
|`za` _/_ `zA`|切换|
|`zv`|这条线的打开折叠|
|`zM`|关闭所有|
|`zR`|打开所有|
|`zm`|折叠更多 _(折叠级别 += 1)_|
|`zr`|少折叠 _(折叠级别 -= 1)_|
|`zx`|更新折叠|

### [](https://ref.15926.tech/docs/vim#%E8%B7%B3%E8%B7%83)跳跃

|   |   |
|---|---|
|`]c`|下一个区别|
|`[c`|以前的区别|

## [](https://ref.15926.tech/docs/vim#%E5%90%84%E7%A7%8D%E5%90%84%E6%A0%B7%E7%9A%84)各种各样的

### [](https://ref.15926.tech/docs/vim#case)Case

|   |   |
|---|---|
|`vU`|_大写_ 字母|
|`vu`|_小写_ 字符|
|`~`|_切换案例_ 字符|
|`viw` `U`|_大写_ 字|
|`viw` `u`|_小写_ 字|
|`viw` `~`|_切换案例_ 字|
|`VU` _/_ `gUU`|_大写_ 行|
|`Vu` _/_ `guu`|_小写_ 行|
|`V~` _/_ `g~~`|_切换案例_ 线|
|`gggUG`|_大写_ 所有文本|
|`ggguG`|_小写_ 所有文本|
|`ggg~G`|_切换大小写_ 所有文本|

### [](https://ref.15926.tech/docs/vim#%E8%B7%B3%E8%B7%83-1)跳跃

|   |   |
|---|---|
|`<C-o>`|返回上一个|
|`<C-i>`|向前|
|`gf`|转到光标中的文件|
|`ga`|显示十六进制、ASCII值|

### [](https://ref.15926.tech/docs/vim#%E5%85%B6%E4%BB%96%E5%91%BD%E4%BB%A4%E8%A1%8C)其他命令行

|   |   |
|---|---|
|`:h`|帮助打开帮助视图|
|`:edit!`|重新加载当前文件|
|`:2,8m0`|将行 `2`-`8` 移动到 `0`|
|`:noh`|清除搜索亮点|
|`:sort`|排序行|
|`:ter`|打开终端窗口|
|`:set paste`|启用插入粘贴子模式|
|`:set nopaste`|禁用插入粘贴子模式|
|`:cq`|退出并出现错误  <br>_(正在中止 Git)_|

### [](https://ref.15926.tech/docs/vim#%E5%AF%BC%E8%88%AA)导航

|   |   |
|---|---|
|`%`|最近/匹配的`{[()]}`|
|`[(` _\|_ `[{`|上一个 `(` 或 `{`|
|`])` _\|_ `]{`|下一个`)`或`}`|
|`[m`|上一个方法开始|
|`[M`|上一个方法结束|

### [](https://ref.15926.tech/docs/vim#%E8%AE%A1%E6%95%B0%E5%99%A8)计数器

|   |   |
|---|---|
|`<C-a>`|增加数量|
|`<C-x>`|减少数量|

### [](https://ref.15926.tech/docs/vim#%E9%80%89%E9%A1%B9%E5%8D%A1-1)选项卡

|   |   |
|---|---|
|`:tag Classname`|跳转到 Classname 的第一个定义|
|`<C-]>`|跳转到定义|
|`g]`|查看所有定义|
|`<C-t>`|回到最后一个标签|
|`<C-o> <C-i>`|后退前进|
|`:tselect Classname`|查找类名的定义|
|`:tjump Classname`|查找类名的定义 _(自动选择第一个)_|

### [](https://ref.15926.tech/docs/vim#%E6%A0%BC%E5%BC%8F%E5%8C%96)格式化

|   |   |
|---|---|
|`:ce 8`|`8` 列之间的中心线|
|`:ri 4`|在 `4` 列右对齐行|
|`:le`|左对齐线|

查看 `:help formatting`

### [](https://ref.15926.tech/docs/vim#%E6%A0%87%E8%AE%B0)标记

|   |   |
|---|---|
|`` `^ ``|插入模式下光标的最后位置|
|`` `. ``|当前缓冲区的最后更改|
|`` `" ``|最后退出的当前缓冲区|
|`` `0 ``|在上次编辑的文件中|
|`''`|返回当前缓冲区中跳出的行|
|` `` `|返回当前缓冲区中跳转的位置|
|`` `[ ``|到先前更改或拉出的文本的开头|
|`` `] ``|到之前更改或拉出的文本的结尾|
|`` `< ``|到最后一个可视化选择的开始|
|`` `> ``|到最后一个可视化选择的结尾|
|`ma`|将此光标位置标记为`a`|
|`` `a ``|跳转到光标位置`a`|
|`'a`|跳转到位置为 `a` 的行首|
|`d'a`|从当前行删除到标记 `a` 的行|
|``d`a``|从当前位置删除到标记 `a` 的位置|
|`c'a`|将文本从当前行更改为 `a` 行|
|``y`a``|将文本从当前位置拉到 `a` 的位置|
|`:marks`|列出所有当前标记|
|`:delm a`|删除标记`a`|
|`:delm a-d`|删除标记`a`、`b`、`c`、`d`|
|`:delm abc`|删除标记`a`、`b`、`c`|

### [](https://ref.15926.tech/docs/vim#%E8%AE%A1%E7%AE%97%E5%99%A8)计算器

|   |   |
|---|---|
|`<C-r>` `=` 7*7|显示结果|
|`<C-r>` `=` 10/2|显示结果|

在 INSERT 模式下执行此操作

### [](https://ref.15926.tech/docs/vim#shell)Shell

|   |   |
|---|---|
|`:!<shell>`|解释 Shell 命令|
|`:r!<shell>`|读入shell的输出|
|`:r!date`|插入日期|
|`:!!date`|用日期替换当前行|

### [](https://ref.15926.tech/docs/vim#%E5%91%BD%E4%BB%A4%E8%A1%8C)命令行

|   |   |
|---|---|
|`<C-r><C-w>`|将当前单词插入命令行|
|`<C-r>"`|从 `注册` 粘贴|
|`<C-x><C-f>`|在插入模式下自动完成路径|

### [](https://ref.15926.tech/docs/vim#%E6%8A%80%E5%B7%A7)技巧

删除重复行

```shell
:sort | %!uniq -u
```

对文件中的行进行编号

```shell
:%!cat -n
```

将整个文档复制到剪贴板

```shell
:%w !pbcopy            # Mac OS X
:%w !xclip -i -sel c   # GNU/Linux
:%w !xsel -i -b        # GNU/Linux
```

## [](https://ref.15926.tech/docs/vim#%E7%90%86%E8%A7%A3-vim)理解 Vim

### [](https://ref.15926.tech/docs/vim#%E5%8A%A8%E8%AF%8D%E7%90%86%E8%A7%A3)动词理解

```shell
d  # 表示删除delete
r  # 表示替换replace
c  # 表示修改change
y  # 表示复制yank
v  # 表示选取visual select
```

动词代表了我们打算对文本进行什么样的操作

### [](https://ref.15926.tech/docs/vim#%E5%90%8D%E8%AF%8D%E7%90%86%E8%A7%A3)名词理解

```shell
w  # 表示一个单词word
s  # 表示一个句子sentence
p  # 表示一个段落paragraph
t  # 表示一个 HTML 标签tag
```

名词代表了我们即将处理的文本。引号或者各种括号所包含的文本称作一个文本块。

### [](https://ref.15926.tech/docs/vim#%E4%BB%8B%E8%AF%8D%E7%90%86%E8%A7%A3)介词理解

```shell
i  # 表示在...之内 inside
a  # 表示环绕... around
t  # 表示到...位置前 to
f  # 表示到...位置上 forward
```

介词界定了待编辑文本的范围或者位置。

### [](https://ref.15926.tech/docs/vim#%E6%95%B0%E8%AF%8D%E7%90%86%E8%A7%A3)数词理解

数词指定了待编辑文本对象的数量，从这个角度而言，数词也可以看作是一种介词。引入数词之后，文本编辑命令的语法就升级成了下面这样：

```shell
动词 介词/数词 名词
```

下面是几个例子：

```shell
c3w  # 修改三个单词：change three words
d2w  # 删除两个单词：delete two words
```

另外，数词也可以修饰动词，表示将操作执行 `n` 次。于是，我们又有了下面的语法：

```shell
数词 动词 名词
```

示例

```shell
2dw # 两次删除单词(等价于删除两个单词): twice delete word
3x  # 三次删除字符(等价于删除三个字符): three times delete character
```

### [](https://ref.15926.tech/docs/vim#%E7%BB%84%E8%AF%8D%E4%B8%BA%E5%8F%A5%E7%90%86%E8%A7%A3)组词为句理解

有了这些基本的语言元素，我们就可以着手构造一些简单的命令了。文本编辑命令的基本语法如下：

```shell
动词 介词 名词
```

下面是一些例子

```shell
dip # 删除一个段落: delete inside paragraph
vis # 选取一个句子: visual select inside sentence
ciw # 修改一个单词: change inside word
caw # 修改一个单词: change around word
dtx # 删除文本直到字符“x”(不包括字符“x”): delete to x
dfx # 删除文本直到字符“x”(包括字符“x”): delete forward x
```

## [](https://ref.15926.tech/docs/vim#%E5%8F%A6%E8%A7%81)另见

- [搞得像IDE一样的 Vim](https://jaywcjlove.github.io/vim-web) _(github.io)_
- [Vim 官方网站](http://www.vim.org/) _(vim.org)_
- [Devhints](https://devhints.io/vim) _(devhints.io)_
- [Vim cheatsheet](https://vim.rtorr.com/lang/zh_cn/) _(vim.rotrr.com)_
- [Vim documentation](http://vimdoc.sourceforge.net/htmldoc/) _(vimdoc.sourceforge.net)_
- [Interactive Vim tutorial](http://openvim.com/) _(openvim.com)_