# Console Configuration


## Scan File Configuration
Swoft scans Bean files when aliases task effect.It set aliases to load configuration files. Default file is app/config/define.php

```php
$aliases = [
    // ......
    '@console'    => '@beans/console.php',
    // ......
];

\Swoft\App::setAliases($aliases);
```

# Router Configuration

Router configuration is simple, it is not required in normal situation. If required, you should config the router information in app/config/beans/console.php.

```php
return [
    // ......
    'commandRoute' => [
        'class' => \Swoft\Console\Router\HandlerMapping::class,
        'suffix' => 'Command',
        'deaultCommand' => 'index',
        'delimiter' => ':',
    ],
    // ......
];
```

- suffix [Optional] Command suffix, to analyze commands automaticly when command is null, default "Command".
- defaultCommand [Optional] Default "index".
- delimeter [Optional] Default ":"
