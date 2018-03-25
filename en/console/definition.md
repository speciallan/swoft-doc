# Define Commands
Swoft use @Command and @Mapping to define commands. And @Command is to define commands group while @Mapping is to define the mapping of operating commands.

## Annotaions

**@Command**

Define commands group

- name [Param] Define the name of commands group, and it will get by configuration when it is null.
- coroutine [Param] Define coroutine commands, default is  false, and Swoft will start a coroutine to run these commands while this param is true.

**@Mapping**

Define mappings of operating commands.

- name [Param] Define the mapping of operating commands, swoft will execute commands by using their method names when this param is null.

## Help

Swoft use annotions to define this.

- 类描述，对应命令组信息描述
- 方法描述，对应该执行命令的信息描述
- @Usage 定义使用命令格式
- @Options 定义命令选项参数
- @Arguments 定义命令参数
- @Example 命令使用例子

## 实例

```php
/**
 * Test command
 *
 * @Command(coroutine=true)
 */
class TestCommand
{
    /**
     * this test command
     *
     * @Usage
     * test:{command} [arguments] [options]
     *
     * @Options
     * -o,--o this is command option
     *
     * @Arguments
     * arg this is argument
     *
     * @Example
     * php swoft test:test arg=stelin -o opt
     *
     * @param Input  $input
     * @param Output $output
     *
     * @Mapping("test2")
     */
    public function test(Input $input, Output $output)
    {
        App::error('this is eror');
        App::trace('this is trace');
        Coroutine::create(function (){
            App::error('this is eror child');
            App::trace('this is trace child');
        });

        var_dump('test', $input, $output, Coroutine::id(),Coroutine::tid());
    }

    /**
     * this demo command
     *
     * @Usage
     * test:{command} [arguments] [options]
     *
     * @Options
     * -o,--o this is command option
     *
     * @Arguments
     * arg this is argument
     *
     * @Example
     * php swoft test:demo arg=stelin -o opt
     *
     * @Mapping()
     */
    public function demo()
    {
        $hasOpt = input()->hasOpt('o');
        $opt    = input()->getOpt('o');
        $name   = input()->getArg('arg', 'swoft');

        App::trace('this is command log');
        Log::info('this is command info log');
        /* @var UserLogic $logic */
        $logic = App::getBean(UserLogic::class);
        $data  = $logic->getUserInfo(['uid1']);
        var_dump($hasOpt, $opt, $name, $data);
    }
}
```

> 命令逻辑里面可以使用 Swoft 所有功能，唯一不一样的是，如果命令不是协程模式运行，所有IO操作，框架底层会自动切换成传统的同步阻塞，但是使用方法是一样的。
