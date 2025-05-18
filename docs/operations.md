# Operations

## Docker 容器

### Docker安装

一键安装命令：

```bash
curl -fsSL https://get.docker.com | bash -s docker --mirror Aliyun
```

国内版(根据网速任选一段执行)

```bash
# 北京外国语大学开源软件镜像站
export DOWNLOAD_URL="https://mirrors.bfsu.edu.cn/docker-ce"
curl -fsSL https://raw.githubusercontent.com/docker/docker-install/master/install.sh | sh
```

若需要使用GPU，需安装 `container-toolkit` -> [技术文档](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/install-guide.html#installation)

Installing with Apt

```bash
# Configure the production repository:
curl -fsSL https://nvidia.github.io/libnvidia-container/gpgkey | sudo gpg --dearmor -o /usr/share/keyrings/nvidia-container-toolkit-keyring.gpg \
  && curl -s -L https://nvidia.github.io/libnvidia-container/stable/deb/nvidia-container-toolkit.list | \
    sed 's#deb https://#deb [signed-by=/usr/share/keyrings/nvidia-container-toolkit-keyring.gpg] https://#g' | \
    sudo tee /etc/apt/sources.list.d/nvidia-container-toolkit.list

# Optionally, configure the repository to use experimental packages:
sed -i -e '/experimental/ s/^#//g' /etc/apt/sources.list.d/nvidia-container-toolkit.list

# Update the packages list from the repository:
sudo apt-get update

# Install the NVIDIA Container Toolkit packages:
sudo apt-get install -y nvidia-container-toolkit
```

Installing with Zypper

```bash
# Configure the production repository:
sudo zypper ar https://nvidia.github.io/libnvidia-container/stable/rpm/nvidia-container-toolkit.repo

# Optionally, configure the repository to use experimental packages:
sudo zypper modifyrepo --enable nvidia-container-toolkit-experimental

# Install the NVIDIA Container Toolkit packages:
sudo zypper --gpg-auto-import-keys install -y nvidia-container-toolkit
```

Configuring Docker

```bash
sudo nvidia-ctk runtime configure --runtime=docker
sudo systemctl restart docker
```

### Docker 自动重启容器

```bash
docker run -d --restart=always
docker container update --restart=always <容器名字>
```

### Docker 镜像的导入和导出

带压缩

```bash
# 导出
docker save myimage:latest | gzip > myimage_latest.tar.gz
# 导入
docker load --input myimage_latest.tar.gz
```

不压缩

```bash
# 导出
docker save --output myimage_latest.tar myimage:latest
# 导入
docker load --input myimage_latest.tar
```

### Ollama 本地部署大语言模型

[可用模型](https://ollama.com/library)

```bash
docker run -d \
  --restart=always \
  -e OLLAMA_HOST="0.0.0.0" \
  -e OLLAMA_ORIGINS="*" \
  --gpus=all \
  -v ollama:/root/.ollama \
  -p 11434:11434 \
  --name ollama \
  ollama/ollama
```

### MiniConda3 Jupyter

准备工作：

1. 在工程文件夹内创建`Dockerfile`
2. 构建镜像并运行

```dockerfile
# Dockerfile

FROM continuumio/miniconda3
ADD . /root
WORKDIR /root
RUN conda install python=3.8 -y && conda install jupyter -y
CMD ["jupyter", "notebook", "--notebook-dir=/root", "--ip='*'", "--port=8888", "--no-browser", "--allow-root"]
```

```bash
docker build -t myjupyter:py38 .
```

启动容器

```bash
docker run -i -t \
  -p 8888:8888 \
  --gpus=all \
  myjupyter:py38
```

> <https://hub.docker.com/r/continuumio/miniconda3>

### 一键构建Gradle项目

```bash
# 使用Wrapper
docker run -it \
  --rm \
  --network=host \
  -v $(pwd):/pwd \
  -w /pwd \
  azul/zulu-openjdk:21.0.3-jdk \
  /bin/bash -c './gradlew shadowJar'
```

```bash
# 使用外部Gradle
docker run -it \
  --rm \
  --network=host \
  -v $(pwd):/pwd \
  -w /pwd \
  gradle:8.8.0-jdk21 \
  /bin/bash -c 'gradle shadowJar'
```

> <https://hub.docker.com/r/azul/zulu-openjdk>
> <https://hub.docker.com/_/gradle>

### Cocechat Server web聊天服务器

```bash
docker run -d --restart=always \
  -p 3009:3000 \
  --name vocechat-server \
  privoce/vocechat-server:latest
```

### Cloudreve  云盘

```bash
docker run -d \
    --name cloudreve \
    --restart always \
    -v /etc/cloudreve:/root/cloudreve \
    -v /data/cloudreve:/data \
    -p 8081:8080 \
    littleplus/cloudreve-3.0.0-rc-1:sqlite
```

### Discuzq  论坛/博客

```bash
docker run -d \
    --restart always \
    -p 80:80 \
    -p 443:443 \
    ccr.ccs.tencentyun.com/discuzq/dzq:latest
```

### Gitlab 代码托管平台

```bash
docker run --detach \
  --hostname doc.oracleclub.store \
  --publish 443:443 --publish 80:80 --publish 1022:22 \
  --name gitlab \
  --restart always \
  --volume /data/gitlab/config:/etc/gitlab \
  --volume /data/gitlab/logs:/var/log/gitlab \
  --volume /data/gitlab/data:/var/opt/gitlab \
  --shm-size 256m \
  gitlab/gitlab-ce:latest
```

### Apache文件服务器

```bash
docker run -itd \
 --restart=always \
 --name apache-app \
 -p 2333:80 
 -v /data/files:/usr/local/apache2/htdocs/:ro \
 httpd:latest
```

### VSCode网页版服务器

```bash
docker run -d \
  --name=code-server \
  -e PUID=1000 \
  -e PGID=1000 \
  -e TZ=Europe/London \
  -e PASSWORD=password `#optional` \
  -e HASHED_PASSWORD= `#optional` \
  -e SUDO_PASSWORD=password `#optional` \
  -e SUDO_PASSWORD_HASH= `#optional` \
  -e PROXY_DOMAIN=code-server.my.domain `#optional` \
  -e DEFAULT_WORKSPACE=/config/workspace `#optional` \
  -p 8443:8443 \
  -v /path/to/appdata/config:/config \
  --restart unless-stopped \
  lscr.io/linuxserver/code-server:latest
```

来自 <https://hub.docker.com/r/linuxserver/code-server>

### MariaDB

```bash
docker run -itd \
--name mariadb \
--restart always \
-p 3333:3306  \
-v /data/mysql/data:/var/lib/mysql \
-e MYSQL_ROOT_PASSWORD=fP6QtQ39fqU9eJ8bh9Lpgmqt9yjEg7UUeUw6SnDgeLymjz8rVeUxGw5QQ5yHa3hU \
mariadb
```

来自 <https://www.jianshu.com/p/351b71c3cd5a>

### IP摄像头

```bash
docker run -itd  \
--name="zoneminder" \
--net="bridge" \
--privileged="false" \
--shm-size="8G" \
--restart always \
--device=/dev/video0 \
--device=/dev/video1 \
--device=/dev/video2 \
--device=/dev/video3 \
-p 8443:443/tcp \
-p 9001:9000/tcp \
-e TZ="Asia/Shanghai" \
-e PUID="99" \
-e PGID="100" \
-e MULTI_PORT_START="0" \
-e MULTI_PORT_END="0" \
-v "/data/zoneminder":"/config":rw \
-v "/data/zoneminder/data":"/var/cache/zoneminder":rw \
dlandon/zoneminder.machine.learning
```

来自 <https://post.smzdm.com/p/ag437dqw/>

### MongoDB

启动容器，挂载本地目录

``` bash
docker run -itd \
--name mongo \
--restart=always \
-p 27017:27017 \
-v /data/mongodb:/data/db \
mongo:5.0.9
```

> 来自 <https://www.bbsmax.com/A/LPdoGLM2d3/>

### CodeFever Communiy

```bash
docker run -d \
--privileged=true \
--name codefever \
-p 80:80 \
-p 22:22 \
--restart=always \
-it pgyer/codefever-community:latest

/usr/sbin/init
```

> 服务启动后尝试访问 `http://127.0.0.1` 或 `http://<server ip>` 登录  
> 如果你希望使用 `22` 端口作为 Git 的 SSH 协议端口，你需要在启动镜像前将宿主系统的 SSH 服务 端口 先修改成其他端口  
> 如果服务异常你可以登录 Shell 去人工维护, 也可以直接重启容器重启服务。  
> 默认管理员用户: `root@codefever.cn`, 密码: `123456`。登录后请修改密码并绑定 MFA 设备。  
> 来自 <https://github.com/PGYER/codefever/blob/master/doc/zh-cn/installation/install_via_docker.md>  

### Gitea 代码托管平台

```bash
docker run -d --privileged=true \
 --restart=always \
 --name=gitea \
-p 1022:22  \
-p 1080:3000  \
-v /data/gitea:/data \
gitea/gitea:latest
```

### Maven私服 reposilite

```bash
docker run -itd \
-v /data/reposilite:/app/data \
-p 8080:8080 \
--restart=always \
dzikoysk/reposilite:nightly
```

> 然后exec进去生成token，设置权限为m即可

### Maven 仓库管理器

```bash
docker run -d \
-p 2333:80 \
dzikoysk/reposilite:3.0.0-alpha.24
```

### Maven 私服 nexus3

```bash
mkdir /data/nexus-data && chmod 777 /data/nexus-data

docker run -itd \
-p 8081:8081 \
--name nexus3 \
-v /data/nexus-data:/nexus-data \
--restart=always \
sonatype/nexus3

docker stop nexus3 && docker rm nexus3
```

来自 <https://www.jianshu.com/p/8b927b9cd5c0>

> 几款开源的maven 私服  
> <https://archiva.apache.org/>  
> <https://maven.apache.org/repository-management.html>  
> <https://github.com/dzikoysk/reposilite>  
> <https://releases.jfrog.io/artifactory/bintray-artifactory/>  
> <https://github.com/sonatype/nexus-public>  
> <https://github.com/apache/archiva>  
> 来自 <https://www.cnblogs.com/rongfengliang/p/15947682.html>

### Clash服务器

```bash
docker run -d \
--name=clash \
-v "$PWD/config.yaml:/root/.config/clash/config.yaml" \
-p "7890:7890" \
-p "9090:9090" \
--restart=unless-stopped \
dreamacro/clash
```

### 直播推流服务器

```bash
docker run -d \
-p 58080:80 \
--restart always \
--name subweb \
careywong/subweb:latest
```

### VPN-PPTP服务器

```bash
docker run -ti \
--net=host \
--privileged \
-p 1723:1723 \
-e client=put_username \
-e server=* \
-e password=put_password \
-e acceptable_local_ip_addresses=* \
mmontagna/docker-vpn-pptp
```

### Cloudreve

```bash
mkdir -vp /cloudreve/{uploads,avatar} \
&& touch /cloudreve/conf.ini \
&& touch /cloudreve/cloudreve.db

docker run -d \
-p 5212:5212 \
--mount type=bind,source=/cloudreve/conf.ini,target=/cloudreve/conf.ini \
--mount type=bind,source=/cloudreve/cloudreve.db,target=/cloudreve/cloudreve.db \
--restart always \
-v /cloudreve/uploads:/cloudreve/uploads \
-v /cloudreve/avatar:/cloudreve/avatar \
-v /data:/data \
cloudreve/cloudreve:latest
```

### Redis

```bash
docker run --name myredis \
--restart=always \
-p 6379:6379 \
-d redis \
--requirepass "7xbC2AckQFQzF3Ra7kVBKahNzxF6JP6WmnZDD8UW5GyYM9hjWHm5fwPWZsw6c8jygbgFWsVDwunGGuBu"
```

### WireGuard

To automatically install & run wg-easy, simply run:

```bash
 docker run -d \
  --name=wg-easy \
  -e WG_HOST=🚨YOUR_SERVER_IP \
  -e PASSWORD=🚨YOUR_ADMIN_PASSWORD \
  -v ~/.wg-easy:/etc/wireguard \
  -p 51820:51820/udp \
  -p 51821:51821/tcp \
  --cap-add=NET_ADMIN \
  --cap-add=SYS_MODULE \
  --sysctl="net.ipv4.conf.all.src_valid_mark=1" \
  --sysctl="net.ipv4.ip_forward=1" \
  --restart unless-stopped \
  weejewel/wg-easy
```

> 💡 Replace YOUR_SERVER_IP with your WAN IP, or a Dynamic DNS hostname.  
> 💡 Replace YOUR_ADMIN_PASSWORD with a password to log in on the Web UI.  
> The Web UI will now be available on <http://0.0.0.0:51821>.  
> 💡 Your configuration files will be saved in ~/.wg-easy  
> 来自 <https://github.com/WeeJeWel/wg-easy>

### RustDesk自建服务器

```yaml
# docker-compose.yaml
services:
  hbbs:
    container_name: hbbs
    image: rustdesk/rustdesk-server:latest
    command: hbbs
    volumes:
      - ./data:/root
    network_mode: "host"
    depends_on:
      - hbbr
    restart: unless-stopped

  hbbr:
    container_name: hbbr
    image: rustdesk/rustdesk-server:latest
    command: hbbr
    volumes:
      - ./data:/root
    network_mode: "host"
    restart: unless-stopped
```

```bash
docker compose up -d
docker logs hbbs
```

> 开放端口：TCP(21115, 21116, 21117, 21118, 21119) & UDP(21116)  
> id服务器(hbbs): server_ip  
> 中续服务器(hbbr): server_ip:21117  

## 常规操作

### 网络配置

#### Windows下各种二进制软件包

[阿里云二进制镜像站](https://registry.npmmirror.com/binary.html)

#### NodeJs

```bash
npm --registry https://registry.npmmirror.com/
```

#### Python

```bash
# Cli 临时调用 （普通包）
pip install -i https://mirrors.bfsu.edu.cn/pypi/web/simple

# 前缀在requirements.txt第一行 或 Cli调用 （pytorch包）
--extra-index-url https://mirror.sjtu.edu.cn/pytorch-wheels/torch_stable.html
```

#### Conda

```bash
# 命令行修改配置文件
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/conda-forge/
```

#### Docker

```bash
# 镜像名前缀<不稳定>
docker.fxxk.dedyn.io/
```

如果你需要使用本地代理服务，请参阅 [官方文档](https://docs.docker.com/engine/daemon/proxy/#httphttps-proxy)
> 警告⚠： 如果你正在将 `Docker守护进程` 作为 `systemd` 服务运行，你应该创建一个systemd的drop-in文件来设置Docker服务的变量。而 `不是` 修改daemon.json或系统/用户的环境变量  
>
> 警告⚠： 如果你选择配置本地代理服务，请务必保证服务可用

#### Huggingface

```bash
# Linux环境变量
export HF_ENDPOINT=https://hf-mirror.com

# Windows Powershell环境变量
$env:HF_ENDPOINT = "https://hf-mirror.com"

# 非侵入式
HF_ENDPOINT=https://hf-mirror.com python your_script.py
```

### Android 删除应用锁

```bash
rm /data/system/access_control.key
```

### CMD 启动自启动

```batch
:: 这是一个管理员权限下的cmd命令，请勿在PowerShell中执行
reg add "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Command Processor" /v AutoRun /t REG_SZ /d @^%USERPROFILE^%\autorun.cmd" "2^>NUL /f
```

手动在`%USERPROFILE%`创建`autorun.cmd`文件，例如配置fnm([Fast Node Manager](https://github.com/Schniz/fnm)):

```batch
@echo off
:: %USERPROFILE%/autorun.cmd
:: for /F will launch a new instance of cmd so we create a guard to prevent an infnite loop
if not defined FNM_AUTORUN_GUARD (
    set "FNM_AUTORUN_GUARD=AutorunGuard"
    FOR /f "tokens=*" %%z IN ('fnm env --use-on-cd') DO CALL %%z
)
```

### Linux 修改主机名

```bash
hostnamectl set-hostname <hostname>
```

### 程序开机启动

```bash
cd "$(dirname$0)"

crontab -e

@reboot screen -dmS ImageCut java -jar /root/.QQ-Image-Cut-Web/target/ImageCut-0.0.1-SNAPSHOT.jar --server.port=2333
```

run server on screen for crontab and auto on boot run  

```bash
@reboot tmux new-session -d -s yourNameOfTheSession "your command to run"
```

### Linux 关闭密码登录

```bash
nano /etc/ssh/sshd_config
```

修改 `PasswordAuthentication no`

`no`:不使用密码登陆

`yes`:使用密码登陆

重启服务 `service sshd restart`

### Windows 10 Pro 激活密钥

`8NWCW-Y4HCC-TKDRT-DGRH3-YBH3B`

### Science Connect Internet (SCI)

v2_hijkpw

`bash <(curl -sL https://raw.githubusercontent.com/hijkpw/scripts/master/ubuntu_install_v2ray.sh)`

v2

`bash <(curl -s -L https://git.io/v2ray.sh)`

openvpn

`wget https://git.io/vpn -O openvpn-install.sh && bash openvpn-install.sh`

wireguard

`wget https://git.io/wireguard -O wireguard-install.sh && bash wireguard-install.sh`

### Linux 解压缩工具

#### Tar 用法

```bash
# 压缩当前路径
tar -zcvf dir.tar.gz ./
# 指定路径解压
tar -zxvf dir.tar.gz -C dir/

# 从文件 foo 和 bar 创建归档文件 archive.tar。
tar -cf archive.tar foo bar
# 从归档文件 archive.tar 中提取所有文件。
tar -xf archive.tar


# 参数含义：
# -z: 使用gzip压缩格式
# -j: 使用bzip2压缩格式
# -c: 创建压缩文件
# -x: 解压压缩文件
# -v: 显示操作过程
# -f: 要操作的文件
```

#### 7z 用法

```bash
# 安装
apt-get install p7zip-full

# 压缩
7z a -r dir.7z dir/

# 解压
7z x filename.7z -o ./files/
```

#### Zip 用法

```bash
# 压缩目录
zip -r dir1.zip dir1/
# 解压到当前目录下
unzip test.zip

# 指定路径解压  
unzip -d /temp test.zip
# 解压但不覆盖
unzip -n test.zip
unzip -n -d /temp test.zip
# 查看包内文件但不解压
unzip -l test.zip
# 测试压缩包是否损坏
unzip -t test.zip
```

#### Zip文件解压编码问题

使用 `unar` 解压工具，自动处理编码问题

```bash
apt install unar

unar -h
# unar v1.10.7 (Nov 29 2023), a tool for extracting the contents of archive files.

# Usage: unar [options] archive [files ...]

unar test.zip
```

---

使用 [`unzip编码补丁`](https://github.com/ikohara/dpkg-unzip-iconv)

```bash
# 安装
yay -S unzip-iconv

# 用法
unzip -O cp936 myzip.zip
```

---

中国国家标准总局发布的中文字符集有`GB2312`、`GBK`与`GB18030`三种:  

- `GB2312` 发布于1980年，收录汉字6763个
- `GBK` 汉字内码扩展规范，发布于1995年，收录汉字21003个
- `GB18030` 现行版本发布于2005年，收录汉字70244个

> `GBK` 在IBM的Code Page中的第936页，所以也叫 `CP936`

### Redis 配置持久化

RDB持久化配置

Redis会将数据集的快照dump到`dump.rdb`文件中。  
此外，我们也可以通过配置文件来修改Redis服务器dump快照的频率，

在打开`redis.conf`文件之后，我们搜索save，可以看到下面的配置信息：

```bash
save 900 1              #在900秒(15分钟)之后，如果至少有1个key发生变化，则dump内存快照。
save 300 10            #在300秒(5分钟)之后，如果至少有10个key发生变化，则dump内存快照。
save 60 10000        #在60秒(1分钟)之后，如果至少有10000个key发生变化，则dump内存快照。
```

关闭方法： `save ""` 并且删掉持久化文件

AOF持久化配置

在Redis的配置文件中存在三种同步方式，它们分别是：

```bash
appendfsync always     #每次有数据修改发生时都会写入AOF文件。
appendfsync everysec  #每秒钟同步一次，该策略为AOF的缺省策略。
appendfsync no          #从不同步。高效但是数据不会被持久化。
```

关闭方法：`appendfsync no` 并且删掉持久化文件  

大 于 百 亿 级 别 百 万 · 百 亿 级 别 小 于 百 万 级 别

> 来自 <https://www.modb.pro/db/192506>

### JVM 配置

```bash
wget https://cdn.azul.com/zulu/bin/zulu17.34.19-ca-fx-jdk17.0.3-linux_x64.tar.gz

tar -zxvf zulu17.34.19-ca-fx-jdk17.0.3-linux_x64.tar.gz

mv zulu17.34.19-ca-fx-jdk17.0.3-linux_x64 /zulu17

nano /etc/profile

export JAVA_HOME=/zulu17
export JRE_HOME=/zulu17/jre
export CLASSPATH=.:$JAVA_HOME/lib:$JRE_HOME/lib
export PATH=$JAVA_HOME/bin:$PATH

source /etc/profile
```

> 来自 <https://www.jianshu.com/p/4ae48d996d66>

### Debian快速换国内源

```bash
wget http://qiniu.xiwen.online/Debian10.list
mv Debian10.list /etc/apt/sources.list
apt update && apt upgrade -y
```

> 来自 <https://www.jianshu.com/p/b4a792945d99>

### Conda 设置自动激活Base环境

```powershell
# 检查当前配置
conda config --show auto_activate_base
# 禁用自动激活
conda config --set auto_activate_base false
# 手动激活
conda activate base

# 取消激活
conda deactivate
```

### Powershell / Cmd 设置环境变量

```powershell
# 查看所有环境变量
Ls env:

# 搜索环境变量
Ls env:NODE*

# 查看单个环境变量
$env:NODE_ENV

# 添加/更新环境变量
$env:NODE_ENV=development

# 删除环境变量
del evn:NODE_ENV
```

```powershell
# cmd设置环境变量

# 查看所有环境变量
set

# 查看单个环境变量
set NODE_ENV

# 添加/更新环境变量
set NODE_ENV=development

# 删除环境变量
set NODE_ENV=
```

### Python wsgidav 安装方法

```bash
pip install pywin32 cheroot wsgidav
```

安装后执行以下命令开启webdav文件管理共享服务：

```bash
wsgidav --host=0.0.0.0 --port=233 --root=./ --auth=anonymous
wsgidav --host=0.0.0.0 --port=233 --root=./ --auth=nt
```

### Btop安装

一款炫酷的性能测试监控分析工具——btop

安装相关依赖

```bash
yum install coreutils sed git build-essential -y
```

升级gcc

> centos7系统，默认安装的gcc版本是4，版本过低，无法编译安装btop，我们需要升级gcc的版本为10及以上的版本

```bash
yum install centos-release-scl -y
yum install devtoolset-10 -y
scl enable devtoolset-10 bash
echo "source /opt/rh/devtoolset-10/enable" >> /etc/profile
```

检查当前系统的gcc版本，可以看到现在gcc的版本为10

```bash
gcc -v
```

获取源码安装

```bash
git clone <https://gitee.com/mirrors/btop.git>

cd btop

make && make install
```

> (可选) 设置为root用户运行  
> `make setuid`

卸载

```bash
make uninstall
make clean
make distclean
```

### GUI for Debian

```bash
apt-get install xfce4 xfce4-goodies thunar-archive-plugin -y
```

### Linux 安装中文字体包

```bash
apt-get install fonts-wqy-microhei fonts-wqy-zenhei
```

### VNC 安装

```bash
apt install tightvncserver

vncserver
```

### VNC 共享剪切板

```bash
apt install autocutsel
autocutsel -f
```

> 来自 <https://superuser.com/questions/507241/how-to-install-gui-for-debian>

### Windows 查看已连接过的wlan密码

```powershell
netsh wlan show profiles
netsh wlan show profile name="Redmi Note 12 Turbo" key=clear
```

### Linux 手动同步系统时间

```bash
date -s "$(curl http://s3.amazonaws.com -v 2>&1 | grep "Date: " | awk '{ print $3 " " $5 " " $4 " " $7 " " $6 " GMT"}')"
```

> 来自 <https://askubuntu.com/questions/429306/ntpdate-no-server-suitable-for-synchronization-found>

### Enabling RSA Key login

> ⚠ 警告：此操作为危险操作  
> ⚠ Warning: This operation is a hazardous action.

Instead of modifying /etc/ssh/sshd_config, which is the default SSH configuration file, we just need to add a /etc/ssh/sshd_config.d/enable_rsa_keys.conf configuration file:

```bash
cat > /etc/ssh/sshd_config.d/enable_rsa_keys.conf << EOF
HostKeyAlgorithms +ssh-rsa
PubkeyAcceptedKeyTypes +ssh-rsa
EOF
```

> 来自 <https://www.sobyte.net/post/2023-06/debian-enable-ssh-rsa-key/#:~:text=This%20article%20will%20guide%20you%20on%20how%20to,to%20enable%20it%20manually.%20Enabling%20RSA%20Key%20login>

### Linux 清空命令日志

```bash
rm  ~/.bash_history && history -c
```

> 来自 <https://zhuanlan.zhihu.com/p/367623771>

### Linux 移动目录下所有文件到当前目录并清空空文件夹

```bash
find . -type f -print0 | xargs -0 mv -t . ; find . -type d -empty -delete
```

### Linux 查找大文件

```bash
# 大文件
find . -type f -size +800M -print0 | xargs -0 du -B 1M | sort -nr | head -30

# 大文件夹
du -B 1M --max-depth=1 | sort -nr | head -30
```

### APT 更新密钥

```bash
sudo apt-key adv --refresh-keys --keyserver keyserver.ubuntu.com
```

### Linux 输入法配置

> 请无脑选择`fcitx5`，否则你会变得不幸

需要的组件有：`fcitx5` `fcitx5-chinese-addons` `fcitx5-configtool`

openSUSE Tumbleweed示例 (其他发行版请手动安装上面三个组件)：

```bash
sudo zypper in fcitx5
```

写环境变量

```bash
nano /etc/profile
```

将下面代码块添加到末尾

```bash
# fcitx
export GTK_IM_MODULE=fcitx
export QT_IM_MODULE=fcitx
export XMODIFIERS="@im=fcitx"
```

### Openssl111 旧版本安装

```bash
wget <https://www.openssl.org/source/openssl-1.1.1o.tar.gz>
tar -zxvf openssl-1.1.1o.tar.gz
cd openssl-1.1.1o
./config
make
make test
sudo make install
```

### 修改 GRUB（GRand Unified Bootloader）的背景

要修改 GRUB（GRand Unified Bootloader）的背景，您可以按照以下步骤进行操作：

- 准备背景图片：首先，准备一张您想要设置为 GRUB 背景的图片。确保图片的分辨率适合您的屏幕分辨率，并将其保存到您选择的位置（例如 `/boot/grub/background.png`）。

- 编辑 GRUB 配置文件：使用文本编辑器打开 `/etc/default/grub` 文件。

```bash
sudo nano /etc/default/grub
```

- 定位到 `GRUB_BACKGROUND` 行，并将其取消注释（删除行首的 #），然后将路径设置为您保存背景图片的路径。例如：

```bash
GRUB_BACKGROUND="/boot/grub/background.png"
```

如果没有该行，请添加上述行并设置正确的背景图片路径。

- 保存并关闭文件。

- 重新生成 GRUB 配置文件：运行以下命令以重新生成 GRUB 配置文件：

```bash
sudo grub-mkconfig -o /boot/grub/grub.cfg
```

这将使用新的背景设置生成新的 GRUB 配置文件。

- 重启系统：重启您的计算机以使更改生效。在 GRUB 启动界面上，您应该能够看到新设置的背景图片。

> 请注意，以上步骤假设您正在使用 GRUB2 版本，并且 /etc/default/grub 文件是默认的配置文件位置。如果您使用的是不同版本的 GRUB 或自定义了配置文件的位置，请相应地调整命令和路径。  
> 此外，请确保您的背景图片与 GRUB 版本兼容，并遵循适当的图像格式和分辨率要求。某些 GRUB 版本可能对图片格式和大小有限制。  
> 如有需要，请参考 Arch Linux 的官方文档或 GRUB 的文档以获取更多关于修改 GRUB 背景的详细信息和选项。  
> 来自 <https://chat.openai.com/c/a82707d9-ad65-453c-8f5d-0e3257f1eb05>

### Visual Studio Code 快捷键

#### Linux 版本

放大：`Ctrl` + `+`  
缩小：`Ctrl` + `-`  
重置缩放：`Ctrl` + `0`  
> (`0`是小键盘数字键)

格式化代码: `Ctrl` + `Shift` + `I`  
重命名标识符: `F2`

### Linux 安装 Visual Studio Code

#### 基于RHEL, Fedora, 和 CentOS 的分发

我们目前在 yum 存储库中分发稳定的 64 位 VS Code，执行以下脚本将安装密钥和存储库：

```bash
sudo rpm --import https://packages.microsoft.com/keys/microsoft.asc
echo -e "[code]\nname=Visual Studio Code\nbaseurl=https://packages.microsoft.com/yumrepos/vscode\nenabled=1\ngpgcheck=1\ngpgkey=https://packages.microsoft.com/keys/microsoft.asc" | sudo tee /etc/yum.repos.d/vscode.repo > /dev/null
```

然后更新软件包缓存并使用 dnf 安装软件包（适用于 Fedora 22 及以上版本）:

```bash
dnf check-update
sudo dnf install code # or code-insiders
```

> 由于手动签名流程和我们用于发布的系统，yum 存储库可能会滞后，无法立即获取 Visual Studio Code 的最新版本。  
> 原文连接 <https://code.visualstudio.com/docs/setup/linux>

## 杂项

### 电信光猫后门

用户名：`useradmin`  
密码：`nE7jA%5`  

### 移动光猫后门

用户名：`CMCCAdmin`  
密码：`aDm8H%MdA`  

### 紫轩的便携在线工具

核心镜像站 <https://www.fastmirror.net>  
资源中心 <https://res.fastmirror.net>  
高速图床 <https://img.fastmirror.net>  
直链云盘 <https://pan.fastmirror.net>  
剪切板 <https://paste.fastmirror.net>  
随机密码生成器 <https://pass.fastmirror.net>  
Maven开发镜像 <https://maven.fastmirror.net>  

### 在线取真随机数

<https://www.random.org/passwords/?num=100&len=16&format=plain&rnd=new>

### 记忆碎片

adduser xrdp ssl-cert

X79 X66

c4d3fc1881b02fb5f781a2dc244feead

```text
Windows 中存在一些设备命名空间，这些命名空间在作为文件名（不包括文件拓展名）时会导致 Windows 文件流将其当作设备流处理传输和读取数据（实际上并没有提供数据），从而导致 Java 进程卡死进而令服务端崩溃。 

这些设备命名空间是：CON, PRN, AUX, NUL, COM1, COM2, COM3, COM4, COM5, COM6, COM7, COM8, COM9, LPT1, LPT2, LPT3, LPT4, LPT5, LPT6, LPT7, LPT8, LPT9。实际测试中，只有 CON 在不同设备上可以稳定复现问题（因为 CON 代表 Console Input，在所有设备中均存在，而 COM1 等串口上并不一定存在设备） 

对于 Windows 11 操作系统，该问题已得到解决，因此无须特别注意。 

为了测试该漏洞是否在您的操作系统上可用，请打开 CMD，输入 type con.yml 查看命令行界面是否卡住（未出现后续回显） 

Minecraft 从 1.7.6 开始采用 UUID 而不是玩家 ID 作为玩家数据存储方式，这时，在所有第三方插件和模组均使用 UUID 作为文件名存储玩家 ID 时没有问题；但是，我们不能确保所有第三方插件和模组也这么做了 —— 换言之，存在风险。所以最好的解决方案是让所有服主都注意此类问题。但是事实上您会发现，所有可能通过用户输入内容作为文件名存储的通道都会有此类风险，因此： 

对于第三方插件/服主开发者，我们恳请您在支持 UUID 的服务端（1.7.6 及以上）中不要在使用玩家 ID 作为文件名存储数据，而是改用 UUID，并且，请不要直接使用用户输入内容直接作为文件名存储数据。 

除此之外，您也可以通过简单的直接封禁这些游戏 ID 来粗暴的解决这个问题，但仍需警惕来自其他来源的用户输入导致的同类问题 
```
