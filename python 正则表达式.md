## python 正则表达式

### 简介

示例：`^[0-9]+abc$`

- `^`匹配输入字符串的开始位置
- `[0-9]`+匹配多个数字，`[0-9]`匹配单个数字，`+`匹配一个或多个
- `abc$`匹配字母`abc`
- `$`为匹配输入字符串的结束位置

示例：`^[a-z0-9_-]{3,15}$`

其中`a-z(字母)` `0-9(数字)`下划线_连字符，`{3-15}`表示匹配15个字符

### 语法



- **`runoo+b`**，可以匹配 `runoob、runooob、runoooooob` 等，`+` 号代表前面的字符必须至少出现一次（1次或多次）。
- **`runoo*b`**，可以匹配 `runob、runoob、runoooooob` 等，`*` 号代表前面的字符可以不出现，也可以出现一次或者多次（0次、或1次、或多次）。
- **`colou?r`** 可以匹配 `color` 或者 `colour`，? 问号代表前面的字符最多只可以出现一次（0次、或1次）。

