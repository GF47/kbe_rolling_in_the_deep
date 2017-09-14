# 二、服务端自定义Entity
***

## 定义Entity
在服务器资产库中的 `scripts/entity_defs` 目录下，可以找到一些后缀为 `.def` 的文件，它们定义了一个实体的具体内容，包括 **Properties[属性]** 、**ClientMethods[客户端方法]** 、 **BaseMethods[Base方法]** 、 **CellMethods[Cell方法]** 等。
``` xml
<root><!--根节点，名称固定为root-->
    
    <Implements>
    </Implements>
    <!--属性节点-->
	<Properties>
	</Properties>

	<ClientMethods>
	</ClientMethods>

	<BaseMethods>
	</BaseMethods>

	<CellMethods>
	</CellMethods>

</root>
```
