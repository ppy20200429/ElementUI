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
#### 例子1：

[自定义模板](https://img-blog.csdn.net/20180822153249699?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3RnOTI4NjAwNzc0/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

<template slot-scope="scope">

在实际的使用过程中，这种用法当然不仅仅局限于此，其他的地方也会用到。到底这里有什么特别之处呢？

[我们看看普通的table用法：](https://img-blog.csdn.net/20180822153710356?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3RnOTI4NjAwNzc0/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

我们先说一说这个基础的用法里面，在el-table中，:data="tableData"是数据集，结构如下

[tableData结构](https://img-blog.csdn.net/20180822154122926?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3RnOTI4NjAwNzc0/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

那么对于每一个el-table-column，我们只需要使用prop="date"，就可以将该列的数据绑定为该数组所有的对象中的“date”属性，我们可以理解为对于tableData，这里始终取的是tableData[$index].date。

table按照tableData这个数组的长度来生成多少行，按照有多少个el-table-column来生成多少列。

 

现在我们可以看更高级的用法，也就是我们标题提到的<template slot-scope="scope">
```js
  <el-table-column
      label="日期"
      width="180">
      <template slot-scope="scope">
        <i class="el-icon-time"></i>
        <span style="margin-left: 10px">{{ scope.row.date }}</span>
      </template>
    </el-table-column>
```

按照我们前面的理解，按照有多少个el-table-column来生成列，因此这里没有使用prop="date"，生成的单元格也就是空白的一个单元格。

template（模版） 在这里属于一个固定用法： <template slot-scope="scope">

我们主要说一下这个scope是个什么东西，按照element上的提示：

通过 Scoped slot 可以获取到 row, column, $index 和 store（table 内部的状态管理）的数据

我们可以理解为：tableData是给到table的记录集，scope是table内部基于tableData生成出来的，我们可以用Excel描绘一下

[excel表格](https://img-blog.csdn.net/20180822163448889?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3RnOTI4NjAwNzc0/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

我们传进去的tableData，在table内部生成了类似于Excel的scope，因此，通过scope.row.date，我们就可以读取到每一行中的date。

还有重要的一点，scope又并非是整个table，我们只是能通过scope.row获得当前的行数据，至于具体为什么，目前我还没有理解得很透彻。只是希望按照这个理解，能记住多点关于scope的使用。

#### 例子2：
[实例效果图](https://upload-images.jianshu.io/upload_images/15605295-e13e2bc5b886e937.png?imageMogr2/auto-orient/strip|imageView2/2/format/webp)

template:

```js
<el-table :data="tableData" style="width: 100%">
//---:data="用于存放请求数据回来的数组" 
    <el-table-column label="索引值" width="400">
        <template slot-scope="scope">//--- 这里取到当前单元格
            <span>{{ scope.$index }}</span>//--- scope.$index 直接取到该单元格值
        </template>
    </el-table-column>
    <el-table-column label="标题" width="350">
        <template slot-scope="scope">//--- 这里取到当前单元格
            <span>{{ scope.row.title }}</span>
            //--- scope.row 直接取到该单元格对象，即是tableData[scope.$index]
            //---.title 是对象里面的title属性的值
        </template>
    </el-table-column>
    <el-table-column label="操作">
        <template slot-scope="scope">//--- 这里取到当前单元格
            <el-dropdown size="medium" split-button type="primary">
                更多
                <el-dropdown-menu slot="dropdown">
                    <el-dropdown-item @click.native.prevent="handleEdit(scope.$index, scope.row)">编辑</el-dropdown-item>
                    <el-dropdown-item @click.native.prevent="getUp(scope.$index, scope.row)">上升</el-dropdown-item>
                    <el-dropdown-item @click.native.prevent="getDown(scope.$index, scope.row)">下降</el-dropdown-item>
                    <el-dropdown-item @click.native.prevent="handleDelete(scope.$index, scope.row)">删除</el-dropdown-item>
                    //---这里的点击事件已经不是在根元素上了，因为多套了几层结构。
                    //---这里的点击事件如果没有加上 .native 则点击无效！
                    //---这里的点击事件要加上 .native 表示监听组件根元素的原生事件。
                    //---这里的点击事件不需要 .prevent 也可以实现相同效果
                </el-dropdown-menu>
            </el-dropdown>
        </template>
    </el-table-column>
</el-table> 
```
javaScript:

前端删除index要+1

```js
data() {
    return {
       tableData: [{title:123,age:11},{title:456,age:18}]
        //---为了效果先给值，一般情况下为空，其实际值是后台接口请求回来的
      }
  },
methods:{
    handleDelete(index, row) {
      this.tableData.splice(index+1, 1);//---前端删除index要+1 !!!!!!!
      //---下面是后端数据删除，可以不看
      axios.post(config.newsDelete,//---后端数据删除
          {
            id: row.id//---传入被删除的对象的id值
          },
          {
            headers: {
              Authorization: "Bearer " + sessionStorage.getItem("token")//---请求头验证
            }
          }
        )
        .then(res => {
          this.rendering()//---删除了重新渲染
        });
    }
}
```
