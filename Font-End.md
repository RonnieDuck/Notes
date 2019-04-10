#DataTables绘制表格
###*.html*
- 给所要绘制表格的table加上id，编写静态的表头thead
```html
<thead>
  <tr>
    <th>column1</th>
    <th>column2</th>
    <th>column3</th>
    <th>column4</th>
    <th>column5</th>
  </tr>
</thead>
<tbody>
js渲染...
</tbody>
```
###*.js*
- 定义传参用表格对象，属性等
```js
var emp = {
  param1: 123,
  param2: 123,
  ...
};
```
- 用定义好id的table调用DataTable()方法
```js
_table = $('#id').DataTable({
  ajax: function(data, callback, settings) {
    $.ajax({
      type: "GET",
      url: hostIp + 'finance_all',
      cache: false, //禁用缓存
      dataType: 'json',
      success: function(result) {
        ...
      },
      error: function(XMLHttpRequest, textStatus, errorThrown) {
        ...
      }
    });
  },
  param1:emp.param1,
  param2:emp.param2,
  ...
}).api();
```
- 报表系统中通常会有多个表，但多表同时拥有相同配置时可以另参数对象实现继承
```js
_table = $('#id').DataTable($extend(true,{},inherited_obj,{
  ...
})).api();
```
- *Attention:*
>- 一个table只能调用一次DataTable()方法初始化，如果想操作表格(如重新加载数据)可以调用同时赋值然后调用各种api
>-  ``
  _table = $('#id').DataTable({
    ...
  });
  _table.ajax().reload(); //调用重新加载表格api
  ``
>- 具体看[DataTables API](http://datatables.club/reference/api/)
- *Reference:*
>- [DataTables配置属性](http://datatables.club/reference/option/)


#DataTables callback函数
###*createRow*
- tr行元素被完整创建时(td已全部渲染)的回调函数
- 注入参数：</vbr>row[node]:已经被创建的tr元素</vbr>data[array/object]:包含这行的数据对象</vbr>dataIndex[integer]:Datatables内部存储的数据索引
- EXAMPLE:
```js
//给指定字符的数据添加一个样式
$('#example').dataTable( {
  "createdRow": function( row, data, dataIndex ) {
    if ( data[4] == "A" ) {
      $(row).addClass( 'important' );
    }
  }
});
```
- *Reference:*
>- [createdRow( row, data, dataIndex )](http://datatables.club/reference/option/createdRow.html)

###*columns.createdCell*
- 单元格生成以后的回调函数，这样你可以在这里改变DOM
- EXAMPLE:
```js
$('#example').dataTable( {
  "columnDefs": [ {
    "targets": 3,
    "createdCell": function (td, cellData, rowData, row, col) {
      if ( cellData < 1 ) {
        $(td).css('color', 'red')
      }
    }
  } ]
} );
```
- *Attention:*
>- 该回调函数需要在配置"columnDefs"中调用


#ECharts绘制图表
###*html*
- 像普通的 JavaScript 库一样用 script 标签引入依赖文件
```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <!-- 引入 ECharts 文件 -->
    <script src="echarts.min.js"></script>
</head>
</html>
```
- 在绘图前需要为Charts准备一个具备高宽的DOM容器
```html
<body>
    <!-- 为 ECharts 准备一个具备大小（宽高）的 DOM -->
    <div id="main" style="width: 600px;height:400px;"></div>
</body>
```

###*js*
- 通过 echarts.init 方法初始化一个echarts实例并通过 setOption 方法生成一个图表(下面为简单的柱状图)
```js
// 基于准备好的dom，初始化echarts实例
var myChart = echarts.init(document.getElementById('main'));
// 指定图表的配置项和数据
var option = {
  title: {
    text: 'ECharts 入门示例'
  },
  tooltip: {},
  legend: {
    data:['销量']
  },
  xAxis: {
    data: ["衬衫","羊毛衫","雪纺衫","裤子","高跟鞋","袜子"]
  },
  yAxis: {},
  series: [{
    name: '销量',
    type: 'bar',
    data: [5, 20, 36, 10, 10, 20]
  }]
};
// 使用刚指定的配置项和数据显示图表。
myChart.setOption(option);
```

###*异步数据加载和更新*
- 初始化图表之后，data通过setOption填入到表中。所以异步数据加载或者更新，直接对option中的data赋值后，调用charts.setOption(option)方法即可
```js
// 异步加载数据
$.get('data.json').done(function (data) {
  // 填入数据
  myChart.setOption({
    xAxis: {
      data: data.categories
    },
    series: [{
      // 根据名字对应到相应的系列
      name: '销量',
      data: data.data
    }]
  });
});
```
- 实时数据更新同理，不同的是要往数据中增加数据。data.push()然后调用charts.setOption(option)，ECharts即可通过合适的动画去表现数据的变化
