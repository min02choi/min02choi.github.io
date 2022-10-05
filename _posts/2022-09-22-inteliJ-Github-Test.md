---
title: "intelliJ github post test"
excerpt: "testing how to use markdown"
categories: [Markdown]
tags: [Markdown, MD]
toc: true
toc_sticky: true
---

## Upload From intelliJ
***
### How to use MarkDown
***
this file is pushed from intelliJ
>testing
> >wow...</br>
> >testing
> 
> >아 이렇게 하면 자를 수 있구나 개쩐다

***

#### Code Block
```cpp
int main() {
  
}
```



```ruby
require 'redcarpet'
markdown = Redcarpet.new("Hello World!")
puts markdown.to_html
```

~~~python
i = 10
print(i)
~~~

***
#### List
list?
+sdfa
+Asdf

1. is this right?
3.dasd
2. asdfasdf
3. wierd

***
#### Table
| Syntax    |Description|     |
|-----------|---|-----|
| Header    | Title     |     |
| Paragraph | Text      |     |

|기본값|왼쪽 정렬|가운데 정렬|오른쪽 정렬|
|---|:---|:---:|---:|
|내용 1|내용 2|내용 3|내용 4|
|내용 5|내용 6|내용 7|내용 8|
|내용 9|내용 10|내용 11|내용 12|


> 테이블이 왜 안뜰까요??

***
#### Link
[JetBrains](https://www.jetbrains.com)
