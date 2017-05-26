###RunTime运行时
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

