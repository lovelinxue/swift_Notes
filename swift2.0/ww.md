- ### private
> ####私有化方法:在当前函数前面添加,是指该函数只支持当前文件中访问,别的文件不能访问

```swift
private func addTitleFunc(title:String,imageName:String)
{
    //addTitleFunc只支持在当前文件中访问调用该函数
    print("打印输出...")      
}

```
- ### NSClassFromString
> ####根据字符串获取到对应的控制器

```swift
    private func addChildViewController(childController: String,title:String,imageName:String)
        {
            //0.动态获取命名空间(当前APP控制器的前缀)
            let obName = NSBundle.mainBundle().infoDictionary!["CFBundleExecutable"] as! String
    
            //1.根据字符串获取相应的class
            guard let childClass = NSClassFromString(obName + "." + childController) else{
                print("没有获取到对应的class")
                return
            }
            
            //2.将对应的Anyobject转换成控制器对象
            guard let classVCtype = childClass as? UIViewController.Type else{
                print("没有获取到对应的控制器类型")
                return
            }
            
            //3.初始化控制器对象
            let vc = classVCtype.init()            
        }


```
- ### NSJSONSerialization
> ####使用系统解析json的方法解析json文件

```swift

    //1.获取json文件路径
    guard let jsonPath = NSBundle.mainBundle().pathForResource("vcJsonFile.json", ofType: nil) else {
            print("没有获取到文件路径")
            return
        }
        
    //2.读取json文件中的内容(将文件内容转换成data数据)
    guard let jsonData = NSData(contentsOfFile: jsonPath) else {
            print("么有读取到json文件内容")
            return
        }
        
    //3.将获取到的data数据转成数组
    guard let dicArr = try? NSJSONSerialization.JSONObjectWithData(jsonData, options: .MutableContainers) else {
            return
        }
    guard let arr = dicArr as? [[String:AnyObject]] else {
            return
        }
        
     //4.获取到解析后的json文件
     print("解析后的json数据\(arr)")
```
