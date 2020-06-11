#### 通过 Scoped slot 可以获取到 row, column, $index 和 store（table 内部的状态管理）的数据：
> {{scope.row}} =>获取整行的数据

> {{scope.$index}} => 行的下标
使用:
```js
<el-table <el-table-column
<template slot-scope="scope">
{{ scope.row.userSex === 1 ? "男" : "女" }} </template>
/el-table-column>
/el-table>
```

[自定义模板](https://img-blog.csdn.net/20180822153249699?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3RnOTI4NjAwNzc0/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

<template slot-scope="scope">

在实际的使用过程中，这种用法当然不仅仅局限于此，其他的地方也会用到。到底这里有什么特别之处呢？

[我们看看普通的table用法：](https://img-blog.csdn.net/20180822153710356?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3RnOTI4NjAwNzc0/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

