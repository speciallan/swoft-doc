# Cache Configuration

The Configuration includes driver and coonection pool.

## Driver Configuration

The default driver is redis, and developers can insert new config in app/config/beans/base.php

```php
return [
    // ......
    'cache'            => [
        'class' => \Swoft\Cache\Cache::class,
        'driver' => 'xxx',
        'drivers' => [
            'xxx' => \Swoft\Xxx::class
        ]
    ]
];
```

- class  [Optional] Defualt 
- driver [Required] Default redis
- drivers [Required] Custom drivers, keys are driver names

## Connnection Pool Configuration
The configuration of connection pool has two modes, properties and env. And env config will cover properties.

### Properties
app/config/properties/cache.php

```php
return [
    'redis' => [
        'name'        => 'redis',
        'uri'         => [
            '127.0.0.1:6379',
            '127.0.0.1:6379',
        ],
        'minActive'   => 8,
        'maxActive'   => 8,
        'maxWait'     => 8,
        'maxWaitTime' => 3,
        'maxIdleTime' => 60,
        'timeout'     => 8,
        'db'          => 1,
        'serialize'   => 0,
    ],
];
```
- name [Required] Node name for service discovery
- uri [Required] Connection Address
- maxActive [Optional] Max active connection num
- maxWait [Optional] Max wait connection num
- minActive [Optional] Min active connection num
- maxIdleTime [Optional] Max free time，by second
- maxWaitTime [Optional] Max wait time, by second
- timeout [Optional] Overtime time, by second
- serialize [Optional] Whether serialize
- db [Optional] Cache database index

### env

.env configuation file
```
REDIS_NAME=redis
REDIS_DB=2
REDIS_URI=127.0.0.1:6379,127.0.0.1:6379
REDIS_MIN_ACTIVE=5
REDIS_MAX_ACTIVE=10
REDIS_MAX_WAIT=20
REDIS_MAX_WAIT_TIME=3
REDIS_MAX_IDLE_TIME=60
REDIS_TIMEOUT=3
REDIS_SERIALIZE=1
```
- REDIS_NAME [Required] Node name for service discovery
- REDIS_URL [Required] Connection Address
- REDIS_MAX_ACTIVE [Optional] Max active connection num
- REDIS_MAX_WAIT [Optional] Max wait connection num
- REDIS_MIN_ACTIVE [Optional] Min active connection num
- REDIS_MAX_IDLE_TIME [Optional] Max free time，by second
- REDIS_MAX_WAIT_TIME [Optional] Max wait time, by second
- REDIS_TIMEOUT [Optional] Overtime time, by second
- REDIS_SERIALIZE [Optional] Whether serialize
- REDIS_DB [Optional] Cache database index
