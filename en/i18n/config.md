# 国际化配置

国际化配置主要是配置，国际化翻译文件路径和默认语言信息。Swoft 提供两种方式配置，properties 和 env，env 配置会覆盖 properties 和 env

##  properties
配置文件，app/config/properties/app.php

```php
return [
    // ......
    'translator' =>[
        'languageDir' => '@root/resources/messages/',
        'defaultCategory' => 'default',
        'defaultLanguage' => 'en',
    ],
    // ......
];

```

- languageDir 路径支持别名，框架底层会自动解析相应路径
- defaultCategory 配置默认分类，默认是 default
- defaultLanguage 配置默认语言，默认是 en


## env
环境变量配置，`.env`文件

```php
TRANSLATOR_LANG_DIR=@root/resources/messages/
TRANSLATOR_DEFAULT_CATEGORY=default
TRANSLATOR_DEFAULT_LANG=en
```

- 同样 env 路径配置也支持别名
