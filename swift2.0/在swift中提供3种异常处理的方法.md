## 在swift中提供3种异常处理的方法

### 什么时候需要使用 **try** ?
>在调用系统某一个方法的时候,如果该方法最后有一个`throws`,说明该方法会抛出异常,那么就需要对该方法的异常进行处理,在swift中,处理异常的方法有以下3种方式.


#####一. 使用try方式(手动处理异常), 程序员手动捕捉异常,手动告诉系统异常出现后怎么处理

```swift
do{
    try NSJSONSerialization.JSONObjectWithData(jsonData, options: .MutableContainers)
} catch{
    //error 是系统的一个变量,用来显示错误信息
    print(error)
}
```

#####二. 使用try?方式(自动处理异常),系统会帮助我们处理异常,如果该方法出现了异常,则该方法返回nil,如果没有异常,则返回该有的对象.


```swift
guard let anyObject = try? NSJSONSerialization.JSONObjectWithData(jsonData, options: .MutableContainers)else{ 
    return 
}
```

#####三. 使用try!方式(没有异常,不建议使用,非常危险)直接告诉系统该方法没有异常,注意:如果该方法出现了异常,那么系统会直接崩溃报错.


```swift
let anyObject = try! NSJSONSerialization.JSONObjectWithData(jsonData, options: .MutableContainers)
```







