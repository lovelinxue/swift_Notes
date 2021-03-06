###一.调试程序定位Bug

#####1.查看输出错误日志
* 错误日志是以栈的方式,先执行的在最下面,后执行的在上面,一般会指出那个控制器的哪个方法出现了什么问题.
![](/assets/5446BDB5-EFEC-4CF8-BBEB-90392F43AB56.png)

#####2.打全局断点,定位bug位置
* 有时候输出信息太多,不方便一个一个查看错误日志,可以在`show the breakpoint navigator`工具中添加一个全局的断点.如果有错误他会自动定位到错误地点.有时候会不管用,定位到第三方库里.**可以打条件断点,以后做个详细记录**
![](/assets/屏幕快照 2017-05-24 上午11.37.09.png)

#####3.真机调试bug
* #####使用第三方定位
   * [**腾讯Bugly**](https://bugly.qq.com/v2/index)腾讯做的一个Bug追踪定位,崩溃日志分析.为移动开发者提供专业的异常上报和运营统计，帮助开发者快速发现并解决异常，同时掌握产品运营动态，及时跟进用户反馈。
   
###一.测试网络状态
>切记不管是使用simulator还是真机，在用Network Link Conditioner测试完之后关掉它！**因为网速限制是对全系统有效的，因此在测试结束后记得关掉网速限制。**

#####1.Xcdoe自带的Hardware IO Tools for Xcode
* 打开Xcode菜单栏 - `Xcode` - `Open Developer Tool` - ` More Developer Tools`
* 登陆你的AppleID,根据你的Xcode版本,选择对应的Hardware IO Tools并下载。目前最新的是支持Xcode7.3.
* 打开下载好的dmg格式文件.运行`Network Link Conditioner.prefPane`
![](/assets/屏幕快照 2017-05-25 下午2.42.11.png)
* 安装完成之后,可以在里面设置不同的网络环境,以方便在虚拟机测试程序.

#####2.真机测试调整不同网络环境步骤
* **使用Hardware IO Tools调试真机步骤**
* 首先将你的iPhone或者iPad连接Mac.
* 在Xcode中，shift+command+2打开Organizer
* 选择你的设备 - Use for Development
- **使用手机自带的网络调试**
* 手机设置 - 开发者 - status - Enable打开 - 开始选择调整网络环境
