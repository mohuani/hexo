---
title: PHP开发规范
date: 2021-04-23 13:30:47
tags: [编程规范, PHP]
categories:
- [编程规范, PHP]
---


```xml
<code_scheme name="mohuani" version="1">
  <PHPCodeStyleSettings>
    <option name="ALIGN_KEY_VALUE_PAIRS" value="true" />
    <option name="ALIGN_PHPDOC_COMMENTS" value="true" />
    <option name="CONCAT_SPACES" value="false" />
    <option name="COMMA_AFTER_LAST_ARRAY_ELEMENT" value="true" />
    <option name="LOWER_CASE_BOOLEAN_CONST" value="true" />
    <option name="FORCE_SHORT_DECLARATION_ARRAY_STYLE" value="true" />
  </PHPCodeStyleSettings>
  <editorconfig>
    <option name="ENABLED" value="false" />
  </editorconfig>
  <codeStyleSettings language="PHP">
    <option name="KEEP_FIRST_COLUMN_COMMENT" value="false" />
    <option name="CLASS_BRACE_STYLE" value="1" />
    <option name="METHOD_BRACE_STYLE" value="1" />
    <option name="ALIGN_MULTILINE_ARRAY_INITIALIZER_EXPRESSION" value="true" />
    <option name="SPACE_BEFORE_METHOD_PARENTHESES" value="true" />
    <option name="KEEP_SIMPLE_METHODS_IN_ONE_LINE" value="true" />
  </codeStyleSettings>
</code_scheme>
```
PHPstorm  code-style 直接导入规范


![image.png](https://cdn.nlark.com/yuque/0/2021/png/85934/1617256307388-a6b2edfa-d0c4-49a6-a03d-0ddc7ee36253.png#height=758&id=P69Xd&margin=%5Bobject%20Object%5D&name=image.png&originHeight=758&originWidth=1225&originalType=binary&size=534753&status=done&style=none&width=1225)



### 插件推荐

- [PHP Annotation](https://plugins.jetbrains.com/plugin/7320-php-annotations)
- [.env files support](https://plugins.jetbrains.com/plugin/9525--env-files-support)
- [BashSupport](https://plugins.jetbrains.com/plugin/4230-bashsupport)
- [IDE Eval Reset](https://plugins.zhile.io)
> 在Settings/Preferences... -> Plugins 内手动添加第三方插件仓库地址：[https://plugins.zhile.io](https://plugins.zhile.io)

- [Rainbow Brackets](https://plugins.jetbrains.com/plugin/10080-rainbow-brackets)
- [Git Commit Template](https://blog.csdn.net/noaman_wgs/article/details/103429171)
### PSR规范

- [https://learnku.com/docs/psr](https://learnku.com/docs/psr)
- PSR-2代码风格规范   （？？好像有冲突）
- 参数类型声明，返回值类型声明



### PHP编码规范

- 跟金钱相关的计算全部使用 [**BC函数**](https://www.php.net/manual/zh/ref.bc.php)，避免浮点数计算产生的精度问题
### 
