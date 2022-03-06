# 使用 GitHub Actions 快速定制编译 OpenWrt 固件

流程文档大量参考[P3TERX](https://github.com/P3TERX/Actions-OpenWrt)、[KFERMercer](https://github.com/KFERMercer/OpenWrt-CI)，
十分感谢！

默认机型小米4A千兆版，源码[coolsnowwolf/lede](https://github.com/coolsnowwolf/lede)，需要其他机型自行修改。

喜欢的话，右上角star一下，方便再找。

## 使用教程:

### 你需要:

- GitHub 账号

- GitHub Actions 基本使用技能，参见[官方文档](https://docs.github.com/cn/actions)

### liunx、openwrt、Actions相关知识: 自行Google搜索 

### 1. 注册GitHub账号并开启GitHub Actions

### 2. fork [hugcabbage/shared-openwrt](https://github.com/hugcabbage/shared-openwrt)

### 3. 设置Actions secrets

进入GitHub Settings(点头像) → Developer settings → Personal access tokens → Generate new token，Note随意填，Expiration建议选`No expiration`，Select scopes里勾选`repo`、`workflow`，点Generate token，复制下长串token。

进入你fork的项目shared-openwrt下，点Settings → Secrets → Actions → New repository secret，Name填`RELEASE_FIRMWARE`，Value填复制的token，点Add secret。

### 4. 自定义固件

以R4A为例，主要修改三个文本

> `1clone.sh`

固件源码和插件源码，新增插件源时，建议先在本地测试下是否缺依赖。

> `1modify.sh`

固件初始化设置，修改登录IP、主机名、WIFI名称等。

> `.config.R4A`

只带luci应用和theme这两部分，流程中会转为.config，并自动补全为完整的。

### 5. Actions中手动开始编译流程

选择你的`Workflow`，点击Run workflow，按需填内容，运行即可。

对部分选项说明一下

> 上传到release: 

推荐，空间无限

> 上传到artifact: 

不推荐，普通账号的空间只有500MB，存满需要删除

> 版本描述: 

可作一些简单记录，会在release中显示。默认文本里的IP、密码与固件并无关联，你怎么改都可以。

### 5. 编译成功:

Actions流程顺利完成(显示为绿色)后，去release(或者artifact)下载你的固件，allfiles.zip是所有文件的打包。

### 关于小米4A千兆版

创建好Actions secrets后，直接运行`Workflow`就能编译出固件，但默认插件数量较少，如有需要到`.config.R4A`中自行选择。若在`1clone.sh`中添加了插件源，在`.config.R4A`要对应作修改。

小米4A千兆版需修改分区才能在breed直刷，参考[帖子](https://www.right.com.cn/forum/thread-4052254-1-1.html)，本项目中已修改好。

该机型闪存小，若编译插件太多，包体积超出16064K，则不会生成sysupgrade.bin。<br/>
可以去[官方插件库](https://downloads.openwrt.org/snapshots/packages/mips_24kc/packages/)参考各插件大小，下方列出了几个较大插件的最近版本的体积:<br/>
UnblockNeteaseMusic-Go_0.2.13 --- 2.05MB<br/>
luci-app-openclash_0.44.16 --- 2.14MB<br/>
luci-app-vssr_1.23 --- 2.87MB<br/>
xray-core_1.5.3 --- 5.63MB<br/>

---

### 最后

不准备出什么详细的教程，自己摸索吧。

本人小白一个，代码都是网上这里学一点、那里搬一点的，有问题最好不要问我，因为我很大概率解决不了你的问题。

利用庞大的网络知识库，能更快解决你的问题。