# ubuntu_bug_fix

记录ubuntu系统的各种bug及解决方法



### 安装搜狗输入法

1、更新源在终端执行 `sudo apt update`

![在终端执行 sudo apt update](https://ife.gtimg.com/build/client/official-website/assets/help5_44adebb.png)

2、安装fcitx输入法框架

1. 在终端输入` sudo apt install fcitx`

![1. 在终端输入 sudo apt install fcitx](https://ife.gtimg.com/build/client/official-website/assets/help6_265ae97.png)

2. 设置fcitx为系统输入法

点击左下角菜单选择语言支持，将语言选择为fcitx（如下图二）

![点击左下角菜单选择语言支持，将语言选择为fcitx（如下图二）](https://ife.gtimg.com/build/client/official-website/assets/help7_75a750e.png)![点击左下角菜单选择语言支持，将语言选择为fcitx（如下图二）](https://ife.gtimg.com/build/client/official-website/assets/help8_4616bd2.png)

3. 设置fcitx开机自启动

在终端执行`sudo cp /usr/share/applications/fcitx.desktop /etc/xdg/autostart/`

![在终端执行sudo cp /usr/share/applications/fcitx.desktop /etc/xdg/autostart/](https://ife.gtimg.com/build/client/official-website/assets/help9_6bb3da9.png)

4. 卸载系统ibus输入法框架

在终端执行 `sudo apt purge ibus`

![在终端执行 sudo apt purge ibus](https://ife.gtimg.com/build/client/official-website/assets/help10_e57ff8c.png)

3、安装搜狗输入法

1. 在官网下载搜狗输入法安装包，并安装，安装命令` sudo dpkg -i 安装包名`

![1. 在官网下载搜狗输入法安装包，并安装，安装命令 sudo dpkg -i 安装包名](https://ife.gtimg.com/build/client/official-website/assets/help11_17ff7e6.png)

2. 安装输入法依赖

在终端执行

```shell
sudo apt install libqt5qml5 libqt5quick5 libqt5quickwidgets5 qml-module-qtquick2 -y
sudo apt install libgsettings-qt1 -y
```

![安装输入法依赖](https://ife.gtimg.com/build/client/official-website/assets/help12_65c2433.png)

4、重启电脑、调出输入法

1.重启电脑

2.查看右上角，可以看到“搜狗”字样，在输入窗口即可且出搜狗输入法。

![查看右上角，可以看到“搜狗”字样，在输入窗口即可且出搜狗输入法](https://ife.gtimg.com/build/client/official-website/assets/help13_1c410df.png)

3. 没有“搜狗”字样，选择配置，将搜狗加入输入法列表即可

![3. 没有“搜狗”字样，选择配置，将搜狗加入输入法列表即可](https://ife.gtimg.com/build/client/official-website/assets/help14_90ff0c1.png)![3. 没有“搜狗”字样，选择配置，将搜狗加入输入法列表即可](https://ife.gtimg.com/build/client/official-website/assets/help15_09f3f5a.png)

至此，搜狗输入法安装完毕

![至此，搜狗输入法安装完毕](https://ife.gtimg.com/build/client/official-website/assets/help16_56116dc.png)



### 搜狗输入法无法在Chrome使用

```shell
sudo apt install fcitx5-frontend-gtk4
sudo shutdown -r now
```

