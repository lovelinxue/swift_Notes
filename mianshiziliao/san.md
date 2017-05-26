###程序打印输出调试pch宏文件配置

1.项目中新建文件,`iOS` - `Other` - `PCH File`
2.配置pch文件,`项目工程` - `Build Settings` - 搜索`prefix header`
3.添加宏文件,`Apple LLVM7.1 - Language` - `Prefix Header`中添加文件`项目名称/pch文件名.pch`
![](/assets/4AEB2FBC-76C9-4647-8D76-607D585310AD.png)
4.编写pch文件,`NSLog`

```swift
#ifdef __OBJC__

#ifdef DEBUG//如果是调试状态
//打印输出:控制器-方法-代码行数-输出内容
#define NSLog(fmt, ...) NSLog((@"%s [Line %d] " fmt), __PRETTY_FUNCTION__,__LINE__, ##__VA_ARGS__)
#else
#define NSLog(...)
#endif

#endif

```

