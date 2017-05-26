###RunLoop运行循环

* **相关文档:**
    - http://blog.ibireme.com/2015/05/18/runloop/
    - http://www.cnblogs.com/zy1987/p/4582466.html

* **目的:** 
    - 保证程序不退出,监听所有事件!(触摸/时钟/网络)
    
* **开发使用:**
        - 实例化时钟,添加到运行循环中.**(一定要销毁时钟,否则会产生循环引用)**
        - socket开发,使用RunLoop能够监听网络端口的数据接收与发送情况.
        - socket开发,通常使用在智能家居/游戏机 开发相关.
* **要了解的:**
    
        * 有很多文章介绍RunLoop的实战使用,都是`AFN2.0`的时候`NSURLConnection`需要使用运行循环.
        * 必须要了解自动释放池的释放与创建,是与RunLoop有关的.
    
* **RunLoop的模式:**
    - Default：NSDefaultRunLoopMode，**默认模式，在Run Loop没有指定Mode的时候，默认就跑在Default Mode下**
    
    - Connection：NSConnectionReplyMode，**用来监听处理网络请求NSConnection的事件**
    
    - Modal：NSModalPanelRunLoopMode，**OS X的Modal面板事件,开发中不使用.**
           
    - Event tracking：UITrackingRunLoopMode，**拖动事件/滚动视图**
    - Common mode：NSRunLoopCommonModes，**是一个模式集合，当绑定一个事件源到这个模式集合的时候就相当于绑定到了集合内的每一个模式**
