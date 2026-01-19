# ubuntu_bug_fix

记录ubuntu系统的各种bug及解决方法

## Personal access tokens (classic)

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



### buntu WPS系统缺失字体symbol、wingdings、wingdings wingdings webding

```shell
git clone https://gitee.com/owlman/wps_fonts.git
cd wps_fonts
chmod 777 install_wps_fonts.sh 
./install_wps_fonts.sh
```





### 本地主机与服务器ROS2无法通信

- 两台主机在同一局域网内

- **主机A (当前主机)**: IP `192.168.1.106`
- **主机B (目标主机)**: IP `192.168.1.XXX` (需要确认实际IP)

#### 步骤1: 创建CycloneDDS配置文件

在新主机上创建配置文件：

```bash
# 创建配置文件
cat > ~/cyclonedds.xml << 'EOF'
<?xml version="1.0" encoding="UTF-8"?>
<CycloneDDS xmlns="https://cdds.io/config" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="https://cdds.io/config https://raw.githubusercontent.com/eclipse-cyclonedds/cyclonedds/master/etc/cyclonedds.xsd">
    <Domain id="any">
        <General>
            <NetworkInterfaceAddress>192.168.1.XXX</NetworkInterfaceAddress>
            <AllowMulticast>true</AllowMulticast>
            <MaxMessageSize>65500</MaxMessageSize>
            <EnableMulticastLoopback>true</EnableMulticastLoopback>
        </General>
        <Discovery>
            <ParticipantIndex>auto</ParticipantIndex>
            <MaxAutoParticipantIndex>100</MaxAutoParticipantIndex>
            <Peers>
                <Peer address="192.168.1.106"/>
                <Peer address="192.168.1.106:7400"/>
                <Peer address="192.168.1.XXX:7400"/>
            </Peers>
            <DefaultMulticastAddress>239.255.0.1</DefaultMulticastAddress>
            <Ports>
                <Base>7400</Base>
                <DomainGain>200</DomainGain>
                <ParticipantGain>2</ParticipantGain>
                <MulticastMetaOffset>0</MulticastMetaOffset>
                <MulticastDataOffset>10</MulticastDataOffset>
                <UnicastMetaOffset>1</UnicastMetaOffset>
                <UnicastDataOffset>11</UnicastDataOffset>
            </Ports>
        </Discovery>
        <Internal>
            <Watermarks>
                <WhcHigh>500</WhcHigh>
            </Watermarks>
        </Internal>
        <Tracing>
            <Verbosity>0</Verbosity>
        </Tracing>
    </Domain>
</CycloneDDS>
EOF
```

**重要**: 将上述配置文件中的 `192.168.1.XXX` 替换为新主机的实际IP地址！

#### 步骤2: 创建ROS2初始化脚本

```bash
# 创建初始化脚本
cat > ~/.ros2_init << 'EOF'
#!/bin/bash
# ROS2网络初始化脚本

# 获取本机IP地址（自动检测）
LOCAL_IP=$(hostname -I | awk '{print $1}')

# 设置ROS2网络环境变量
export ROS_DOMAIN_ID=24
export ROS_IP=$LOCAL_IP
export CYCLONEDDS_URI=file://$HOME/cyclonedds.xml

# 重启ROS2守护进程以应用新配置
echo "初始化ROS2网络配置..."
ros2 daemon stop >/dev/null 2>&1
ros2 daemon start >/dev/null 2>&1

echo "ROS2网络配置完成！"
echo "ROS_DOMAIN_ID=$ROS_DOMAIN_ID"
echo "ROS_IP=$ROS_IP"
echo "本机IP: $LOCAL_IP"
EOF

# 给脚本添加执行权限
chmod +x ~/.ros2_init
```

#### 步骤3: 配置全局环境变量

```bash
# 备份原bashrc
cp ~/.bashrc ~/.bashrc.backup

# 添加ROS2网络配置到bashrc
cat >> ~/.bashrc << 'EOF'

# >>> ROS2 Network Configuration >>>
# 配置ROS2跨机器节点发现
export ROS_DOMAIN_ID=24
export ROS_IP=$(hostname -I | awk '{print $1}')
export CYCLONEDDS_URI=file://$HOME/cyclonedds.xml

# 自动初始化ROS2网络配置（仅在交互式shell中）
if [[ $- == *i* ]] && command -v ros2 >/dev/null 2>&1; then
    ~/.ros2_init >/dev/null 2>&1
fi

# ROS2网络配置别名
alias ros2-init="~/.ros2_init"
alias ros2-network="echo 'ROS_DOMAIN_ID=$ROS_DOMAIN_ID, ROS_IP=$ROS_IP, CYCLONEDDS_URI=$CYCLONEDDS_URI'"
# <<< ROS2 Network Configuration <<<
EOF

# 重新加载bashrc
source ~/.bashrc
```

#### 步骤5: 配置防火墙（如果需要）

```bash
# 检查UFW状态
sudo ufw status

# 如果UFW启用，添加ROS2规则
sudo ufw allow 7400/udp  # ROS2发现端口
sudo ufw allow 7410/udp  # ROS2数据端口

# 或者完全禁用UFW进行测试（生产环境不推荐）
# sudo ufw disable
```
