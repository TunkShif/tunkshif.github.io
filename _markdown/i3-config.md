TITLE: 桌面环境微科普与 i3 配置记录
TAG: linux
桌面环境微科普与 i3 配置记录
============================

[TOC]

### 简介

#### 桌面环境

即 **DE**(*Desktop Environment*), 诸如`gnome kde xfce mate lxde cinnamon budgie dde`之类的皆为完整的桌面环境,
一般完整的桌面环境都会提供*窗口管理器*, *面板(任务栏)*, *配套应用程序(如文件管理器, 终端模拟器, 文本编辑器等)*,
完整的桌面环境一般能够提供一个开箱即用的桌面环境, 对普通用户比较友好.


#### 窗口管理器

即 **WM**(*Window Manager*), 提供窗口边框, 标题栏等的图形界面, 有些窗口管理器还有更多的功能,
不同的桌面环境采用不同的窗口管理器, 比如 *gnome3* 的 *Mutter*, *gnome2* 的 *Metacity*, *xfce* 的 *xfwm,
*cinnamon 的窗口管理器 *Muffin* 则是 fork 自 *Mutter*, *mate* 的 *Marco* 则是 fork 自 *Metacity*.
还有一些不被包含在桌面环境内的可供独立使用的窗口管理器, 比如 *Openbox*, *Fluxbox* 等.

窗口管理器又分为三类, **堆栈/悬浮式**(*Stacking*), **平铺/瓦片式**(*Tiling*), **动态式**(*Dynamic*),
上面提到的窗口管理器全部为传统的堆栈式, 即一个个窗口可以堆叠在一起, 而平铺式则是将一个个窗口互不重合的全部展示在屏幕上.
比如 *i3wm*, *herbstluftwm*, *awesome* 等都属于平铺式的窗口管理器.

此外, 在 Windows Vista 和 Windows 7 中, 你可以在任务管理器里面发现一个叫 *dwm.exe* 的进程, 这便是 Windows 的窗口管理器,
Aero 特效等都依靠此实现.

#### 显示管理器
即 **DM**(*Display Manager*), 又叫登陆管理器, 提供了在电脑开机启动后, 在进入桌面环境前的用户登陆界面,
有些桌面环境也会附带有显示管理器, 比如 *gnome* 的 *GDM*, *kde* 的 *SDDM*, *lxde* 的 *lxdm*,
还有我个人喜欢的 *lightdm*, lightdm 又需要安装不同的 *greeter*, 比如基于 gtk 的 *lightdm-gtk-greeter*,
适用于 kde4 的 *lightdm-kde-greeter*, ubuntu unity 环境下的 *lightdm-unity-greeter*, eos 的 *lightdm-pantheon-greeter*,
以及 deepin 的 *deepin-session-ui* (~~包名不规范差评~~)

好了前面讲了这么多, 现在如果你不满足大型桌面环境的臃肿, ~~或者和我一样因配置太差~~, 那么可以尝试仅使用窗口管理器,
不过纯窗口管理器门槛比较高, 因为大多都需要手动编辑配置文件, 但配置好了以后可谓是一劳永逸
如果还想要体验纯键盘操作桌面, 那么可以试试平铺窗口管理器.
下面即为在 **ArchLinux** 下安装配置 **i3wm** 的记录

### i3

#### 安装

使用 pacman 直接安装 i3 应用组即可

```sh
sudo pacman -S i3
```

![i3-group](https://i.loli.net/2018/08/02/5b6279bccfedf.png)

注意 i3 组中包含了 i3-wm 和 i3-gaps 两个互相冲突的包, i3-gaps 是 fork 自 i3-wm 的项目, 提供了窗口间距的设置,
推荐安装 i3-gaps 即可, 默认也是安装的 i3-gaps

```sh
sudo pacman -S lightdm lightdm-gtk-greeter lightdm-gtk-greeter-settings
sudo systemctl enable lightdm

sudo pacman -S network-manager-applet
sudo systemctl enable NetworkManager
```

接下来安装显示管理器, 这里安装的是 lightdm 和 gtk-greeter, lightdm-gtk-greeter-settings 包提供了一个图形界面的配置工具
然后开启显示管理器服务, 除此之外对于使用无线网络的用户安装 networkmanager 提供 wifi 管理的托盘程序, 开启服务
重启后就会看到进入了显示管理器, 第一次登陆进入 i3wm 会有一个简单的配置向导,
会提示设置 mod 键， 这是一个很重要的按键, 对窗口的操作等都需要用到这个键, 设置为 meta 键(即 windows 徽标键)个人觉得比较方便使用.

接下来再安装一些常用应用, 方便后面的编辑配置文件

```sh
sudo pacman -S thunar xfce4-terminal mousepad # xfce 下的文件管理器, 终端模拟器和文本编辑器
sudo pacman -S nano vim # 终端下的文本编辑器, nano和vim
```

#### 基本操作

现在按下 `mod+回车` , 如果按照上面设置的话, 也就是 Win+回车 组合键, 会打开安装的终端模拟器, 接下来再熟悉以下基本操作

![main](https://i.loli.net/2018/08/02/5b6280c8aef4b.png)

如上图打开了三个终端窗口, 可以看到平铺式窗口管理器的特点了, 如果你要问为什么自己的界面为什么那么复古, 那是因为没有完全配置好,
先别急着搞配置文件, 首先用熟练后再美化也不迟.

用 `mod+上下左右方向键` 来移动焦点选择窗口, 用 `mod+Shift+上下左右方向键` 来交换移动窗口的位置, 可以实现更改窗口铺放的布局

![layout-1](https://i.loli.net/2018/08/02/5b62822778bdd.png)
![layout-2](https://i.loli.net/2018/08/02/5b62826e68239.png)
![layout-3](https://i.loli.net/2018/08/02/5b628298c796a.png)

用 `mod+h` 和 `mod+v` 可以横向纵向地分割窗口
用 `mod+w` 开启 tab 模式, 这样几个窗口可以合并到一个窗口, 并且以 tab 栏的模式切换

![layout-4](https://i.loli.net/2018/08/02/5b62834171665.png)

用 `mod+f` 开启全屏模式

![layout-5](https://i.loli.net/2018/08/02/5b628379599b9.png)

用 `mod+Shift+空格` 将某一个窗口设置为悬浮模式, 也就是传统的堆栈式, 可以用鼠标拖动

![layout-6](https://i.loli.net/2018/08/02/5b6283cba881d.png)

用 `mod+r` 进入调整窗口模式, 然后按动方向键即可调整窗口大小

![layout-7](https://i.loli.net/2018/08/02/5b62b6856a991.png)

用 `mod+Shift+q` 来关闭窗口

下面引入工作区的概念, 平铺式的窗口管理器由于打开的窗口会全部铺开在屏幕上面, 所以同时开很多个窗口不太现实,
因此引入了工作区的概念, 其实工作区在堆栈式的窗口管理器内也有, 最多可设定10个工作区,
用 `mod+数字键` 即可进入对应号码的工作区, 如图中状态栏左上角显示的3个标签, 图中是经过配置过后的效果
默认的话 i3bar 是在下方, 所以工作区显示在左下角

![layout-8](https://i.loli.net/2018/08/02/5b62b70618b7b.png)

用 `mod+Shift+数字键` 即可将选中的窗口移动到指定工作区


#### 配置

熟练基本操作以后, 可以对 i3wm 进行配置, 使其更加有效率和更加美观
第一次打开 i3wm 时配置向导生成的配置文件位于 `~/.config/i3/config`, 后文为方便称为 `i3 配置文件`
用文本编辑器打开该配置文件, 进行一系列的自定义

每一次改完配置后, 可以用 `mod+Shift+c` 来重新读取配置文件, 用 `mod+Shift+r` 重启 i3wm
当然自启动和壁纸这类配置可能需要重启或注销后才能生效

##### 设置开机自启的应用程序
推荐建立 `~/.config/i3/autostart` 文件, 里面写上需要开机自启的程序, 或者需要执行的脚本, 下面是个人的配置供参考

```sh
#!/bin/sh

# 触摸板设置  
xinput set-prop 13 270 1
xinput set-prop 13 283 0, 1, 0

# 启用网络管理的托盘程序
nm-applet &

# 开启compton, 一个合成管理器, 用以提供窗口的透明, 动画特效等, 下面会介绍
compton &

# 开启输入法
fcitx &
```

然后在 i3 配置文件中写入

```sh
exec --no-startup-id sh ~/.config/i3/autostart
```

##### 设置壁纸

通常用 feh 来设置壁纸, feh 还可以作为图片浏览器, 直接用 pacman 安装即可
当然也可以用 nitrogen 来设置壁纸, 个人没有用过所以不作介绍
首先在终端内执行

```sh
feh --bg-scale /path/to/your/wallpaper.png
```

然后会生成一个 `~/.fehbg` 文件, 接着在 i3 配置文件中写入

```sh
exec --no-startup-id sh ~/.fehbg
```

当然也可以直接在配置文件中写入

```sh
exec --no-startup-id feh --bg-scale /path/to/your/wallpaper.png
```

##### 窗口特效

i3wm 并不原生支持窗口合成功能, 也就意味着窗口透明无法实现, 可以安装 compton 包, 为 i3wm 添加窗口透明和动画特效支持,
安装后加入自启动即可, 参考上文

##### 通知提醒

推荐使用 dunst, 用 pacman 安装即可, 配置文件可参照官网文档写, ~~或者照着别人的抄~~
效果如下

![dunst](https://i.loli.net/2018/08/02/5b62bb522eeba.png)

##### 应用启动器

我开始用的是 dmenu, 这也是 i3wm 默认的启动器, 安装后直接用 `mod+d` 呼出,
不过后面我换用 rofi 了, 效果如图

![rofi](https://i.loli.net/2018/08/02/5b62bc7d3cea4.png)

在 i3 配置文件中删除原来的按键绑定, 加入以下即可

```sh
bindsym $mod+d exec --no-startup-id rofi -show drun
```

##### 自定义按键绑定

只需要按照以下格式在配置文件中写入即可, 比如下面的例子就是按下 `mod+e` 组合键即可打开 thunar 文件管理器

```
# bindsym <按键组合> exec --no-startup-id <执行的命令或脚本程序>
bindsym $mod+e exec --no-startup-id thunar
```

##### 配置工作区

上面的图中可以看到左上角显示的即为工作区, 默认 i3bar 是在下方, 后面会讲如何设置 i3bar 的位置
首先推荐安装 font-awesome 的图标字体, 可以使得显示更加美观, 如果没安装下面代码显示的可能是方框
按照下面的格式设置工作区以及工作区显示名称即可

```sh
set $ws1 "1:Home"
set $ws2 "2:Files"
set $ws3 "3:Web"
set $ws4 "4:Code"
set $ws5 "5:Settings"
set $ws6 "6:Misc"
```

##### 自动归类

上面设定好工作区后, 接下来可以进一步设置, 打开某一程序, 自动将该程序归类到指定工作区
例如在第一工作区打开了 mousepad, 自动归类到了名为 Code 的工作区, 并以另外的颜色提示

![assign](https://i.loli.net/2018/08/02/5b62beef0dc72.png)

要实现该功能只需要按照下文在配置文件中写入

```sh
# assign [class="应用窗口的类名"]               → 工作区
assign [class="Mousepad"]                       → $ws4
assign [class="Chromium"]                       → $ws3
```

要获取某一应用程序的类名, 只需要在终端中运行 xprop 然后点击目标窗口即可出现信息

![xprop](https://i.loli.net/2018/08/02/5b62bfcf195f8.png)

当然还可以根据窗口标题分类, 只需要将 class 改为 title 即可
还可以实现特定窗口以悬浮模式打开, 可以应用于一些设置窗口等等
比如下面设定可以设定为默认以悬浮模式打开 lightdm-gtk-greeter 的设置窗口

```sh
for_window [class="Lightdm-gtk-greeter-settings"]   floating enable
for_window [class="Lxappearance"]                   floating enable
```

##### 界面样式

最后进行一下美化, 自己设置字体和窗口边框颜色

```sh
# 设置字体
font pango:Ubuntu Bold 9              


# 项目名                 边框     背景    文字  指示器     子边框
client.focused          #5294E2 #5294E2 #FFFFFF #DDDDDD   #5294E2
client.focused_inactive #2F343F #2F343F #86888C #292D2E   #5A5A5A
client.unfocused        #2F343F #2F343F #86888C #292D2E   #5A5A5A
client.urgent           #D64E4E #D64E4E #FFFFFF #D64E4E   #D64E4E
client.placeholder      #2F343F #0C0C0C #FFFFFF #2F343F   #262626
client.background       #2F343F       

# 窗口间距 需要安装 i3-gaps 包而不是 i3-wm
smart_gaps on                         
gaps inner 4                          
gaps outer 2                    
```

关于 i3bar 也就是上文图中的面板/状态栏的配置比较复杂, 这里就不怎么讲解, 个人用的是 i3blocks 作为信息输出
也可以用 conky 之类的, 具体的话搜索可以得到很多详细教程和例子, 这里只是简单说一下
i3bar 的信息定义在配置文件的 `bar {}` 区块内

```sh
bar {
    position top # 位置
    workspace_buttons yes # 显示工作区按钮
    strip_workspace_numbers yes # 隐藏工作区编号只显示名称
}
```

##### 关机注销

写入以下配置, 按下 `mod+Pause` 组合键后会出现提示, 再按下括号内对应按键即可实现对应操作

```sh
set $Locker i3lock && sleep 1         
set $mode_system :Lock (l) | :Logout (e) | :Reboot (r) | :Shutdown (q)
mode "$mode_system" {                 
    bindsym l exec --no-startup-id $Locker, mode "default"
    bindsym e exec --no-startup-id i3-msg exit, mode "default"
    bindsym r exec --no-startup-id systemctl reboot, mode "default"
    bindsym q exec --no-startup-id systemctl poweroff -i, mode "default"
                                      
    # back to normal: Enter or Escape 
    bindsym Return mode "default"     
    bindsym Escape mode "default"     
}  
```

最后贴出个人的[配置文件](https://github.com/TunkShif/dotfiles)

