#面试相关资料

* ###RunLoop运行循环

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
    
* ###RunTime运行时
- **应用场景:**
    - 动态获取分类的属性
    - 关联对象
        - 仿SDWebimage
        - 最多出现在分类中
            - 给分类动态的添加属性
            - 做到更好的解耦
            - 简化使用
    

- **动态获取分类的属性(字典转模型的时候使用)步骤:**
>`class_copyPropertyList`(要获取的类, 类属性的个数指针)类的属性
 `class_copyIvarList`(, )获取成员变量
 `class_copyMethodList`(, )获取类的方法
 `class_copyProtocolList`(, )获取类的协议
 >>**返回值:**可以option + click 看方法详解里有 `objc_property_t * class_copyPropertyList
`
关于是否需要释放:retain/create/copy   需要release.如果不确定可以option + click 看方法详解

        - 首先建立NSObject的分类
        - 使用**class_copyPropertyList**获取类属性的数组
        - 遍历数组,从中取出每个元素使用**property_getName**获取到属性名称.
        - 将获取到的属性转换成输出字符串并将数据存入数组.
        - `free()`释放运行时数组



```swift
#import "NSObject+time.h"

@implementation NSObject (time)

+ (NSArray *)a_getClassAttributeArr
{
    //创建返回的数组
    NSMutableArray *arrM = [NSMutableArray new];
    
    //创建属性指针
    unsigned int coun = 0;
    
    //调用运行时的方法,取得类的属性列表
    objc_property_t *classAttribute = class_copyPropertyList([self class], &coun);

    for (unsigned int i = 0; i < coun; i++)
    {
        //从数组中取出属性
        objc_property_t attribute = classAttribute[i];
        
        //从数据中获取到属性名称
        const char *attributeName = property_getName(attribute);
        
        //将获取到的属性转换成输出字符串
        NSString *attributeClassName = [NSString stringWithCString:attributeName encoding:NSUTF8StringEncoding];
        
        //将数据存入数组
        [arrM addObject:attributeClassName];
        
    }
    
    
    //释放
    free(classAttribute);
    
    return arrM;
}

@end

```

