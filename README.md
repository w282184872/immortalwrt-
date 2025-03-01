编译固件对小白来说真的很难，本地编译又很容易失败，而且找包总是找不到自己想要的插件。
所以整合各位大佬的项目做一个保镖级教程！配置好后傻瓜式一键编译，废话不多说，开始！！
                            
准备：一个ubuntu 64Bit系统的服务器或者虚拟机
          还有一个Github账号.
                          
1.SSH连接到Ubuntu
                             
2.拉取源码
终端输入 ,其他源码自行git，此处只做示例！

git clone --depth=1https://github.com/hanwckf/immortalwrt-mt798x.git

终端输入
cd immortalwrt-mt798x

#MT7981
cp -f defconfig/mt7981-ax3000.config .config

#MT7986
cp -f defconfig/mt7986-ax6000.config .config

#MT7986 256M Low Memory
cp -f defconfig/mt7986-ax6000-256m.config .config
                  
4.终端输入
 make menuconfig
                    
进入配置文件,开始选择型号，添加插件
                   
PS：menuconfig界面下，按两次ESC退到上一级，上下键选中选项按空格确认，M状态为手动（即编译出ipk不安装进固件），*状态为编译进固件
                  
进入Target profile，选择你的路由器型号，回车确定
                 
向下翻找到Luci，进入选择Applicatinos，回车确定，开始添加插件（上下键选中选项按空格确认，M状态为手动（即编译出ipk不安装进固件），*状态为编译进固件）

luci插件对照表
https://kdocs.cn/l/cquwbh9lli8J

选择好插件后，Save保存，然后把.config文件导出到桌面，重命名为1.config
                
Gihub操作：                                 
1.进入P3TERX/Actions-OpenWrt项目页面，点击页面中的Use this template
                                
2.填写仓库名称，然后点击Create repository from template（从模版创建储存库）
                  
3.找到刚才创建好的项目，点击Addfile-Upload files上传1.config文件
                      
4.编辑.github/workflows/openwrt-builder.yml文件
          
name: OpenWrt Builder #你的工作流名称（yml文件名称）

on:

repository_dispatch:

workflow_dispatch:

env:
REPO_URL: https://github.com/coolsnowwolf/lede #源码仓库地址

REPO_BRANCH: master #源码分支

FEEDS_CONF: feeds.conf.default

CONFIG_FILE: .config #你的配置文件名

DIY_P1_SH: diy-part1.sh

DIY_P2_SH: diy-part2.sh

UPLOAD_BIN_DIR: false #上传 bin 目录。即包含所有 ipk 文件和固件的目录。默认false

UPLOAD_FIRMWARE: true #上传固件目录。默认true（建议打开，不然编译好没有固件）

UPLOAD_RELEASE: true #上传固件到 releases 。默认false

TZ: Asia/Shanghai
                  
以上是需要修改的部分代码注释，按需求修改即可
默认登录地址：192.168.50.5，可以去diy-part2.sh文件内修改
              
5.运行Actions工作流,  路径：项目目录--Actions--OpenWrt Builder--Run workflow
            
6.N小时后去releases下载编译好的固件，教程到此结束！
               

地址：192.168.3.1  密码：无
              
感谢P3TERX/Actions-OpenWrt的云编译项目
其他进阶使用移步大佬博客P3TERX






