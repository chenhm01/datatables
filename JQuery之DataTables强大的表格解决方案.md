JQuery之DataTables强大的表格解决方案
1、DataTables的默认配置

$(document).ready(function() {
$(‘#example’).dataTable();
} );
示例：http://www.guoxk.com/html/DataTables/Zero-configuration.html

2、DataTables的一些基础属性配置

复制代码
“bPaginate”: true, //翻页功能
“bLengthChange”: true, //改变每页显示数据数量
“bFilter”: true, //过滤功能
“bSort”: false, //排序功能
“bInfo”: true,//页脚信息
“bAutoWidth”: true//自动宽度
复制代码


示例：http://www.guoxk.com/html/DataTables/Feature-enablement.html

3、数据排序

复制代码
$(document).ready(function() {
$(‘#example’).dataTable( {
“aaSorting”: [
[ 4, "desc" ]
]
} );
} );
从第0列开始，以第4列倒序排列
复制代码
示例：http://www.guoxk.com/html/DataTables/Sorting-data.html

4、多列排序

示例：http://www.guoxk.com/html/DataTables/Multi-column-sorting.html

 

5、隐藏某些列

复制代码
$(document).ready(function() {
$(‘#example’).dataTable( {
“aoColumnDefs”: [
{ "bSearchable": false, "bVisible": false, "aTargets": [ 2 ] },
{ “bVisible”: false, “aTargets”: [ 3 ] }
] } );
} );
复制代码
示例：http://www.guoxk.com/html/DataTables/Hidden-columns.html

6、改变页面上元素的位置

复制代码
$(document).ready(function() {
$(‘#example’).dataTable( {
“sDom”: ‘<”top”fli>rt<”bottom”p><”clear”>’
} );
} );
//l- 每页显示数量
//f – 过滤输入
//t – 表单Table
//i – 信息
//p – 翻页
//r – pRocessing
//< and > – div elements
//<”class” and > – div with a class
//Examples: <”wrapper”flipt>, <lf<t>ip>
复制代码
示例：http://www.guoxk.com/html/DataTables/DOM-positioning.html

7、状态保存，使用了翻页或者改变了每页显示数据数量，会保存在cookie中，下回访问时会显示上一次关闭页面时的内容。

$(document).ready(function() {
$(‘#example’).dataTable( {
“bStateSave”: true
} );
} );
示例：http://www.guoxk.com/html/DataTables/State-saving.html

8、显示数字的翻页样式

$(document).ready(function() {
$(‘#example’).dataTable( {
“sPaginationType”: “full_numbers”
} );
} );
示例：http://www.guoxk.com/html/DataTables/Alternative-pagination-styles.html

9、水平限制宽度

复制代码
$(document).ready(function() {
$(‘#example’).dataTable( {
“sScrollX”: “100%”,
“sScrollXInner”: “110%”,
“bScrollCollapse”: true
} );
} );
复制代码
示例：http://www.guoxk.com/html/DataTables/Horizontal.html

10、垂直限制高度

示例：http://www.guoxk.com/html/DataTables/Vertical.html

11、水平和垂直两个方向共同限制

示例：http://www.guoxk.com/html/DataTables/HorizontalVerticalBoth.html

12、改变语言

复制代码
$(document).ready(function() {
$(‘#example’).dataTable( {
“oLanguage”: {
“sLengthMenu”: “每页显示 _MENU_ 条记录”,
“sZeroRecords”: “抱歉， 没有找到”,
“sInfo”: “从 _START_ 到 _END_ /共 _TOTAL_ 条数据”,
“sInfoEmpty”: “没有数据”,
“sInfoFiltered”: “(从 _MAX_ 条数据中检索)”,
“oPaginate”: {
“sFirst”: “首页”,
“sPrevious”: “前一页”,
“sNext”: “后一页”,
“sLast”: “尾页”
},
“sZeroRecords”: “没有检索到数据”,
“sProcessing”: “<img src=\'#\'" /loading.gif’ />”
}
} );
} );
复制代码
示例：http://www.guoxk.com/html/DataTables/Change-language-information.html

13、click事件

示例：http://www.guoxk.com/html/DataTables/event-click.html

14/配合使用tooltip插件

示例：http://www.guoxk.com/html/DataTables/tooltip.html

15、定义每页显示数据数量

$(document).ready(function() {
$(‘#example’).dataTable( {
“aLengthMenu”: [[10, 25, 50, -1], [10, 25, 50, "All"]]
} );
} );
示例：http://www.guoxk.com/html/DataTables/length_menu.html

16、row callback

示例：http://www.guoxk.com/html/DataTables/RowCallback.html

最后一列的值如果为“A”则加粗显示

17、排序控制

复制代码
$(document).ready(function() {
$(‘#example’).dataTable( {
“aoColumns”: [
null,
{ "asSorting": [ "asc" ] },
{ “asSorting”: [ "desc", "asc", "asc" ] },
{ “asSorting”: [ ] },
{ “asSorting”: [ ] }
]
} );
} );
复制代码
示例：http://www.guoxk.com/html/DataTables/sort.html
说明：第一列点击按默认情况排序，第二列点击已顺序排列，第三列点击一次倒序，二三次顺序，第四五列点击不实现排序

18、从配置文件中读取语言包

复制代码
$(document).ready(function() {
$(‘#example’).dataTable( {
“oLanguage”: {
“sUrl”: “cn.txt”
}
} );
} );
复制代码
19、是用ajax源

复制代码
$(document).ready(function() {
$(‘#example’).dataTable( {
“bProcessing”: true,
“sAjaxSource”: ‘./arrays.txt’
} );
} );
复制代码
示例：http://www.guoxk.com/html/DataTables/ajax.html

20、使用ajax，在服务器端整理数据

复制代码
$(document).ready(function() {
$(‘#example’).dataTable( {
“bProcessing”: true,
“bServerSide”: true,
“sPaginationType”: “full_numbers”,
“sAjaxSource”: “./server_processing.php”,
/*如果加上下面这段内容，则使用post方式传递数据
“fnServerData”: function ( sSource, aoData, fnCallback ) {
$.ajax( {
“dataType”: ‘json’,
“type”: “POST”,
“url”: sSource,
“data”: aoData,
“success”: fnCallback
} );
}*/
“oLanguage”: {
“sUrl”: “cn.txt”
},
“aoColumns”: [
{ "sName": "platform" },
{ "sName": "version" },
{ "sName": "engine" },
{ "sName": "browser" },
{ "sName": "grade" }
]//$_GET['sColumns']将接收到aoColumns传递数据
} );
} );
复制代码
示例：http://www.guoxk.com/html/DataTables/ajax-serverSide.html

21、为每行加载id和class

复制代码
服务器端返回数据的格式：
{
“DT_RowId”: “row_8″,
“DT_RowClass”: “gradeA”,
“0″: “Gecko”,
“1″: “Firefox 1.5″,
“2″: “Win 98+ / OSX.2+”,
“3″: “1.8″,
“4″: “A”
},
复制代码
示例：http://www.guoxk.com/html/DataTables/add_id_class.html

22、为每行显示细节，点击行开头的“+”号展开细节显示

示例：http://www.guoxk.com/html/DataTables/with-row-information.html



 

23、选择多行

示例：http://www.guoxk.com/html/DataTables/selectRows.html

22、集成jQuery插件jEditable

示例：http://www.guoxk.com/html/DataTables/jEditable-integration.html

 更过参考：

要注意的是，要被dataTable处理的table对象，必须有thead与tbody，而且，结构要规整（数据不一定要完整），这样才能正确处理。

以下是在进行dataTable绑定处理时候可以附加的参数：

属性名称    取值范围    解释
bAutoWidth  true or false, default true 是否自动计算表格各列宽度
bDeferRender    true or false, default false    用于渲染的一个参数
bFilter true or false, default true 开关，是否启用客户端过滤功能
bInfo   true or false, default true 开关，是否显示表格的一些信息
bJQueryUI   true or false, default false    是否使用jquery ui themeroller的风格
bLengthChange   true or false, default true 开关，是否显示一个每页长度的选择条（需要分页器支持）
bPaginate   true or false, default true 开关，是否显示（使用）分页器
bProcessing true or false, defualt false    开关，以指定当正在处理数据的时候，是否显示“正在处理”这个提示信息
bScrollInfinite true or false, default false    开关，以指定是否无限滚动（与sScrollY配合使用），在大数据量的时候很有用。当这个标志为true的时候，分页器就默认关闭
bSort   true or false, default true 开关，是否让各列具有按列排序功能
bSortClasses    true or false, default true 开关，指定当当前列在排序时，是否增加classes 'sorting_1', 'sorting_2' and 'sorting_3'，打开后，在处理大数据时，性能有所损失
bStateSave  true or false, default false    开关，是否打开客户端状态记录功能。这个数据是记录在cookies中的，打开了这个记录后，即使刷新一次页面，或重新打开浏览器，之前的状态都是保存下来的
sScrollX    'disabled' or  '100%' 类似的字符串    是否开启水平滚动，以及指定滚动区域大小
sScrollY    'disabled' or '200px' 类似的字符串    是否开启垂直滚动，以及指定滚动区域大小
--  --  --
选项       
aaSorting   array array[int,string], 如[], [[0,'asc'], [0,'desc']]   指定按多列数据排序的依据
aaSortingFixed  同上  同上。唯一不同点是不能被用户的自定义配置冲突
aLengthMenu default [10, 25, 50, 100]，可以为一维数组，也可为二维数组，比如：[[10, 25, 50, -1], [10, 25, 50, "All"]]    这个为选择每页的条目数，当使用一个二维数组时，二维层面只能有两个元素，第一个为显示每页条目数的选项，第二个是关于这些选项的解释
aoSearchCols    default null, 类似：[null, {"sSearch": "My filter"}, null,{"sSearch": "^[0-9]", "bEscapeRegex": false}]    给每个列单独定义其初始化搜索列表特性（这一块还没搞懂）
asStripClasses  default ['odd', 'even'], 比如['strip1', 'strip2', 'strip3']   指定要被应用到各行的class风格，会自动循环
bDestroy    true or false, default false    用于当要在同一个元素上执行新的dataTable绑定时，将之前的那个数据对象清除掉，换以新的对象设置
bRetrieve   true or false, default false    用于指明当执行dataTable绑定时，是否返回DataTable对象
bScrollCollapse true or false, default false    指定适当的时候缩起滚动视图
bSortCellsTop   true or false, default false    （未知的东东）
iCookieDuration 整数，默认7200，单位为秒  指定用于存储客户端信息到cookie中的时间长度，超过这个时间后，自动过期
iDeferLoading   整数，默认为null  延迟加载，它的参数为要加载条目的数目，通常与bServerSide，sAjaxSource等配合使用
iDisplayLength  整数，默认为10    用于指定一屏显示的条数，需开启分页器
iDisplayStart   整数，默认为0 用于指定从哪一条数据开始显示到表格中去
iScrollLoadGap  整数，默认为100   用于指定当DataTable设置为滚动时，最多可以一屏显示多少条数据
oSearch 默认{ "sSearch": "", "bRegex": false, "bSmart": true }    又是初始时指定搜索参数相关的，有点复杂，没搞懂目前
sAjaxDataProp   字符串，default 'aaData'    指定当从服务端获取表格数据时，数据项使用的名字
sAjaxSource URL字符串，default null 指定要从哪个URL获取数据
sCookiePrefix   字符串，default 'SpryMedia_DataTables_' 当打开状态存储特性后，用于指定存储在cookies中的字符串的前缀名字
sDom    default lfrtip (when bJQueryUI is false) or <"H"lfr>t<"F"ip> (when bJQueryUI is true)   这是用于定义DataTable布局的一个强大的属性，另开专门文档来补充说明吧
sPaginationType 'full_numbers' or 'two_button', default 'two_button'    用于指定分页器风格
sScrollXInner   string default 'disabled'   又是水平滚动相关的，没搞懂啥意思
DataTable支持如下回调函数 

回调函数名称  参数  返回值 默认  功能
fnCookieCallback    1.string: Name of the cookie defined by DataTables 2.object: Data to be stored in the cookie 3.string: Cookie expires string 4.string: Path of the cookie to set    string: cookie formatted string (which should be encoded by using encodeURIComponent()) null    当每次cookies改变时，会触发这个函数调用
fnDrawCallback  无   无   无   在每次table被draw完后调用，至于做什么就看着办吧
fnFooterCallback    1.node : "TR" element for the footer 2.array array strings : Full table data (as derived from the original HTML) 3.int : Index for the current display starting point in the display array< 4.int : Index for the current display ending point in the display array 5.array int : Index array to translate the visual position to the full data array   无   无   用于在每次重画的时候修改表格的脚部
fnFormatNumber  1.int : number to be formatted  String : formatted string for DataTables to show the number 有默认的    用于在大数字上，自动加入一些逗号，分隔开
fnHeaderCallback    1.node : "TR" element for the header 2.array array strings : Full table data (as derived from the original HTML) 3.int : Index for the current display starting point in the display array 4.int : Index for the current display ending point in the display array 5.array int : Index array to translate the visual position to the full data array    无   无   用于在每次draw发生时，修改table的header
fnInfoCallback  1.object: DataTables settings object 2.int: Starting position in data for the draw 3.int: End position in data for the draw 4.int: Total number of rows in the table (regardless of filtering) 5.int: Total number of rows in the data set, after filtering 6.string: The string that DataTables has formatted using it's own rules string: The string to be displayed in the information element.  无   用于传达table信息
fnInitComplete  1.object:oSettings - DataTables settings object 无   无   表格初始化完成后调用
fnPreDrawCallback   1.object:oSettings - DataTables settings object Boolean 无   用于在开始绘制之前调用，返回false的话，会阻止draw事件发生；返回其它值，draw可以顺利执行
fnRowCallback   1.node : "TR" element for the current row 2.array strings : Raw data array for this row (as derived from the original HTML) 3.int : The display index for the current table draw 4.int : The index of the data in the full list of rows (after filtering)   node : "TR" element for the current row 无   当创建了行，但还未绘制到屏幕上的时候调用，通常用于改变行的class风格
fnServerData    1.string: HTTP source to obtain the data from (i.e. sAjaxSource) 2.array objects: A key/value pair object containing the data to send to the server 3.function: Function to be called on completion of the data get process that will draw the data on the page.    void    $.getJSON   用于替换默认发到服务端的请求操作
fnStateLoadCallback 1.object:oSettings - DataTables settings object 2.object:oData - Object containing information retrieved from the state saving cookie which should be restored. For the exact properties please refer to the DataTables code.   Boolean - false if the state should not be loaded, true otherwise   无   在cookies中的数据被加载前执行，可以方便地修改这些数据
fnStateSaveCallback 1.object:oSettings - DataTables settings object 2.String:sValue - a JSON string (without the final closing brace) which should be stored in the state saving cookie.    String - the full string that should be used to save the state  无   在状态数据被存储到cookies前执行，可以方便地做一些预操作
好文要顶 关注我 收藏该文    
呆河马
关注 - 10
粉丝 - 32
+加关注
4 0
« 上一篇：MySql and Oracle Data Type Mappings
» 下一篇：HTTP状态代码列表
ADD YOUR COMMENT
#1楼 dongqinglove  
2016-06-07 15:14
hi，请问在jquery.datatables.js在哪儿设置总页数和总记录数
支持(0)反对(0)
回复引用
#2楼[楼主] 呆河马  
2016-06-12 11:42
@ dongqinglove
“sInfo”: “从 _START_ 到 _END_ /共 _TOTAL_ 条数据--可以参照这里”_END_ 是页数，_TOTAL_是总记录数。
支持(0)反对(0)