### Oracle Cloud 甲骨文云脚本集合，打造最强甲骨文云（时时更新）

### Oracle甲骨文云添加WARP双栈IPV6+IPV4，针对KVM架构的IPV4 only VPS，默认IPV4优先!

### Youtube视频教程....稍后更新.......

### 给ipv4 only VPS添加WARP的好处：

1：使只有IPV4的VPS获取访问IPV6的能力，套上WARP的ip,变成双栈VPS！

2：基本能隐藏VPS的真实IP！

3：WARP双栈IPV6+IPV4的IP段都支持奈非Netflix流媒体，无视原IP限制！

4：加速VPS到CloudFlare CDN节点访问速度！

5：避开原VPS的IP需要谷歌验证码问题！

6：替代HE tunnelbroker隧道方案，做IPV6 VPS跳板机更加稳定、高效！

--------------------------------------------------------------------------------------------------------
### 一：设置Root密码一键脚本（抛弃秘钥文件，默认ROOT权限，方便登录与编辑文件）
```
bash <(curl -sSL https://raw.githubusercontent.com/YG-tsj/Oracle-warp/main/root.sh)
```
-----------------------------------------------------------------------------------------------------
### 二：更新甲骨文Ubuntu系统内核一键脚本

###### 目前甲骨文Ubuntu20.04系统内核为5.4版本，而5.6版本以上内核才自带Wireguard，更新内核方案在理论上网络效率最高的！

###### 任选以下两个内核脚本中的一个进行升级，选其一即可！！都集成删除iptables代码```rm -rf /etc/iptables && reboot```，解决甲骨文Ubuntu系统Nginx等证书申请报错问题！

##### 1、通用内核5.11版本（推荐：通用稳定版，后续会更新）
```
bash <(curl -sSL https://raw.githubusercontent.com/YG-tsj/Oracle-warp/main/generic-kernel.sh)
```
##### 2、第三方xanmod内核（安装时自动安装最新版本）
```
bash <(curl -sSL https://raw.githubusercontent.com/YG-tsj/Oracle-warp/main/xanmod-kernel.sh)
```
-------------------------------------------------------------------------------------------------------------
### 三：开启BBR加速（秋水逸冰大老-传统版）
```
wget --no-check-certificate https://github.com/teddysun/across/raw/master/bbr.sh && chmod +x bbr.sh && ./bbr.sh
```
##### 检测BBR是否生效(显示有BBR，说明成功)：```lsmod | grep bbr```
-------------------------------------------------------------------------------------------------------------
### 四：重装系统能解决99%的问题，warp单双栈ipv4+ipv6脚本

##### 推荐Ubuntu 20.04系统，根据自己需求选择脚本1或者脚本2（有无成功可查看脚本末尾提示）

###### 脚本1：Warp仅接管IPV6网络
```
bash <(curl -sSL https://raw.githubusercontent.com/YG-tsj/Oracle-warp/main/warp6.sh)
```
###### 脚本2：双栈Warp接管IPV4与IPV6网络
```
bash <(curl -sSL https://raw.githubusercontent.com/YG-tsj/Oracle-warp/main/warp64.sh)
```
----------------------------------------------------------------------------------------------------
##### 查看WARP当前统计状态
```
wg
```

##### 提示：配置文件wgcf.conf和注册文件wgcf-account.toml都已备份在/etc/wireguard目录下！

-------------------------------------------------------------------------------------------------------------

##### IPV4 VPS专用分流配置文件(以下默认全局IPV4优先，IP、域名自定义教程，参考https://youtu.be/fY9HDLJ7mnM)
```
{ 
"outbounds": [
    {
      "tag":"IP4-out",
      "protocol": "freedom",
      "settings": {}
    },
    {
      "tag":"IP6-out",
      "protocol": "freedom",
      "settings": {
        "domainStrategy": "UseIPv6" 
      }
    }
  ],
  "routing": {
    "rules": [
      {
        "type": "field",
        "outboundTag": "IP4-out",
        "domain": [""] 
      },
      {
        "type": "field",
        "outboundTag": "IP6-out",
        "network": "udp,tcp" 
      }
    ]
  }
}
``` 
-----------------------------------------------------------------------------------------------
#### 相关WARP进程命令

手动临时关闭WARP网络接口
```
wg-quick down wgcf
```
手动开启WARP网络接口 
```
wg-quick up wgcf
```

启动systemctl enable wg-quick@wgcf

开始systemctl start wg-quick@wgcf

重启systemctl restart wg-quick@wgcf

停止systemctl stop wg-quick@wgcf

关闭systemctl disable wg-quick@wgcf


---------------------------------------------------------------------------------------------------------------------

感谢P3terx大及原创者们，参考来源：
 
https://p3terx.com/archives/debian-linux-vps-server-wireguard-installation-tutorial.html

https://p3terx.com/archives/use-cloudflare-warp-to-add-extra-ipv4-or-ipv6-network-support-to-vps-servers-for-free.html

https://luotianyi.vc/5252.html

https://hiram.wang/cloudflare-wrap-vps/
