---
title: sql and freemaker format
date: 2020-04-05
categories:
- [sql,format]
tags:
- format
---
## sql and freemaker format

1. 格式化换行
```
&lt;[\/|#].*?&gt;  \n$0\n
```
<!--more-->
2. 删除空行
```
^(\s*)\n 空
```
3. sql内容
```sql
sql=" $0\n\n
" tables= \n\n$0

(&lt;[\/|#].*?&gt;)|(sql=")|(" tables)
(&lt;[\/|#].*?&gt;)

\n$0\n    
/// \r$0\r
```
4. 删除空行
```
^(\s*)\n 空
```
