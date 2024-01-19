# sgx-ubuntu20.04-
sgx安装ubuntu20.04


sgx环境安装
https://zhuanlan.zhihu.com/p/560110720?utm_medium=social&utm_oi=864207061897084928&utm_psn=1731469677329649664&utm_source=wechat_session
https://zhuanlan.zhihu.com/p/415812610?utm_medium=social&utm_oi=864207061897084928&utm_psn=1731470522913873920&utm_source=wechat_session
https://zhuanlan.zhihu.com/p/423209850?utm_medium=social&utm_oi=864207061897084928&utm_psn=1731470039079559168&utm_source=wechat_session

ubuntu启动盘制作
https://blog.csdn.net/qq_36661831/article/details/79724930
https://zhuanlan.zhihu.com/p/378668860?utm_medium=social&utm_oi=864207061897084928&utm_psn=1731599629186232320&utm_source=wechat_session
https://zhuanlan.zhihu.com/p/197908477?utm_medium=social&utm_oi=864207061897084928&utm_psn=1731599543358214144&utm_source=wechat_session

一、driver安装

1.在该下载地址将3个.bin文件下载下来，下载地址：https://download.01.org/intel-sgx/latest/linux-latest/distro/ubuntu20.04-server/

2.到下载文件夹下输入下面命令，以赋予.bin文件的执行权限

sudo chmod 777 sgx_linux_x64_driver_2.11.054c9c4c.bin
3.运行该bin文件，完成驱动安装

二、准备阶段

1.安装编译SGX SDK要用到的工具

sudo apt-get install build-essential ocaml ocamlbuild automake autoconf libtool wget python-is-python3 libssl-dev git cmake perl
2.安装编译SGX PSW要用到的工具

sudo apt-get install libssl-dev libcurl4-openssl-dev protobuf-compiler libprotobuf-dev debhelper cmake reprepro unzip pkgconf libboost-dev libboost-system-dev protobuf-c-compiler libprotobuf-c-dev lsb-release
3.从仓库获取源码，到下载文件夹下，输入

git clone https://github.com/intel/linux-sgx.git
cd linux-sgx && make preparation
此处可能会因为网络原因执行失败，多次尝试执行make preparation后一般会成功

4.把准备好的工具列表添加到全局变量中，方便之后编译工作的展开。到linux-sgx文件夹下，输入以下命令：

sudo cp external/toolset/{current_distr}/* /usr/local/bin
将{current_distr}替换为当前的操作系统。

再用下面这个语句检查是不是添加成功：

which as ld objdump
5.编译SGX SDK和SGX SDK安装工具(installer)，进入linux-sgx文件夹，输入以下命令

make sdk
运行完成后，再输入以下命令

make sdk_install_pkg
成功运行的话，在linux-sgx/linux/installer/bin文件夹下会有一个sgx_linux_x64_sdk_${version}.bin文件生成。

三、sdk安装

1.安装好需要用到的工具，输入以下命令

sudo apt-get install build-essential python
2.安装sdk，进入linux-sgx文件夹，输入以下两条命令

cd linux/installer/bin
./sgx_linux_x64_sdk_${version}.bin
注意：运行第二条命令时，它询问是否安装在当前文件夹的时候，最好选择“no”，然后输入/opt/intel/, 即将SGX SDK安装在/opt/intel/文件夹下。
3.安装完成后，根据提示输入source命令。

四、psw安装

1.命令行运行以下命令添加下载Intel sgx psw的下载路径

echo 'deb [arch=amd64] https://download.01.org/intel-sgx/sgx_repo/ubuntu focal main' | sudo tee /etc/apt/sources.list.d/intel-sgx.list
注意，与ubuntu18.04不同，ubuntu20为ubuntu focal main。

2.进入如下网址下载密钥intel-sgx-deb.key

https://download.01.org/intel-sgx/sgx_repo/ubuntu/

3.进入下载目录，通过如下命令添加进仓库

sudo apt-key add intel-sgx-deb.key
运行后等一会儿看到【ok】就是运行成功。

4.更新一下apt-get的列表

sudo apt-get update
如果系统报错deb无法识别，进入/etc/apt/sources.list.d目录，修改intel-sgx.list文件，去掉deb [arch=amd64] https://download.01.org/intel-sgx/sgx_repo/ubuntu focal main两侧的引号。

5.分别安装SGX PSW 提供的3个服务
分别是launch、EPID-based attestation和Algorithm agnostic attestation，输入以下命令

sudo apt-get install libsgx-launch libsgx-urts
sudo apt-get install libsgx-epid libsgx-urts
sudo apt-get install libsgx-quote-ex libsgx-urts
sudo apt-get install libsgx-dcap-ql
五、测试是否安装成功

进入安装目录（我的是/opt/intel/sgxsdk），再进入/SampleCode/SampleEnclave目录

1.首先准备一下编译环境，输入如下命令

source /opt/intel/sgxsdk/environment
2.编译

make
3.运行

./app
结果返回如下

Checksum(0x0x7ffda4d55720, 100) = 0xfffd4143
Info: executing thread synchronization, please wait...
Info: SampleEnclave successfully returned.
Enter a character before exit ...

恭喜，环境配置成功！



https://blog.csdn.net/myt1018/article/details/124393622

其余参考链接

https://github.com/intel/linux-sgx

https://download.01.org/intel-s
