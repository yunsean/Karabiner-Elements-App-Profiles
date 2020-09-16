#### Karabiner-Elements简介
Karabiner-Elements是OSX下非常好用的按键映射工具，可以为不同的应用做不同的映射。举个例子：我希望在OSX下使用Control+C作为拷贝键（维持Windows的快捷键风格），那么可以设置一个Default的profile对Control和Command键进行互换；而对于远程桌面等要操作windows的应用，又不需要进行这两个按键的映射，那么就可以新建远程桌面对应的Profile（使用远程桌面的Bundle Identifier作为Profile的名字，Bundle Identifier的获取可以使用状态栏图标中点击Launcher EventViewer获取），比如我的配置如下：
![WX20200509-091010](http://www.yunsean.com/content/images/2020/05/WX20200509-091010.png)
而只有Default做了按键映射：
![WX20200509-091019](http://www.yunsean.com/content/images/2020/05/WX20200509-091019.png)
当然还有很多高级功能，可以查看官方Git文档来设置：
[Karabiner-Elements](https://github.com/pqrs-org/Karabiner-Elements)

#### 根据应用自动切换Profile
需要使用一个叫做Karabinar Elements App Profile的工具，操作比较麻烦，需要安装XCode开发工具。
##### 安装XCode
从App Store下载XCode，启动，按照提示安装依赖环境。
##### 获取代码
在命令上中执行
``` shell
cd /tmp
git clone https://github.com/yunsean/Karabiner-Elements-App-Profiles.git
```
##### 编译
使用XCode打开工程文件：
/tmp/Karabiner-Elements-App-Profiles/Karabiner-Elements App Profiles.xcodeproj
然后点击XCode左上角的调试运行按钮：
![111](http://www.yunsean.com/content/images/2020/09/111.png)
然后点击右上角的调试窗口按钮，然后切换到你需要自动更换按键映射的应用上，如果下方提示如下图则代表成功了。
![222](http://www.yunsean.com/content/images/2020/09/222.png)
##### 安装
点击XCode菜单中的Archive：
![333](http://www.yunsean.com/content/images/2020/09/333.png)
在弹出的框中选择Distribute Content
![444](http://www.yunsean.com/content/images/2020/09/444.png)
继续选择Built Products，点击Next
![555](http://www.yunsean.com/content/images/2020/09/555.png)
然后在弹出的框中选择保存路径，点击Export
![666](http://www.yunsean.com/content/images/2020/09/666.png)
之后即可在指定的路径找到导出的文件。
将该文件复制到/usr/local/bin目录下。
并且将代码中的[net.sabi.Karabiner-Elements-App-Profiles.plist](https://raw.githubusercontent.com/yunsean/Karabiner-Elements-App-Profiles/master/net.sabi.Karabiner-Elements-App-Profiles.plist)文件复制到~/Library/LaunchAgents下，该文件的目的是自动运行/usr/local/bin/Karabiner-Elements-App-Profiles。
完成安装，重启系统应该就能实现自动切换Profile了。
如果觉得编译太过麻烦，也可以下载源码中的[bin](https://github.com/yunsean/Karabiner-Elements-App-Profiles/tree/master/bin)目录下对应版本的文件使用。
##### 切换
切换Profile是根据应用程序的Bundle Identifier名字来完成的。也就是说，需要为每一个需要特殊键盘映射的应用创建的对应名字的Profile。Profile的获取方式：
运行EventViewer：
![888](http://www.yunsean.com/content/images/2020/09/888.png)
启动后切换到Frontmost application页面，然后切换应用到你需要获取Bundle Identifier的应用，将在下方窗口显示出其对应的Bundle Idetifier。
![999](http://www.yunsean.com/content/images/2020/09/999.png)

#### 无法添加Profile的问题
但是是用一段时间后，发现无法添加Profile，表现就是如果添加了Profile，在重启应用或者重启系统，或者进入到之前的Profile对应的应用做了一次切换后，新添加的Profile就备删除了，折腾了很长时间，尝试过直接修改配置文件，都无效。
最后的解决方案是：
**添加配置文件后，在OSX的命令行工具中执行sudo reboot强制重启系统，就会生效了**
推测可能是有某个自动切换Profile的应用在退出时候会将内存中的配置写入到磁盘？可能是Karabinar Elements App Profile这个插件干的？反正能解决就好！


#### 映射Ctrl+左右箭头为Home和End
在OSX下，默认是Command+左右箭头为跳转到行首行尾，当远程使用Windows时，也可以使用这个组合来代替Home和End按键，以及用Ctrl+Shift+左右键头尾选中到行首行位，同时修改使用Ctrl+Tab来完成应用切换（映射为Alt+Tab)，Alt+Tab和Ctrl+~（1前面那个键）在应用内切换窗口（映射为Ctl+Tab），使用Ctl+Space来切换输入法（Windows10下不知道为什么不好配置这个快捷键，但是可以配置为使用Alt+Shift来切换输入法），修改配置文件：
``` json
      "rules" : [
          {
            "manipulators" : [
              {
                "to" : [
                  {
                    "key_code" : "tab",
                    "modifiers" : "left_option"
                  }
                ],
                "type" : "basic",
                "from" : {
                  "key_code" : "tab",
                  "modifiers" : {
                    "mandatory" : [
                      "left_control"
                    ]
                  }
                }
              },
              {
                "to" : [
                  {
                    "key_code" : "tab",
                    "modifiers" : "left_control"
                  }
                ],
                "type" : "basic",
                "from" : {
                  "key_code" : "tab",
                  "modifiers" : {
                    "mandatory" : [
                      "left_option"
                    ]
                  }
                }
              },
              {
                "to" : [
                  {
                    "key_code" : "tab",
                    "modifiers" : "left_control"
                  }
                ],
                "type" : "basic",
                "from" : {
                  "key_code" : "grave_accent_and_tilde",
                  "modifiers" : {
                    "mandatory" : [
                      "left_control"
                    ]
                  }
                }
              },
              {
                "to" : [
                  {
                    "key_code" : "left_shift",
                    "modifiers" : "left_option"
                  }
                ],
                "type" : "basic",
                "from" : {
                  "key_code" : "spacebar",
                  "modifiers" : {
                    "mandatory" : [
                      "left_control"
                    ]
                  }
                }
              },
              {
                "to" : [
                  {
                    "key_code" : "home"
                  }
                ],
                "type" : "basic",
                "from" : {
                  "key_code" : "left_arrow",
                  "modifiers" : {
                    "mandatory" : [
                      "left_control"
                    ]
                  }
                }
              },
              {
                "to" : [
                  {
                    "key_code" : "end"
                  }
                ],
                "type" : "basic",
                "from" : {
                  "key_code" : "right_arrow",
                  "modifiers" : {
                    "mandatory" : [
                      "left_control"
                    ]
                  }
                }
              },
              {
                "to" : [
                  {
                    "key_code" : "home",
                    "modifiers" : "left_shift"
                  }
                ],
                "type" : "basic",
                "from" : {
                  "key_code" : "left_arrow",
                  "modifiers" : {
                    "mandatory" : [
                      "left_control",
                      "left_shift"
                    ]
                  }
                }
              },
              {
                "to" : [
                  {
                    "key_code" : "end",
                    "modifiers" : "left_shift"
                  }
                ],
                "type" : "basic",
                "from" : {
                  "key_code" : "right_arrow",
                  "modifiers" : {
                    "mandatory" : [
                      "left_control",
                      "left_shift"
                    ]
                  }
                }
              }
            ],
            "description" : "Contrl Arrow to Home End"
          }
        ],
```
