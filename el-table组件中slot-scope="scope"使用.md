#### 通过 Scoped slot 可以获取到 row, column, $index 和 store（table 内部的状态管理）的数据：
>> {{scope.row}} =>获取整行的数据
>> {{scope.$index}} => 行的下标
使用:
```js
<el-table <el-table-column
<template slot-scope="scope">
{{ scope.row.userSex === 1 ? "男" : "女" }} </template>

/el-table-column>
/el-table>
```
