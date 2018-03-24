# Redis
Redis provides varities of opertaions, includes global functions, objects and closure.

## Global functions

Using global functions

```php
/**
 * @Controller(prefix="/redis")
 */
class RedisController
{
   
    public function testFunc()
    {
        $result = cache()->set('nameFunc', 'stelin3');
        $name   = cache()->get('nameFunc');

        return [$result, $name];
    }

    public function testFunc2()
    {
        $result = cache()->set('nameFunc2', 'stelin3');
        $name   = cache('nameFunc2');
        $name2   = cache('nameFunc3', 'value3');

        return [$result, $name, $name2];
    } 
}
```

- cache(string $key = null, $default = null) If params are default，it will return Cache object, if key is not null, this will return value of the key, if key doesn't exist, this will return the default value.


## Object

Using injected driver objects to operate cache and redis, the only difference is that the injected objects of cache use default driver in configuration.

```php
/**
 * @Controller(prefix="/redis")
 */
class RedisController
{
    /**
     * @Inject("cache")
     * @var Cache
     */
    private $cache;

    /**
     * @Inject()
     * @var \Swoft\Redis\Redis
     */
    private $redis;

    public function testCache()
    {
        $result = $this->cache->set('name', 'stelin');
        $name   = $this->cache->get('name');

        $this->redis->incr("count");

        $this->redis->incrBy("count2", 2);

        return [$result, $name, $this->redis->get('count'), $this->redis->get('count2')];
    }
}
```

- Cache and redis driver objects have the same operations, which provide a series of cache operating functions.

## Closure
Closure supports multiple IO concurrent execution, not only multi cache IO, but mysql/rpc/http hybrid requests for notably improving the execution performance.

```php
/**
 * @Controller(prefix="/redis")
 */
class RedisController
{
    public function testDefer()
    {
        $ret1 = $this->redis->deferCall('set', ['name1', 'stelin1']);
        $ret2 = $this->redis->deferCall('set', ['name2', 'stelin2']);

        $r1 = $ret1->getResult();
        $r2 = $ret2->getResult();

        $ary = $this->redis->getMultiple(['name1', 'name2']);

        return [$r1, $r2, $ary];
    }
}
```

- Now we provide a temporary uniform function deferCall to execute delay operations.Next we will add other defer functions.
