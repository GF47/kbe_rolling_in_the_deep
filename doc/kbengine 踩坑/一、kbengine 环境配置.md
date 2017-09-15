# 一、kbengine 环境配置
***

详见官网教程，不多赘述

## 1、还是说两句吧
自己研究暂时还是在 **Windows** 上进行，主要是客户端服务端来回切换，开虚拟机有点卡……
**Windows** 环境下， **kbengine** 大概只能跑16个客户端？忘了，应该是。

## 2、配置步骤

### 2.1、使用官方提供的安装脚本
可以选择使用 `kbengine/kbe/tools/server/install/installer.py` 这个脚本来安装，只需要在命令行输入 `python 脚本路径 install` 即可。

### 2.2、手动安装

#### 2.2.1、数据库
默认支持 **MySQL** ，可以自己添加中间层。
* 安装 **MySQL** ，注意在 **Windows** 环境下， **MySQL** 默认忽略大小写，所以在 `my.ini` 中添加设置大小写敏感的项。
```ini
[mysqld]
lower_case_table_names = 2
```
> 1是不对的， **MySQL** 官网上有解释，设置成2是对的。不然服务开启不了。

* 确保 **MySQL** 的端口和 **kbengine** 一致，具体路径为 `%KBE_ROOT%\kbe\res\server\kbengine_def.xml` 中的 `root->dbmgr->...->port` 参数。

* 创建名为 `kbe` 的数据库，如果不使用默认的名字，需要配置好 **kbengine** 之后修改 `%KBE_ROOT%\kbe\res\server\kbengine_def.xml` 中的 `dbmgr->...->databaseName` 参数。
```sql
myslq>create database kbe;
```
> 最好是不改动引擎自带的配置文件，而是修改 `%KBE_ROOT%\assets\res\server\kbengine.xml` 中的值，避免多项目之间产生冲突。

* 删除匿名用户。
```sql
mysql>use mysql
mysql>delete from user where user='';
mysql>FLUSH PRIVILEGES;
```

* 创建kbe用户，用户名和密码暂设为 `kbe` 。
```sql
mysql>grant all privileges on *.* to kbe@'%' identified by 'kbe';
myslq>grant select,insert,update,delete,create,drop on *.* to kbe@'%' identified by 'kbe';
myslq>FLUSH PRIVILEGES;
```

* 测试是否可以正常登陆 **MySQL** 。
```cmd
mysql -ukbe -pkbe
```

* 用到的参数位置
```xml
<root>
    <dbmgr>
        <databaseInterface>
            <default>
                <host>localhost</host>
                <port>3306</port>
                <auth>
                    <username>kbe</username>
                    <password>kbe</password>
                </auth>
                <databaseName>kbe</databaseName>
            </default>
        </databaseInterface>
    </dbmgr>
</root>
```

#### 2.2.2、设置环境变量
```
UID          服务器组，高深知识请参考官网，我等小白退散
KBE_ROOT     引擎的根目录
KBE_RES_PATH 引擎资产库目录和项目资产库目录，用分号隔开
KBE_BIN_PATH 引擎可执行文件目录
```

### 2.3、卸载
执行如下命令：
```cmd
python %KBE_ROOT%/kbe/tools/server/install/installer.py uninstall
```
或者想手动卸载，删了 `KBE_ROOT` 文件夹，把环境变量里的改动删了，数据库删掉，就完了。
