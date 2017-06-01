###RunTime运行时`写在分类里面直接使用`
- **应用场景:**
    - 字典转模型
    - 关联对象
    - 动态获取分类的属性
        - 仿SDWebimage
        - 最多出现在分类中
            - 给分类动态的添加属性
            - 做到更好的解耦(开发框架时解耦)
            - 简化使用
    - 交换方法,在无法修改系统或者第三方框架的时候.
        - 利用交换方法,先执行自己的方法.
        - 然后再执行系统或者第三方的方法.
        - `黑魔法`对系统,框架有很强的依赖性.

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


###动态获取类的属性数组
**NSObject+time.h**
```swift
#import <Foundation/Foundation.h>
#import <objc/runtime.h>

@interface NSObject (time)
/**
 *  获取类的属性列表数组
 *
 *  @return 返回值数组
 */
+ (NSArray *)a_getClassAttributeArr;

@end


```

**NSObject+time.m**




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
**调用方法**
```swift
    NSArray *classArrtibuteArr = [Preson a_getClassAttributeArr];
    NSLog(@"%@",classArrtibuteArr);
    
```



###使用KVC让字典转模型
**NSObject+time.h**
```swift
/**
 *  给一个字典,创建相对应的对象
 *
 *  @param dic 要变对象的字典
 *
 *  @return 返回对象
 */
+ (instancetype)a_objWithDict:(NSDictionary *)dic;
```

**NSObject+time.m**
```Swift
+ (instancetype)a_objWithDict:(NSDictionary *)dic
{
    //1.实例化要返回的对象
    id object = [[self alloc]init];
    
    //2.获取到当前对象已经有的属性值
    NSArray *objAttributeArr = [self a_getClassAttributeArr];
    
    //3.遍历字典的键跟值
    [dic enumerateKeysAndObjectsUsingBlock:^(id  _Nonnull key, id  _Nonnull obj, BOOL * _Nonnull stop) {
        
        NSLog(@"key : %@ ----- value : %@",key,obj);
        
        //4.判断当前对象objAttributeArr数组中是否有字典中的该属性
        if ([objAttributeArr containsObject:key]) {
            
            //5.如果有就用KVC赋值过去
            [object setValue:obj forKey:key];
            
        }
        
    }];
    
    //6.返回对象
    return object;
}
```

**方法调用**

```Swift
    Preson *pre = [Preson a_objWithDict:@{@"userName":@"wwwwww",
                                          @"userAge":@12,
                                          @"userNumber":@3333333}];
    
    NSLog(@"%@",pre.userName);
```


