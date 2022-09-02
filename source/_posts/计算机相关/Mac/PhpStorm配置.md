---
title: PhpStorm配置
date: 2017-12-04 21:22:30
draft: true
tags: [PHP, PhpStorm]
categories:
- [PHP, PhpStorm]
---

### CodeStyle配置
```xml
<code_scheme name="Mohuani-PHP" version="11">
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


###  PSR 配置
![114130537-5bf38580-9933-11eb-91fb-4c3f6627bf93](../images/114130537-5bf38580-9933-11eb-91fb-4c3f6627bf93.png)

- https://learnku.com/docs/psr

### 插件推荐

- [PHP Annotation](https://plugins.jetbrains.com/plugin/7320-php-annotations)
- [.env files support](https://plugins.jetbrains.com/plugin/9525--env-files-support)
- [BashSupport](https://plugins.jetbrains.com/plugin/4230-bashsupport)
- [IDE Eval Reset](https://plugins.zhile.io)
> 在Settings/Preferences... -> Plugins 内手动添加第三方插件仓库地址：[https://plugins.zhile.io](https://plugins.zhile.io)，然后再仓库市场里面搜索并安装插件 `IDE Eval Reset`

- [Rainbow Brackets](https://plugins.jetbrains.com/plugin/10080-rainbow-brackets)
- [Git Commit Template](https://blog.csdn.net/noaman_wgs/article/details/103429171)
- [.ignore](https://plugins.jetbrains.com/plugin/7495--ignore)
- [kubernetes](https://plugins.jetbrains.com/plugin/10485-kubernetes)
  - 进行配置，编写yaml文的时候就会有提示了  ![image](https://user-images.githubusercontent.com/21000558/188113620-17005c07-038c-48f1-9a2a-783086595db8.png)

