# 使用实例

使用比较简单，类似注入普通一个类的实例一样，唯一区别是会有额外的参数。

## 注解

**@Reference**    

- name 定义引用那个服务的接口，缺省使用类名，一般都需要定义
- version 使用该服务的那个版本，用于区别不同版本
- pool 定义使用哪个连接池，如果不为空，不会根据name配置的名称去查找连接池，而是直接使用配置的连接池。
- breaker 定义使用哪个熔断器，如果不为空，不会根据name配置的名称去查找熔断器，而是直接使用配置的熔断器
- packer RPC服务调用，会有一个默认的数据解包器，此参数是指定其它的数据解包器，不使用默认的。

## 实例

```php

/**
 * rpc controller test
 *
 * @Controller(prefix="rpc")
 */
class RpcController
{

    /**
     * @Reference("user")
     *
     * @var DemoInterface
     */
    private $demoService;

    /**
     * @Reference(name="user", version="1.0.1")
     *
     * @var DemoInterface
     */
    private $demoServiceV2;

    /**
     * @Reference("user")
     * @var \App\Lib\MdDemoInterface
     */
    private $mdDemoService;

    /**
     * @RequestMapping(route="call")
     * @return array
     */
    public function call()
    {
        $version  = $this->demoService->getUser('11');
        $version2 = $this->demoServiceV2->getUser('11');

        return [
            'version'  => $version,
            'version2' => $version2,
        ];
    }

    /**
     * Defer call
     */
    public function defer(){
        $defer1 = $this->demoService->deferGetUser('123');
        $defer2 = $this->demoServiceV2->deferGetUsers(['2', '3']);
        $defer3 = $this->demoServiceV2->deferGetUserByCond(1, 2, 'boy', 1.6);

        $result1 = $defer1->getResult();
        $result2 = $defer2->getResult();
        $result3 = $defer3->getResult();

        return [$result1, $result2, $result3];
    }
}
```
> @Reference 注解可以任何Bean实例的类中使用，不仅仅是controller，这里只是测试。如果要使用延迟收包或并发，必须使用deferXxx方法。
