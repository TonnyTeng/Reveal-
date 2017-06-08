# Reveal-
Reveal在真机和模拟器环境的配置

Reveal工具适合调试非Interface Builder创建的界面，Interface Builder中创建的xib和storyboard在企业开发中并不是总能胜任.
步骤：
1.首先打开Terminal，输入 vim ~/.lldbinit 创建一个名为.lldbinit的文件，然后将如下内容输入到该文件中：
command alias reveal_load_sim expr (void*)dlopen("/Applications/Reveal.app/Contents/SharedSupport/iOS-Libraries/libReveal.dylib", 0x2);
command alias reveal_load_dev expr (void*)dlopen([(NSString*)[(NSBundle*)[NSBundle mainBundle]               pathForResource:@"libReveal" ofType:@"dylib"] cStringUsingEncoding:0x4], 0x2);
command alias reveal_start expr (void)[(NSNotificationCenter*)[NSNotificationCenter defaultCenter]           postNotificationName:@"IBARevealRequestStart" object:nil];
command alias reveal_stop expr (void)[(NSNotificationCenter*)[NSNotificationCenter defaultCenter]            postNotificationName:@"IBARevealRequestStop" object:nil];
该步骤其实是为lldb设置了4个别名，为了后续方便操作，这4个别名意义如下：
1. reveal_load_sim 为模拟器加载reveal调试用的动态链接库
2. reveal_load_dev 为真机加载reveal调试用的动态链接库
3. reveal_start 启动reveal调试功能
4. reveal_stop  结束reveal调试功能

模拟器：
接下来，我们在AppDelegate类的 application: didFinishLaunchingWithOptions: 方法中，作如下3步操作（如下图所示）：
1. 点击该方法左边的行号区域，增加一个断点，之后右击该断点，选择“Edit Breakpoint”。
2. 点击”Action”项边右的”Add Action”,然后输入“reveal_load_dev”reveal_load_sim也可以
3. 勾选上Options上的”Automatically continue after evaluating”选项。

之后我们运行模拟器，然后打开Reveal，就可以在Reveal界面的左上角，看到有模拟器可以连接调试，选择它，则可以在Reveal中查看和调试该iOS程序的界面了。


真机：
接下来，我们在AppDelegate类的 application: didFinishLaunchingWithOptions: 方法中，作如下3步操作（如下图所示）：
4. 点击该方法左边的行号区域，增加一个断点，之后右击该断点，选择“Edit Breakpoint”。
5. 点击”Action”项边右的”Add Action”,然后输入“reveal_load_sim”
6. 勾选上Options上的”Automatically continue after evaluating”选项。



先把Reveal的动态链接库上传到真机上。由于iOS设备有沙盒存在，所以我们只能将Reveal的动态链接库添加到工程中。
点击Reveal菜单栏的”Help”->”Show Reveal Library in Finder”选项（如下图所示），可以在Finder中显示出Reveal的动态链接库：libReveal.dylib ，Reveal.framework

将Reveal.framework文件拖动到目标Xcode工程中，将其中”Link Binary With Libraries”中的Reveal.framework删除

选中Build Settings 在搜索栏中输入 Other Linker Flags，在Other Linker Flags（也可以在Other Linker Flags下的Debug中）输入-ObjC -lz -framework Reveal。



参考资料：
1.http://www.jianshu.com/p/51c539f61ab0使用与破解
2.http://www.jianshu.com/p/662ec5b57a18从Xcode添加和移除Reveal
3.http://www.jianshu.com/p/969a586e00cb下载
4.http://chuansong.me/n/1308113模拟器和真机使用
5.http://www.jianshu.com/p/0b07b7318a3aUI调试
8.http://www.jianshu.com/p/91fe1501a720真机
