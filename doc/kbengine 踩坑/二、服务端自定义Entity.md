# 二、服务端自定义Entity
***

## 1、定义Entity
在服务器资产库中的 `scripts/entity_defs` 目录下，可以找到一些后缀为 `.def` 的文件，它们定义了一个实体的具体内容，包括 **Properties[属性]** 、**ClientMethods[客户端方法]** 、 **BaseMethods[Base方法]** 、 **CellMethods[Cell方法]** 等。
需要指出的是，如果定义某个实体为登陆账号用的，则需要在 **kbengine_def.xml** 或是 **kbengine.xml** 中将 **accountEntityScriptType** 改为对应的实体名字。之后的py脚本里需要继承自 **Proxy** 而不是 **Entity**
```xml
<!--根节点，名称固定为root-->
<root>
    <!--
        该实体的父类def
        这个标签只在Entity.def中有效，如果本身就是一个接口def则该标签被忽略
    -->
    <Parent>Avatar</Parent>

    <!--易变属性同步控制-->
    <Volatile>
        <!--这样设置则总是同步到客户端-->
        <position/>

        <!--没有显式的设置则总是同步到客户端-->
        <!-- <yaw/> -->

        <!--设置为0则不同步到客户端-->
        <pitch>0</pitch>

        <!--距离10米及以内同步到客户端-->
        <roll>10</roll>

        <!--
            如果为true，在一些行为(如：navigate)导致服务器能确定实体在地面时，服务器不同步实体的Y轴坐标。
            当同步大量实体时能节省大量带宽。
        -->
        <optimized>true</optimized>
    </Volatile>

    <!--
        注册接口def，类似于C#中的接口
        这个标签只在Entity.def中有效，如果本身就是一个接口def则该标签被忽略
    -->
    <Implements>
        <!--所有的接口def必须写在entity_defs/interfaces中-->
        <Interface>GameObject</Interface>
    </Implements>

    <Properties>
        <!--属性名称-->
        <accountName>
            <!--属性类型-->
            <Type>UNICODE</Type>

            <!--
                (可选)
                属性的自定义协议ID，如果客户端不使用kbe配套的SDK来开发，客户端需要开发跟kbe对接的协议,
                开发者可以定义属性的ID便于识别，c++协议层使用一个uint16来描述，如果不定义ID则引擎会使用
                自身规则所生成的协议ID, 这个ID必须所有def文件中唯一
            -->
            <Utype>1000</Utype>

            <!--属性的作用域 (参考下方:属性作用域章节)-->
            <Flags>BASE</Flags>

            <!--(可选)是否存储到数据库-->
            <Persistent>true</Persistent>
            
            <!--
                (可选)
                存储到数据库中的最大长度
            -->
            <DatabaseLength>100</DatabaseLength>
            
            <!--
                (可选， 不清楚最好不要设置)
                默认值
            -->
            <Default>kbengine</Default>

            <!--
                (可选)
                数据库索引， 支持UNIQUE与INDEX
            -->
            <Index>UNIQUE</Index>
        </accountName>

        <!--
            .
            .
            .
        -->
    </Properties>

    <ClientMethods>
        
        <!--
            客户端暴露的远程方法名称
        -->
        <onReqAvatarList>
            
            <!--
                远程方法的参数
            -->
            <Arg>AVATAR_INFOS_LIST</Arg>
            
            <!--
                (可选)
                方法的自定义协议ID，如果客户端不使用kbe配套的SDK来开发，客户端需要开发跟kbe对接的协议,
                开发者可以定义属性的ID便于识别，c++协议层使用一个uint16来描述，如果不定义ID则引擎会使用
                自身规则所生成的协议ID, 这个ID必须所有def文件中唯一
            -->
            <Utype>1001</Utype>
        </onReqAvatarList>

        <!--
            .
            .
            .
        -->
    </ClientMethods>

    <BaseMethods>
        
        <!--
            Baseapp暴露的远程方法名称
        -->
        <reqAvatarList>
            
            <!--
                (可选)
                定义了此标记则允许客户端调用,否则仅服务端内部暴露
            -->
            <Exposed/> 
            
            <!--
                (可选)
                方法的自定义协议ID，如果客户端不使用kbe配套的SDK来开发，客户端需要开发跟kbe对接的协议,
                开发者可以定义属性的ID便于识别，c++协议层使用一个uint16来描述，如果不定义ID则引擎会使用
                自身规则所生成的协议ID, 这个ID必须所有def文件中唯一
            -->
            <Utype>1002</Utype>
        </reqAvatarList>

        <!--
            .
            .
            .
        -->
    </BaseMethods>

    <CellMethods>
        
        <!--
            Cellapp暴露的远程方法名称
        -->
        <hello>
            
            <!--
                (可选)
                定义了此标记则允许客户端调用,否则仅服务端内部暴露
            -->
            <Exposed/> 
            
            <!--
                (可选)
                方法的自定义协议ID，如果客户端不使用kbe配套的SDK来开发，客户端需要开发跟kbe对接的协议,
                开发者可以定义属性的ID便于识别，c++协议层使用一个uint16来描述，如果不定义ID则引擎会使用
                自身规则所生成的协议ID, 这个ID必须所有def文件中唯一
            -->
            <Utype>1003</Utype>
        </hello>
    </CellMethods>
</root>
```
