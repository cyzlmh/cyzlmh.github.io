---
layout: page
title: RegEx
category: note
tags: Others
---

* content
{:toc}

## Literal Characters

- 普通字符: "a"
- 转义字符: 十二个
  - "\\"
  - "\^"
  - "\$"
  - "\."
  - "\|"
  - "\?"
  - "\*"
  - "\+"
  - "\("
  - "\)"
  - "\["
  - "\{"

## Character Sets

用中括号括起来的一组字符，匹配其中一个char
ex: gr[ae]y: 匹配"gray"或"grey"

在中括号中使用"-"来表示一个范围
ex: [0-9a-zA-Z]匹配一个字母或数字

在中括号中使用"^"来表示去掉某些字符
ex: "q[^u]"表示"q"后面不接"u"字符

## Shorthand Character Classes

- "\d"表示一个数字字符
- "\w"表示一个字母字符，包括a-z,A-Z和_，等价于[a-zA-Z_]
- "\s"表示一个空白字符，包括空格、tab、换行符

## Non-Printable Characters

"\t"、"\n"等，使用"\uFFFF"或"\x{FFFF}"表示unicode字符

## The Dot Matches Almost Any Character

"."匹配任一字符
"gr.y"可以匹配"gray","grey"或"gr%y"
因尽量少使用"."来匹配

## Anchors

- "^"匹配一行之首
- "&"匹配一行之尾
- "\b"匹配一个单词之首或之尾，例如"\bhi\b"可以匹配"say hi tom"中的"hi"

## Alternation

"|"表示选择
例如"cat|dog"可以匹配"cat"或"dog"
可以连续使用，例如"cat|dog|fish"
使用括号将选择括起，例如"cat|dog food"匹配"cat"或"dog food"，而"(cat|dog) food"匹配"cat food"或"dog food"

## Repetition

- "?"表示0次或1次，例如"colou?r"匹配"color"或"colour"
- "*"表示0次或多次
- "+"表示1次或多次
- "{n}"表示n次，"{n,m}"表示n到m次

例子："<[A-Za-z][A-Za-z0-9]*>"表示"*"前面的[A-Za-z0-9]重复0次或多次，可以匹配xml中的一个tag

## Greedy and Lazy Repetition

上面的"?","*","+"默认为贪婪匹配
例如"<.+>"会匹配"<EM>first</EM>"而不是其中的"<EM>"
在quantifier后面加上"?"改变为懒惰匹配，例如"<.+?>"会匹配"<EM"

## Grouping and Capturing

用小括号将一组字符组合，在匹配后可以capture该组

使用"Set(?:Value)?"创建group但是不会创建capturing group

## Backreferences

"([abc]=\1)"可以匹配"a=a","b=b","c=c"等

## Lookaround

匹配一个group周边的字符
"q(?=u)"匹配"question"中的"q"
"q(?!u)"匹配"Iraq"中的"q"
"(?<=a>)b"匹配"abc"中的b