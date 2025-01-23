# Developer

## Maven

### Maven 打包依赖并设置主类

```xml
<build>
    <plugins>
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-dependency-plugin</artifactId>
            <executions>
                <execution>
                    <phase>prepare-package</phase>
                    <goals>
                        <goal>unpack-dependencies</goal>
                    </goals>
                    <configuration>
                        <outputDirectory>${project.build.directory}/classes</outputDirectory>
                        <includeArtifactIds>mosslib</includeArtifactIds>
                    </configuration>
                </execution>
            </executions>
        </plugin>
        <plugin>
            <artifactId>maven-assembly-plugin</artifactId>
            <version>3.7.1</version>
            <configuration>
                <descriptorRefs>
                    <descriptorRef>jar-with-dependencies</descriptorRef>
                </descriptorRefs>
                <archive>
                    <manifest>
                        <mainClass>fun.fifu.Neko</mainClass>
                    </manifest>
                </archive>
            </configuration>
            <executions>
                <execution>
                    <id>make-assembly</id>
                    <phase>package</phase>
                    <goals>
                        <goal>single</goal>
                    </goals>
                </execution>
            </executions>
        </plugin>
    </plugins>
</build>
```

### Maven下载源码

```bash
mvn dependency:resolve -Dclassifier=sources
```

### 常用仓库

```xml
<repository> 
    <id>aliyun maven</id> 
    <url>http://maven.aliyun.com/nexus/content/groups/public/</url> 
</repository> 
<repository> 
    <id>minecraft</id> 
    <url>https://lss233.littleservice.cn/repositories/minecraft</url> 
</repository> 
<repository> 
    <id>spigot-repo</id> 
    <url>https://hub.spigotmc.org/nexus/content/repositories/snapshots/</url> 
</repository> 
<repository> 
    <id>papermc</id> 
    <url>https://papermc.io/repo/repository/maven-public/</url> 
</repository> 
<repository> 
    <id>jitpack.io</id> 
    <url>https://jitpack.io</url> 
</repository> 
```

### Maven添加私服

`apache-maven-3.5.0/conf/settings.xml`

```xml
<servers> 
    <server> 
        <id>fifu-datacenter-repository</id> 
        <username>admin</username> 
        <password>password</password> 
    </server> 
</servers> 
```

`pom.xml`

```xml
<distributionManagement> 
    <repository> 
        <id>fifu-datacenter-repository</id> 
        <url>http://192.168.1.3:8080/releases</url> 
    </repository> 
</distributionManagement> 
```

### 依赖重定位命名空间

```xml
<build> 
    <plugins> 
        <plugin> 
            <groupId>org.apache.maven.plugins</groupId> 
            <artifactId>maven-shade-plugin</artifactId> 
            <version>${shade.version}</version> <!--The version must be at least 3.3.0 --><executions> 
                <execution> 
                    <phase>package</phase> 
                    <goals> 
                        <goal>shade</goal> 
                    </goals> 
                    <configuration> 
                        <relocations> 
                            <relocation> 
                                <pattern>net.wesjd.anvilgui</pattern> 
                                <shadedPattern>[YOUR_PLUGIN_PACKAGE].anvilgui</shadedPattern> <!--Replace [YOUR_PLUGIN_PACKAGE] with your namespace --></relocation> 
                        </relocations> 
                        <filters> 
                            <filter> 
                                <artifact>*:*</artifact> 
                                <excludeDefaults>false</excludeDefaults> 
                                <includes> 
                                    <include>[YOUR_PLUGIN_PACKAGE].anvilgui</include> 
                                </includes> 
                            </filter> 
                        </filters>  
                    </configuration> 
                </execution> 
            </executions> 
        </plugin> 
    </plugins> 
</build> 
```

> 来自 <https://github.com/WesJD/AnvilGUI/tree/56b66fe757a7fa46dfcf4a7d9855674c995a3316>

### JOOQ 配置

```bash
mvn jooq-codegen:generate 
```

```xml
<dependencies> 
    <dependency> 
        <groupId>mysql</groupId> 
        <artifactId>mysql-connector-java</artifactId> 
        <version>8.0.18</version> 
    </dependency> 
    <!-- base jooq dependency --> 
    <dependency> 
        <groupId>org.jooq</groupId> 
        <artifactId>jooq</artifactId> 
        <version>3.12.3</version> 
    </dependency> 
</dependencies> 
<build> 
    <plugins> 
        <plugin> 
            <artifactId>maven-compiler-plugin</artifactId> 
            <configuration> 
                <source>1.8</source> 
                <target>1.8</target> 
            </configuration> 
        </plugin> 
        <!-- 代码生成器插件 --> 
        <plugin> 
            <groupId>org.jooq</groupId> 
            <artifactId>jooq-codegen-maven</artifactId> 
            <version>3.12.3</version> 
            <configuration> 
                <jdbc> 
                    <driver>com.mysql.cj.jdbc.Driver</driver> 
                    <url>jdbc:mysql://127.0.0.1:3306/ry-vue?serverTimezone=GMT%2B8</url> 
                    <user>root</user> 
                    <password>X5q9MLnXyt85fmGgPxyVJzy4AUcy5rePnPAwgDDgpdsVtA4KT5NKLdACqL5mnqze</password> 
                </jdbc> 
                <generator> 
                    <database> 
                        <!--<includes>*</includes>--> 
                        <inputSchema>ry-vue</inputSchema> 
                    </database> 
                    <target> 
                        <packageName>com.diamondfsd.jooq.learn.codegen</packageName> 
                        <directory>/src/main/java</directory> 
                    </target> 
                </generator> 
            </configuration> 
        </plugin> 
    </plugins> 
</build> 
```

> 来自 <https://zhuanlan.zhihu.com/p/103834378?utm_id=0>  

### ButtplugMc 依赖安装

```bash
mvn install:install-file \ 
  -Dfile=lib/build/libs/lib-0.0.1.jar \ 
  -DgroupId=io.buttplug \ 
  -DartifactId=buttplug \ 
  -Dversion=0.0.1 \ 
  -Dpackaging=jar \ 
  -DgeneratePom=true 
```

> 来自 <https://github.com/Cyloci/ButtplugMc>

## Gradle

### Gradle配置内网maven仓库

```gradle
repositories{ 
    mavenLocal() 
 
    maven{ 
        url'http://192.168.1.3:8080/releases' 
        allowInsecureProtocol=true 
        credentials{ 
            username'nekocore' 
            password'mfV0MK3XD0UMMppIrfvx4DlplhAGKRkgohdCvS4nW9z9P2H45/i9TEg5gxDkUUdP' 
        } 
    } 
} 
build.gradle 
 
plugins { 
    id 'maven-publish' 
} 
 
publishing { 
    publications { 
        mavenJava(MavenPublication) { 
            groupId group 
            artifactId rootProject.name 
            version version 
 
            from components.java 
            artifact kotlinSourcesJar 
//            artifact javadocJar 
            // more goes in here 
        } 
    } 
 
    repositories { 
        mavenLocal() 
 
        maven { 
            url 'http://192.168.1.3:8080/releases' 
            allowInsecureProtocol = true 
            credentials { 
                username 'nekocore' 
                password 'mfV0MK3XD0UMMppIrfvx4DlplhAGKRkgohdCvS4nW9z9P2H45/i9TEg5gxDkUUdP' 
            } 
        } 
    } 
}
```

### Gradle Wrapper 国内加速

`gradle/wrapper/gradle-wrapper.properties`

```conf
distributionUrl=https\://mirrors.cloud.tencent.com/gradle/gradle-8.7-bin.zip
```

### 配置HTTP代理

`gradle.propertis`

```conf
systemProp.https.proxyHost=127.0.0.1 
systemProp.https.proxyPort=20171 
```

### 手动指定 Java Home

`gradle.propertis`

```conf
org.gradle.java.home=D:\\_SoftWare_Win\\zulu17.38.21-ca-fx-jdk17.0.5-win_x64
```

### Gradle重定向包名

```gradle
relocate("io.vertx", "fun.fifu.biligift.vertx")
```

## Java

### 原生GET请求

```java
Scanner s = new Scanner(new URL("https://www.baidu.com").openStream(), "UTF-8"); 
s.forEachRemaining(System.out::print); 
```

### Android 版本与 API 级别对照表

| 代号 | 版本 | API 级别/NDK 版本 |
| --- | --- | --- |
| Android15 | 15 | API 级别 35 |
| Android14 | 14 | API 级别 34 |
| Android13 | 13 | API 级别 33 |
| Android12L | 12 | API 级别 32 |
| Android12 | 12 | API 级别 31 |
| Android11 | 11 | API 级别 30 |
| Android10 | 10 | API 级别 29 |
| Pie | 9 | API 级别 28 |
| Oreo | 8.1.0 | API 级别 27 |
| Oreo | 8.0.0 | API 级别 26 |
| Nougat | 7.1 | API 级别 25 |
| Nougat | 7.0 | API 级别 24 |
| Marshmallow | 6.0 | API 级别 23 |
| Lollipop | 5.1 | API 级别 22 |
| Lollipop | 5.0 | API 级别 21 |
| KitKat | 4.4 - 4.4.4 | API 级别 19 |
| Jelly Bean | 4.3.x | API 级别 18 |
| Jelly Bean | 4.2.x | API 级别 17 |
| Jelly Bean | 4.1.x | API 级别 16 |
| Ice Cream Sandwich | 4.0.3 - 4.0.4 | API 级别 15，NDK 8 |
| Ice Cream Sandwich | 4.0.1 - 4.0.2 | API 级别 14，NDK 7 |
| Honeycomb | 3.2.x | API 级别 13 |
| Honeycomb | 3.1 | API 级别 12，NDK 6 |
| Honeycomb | 3.0 | API 级别 11 |
| Gingerbread | 2.3.3 - 2.3.7 | API 级别 10 |
| Gingerbread | 2.3 - 2.3.2 | API 级别 9，NDK 5 |
| Froyo | 2.2.x | API 级别 8，NDK 4 |
| Eclair | 2.1 | API 级别 7，NDK 3 |
| Eclair | 2.0.1 | API 级别 6 |
| Eclair | 2.0 | API 级别 5 |
| Donut | 1.6 | API 级别 4，NDK 2 |
| Cupcake | 1.5 | API 级别 3，NDK 1 |
| （无代号） | 1.1 | API 级别 2 |
| （无代号） | 1.0 | API 级别 1 |

### JDK版本与类文件版本对照表

| JDK Version | Class File Version |
|-------------|--------------------|
| Java 1.0    | 45.0               |
| Java 1.1    | 45.3               |
| Java 1.2    | 46.0               |
| Java 1.3    | 47.0               |
| Java 1.4    | 48.0               |
| Java 5      | 49.0               |
| Java 6      | 50.0               |
| Java 7      | 51.0               |
| Java 8      | 52.0               |
| Java 9      | 53.0               |
| Java 10     | 54.0               |
| Java 11     | 55.0               |
| Java 12     | 56.0               |
| Java 13     | 57.0               |
| Java 14     | 58.0               |
| Java 15     | 59.0               |
| Java 16     | 60.0               |
| Java 17     | 61.0               |
| Java 18     | 62.0               |
| Java 19     | 63.0               |
| Java 20     | 64.0               |
| Java 21     | 65.0               |
| Java 22     | 66.0               |
| Java 23     | 67.0               |
| Java 24     | 68.0               |

## Database

### Windows 批量csv导入

```cmd
@echo off 
for /r %%f in (*.csv) do ( 
    echo %%f 
    mongoimport -d <数据库> -c <容器> --file "%%f" --type csv --headerline 
) 
pause 
```

### Mongodb

-------------------MongoDB数据导入与导出-------------------

```text
1、导出工具：mongoexport 
    1、概念： 
        mongoDB中的mongoexport工具可以把一个collection导出成JSON格式或CSV格式的文件。可以通过参数指定导出的数据项，也可以根据指定的条件导出数据。 
    2、语法： 
        mongoexport -d dbname -c collectionname -o file --type json/csv -f field 
        参数说明： 
            -d ：数据库名 
            -c ：collection名 
            -o ：输出的文件名 
            --type ： 输出的格式，默认为json 
            -f ：输出的字段，如果-type为csv，则需要加上-f "字段名" 
    3、示例： 
        sudo mongoexport -d mongotest -c users -o /home/python/Desktop/mongoDB/users.json --type json -f  "_id,user_id,user_name,age,status" 
  
2、数据导入：mongoimport 
    1、语法： 
        mongoimport -d dbname -c collectionname --file filename --headerline --type json/csv -f field 
        参数说明： 
            -d ：数据库名 
            -c ：collection名 
            --type ：导入的格式默认json 
            -f ：导入的字段名 
            --headerline ：如果导入的格式是csv，则可以使用第一行的标题作为导入的字段 
            --file ：要导入的文件 
  
    2、示例： 
        sudo mongoimport -d mongotest -c users --file /home/mongodump/articles.json --type json 
  
```

> 来自 <https://www.cnblogs.com/qingtianyu2015/p/5968400.html>  

-------------------MongoDB备份与恢复-------------------

```text
1、MongoDB数据库备份 
    1、语法： 
        mongodump -h dbhost -d dbname -o dbdirectory 
        参数说明： 
            -h： MongDB所在服务器地址，例如：127.0.0.1，当然也可以指定端口号：127.0.0.1:27017 
            -d： 需要备份的数据库实例，例如：test 
            -o： 备份的数据存放位置，例如：/home/mongodump/，当然该目录需要提前建立，这个目录里面存放该数据库实例的备份数据。 
    2、实例： 
        sudo rm -rf /home/momgodump/ 
        sudo mkdir -p /home/momgodump 
        sudo mongodump -h 192.168.17.129:27017 -d itcast -o /home/mongodump/ 
        - 
2、MongoDB数据库恢复 
    1、语法： 
        mongorestore -h dbhost -d dbname --dir dbdirectory 
  
        参数或名： 
            -h： MongoDB所在服务器地址 
            -d： 需要恢复的数据库实例，例如：test，当然这个名称也可以和备份时候的不一样，比如test2 
            --dir： 备份数据所在位置，例如：/home/mongodump/itcast/ 
            --drop： 恢复的时候，先删除当前数据，然后恢复备份的数据。就是说，恢复后，备份后添加修改的数据都会被删除，慎用！ 
    2、实例： 
    mongorestore -h 192.168.17.129:27017 -d itcast_restore --dir /home/mongodump/itcast/ 
 

```

> 来自 <https://www.cnblogs.com/qingtianyu2015/p/5968400.html>  

```text
Overview 
Use this tutorial to install MongoDB 6.0 Community Edition using the apt package manager. 
MongoDB Version 
This tutorial installs MongoDB 6.0 Community Edition. To install a different version of MongoDB Community, use the version drop-down menu in the upper-left corner of this page to select the documentation for that version. 
Considerations 
Platform Support 
MongoDB 6.0 Community Edition supports the following 64-bit Debian releases on x86_64 architecture: 
    Debian 11 "Bullseye" 
    Debian 10 "Buster" 
    Debian 9 "Stretch" 
MongoDB only supports the 64-bit versions of these platforms. 
See Platform Support for more information. 
Production Notes 
Before deploying MongoDB in a production environment, consider the Production Notes document which offers performance considerations and configuration recommendations for production MongoDB deployments. 
Official MongoDB Packages 
To install MongoDB Community on your Debian system, these instructions will use the official mongodb-org package, which is maintained and supported by MongoDB Inc. The official mongodb-org package always contains the latest version of MongoDB, and is available from its own dedicated repo. 
IMPORTANT 
The mongodb package provided by Debian is not maintained by MongoDB Inc. and conflicts with the official mongodb-org package. If you have already installed the mongodb package on your Debian system, you must first uninstall the mongodb package before proceeding with these instructions. 
See MongoDB Community Edition Packages for the complete list of official packages. 
 
Install MongoDB Community Edition 
Follow these steps to install MongoDB Community Edition using the apt package manager. 
1 .Import the public key used by the package management system. 
From a terminal, issue the following command to import the MongoDB public GPG Key from  https://www.mongodb.org/static/pgp/server-6.0.asc: 
 
wget -qO - https://www.mongodb.org/static/pgp/server-6.0.asc | sudo apt-key add - 
The operation should respond with an OK. 
However, if you receive an error indicating that gnupg is not installed, you can: 
    Install gnupg and its required libraries using the following command: 
     
sudo apt-get install gnupg 
    Once installed, retry importing the key: 
     
wget -qO - https://www.mongodb.org/static/pgp/server-6.0.asc | sudo apt-key add - 
2 .Create a /etc/apt/sources.list.d/mongodb-org-6.0.list file for MongoDB. 
Create the list file using the command appropriate for your version of Debian: 
Debian 11 "Bullseye" 
Debian 10 "Buster" 
Debian 9 "Stretch" 
 
echo"deb http://repo.mongodb.org/apt/debian bullseye/mongodb-org/6.0 main"| sudo tee/etc/apt/sources.list.d/mongodb-org-6.0.list 
3 .Reload local package database. 
Issue the following command to reload the local package database: 
 
sudo apt-get update 
4 .Install the MongoDB packages. 
You can install either the latest stable version of MongoDB or a specific version of MongoDB. 
Install the latest version of MongoDB. 
Install a specific release of MongoDB. 
To install the latest stable version, issue the following 
 
sudo apt-get install -y mongodb-org 
Optional. Although you can specify any available version of MongoDB, apt-get will upgrade the packages when a newer version becomes available. To prevent unintended upgrades, you can pin the package at the currently installed version: 
 
echo"mongodb-org hold"| sudo dpkg --set-selections 
echo"mongodb-org-database hold"| sudo dpkg --set-selections 
echo"mongodb-org-server hold"| sudo dpkg --set-selections 
echo"mongodb-mongosh hold"| sudo dpkg --set-selections 
echo"mongodb-org-mongos hold"| sudo dpkg --set-selections 
echo"mongodb-org-tools hold"| sudo dpkg --set-selections 
Run MongoDB Community Edition 
ulimit Considerations 
Most Unix-like operating systems limit the system resources that a process may use. These limits may negatively impact MongoDB operation, and should be adjusted. See UNIX ulimit Settings for the recommended settings for your platform. 
NOTE 
Starting in MongoDB 4.4, a startup error is generated if the ulimit value for number of open files is under 64000. 
Directories 
By default, a MongoDB instance stores: 
    its data files in /var/lib/mongodb 
    its log files in /var/log/mongodb 
If you installed via the package manager, these default directories are created during the installation.If you installed manually by downloading the tarballs, you can create the directories using mkdir -p <directory> or sudo mkdir -p <directory> depending on the user that will run MongoDB. (See your linux man pages for information on mkdir and sudo.)By default, MongoDB runs using the mongodb user account. If you change the user that runs the MongoDB process, you must also modify the permission to the /var/lib/mongodb and /var/log/mongodb directories to give this user access to these directories.To specify a different log file directory and data file directory, edit the systemLog.path and storage.dbPath settings in the /etc/mongod.conf. Ensure that the user running MongoDB has access to these directories. 
Procedure 
Follow these steps to run MongoDB Community Edition on your system. These instructions assume that you are using the official mongodb-org package -- not the unofficial mongodb package provided by Debian -- and are using the default settings. 
Init System 
To run and manage your mongod process, you will be using your operating system's built-in init system. Recent versions of Linux tend to use systemd (which uses the systemctl command), while older versions of Linux tend to use System V init (which uses the service command). 
If you are unsure which init system your platform uses, run the following command: 
 
ps --no-headers -o comm1 
Then select the appropriate tab below based on the result: 
    systemd - select the systemd (systemctl) tab below. 
    init - select the System V Init (service) tab below. 
systemd (systemctl) 
System V Init (service) 
1 .Start MongoDB. 
You can start the mongod process by issuing the following command: 
 
sudo systemctl start mongod 
If you receive an error similar to the following when starting mongod: 
Failed to start mongod.service: Unit mongod.service not found. 
Run the following command first: 
 
sudo systemctl daemon-reload 
Then run the start command above again. 
2 .Verify that MongoDB has started successfully. 
 
sudo systemctl status mongod 
You can optionally ensure that MongoDB will start following a system reboot by issuing the following command: 
 
sudo systemctl enable mongod 
3 .Stop MongoDB. 
As needed, you can stop the mongod process by issuing the following command: 
 
sudo systemctl stop mongod 
4 .Restart MongoDB. 
You can restart the mongod process by issuing the following command: 
 
sudo systemctl restart mongod 
You can follow the state of the process for errors or important messages by watching the output in the /var/log/mongodb/mongod.log file. 
5 .Begin using MongoDB. 
Start a mongosh session on the same host machine as the mongod. You can run mongosh without any command-line options to connect to a mongod that is running on your localhost with default port 27017. 
 
mongosh 
For more information on connecting using mongosh, such as to connect to a mongod instance running on a different host and/or port, see the mongosh documentation. 
To help you start using MongoDB, MongoDB provides Getting Started Guides in various driver editions. For the driver documentation, see Start Developing with MongoDB. 
Uninstall MongoDB Community Edition 
To completely remove MongoDB from a system, you must remove the MongoDB applications themselves, the configuration files, and any directories containing data and logs. The following section guides you through the necessary steps. 
WARNING 
This process will completely remove MongoDB, its configuration, and all databases. This process is not reversible, so ensure that all of your configuration and data is backed up before proceeding. 
1 .Stop MongoDB. 
Stop the mongod process by issuing the following command: 
 
sudo service mongod stop 
2 .Remove Packages. 
Remove any MongoDB packages that you had previously installed. 
 
sudo apt-get purge mongodb-org* 
3 .Remove Data Directories. 
Remove MongoDB databases and log files. 
 
sudo rm -r /var/log/mongodb 
sudo rm -r /var/lib/mongodb 

```

> 来自 <https://www.mongodb.com/docs/manual/tutorial/install-mongodb-on-debian/>

## Golang

### 编译动态链接库

如果是windows系统，则编译命令为：

```powershell
go build -buildmode=c-shared -o test.dll .\test1.go 
```

如果是linux系统，则编译命令为：

```bash
go build -buildmode=c-shared -o libtest.so .\test1.go 
```

> 来自 <https://www.xzhongwei.com/post/464>

## Flutter

### 构建命令

```bash
flutter run -d edge --web-port=8080 --web-hostname=127.0.0.1  --no-sound-null-safety 
```

### 安桌设备设置横屏

```xml
android:screenOrientation="landscape" 
```

> 来自 <https://www.jianshu.com/p/5f3437f11f96>

### 安桌设备联网权限

```xml
<uses-permission android:name="android.permission.INTERNET" /> 
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" /> 
```

windows修改本地时间

```powershell
Set-Date "2023-04-11 09:27:20+08:00" 
w32tm /resync
```

## Python

### Python编译可执行文件

```bash
nuitka --standalone --onefile demo.py
```

## Rust

### 动态链接法减小编译产物体积

```bash
cargo rustc --release -- -C prefer-dynamic 
```

需要在Cargo.toml中配置如下内容

```toml
[dependencies] 
 prefer-dynamic = "0"  
[dev-dependencies]  
prefer-dynamic = { 
 version = "0", 
 features = ["link-test"] 
} 
```

> 来自 <https://crates.io/crates/prefer-dynamic>  

### 读取文件并存储为字符串

```rust
let mut json_str = String::new(); 
std::fs::File::open("test114514.json") 
    .unwrap() 
    .read_to_string(&mut json_str) 
    .unwrap(); 
// 解析json文本为map 
let mut jmap: HashMap<String, Value> = serde_json::from_str(&json_str).unwrap(); 
for (k, _) in jmap { 
    println!("k:{}", k) 
}
```

### RUST编写和调用DLL

1  执行 `cargo new  hellolib --lib` 创建库项目

修改 `cargo.toml`

```toml
[lib] 
name = "myfirst_rust_dll" #生成dll的文件名 
crate-type = ["dylib"] 
```

`lib.rs`

```rust
#[no_mangle] 
pub extern fn hello_rust(){ 
    println!("Hello rust dll!"); 
} 
```

执行：

```bash
cargo build --release
```

生成了`myfirst_rust_dll.dll`

2、现在准备调用上面的`myfirst_rust_dll.dll`

执行 `cargo new hello` 创建二进制项目

修改 `cargo.toml`

```toml
[dependencies] 
libloading = "0.7" 
```

`main.rs`

```rust
extern crate libloading; 
 
fn call_dynamic() -> Result<u32, Box<dyn std::error::Error>> { 
    unsafe { 
        let lib = libloading::Library::new("myfirst_rust_dll.dll")?; 
        let func: libloading::Symbol<unsafe extern fn() -> u32> = lib.get(b"hello_rust")?; 
        Ok(func()) 
    } 
} 
fn main() { 
    let _a =call_dynamic(); 
 
} 
```

将`myfirst_rust_dll.dll`复制到`hello`目录下，在VSCode中调试，将输出："`Hello rust dll!`"

或将`myfirst_rust_dll.dll`复制到`debug`目录下，在控制台中运行`hello.exe` ，也将输出："`Hello rust dll!`"

## TypeScript

### 按频率从大到小排序并去重

```typescript

function SortingByFrequencyAndRemovingDuplicates<T>(arr: T[]): T[] {
    if (arr == null || arr.length <= 1) {
        return arr
    }

    const frequencyMap = new Map<T, number>()
    arr.forEach((el) => {
        if (frequencyMap.has(el)) {
            frequencyMap.set(el, frequencyMap.get(el)! + 1)
        } else {
            frequencyMap.set(el, 1)
        }
    })

    const frequencyList = Array.from(frequencyMap.entries()).sort((a, b) => b[1] - a[1])

    const result: T[] = []
    frequencyList.forEach(([el, count]) => {
        result.push(el)
    })
    return result
}
```

## JavaScript

### 原生js取URL参数

```javascript
function getQueryVariable(variable) 
{ 
    var query = window.location.search.substring(1); 
    var vars = query.split("&"); 
    for (var i=0;i<vars.length;i++) { 
        var pair = vars[i].split("="); 
        if(pair[0] == variable){return pair[1];} 
    } 
    return(false); 
} 
```

> 来自 <https://www.runoob.com/w3cnote/js-get-url-param.html>  

### NodeJS无视SSL调试项目

```bash
setNODE_OPTIONS=--openssl-legacy-provider && npm run dev 
```

> 来自 <https://levelup.gitconnected.com/how-to-fix-the-err-ossl-evp-unsupported-error-in-node-js-197082f42185>  

## Git

### 优雅地查阅git logs

```bash
git log --graph --format=format:'%C(bold blue)%h%C(reset) - %C(bold green)(%ar)%C(reset) %C(white)%an%C(reset)%C(bold yellow)%d%C(reset) %C(dim white)- %s%C(reset)' --all 
```

## GDScript

### 跳跃按钮

```gdscript
#跳跃按钮 
extends Control 
 
enum Visibility_mode { 
  ALWAYS, ## Always visible 
  TOUCHSCREEN_ONLY ## Visible on touch screens only 
} 
 
## If the joystick is always visible, or is shown only if there is a touchscreen 
@export var visibility_mode := Visibility_mode.TOUCHSCREEN_ONLY 
 
func _ready() -> void: 
  if not DisplayServer.is_touchscreen_available() and visibility_mode == Visibility_mode.TOUCHSCREEN_ONLY: 
    hide() 
 
func _input(event: InputEvent) -> void: 
  if event is InputEventScreenTouch: 
    if event.pressed: 
      if _is_point_inside_joystick_area(event.position): 
        Input.action_press("ui_accept") 
        print("Wheel up") 
    elif event.is_released(): 
      if _is_point_inside_joystick_area(event.position): 
        Input.action_release("ui_accept") 
 
func _is_point_inside_joystick_area(point: Vector2) -> bool: 
  var x: bool = point.x >= global_position.x and point.x <= global_position.x + (size.x * get_global_transform_with_canvas().get_scale().x) 
  var y: bool = point.y >= global_position.y and point.y <= global_position.y + (size.y * get_global_transform_with_canvas().get_scale().y) 
  return x and y
```

## Unity

### 第一人称玩家控制器

```csharp
// 第一人称玩家控制器 

using UnityEngine;

public class PlayerControll : MonoBehaviour
{
    public FloatingJoystick jotstick;
    public DynamicJoystick camraStick;

    public void FixedUpdate()
    {
        PlayerController.Move(0.02f * MoveSpeed * (transform.forward * jotstick.Vertical + transform.right * jotstick.Horizontal));
        // 虚空回弹
        if (playerBody.transform.position.y < -20f)
        {
            playerBody.transform.position = new Vector3(0, 2, 0);
        }
    }

    public void Look()
    {

        float mouseX = camraStick.Horizontal * mouseSpeed * Time.deltaTime;
        float mouseY = camraStick.Vertical * mouseSpeed * Time.deltaTime;

        xRotation -= mouseY;
        xRotation = Mathf.Clamp(xRotation, -90f, 90f);

        Cinema.transform.localRotation = Quaternion.Euler(xRotation, 0f, 0f);

        playerBody.Rotate(Vector3.up * mouseX);
    }

    // -------------------------------------------------

    public float MoveSpeed;
    public float JumpSpeed = 15f;

    public CharacterController PlayerController;

    private void Start()
    {
        //Cursor.lockState = CursorLockMode.Locked;
    }

    private void Update()
    {
        debugMove();
        Look();
        //debugLook();
    }

    private float horizontal;
    private float vertical;

    private float gravity = 9.8f;

    Vector3 Player_Move;
    void debugMove()
    {
        if (PlayerController.isGrounded)
        {
            horizontal = Input.GetAxis("Horizontal");
            vertical = Input.GetAxis("Vertical");
            Player_Move = (transform.forward * vertical + transform.right * horizontal) * MoveSpeed;

            if (Input.GetAxis("Jump") == 1) Player_Move.y += JumpSpeed;
        }
        Player_Move.y = Player_Move.y - gravity * Time.deltaTime;
        PlayerController.Move(Player_Move * Time.deltaTime);
    }

    public float mouseSpeed;
    public Transform playerBody;
    public Transform Cinema;

    float xRotation = 0f;
    void debugLook()
    {
        float mouseX = Input.GetAxis("Mouse X") * mouseSpeed * Time.deltaTime;
        float mouseY = Input.GetAxis("Mouse Y") * mouseSpeed * Time.deltaTime;

        xRotation -= mouseY;
        xRotation = Mathf.Clamp(xRotation, -90f, 90f);

        Cinema.transform.localRotation = Quaternion.Euler(xRotation, 0f, 0f);

        playerBody.Rotate(Vector3.up * mouseX);
    }


    private float _pushForce = 3;

    private void OnControllerColliderHit(ControllerColliderHit hit)
    {
        Rigidbody rb = hit.collider.attachedRigidbody;
        if (rb != null && !rb.isKinematic)
        {
            rb.velocity = hit.moveDirection * _pushForce;
        }
    }

}
```

```csharp
//FixedButton.cs

using UnityEngine;
using UnityEngine.EventSystems;

public class FixedButton : MonoBehaviour, IPointerUpHandler, IPointerDownHandler
{
    [HideInInspector]
    public bool Pressed;

    // Use this for initialization
    void Start()
    {

    }

    // Update is called once per frame
    void Update()
    {

    }

    public void OnPointerDown(PointerEventData eventData)
    {
        Pressed = true;
    }

    public void OnPointerUp(PointerEventData eventData)
    {
        Pressed = false;
    }
}
```

```csharp
//FixedTouchField.cs

using UnityEngine;
using UnityEngine.EventSystems;

public class FixedTouchField : MonoBehaviour, IPointerDownHandler, IPointerUpHandler
{
    [HideInInspector]
    public Vector2 TouchDist;
    [HideInInspector]
    public Vector2 PointerOld;
    [HideInInspector]
    protected int PointerId;
    [HideInInspector]
    public bool Pressed;

    // Use this for initialization
    void Start()
    {

    }

    // Update is called once per frame
    void Update()
    {
        if (Pressed)
        {
            if (PointerId >= 0 && PointerId < Input.touches.Length)
            {
                TouchDist = Input.touches[PointerId].position - PointerOld;
                PointerOld = Input.touches[PointerId].position;
            }
            else
            {
                TouchDist = new Vector2(Input.mousePosition.x, Input.mousePosition.y) - PointerOld;
                PointerOld = Input.mousePosition;
            }
        }
        else
        {
            TouchDist = new Vector2();
        }
    }

    public void OnPointerDown(PointerEventData eventData)
    {
        Pressed = true;
        PointerId = eventData.pointerId;
        PointerOld = eventData.position;
    }


    public void OnPointerUp(PointerEventData eventData)
    {
        Pressed = false;
    }
    
} 
```

## LaTex

### 快速开始

```tex
\documentclass[a4paper, 12pt]{article}
\usepackage{xeCJK}      % 引入 CJK 宏包支持中文
\usepackage{amsmath}    % 引入 amsmath 宏包支持数学公式
\usepackage{hyperref}   % 引入 hyperref 宏包生成书签
\usepackage{geometry}   % 引入 geometry 宏包设置页面边距
\usepackage{setspace}   % 引入 setspace 宏包设置行间距

% 配置 hyperref 宏包
\hypersetup{
    colorlinks=true,        % 链接颜色
    linkcolor=black,        % 内部链接颜色
    citecolor=blue,         % 引用链接颜色
    filecolor=magenta,      % 文件链接颜色
    urlcolor=cyan           % URL 链接颜色
}

% 设置页面边距
\geometry{left=2cm,right=2cm,top=2cm,bottom=2cm}
% 设置行间距
\onehalfspacing

% 设置中文字体为宋体
\setCJKmainfont{SimSun}   % 正文主要字体
\setCJKsansfont{SimHei}   % 无衬线字体（用于标题等）
\setCJKmonofont{FangSong} % 等宽字体（用于代码等）

% 设置西文字体
\setmainfont{Times New Roman} % 正文主要字体
\setsansfont{Arial}           % 无衬线字体（用于标题等）
\setmonofont{Courier New}     % 等宽字体（用于代码等）

\begin{document}
  \title{\LaTeX 快速开始}
  \author{Core}
  \date{\today}
  \maketitle
  % \newpage

  \tableofcontents % 生成目录
  \newpage

  \section{绪论}
  \LaTeX 是基于 TeX 语言的擅长公式表达的自动化排版系统。

  \section{快速开始}

  \subsection{软件安装}
  TeX Live 是 \LaTeX 的发行版之一。你可以到 \href{https://mirrors.tuna.tsinghua.edu.cn}{清华大学开源软件镜像站} 下载并安装。
  
  \subsection{文档攥写}
  我已经猜到了你不知道写什么，你可以复刻本文档，起名为hello.tex。
  
  \subsection{文档排版}
  XeTeX是Unicode友好的 \LaTeX 引擎。我们使用 XeLaTeX 编译器排版 \LaTeX 文档。
  
  \subsubsection{可选前置}
  新建 latexmkrc 文件，写入以下配置：
  \begin{verbatim}
    $pdflatex = 'xelatex %O %S';
  \end{verbatim}

  \subsubsection{编译文档}
  输入命令：
  \begin{verbatim}
    latexmk -xelatex hello.tex
  \end{verbatim}
  可见编译产物 hello.pdf

  \section{最终章}
  \[ mz\mbox{快干活} \]
\end{document}
```
