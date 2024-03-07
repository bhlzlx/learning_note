# FairyGUI 源码结构

编辑器本身依然是用的FairyGUI系统实现的，但是它有额外实现了一套被编辑的对象。
即FObject，界面上的标准UI都是GObject，但是编辑的内容对应的都是FObject。

## 序列化

序列化有两类
1. xml工程
2. 导出二进制

编辑的工程文件都是XML格式的，因此，我们需要关心下FObject的Write()方法，根据当前支持的特性去序列化数据。
同时我们也得在读取的时候分辨好数据内容以兼容旧的版本。
如下所示：

```C#
string[] items = xml.GetAttributeArray("rotation");
if(null != items) {
    if(0 != items.Length) {
        Vector3 rotation = new Vector3(); 
        if(items.Length == 1) { // 支持旧的
            rotation.z = float.Parse(items[0]);
        } else if(items.Length == 3) { // 新的有3个角度
            rotation.x = float.Parse(items[0]);
            rotation.y = float.Parse(items[1]);
            rotation.z = float.Parse(items[2]);
        }
        this.rotation = rotation;
    }
}
```

### 导出二进制

主要关注 `BinaryEncoder.cs`， 
里面有个`const int VERSION = *;`
这里定义着序列化为二进制时，二进制的版本。
这样我们在读取二进制进可以针对这个版本信息辨别有什么数据可以读取。 
