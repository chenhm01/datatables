最全的jquery datatables api 使用详解
学习可参考：

http://www.guoxk.com/node/jquery-datatables

http://yuemeiqing2008-163-com.iteye.com/blog/2006942

分别导入css和js文件

<link href="~/Content/bootstrap.css" rel="stylesheet" />
<link href="~/Content/datatables/css/dataTables.bootstrap.css" rel="stylesheet" />
加载

<script type="text/javascript">  
        $(document).ready(function() {  
            $('#example').dataTable();  
        });  
    </script>  
 表单

<div class="col_2_3_right">  
                <div class="index_viewport">  
                    <table id="example" cellpadding="0" cellspacing="0" border="0" width="100%">  
                        <thead>  
                            <tr>  
                                <th width="20%">First name</th>  
                                <th width="20%">Last name</th>  
                                <th width="35%">City</th>  
                                <th width="25%">Date</th>  
                            </tr>  
                        </thead>  
                    </table>  
                </div>  
            </div>  
问题：有时提示找不到datatable方法

原因
window.onload必须等到页面内包括图片的所有元素加载完毕后才能执行。 
$(document).ready()是DOM结构绘制完毕后就执行，不必等到加载完毕。
 从后台ajax得数据重建datatables（表单的id要与json的key一致）

$.ajax({  
                    　　type:'get',//可选get  
                    　　url:'${projectPath}/search',  
                    　　data:{"channelType":$('#channelType').val(),"channel":$('#channel').val(),"day":$('#day').val(),"startTime":$('#startTime').val(),"endTime":$('#endTime').val(),"database":$('#database').val()},  
                    　　dataType:'text',//服务器返回的数据类型 可选XML ,Json jsonp script htmltext等  
                    　　success:function(msg){  
                        var msgObj=JSON.parse(msg);  
                        //重新构建table  
                         $('#example').dataTable().fnClearTable();   //将数据清除  
                    　    $('#example').dataTable().fnAddData(packagingdatatabledata(msgObj),true);  //数据必须是json对象或json对象数组  
                    　  
                　　},  
                    　　error:function(){  
                    　　alert('error');  
                    　　}  
            })})  
传入的数据

//把服务器返回的数据转成datatable须要的格式  
        function packagingdatatabledata(msgObj){  
            var editHtml="<a class='btn' data-toggle='modal' href='#modalbackdroptrue' >编辑</a>";  
            //var editHtml="<a class='btn' href='#modalbackdroptrue' data-toggle='modal' >编辑</a>";  
            var a=[];  
            var tableName=['day','data','indata','edit'];  
            var banddata=storjson(msgObj['data']);  
            var bandindata=storjson(msgObj['indata']);  
            for(var key in banddata){  
                var tempObj=new Object();  
                tempObj.day=key;  
                tempObj.data=banddata[key];  
                tempObj.indata=bandindata[key];  
                tempObj.edit=editHtml;  
                a.push(JSON.parse(JSON.stringify(tempObj,tableName)));  
                }  
            return a;  
        }  
 传入的是一个对象数组，每个对象代表一行，对象的属性即是列

 

单击时得到某行的值

   须要引入 jQuery.dataTables.nightly.js  在附件中有  

$(document).ready(function() {  
    /* Init DataTables */  
    $('#example').dataTable();  
$('#example tbody tr').live('click', function () {  
                var sTitle;  
                var nTds = $('td', this);  
                var sday = $(nTds[0]).text();  //得到第1列的值  
                var sout = $(nTds[1]).text();  
                var sin = $(nTds[2]).text();  
                  
            } );  
  
} );  
   

2：详细配置

var docrTable = $('#docrevisontable').dataTable({  
            "bProcessing" : true, //DataTables载入数据时，是否显示‘进度’提示  
            "bServerSide" : true, //是否启动服务器端数据导入  
            "bStateSave" : true, //是否打开客户端状态记录功能,此功能在ajax刷新纪录的时候不会将个性化设定回复为初始化状态  
            "bJQueryUI" : true, //是否使用 jQury的UI theme  
            "sScrollY" : 450, //DataTables的高  
            "sScrollX" : 820, //DataTables的宽  
            "aLengthMenu" : [20, 40, 60], //更改显示记录数选项  
            "iDisplayLength" : 40, //默认显示的记录数  
            "bAutoWidth" : false, //是否自适应宽度  
            //"bScrollInfinite" : false, //是否启动初始化滚动条  
            "bScrollCollapse" : true, //是否开启DataTables的高度自适应，当数据条数不够分页数据条数的时候，插件高度是否随数据条数而改变  
            "bPaginate" : true, //是否显示（应用）分页器  
            "bInfo" : true, //是否显示页脚信息，DataTables插件左下角显示记录数  
            "sPaginationType" : "full_numbers", //详细分页组，可以支持直接跳转到某页  
            "bSort" : true, //是否启动各个字段的排序功能  
            "aaSorting" : [[1, "asc"]], //默认的排序方式，第2列，升序排列  
            "bFilter" : true, //是否启动过滤、搜索功能  
                    "aoColumns" : [{  
                        "mDataProp" : "USERID",  
                        "sDefaultContent" : "", //此列默认值为""，以防数据中没有此值，DataTables加载数据的时候报错  
                        "bVisible" : false //此列不显示  
                    }, {  
                        "mDataProp" : "USERNAME",  
                        "sTitle" : "用户名",  
                        "sDefaultContent" : "",  
                        "sClass" : "center"  
                    }, {  
                        "mDataProp" : "EMAIL",  
                        "sTitle" : "电子邮箱",  
                        "sDefaultContent" : "",  
                        "sClass" : "center"  
                    }, {  
                        "mDataProp" : "MOBILE",  
                        "sTitle" : "手机",  
                        "sDefaultContent" : "",  
                        "sClass" : "center"  
                    }, {  
                        "mDataProp" : "PHONE",  
                        "sTitle" : "座机",  
                        "sDefaultContent" : "",  
                        "sClass" : "center"  
                    }, {  
                        "mDataProp" : "NAME",  
                        "sTitle" : "姓名",  
                        "sDefaultContent" : "",  
                        "sClass" : "center"  
                    }, {  
                        "mDataProp" : "ISADMIN",  
                        "sTitle" : "用户权限",  
                        "sDefaultContent" : "",  
                        "sClass" : "center"  
                    }],  
                    "oLanguage": { //国际化配置  
                "sProcessing" : "正在获取数据，请稍后...",    
                "sLengthMenu" : "显示 _MENU_ 条",    
                "sZeroRecords" : "没有您要搜索的内容",    
                "sInfo" : "从 _START_ 到  _END_ 条记录 总记录数为 _TOTAL_ 条",    
                "sInfoEmpty" : "记录数为0",    
                "sInfoFiltered" : "(全部记录数 _MAX_ 条)",    
                "sInfoPostFix" : "",    
                "sSearch" : "搜索",    
                "sUrl" : "",    
                "oPaginate": {    
                    "sFirst" : "第一页",    
                    "sPrevious" : "上一页",    
                    "sNext" : "下一页",    
                    "sLast" : "最后一页"    
                }  
            },  
                      
                    "fnRowCallback" : function(nRow, aData, iDisplayIndex) {  
                        /* 用来改写用户权限的 */  
                        if (aData.ISADMIN == '1')  
                            $('td:eq(5)', nRow).html('管理员');  
                        if (aData.ISADMIN == '2')  
                            $('td:eq(5)', nRow).html('资料下载');  
                        if (aData.ISADMIN == '3')  
                            $('td:eq(5)', nRow).html('一般用户');  
                          
                        return nRow;  
                    },  
                      
                    "sAjaxSource" : "../use/userList.do?now=" + new Date().getTime(),  
                        //服务器端，数据回调处理  
                            "fnServerData" : function(sSource, aDataSet, fnCallback) {  
                                $.ajax({  
                                    "dataType" : 'json',  
                                    "type" : "POST",  
                                    "url" : sSource,  
                                    "data" : aDataSet,  
                                    "success" : fnCallback  
                                });  
                            }  
                });  
                  
                $("#docrevisontable tbody").click(function(event) { //当点击表格内某一条记录的时候，会将此记录的cId和cName写入到隐藏域中  
                    $(docrTable.fnSettings().aoData).each(function() {  
                        $(this.nTr).removeClass('row_selected');  
                    });  
                    $(event.target.parentNode).addClass('row_selected');  
                    var aData = docrTable.fnGetData(event.target.parentNode);  
                      
                    $("#userId").val(aData.USERID);  
                    $("#userN").val(aData.USERNAME);  
                });  
                  
                $('#docrevisontable_filter').html('<span>用户管理列表');  
                $('#docrevisontable_filter').append('   <input type="button" class="addBtn" id="addBut" value="创建"/>');  
                $('#docrevisontable_filter').append('   <input type="button" class="addBtn" id="addBut" value="修改"/>');  
                $('#docrevisontable_filter').append('   <input type="button" class="addBtn" id="addBut" value="删除"/>');  
                $('#docrevisontable_filter').append('</span>');  
        }  
 设置隐藏列

var oTable = $('#example').dataTable();  
oTable.fnSetColumnVis( 0, false);//隐藏列  
 得到当前页面中的数据

var alldata=$('#example').dataTable().fnGetData();//得到页面中所有对象
 

// Row data 
$(document).ready(function() { 
oTable = $('#example').dataTable(); 

oTable.$('tr').click( function () { 
var data = oTable.fnGetData( this ); 
// ... do something with the array / object of data for the row 
} ); 
} ); 


// Individual cell data 
$(document).ready(function() { 
oTable = $('#example').dataTable(); 

oTable.$('td').click( function () { 
var sData = oTable.fnGetData( this ); 
alert( 'The cell clicked on had the value of '+sData ); 
} ); 
} );
 刷新表中的数据(最后一个参数是否重绘表格，false你浏览到第二页不会刷跑到第一页，但数据不会改变)

$('#example').dataTable().fnUpdate( 'a' , 1 , 1 ,false); //coll 
$('#example').dataTable().fnUpdate( ['a','b'] , 1 ); //row
 得到当前浏览的datatables页码

var tableSetings=$('#example').dataTable().fnSettings()  
var paging_length=tableSetings._iDisplayLength;//当前每页显示多少  
var page_start=tableSetings._iDisplayStart;//当前页开始  
var page=Div(page_start,paging_length);  
  
function Div(exp1, exp2) {  //整除  
    var n1 = Math.round(exp1); //四舍五入     
    var n2 = Math.round(exp2); //四舍五入    
  
    var rslt = n1 / n2; //除    
    if (rslt >= 0) {  
        rslt = Math.floor(rslt); //返回小于等于原rslt的最大整数。     
    }  
    else {  
        rslt = Math.ceil(rslt); //返回大于等于原rslt的最小整数。     
    }  
    return rslt;  
}  
 设置datatables跳转到某页

$('#example').dataTable().fnPageChange(page);   
 注意：由于后台数据可能已经被其它人改变（记录个数与现在前台不一致），所以数据fnUpdate时须要判断前后端数据的一致性，只刷 新前台有的数据的状态

 

dom:

http://datatables.net/release-datatables/examples/api/select_single_row.html 选择一行 http://datatables.net/release-datatables/examples/api/select_row.html选择多行 http://datatables.net/release-datatables/examples/api/editable.html 即时编辑行 http://datatables.net/release-datatables/examples/api/tabs_and_scrolling.html 多个tab http://datatables.net/release-datatables/examples/api/add_row.html添加一行（静态） http://datatables.net/release-datatables/examples/api/multi_filter.html多列搜索 http://datatables.net/release-datatables/examples/api/multi_filter_select.html多列搜索（扩展） http://datatables.net/release-datatables/examples/api/highlight.html行列
 高亮 http://datatables.net/release-datatables/examples/advanced_init/highlight.html 鼠标移上去亮 http://datatables.net/release-datatables/examples/api/row_details.html行详细信息 http://datatables.net/release-datatables/examples/api/counter_column.html添加行数 http://datatables.net/release-datatables/examples/api/show_hide.html隐藏列 http://datatables.net/release-datatables/examples/api/api_in_init.html点中即为搜索条件 http://datatables.net/release-datatables/examples/advanced_init/events_live.html 给每一个行添加事件显示行的信息 http://datatables.net/release-datatables/examples/advanced_init/events_pre_init.html鼠标事件
 移到某一行显示信息 http://datatables.net/release-datatables/examples/advanced_init/events_post_init.html 同上 http://datatables.net/release-datatables/examples/advanced_init/dom_multiple_elements.html 用sdom属性控制插件位置 http://datatables.net/release-datatables/examples/advanced_init/dom_toolbar.htmlsdom应用 http://datatables.net/release-datatables/examples/advanced_init/length_menu.html更改按多少数据显示 http://datatables.net/release-datatables/examples/advanced_init/complex_header.html表头组 http://datatables.net/release-datatables/examples/advanced_init/row_grouping.html按组显示行 http://datatables.net/release-datatables/examples/advanced_init/row_callback.html列回调函数 http://datatables.net/release-datatables/examples/advanced_init/footer_callback.html总计（footer回调） http://datatables.net/release-datatables/examples/advanced_init/sorting_control.html自定义排序 
http://datatables.net/release-datatables/examples/advanced_init/language_file.html国际化

 

一：api

bAutoWidth ：启用或禁用自动列宽度的计算。

默认值 true
类型  boolean
$(document).ready( function () {
    $('#example').dataTable( {
        "bAutoWidth": false  //关闭后，表格将不会自动计算表格大小，在浏览器大化小化的时候会挤在一坨
    } );
} );
bDeferRender ：根据官网的介绍翻译过来就是，延期渲染，可以有个速度的提升，当datatable 使用Ajax或者JS源表的数据。这个选项设置为true,将导致datatable推迟创建表元素每个元素,直到他们都创建完成——本参数的目的是节省大量的时间。

默认值：    false
类型： boolean
$(document).ready( function() {
    var oTable = $('#example').dataTable( {
        "sAjaxSource": "sources/arrays.txt",
        "bDeferRender": true   //这个参数我个人没有使用过，到底是不是这个效果，大家自己去尝试一下
    } );
} );
bFilter ：这个很容易明白，启用或禁用过滤数据。在datatable过滤是“智能”,它允许用户 最终输入多个关键字(空格分隔)，和每一行数据匹配,即使不是在被指定的顺序(这允许匹配多个列)。注意,如果您希望使用过滤，在datatable中必须设置为true，如果要删除默认过滤输入框和留住过滤功能,请使用{ @link DataTable.defaults.sDom }。这个最后一句不明白，不知道大家怎么理解。

默认值：    true
类型： boolean
$(document).ready( function () {
    $('#example').dataTable( {
        "bFilter": false
    } );
} );
bJQueryUI ：这个不用多说了，一看就懂，启用jQuery UI样式，(需要在页面添加jQuery的样式文件)。

默认值：    false
类型： boolean
$(document).ready( function() {
    $('#example').dataTable( {
       "bJQueryUI": true
    } );
} );
bLengthChange ：开启一页显示多少条数据的下拉菜单，允许用户从下拉框(10、25、50和100)，注意需要分页(bPaginate：true)。

默认值：    true
类型： boolean
$(document).ready( function () {
    $('#example').dataTable( {
        "bLengthChange": false,
bPaginate ：开启分页功能，如果不开启，将会全部显示

默认值：    true
类型： boolean
$(document).ready( function () {
    $('#example').dataTable( {
        "bPaginate": false   
    } );
} );
bProcessing ：开启读取服务器数据时显示正在加载中……特别是大数据量的时候，开启此功能比较好

默认值：    false
类型: boolean
$(document).ready( function () {
    $('#example').dataTable( {
        "bProcessing": true
    } );
} );
bScrollInfinite ：是否开启不限制长度的滚动条（和sScrollY属性结合使用），不限制长度的滚动条意味着当用户拖动滚动条的时候DataTable会不断加载数据当数据集十分大的时候会有些用处，该选项无法和分页选项同时使用，分页选项会被自动禁止，注意，额外推荐的滚动条会优先与该选项

默认值：    false
类型： boolean
$(document).ready( function() {
    $('#example').dataTable( {
        "bScrollInfinite": true,
        "bScrollCollapse": true,
        "sScrollY": "200px"//长200像素
    } );
} );
bServerSide ：开启服务器模式，使用服务器端处理配置datatable。注意：sAjaxSource参数也必须被给予为了给datatable源代码来获取所需的数据对于每个画。 这个翻译有点别扭。开启此模式后，你对datatables的每个操作 每页显示多少条记录、下一页、上一页、排序（表头）、搜索，这些都会传给服务器相应的值。

默认值：    false
类型： boolean
$(document).ready( function () {
      $('#example').dataTable( {
          "bServerSide": true,
          "sAjaxSource": "xhr.php"
       } );
} );
bInfo ：启用或禁用表信息显示。这显示了关于数据的信息,目前在网页上,包括信息过滤数据,如果行为被执行。这个参数在bServerSide配置后需要用到。

默认值：    true
类型： boolean
$(document).ready( function () {
    $('#example').dataTable( {
       "bInfo": false//如果这个参数不穿到后台去，服务器分页会报错，据说这个参数包含了表的所有信息
    } );
} );
bSort ：开启排序功能，每一列都有排序功能，如果关闭了，排序功能将失效，也可以自定义排序功能。单个列的排序可以禁用“bSortable”选项。

默认值：    true
类型： boolean
$(document).ready( function () {
    $('#example').dataTable( {
        "bSort": false
    } );
} );
bSortClasses ：是否在当前被排序的列上额外添加sorting_1,sorting_2,sorting_3三个class，当该列被排序的时候，可以切换其背景颜色，该选项作为一个来回切换的属性会增加执行时间（当class被移除和添加的时候），当对大数据集进行排序的时候你或许希望关闭该选项

默认值：    true
类型： boolean
$(document).ready( function () {
    $('#example').dataTable( {
        "bSortClasses": false
     } );
} );
复
bStateSave ：是否开启状态保存，当选项开启的时候会使用一个cookie保存表格展示的信息的状态，例如分页信息，展示长度，过滤和排序等。这样当终端用户重新加载这个页面的时候可以使用以前的设置

默认值：    false
类型： boolean
$(document).ready( function () {
    $('#example').dataTable( {
        "bStateSave": true
    } );
} );
sScrollX ：是否开启水平滚动，当一个表格过于宽以至于无法放入一个布局的时候，或者表格有太多列的时候，你可以开启该选项从而在一个可横向滚动的视图里面展示表格，该属性可以是css设置，或者一个数字（作为像素量度来使用）

默认值：    blank
 string - i.e. disabled
类型： string
$(document).ready( function() {
    $('#example').dataTable( {
        "sScrollX": "100%",
        "bScrollCollapse": true
     } );
} );
sScrollY：是否开启垂直滚动，垂直滚动会驱使DataTable设置为给定的长度，任何溢出到当前视图之外的数据可以通过垂直滚动进行察看当在小范围区域中显示大量数据的时候，可以在分页和垂直滚动中选择一种方式，该属性可以是css设置，或者一个数字（作为像素量度来使用）

默认值：    blank
 string - i.e. disabled
类型： string
$(document).ready( function() {
    $('#example').dataTable( {
        "sScrollY": "200px",
        "bPaginate": false
     } );
} );
bDestroy ：使用传递的新的初始化对象中的属性构造一个新的表格，并替换一个匹配指定的选择器的表格，如果没有匹配到表格，新的表格会被作为一个普通表格被构建

默认值：    false
类型： boolean
$(document).ready( function() {
    $('#example').dataTable( {
        "sScrollY": "200px",
        "bPaginate": false
    } );
   
  // Some time later....
    $('#example').dataTable( {
        "bFilter": false,
        "bDestroy": true
    } );
} );
bRetrieve ：使用指定的选择器检索表格，注意，如果表格已经被初始化，该参数会直接返回已经被创建的对象，并不会顾及你传递进来的初始化参数对象的变化，将该参数设置为true说明你确认已经明白这一点，如果你需要的话，bDestroy可以用来重新初始化表格

默认值：    false
类型： boolean
$(document).ready( function() {
    initTable();
    tableActions();
} );
function initTable ()
{
    return $('#example').dataTable( {
    "sScrollY": "200px",
    "bPaginate": false,
    "bRetrieve": true
    } );
}
function tableActions ()
{
    var oTable = initTable();
    // perform API operations with oTable
}
bScrollAutoCss ：指明DataTable中滚动的标题元素是否被允许设置内边距和外边距等

默认值：    true
类型： boolean
$(document).ready( function() {
    $('#example').dataTable( {
        "bScrollAutoCss": false,
        "sScrollY": "200px"
    } );
} );
bScrollCollapse ：当垂直滚动被允许的时候，DataTable会强制表格视图在任何时候都是给定的高度（对布局有利）不过，当把数据集过滤到十分小的时候看起来会很古怪，而且页脚会留在最下面当结果集的高度比给定的高度小时该参数会使表格高度自适应

默认值：    false
类型：
$(document).ready( function() {
    $('#example').dataTable( {
        "sScrollY": "200",
        "bScrollCollapse": true
    } );
} );
bSortCellsTop ：是否允许DataTable使用顶部（默认为true）的单元格，或者底部（默认为false）的单元格，当使用复合表头的时候会有些用处

默认值：    false
类型： boolean
$(document).ready( function() {
    $('#example').dataTable( {
        "bSortCellsTop": true
    } );
} );
iCookieDuration ：指定用于存储客户端信息到cookie中的时间长度，超过这个时间后，自动过期

默认值：    7200 (2
 hours)
类型： int
$(document).ready( function() {
    $('#example').dataTable( {
       "iCookieDuration": 60*60*24; // 一天
    } );
} )
iDeferLoading ：当选项被开启的时候，DataTable在非加载第一次的时候不会向服务器请求数据，而是会使用页面上的已有数据（不会应用排序等），因此在加载的时候保留一个XmlHttpRequest，iDeferLoading被用来指明需要延迟加载，而且也用来通知DataTable一个满的表格有多少条数据，信息元素和分页会被正确保留

默认值：    null
类型： int
// 57 records available in the table, no filtering applied
$(document).ready( function() {
    $('#example').dataTable( {
        "bServerSide": true,
        "sAjaxSource": "scripts/server_processing.php",
        "iDeferLoading": 57
    } );
} );
// 57 records after filtering, 100 without filtering (an initial filter applied)
$(document).ready( function() {
     $('#example').dataTable( {
        "bServerSide": true,
        "sAjaxSource": "scripts/server_processing.php",
        "iDeferLoading": [ 57, 100 ],
            "oSearch": {
            "sSearch": "my_filter"
          }
      } );
} );
iDisplayLength ：单页显示的数据的条数，如果bLengthChange属性被开启，终端用户可以通过一个弹出菜单重写该数值

默认值：    10
类型： int
$(document).ready( function() {
       $('#example').dataTable( {
            "iDisplayLength": 50
        } );
} )
iDisplayStart ：当开启分页的时候，定义展示的记录的起始序号，不是页数，因此如果你每个分页有10条记录而且想从第三页开始，需要把该参数指定为20

默认值 0
类型： int
$(document).ready( function() {
    $('#example').dataTable( {
        "iDisplayStart": 20
    } );
} )
iScrollLoadGap ：滚动余界是指DataTable在当前页面还有多少条数据可供滚动时自动加载新的数据，你可能希望指定一个足够大的余界，以便滚动加载数据的操作对用户来说是平滑的，同时也不会大到加载比需要的多的多的数据

默认值：    100
类型: int
$(document).ready( function() {
    $('#example').dataTable( {
        "bScrollInfinite": true,
        "bScrollCollapse": true,
        "sScrollY": "200px",
        "iScrollLoadGap": 50
    } );
} );
iTabIndex :默认情况下DataTable允许通过为需要键盘导航的元素添加tabindex属性来进行导航（排序、分页、过滤），允许你通过tab键切换控制组件，使用回车键去激活他们，默认为0表示按照文档流来切换，如果需要的话，你可以使用该参数重写切换顺序，使用-1来禁止内建的键盘导航

默认值：    0
类型： int
$(document).ready( function() {
    $('#example').dataTable( {
        "iTabIndex": 1
    } );
} );
oSearch ：该参数允许你在初始化的时候使用已经定义的全局过滤状态，sSearch对象必须被定义，但是所有的其它选项都是可选的，当bRegex为true的时候，搜索字符串会被当作正则表达式，当为false（默认）的时候，会被直接当作一个字符串，当bSmart为true的时候，DataTable会使用使用灵活过滤策略（匹配任何可能的数据），为false的时候不会这样做

$(document).ready( function() {
      $('#example').dataTable( {
          "oSearch": {"sSearch": "Initial search"}
       } );
} )
sAjaxDataProp ：当使用Ajax数据源或者服务器端处理的时候，DataTable会默认搜索aaData属性作为数据源，该选项允许变更数据源的名称，你可以使用JavaScript的点号对象表示法去访问多级网状数据源

默认值：    aaData
类型： string
// Get data from { "data": [...] }
$(document).ready( function() {
    var oTable = $('#example').dataTable( {
        "sAjaxSource": "sources/data.txt",
       "sAjaxDataProp": "data"
    } );
} );
// Get data from { "data": { "inner": [...] } }
$(document).ready( function() {
    var oTable = $('#example').dataTable( {
       "sAjaxSource": "sources/data.txt",
      "sAjaxDataProp": "data.inner"
   } );
} );
sAjaxSource ：该参数用来向DataTable指定加载的外部数据源（如果想使用现有的数据，请使用aData），可以简单的提供一个可以用来获得数据url或者JSON对象，该对象必须包含aaData，作为表格的数据源

默认值：    null
类型： string
$(document).ready( function() {
    $('#example').dataTable( {
        "sAjaxSource": "list.action"
     } );
} )
sCookiePrefix :该参数可以用来重写DataTable默认指定的用来存储状态信息的cookie的前缀

默认值：    SpryMedia_DataTables_
类型： string
$(document).ready( function() {
    $('#example').dataTable( {
      "sCookiePrefix": "my_datatable_"
    } );
} );
sDom ：这是用于定义DataTable布局的一个强大的属性，包括分页，显示多少条数据和搜索，格式如下：

The following options are allowed:
    'l' - 左上角按个下拉框，10个，20个，50个，所有的哪个
    'f' - 快速过滤框
    't' - 表格本身
    'i' - 分页信息
    'p' -分页按钮
    'r' - 现在正在加载中……
The following constants are allowed:
    'H' - jQueryUI theme "header" classes ('fg-toolbar ui-widget-header ui-corner-tl ui-corner-tr ui-helper-clearfix')
    'F' - jQueryUI theme "footer" classes ('fg-toolbar ui-widget-header ui-corner-bl ui-corner-br ui-helper-clearfix')
The following syntax is expected:
    '<' and '>' - div 元素
    '<"class" and '>' - 给div加clasa
    '<"#id" and '>' - 给div加上id
Examples:
    '<"wrapper"flipt>'
    '<lf<t>ip>'
默认值：    lfrtip (when
 bJQueryUI is false) or <"H"lfr>t<"F"ip> (when bJQueryUI is true)
类型： string
$(document).ready( function() {
    $('#example').dataTable( {
       "sDom": '<"top"i>rt<"bottom"flp><"clear">'
    } );
} );
'<"top"i>rt<"bottom"flp><"clear">'

这段代码翻译为html就是这样子的：

<div class="top">
    i
</div>
rt
<div class="bottom">
    flp
</div>
<div class="clear"></div>
这样一对比起来，就容易理解多了.Datatables之强大的sDom属性的应用
sPaginationType ：DataTable内建了两种交互式分页策略，两个按钮和全页数，展现给终端用户不同的控制方式，可以通过API增加策略

默认值：    two_button
类型： string
$(document).ready( function() {
    $('#example').dataTable( {
       "sPaginationType": "full_numbers"
    } );
} )
sScrollXInner ：当横向滚动可用的时候，该属性可以用来强制DataTable的宽度比需要的更长，比如你需要表格彼此相隔适宜，该变量可以用来使表格变大，而且强制滚动，该该属性可以是css设置，或者一个数字（作为像素量度来使用）

默认值：    blank
 string - i.e. disabled
类型： string
$(document).ready( function() {
    $('#example').dataTable( {
       "sScrollX": "100%",
       "sScrollXInner": "110%"
   } );
} );
sServerMethod ：设置使用Ajax方式调用的服务器端的处理方法或者Ajax数据源的HTTP请求方式

默认值：    GET
类型： string
$(document).ready( function() {
    $('#example').dataTable( {
       "bServerSide": true,
       "sAjaxSource": "list.action",
       "sServerMethod": "POST"   //以post的方式提交数据
    } );
} )
二： 列属性
aoColumnDefs: This array allows you to target a specific column, multiple columns, or all columns, using the aTargets property of each object in the array (please note that aoColumnDefs was introduced in DataTables 1.7). This allows great flexibility when creating tables, as the aoColumnDefs arrays can be of any length, targeting the columns you specifically want. The aTargets property is an array to target one of many columns and each element in it can be:

aoColumnDefs:这个数组允许您针对一个特定列,多个列,或者所有列,使用aTargets属性的数组中的每个对象(请注意,介绍了aoColumnDefs datatable 1.7)。这提供了很大的灵活性在创建表,因为aoColumnDefs数组可以是任意长度,目标是你特别想要的列。aTargets的属性是一个数组来目标众多列和每个元素在它可以:

a string - class name will be matched on the TH for the column
0 or a positive integer - column index counting from the left
a negative integer - column index counting from the right
the string "_all" - all columns (i.e. assign a default)

aoColumns: If specified, then the length of this array must be equal to the number of columns in the original HTML table. Use 'null' where you wish to use only the default values and automatically detected options.

aoColumnDefs参数和aoColumns可以一起使用,尽管aoColumnDefs优先aoColumns的灵活性。 如果两者都使用,aoColumns定义将最高优先级。同样,如果相同的列的目标是在aoColumnDefs多次,第一个元素的数组将最高优先级,最后一个最低的。


aDataSort：定义一个列或多个列自定义排序
Default:    null Takes
 the value of the column index automatically
Type:   array
// Using aoColumnDefs
$(document).ready( function() {
  $('#example').dataTable( {
    "aoColumnDefs": [
      { "aDataSort": [ 0, 1 ], "aTargets": [ 0 ] },
      { "aDataSort": [ 1, 0 ], "aTargets": [ 1 ] },
      { "aDataSort": [ 2, 3, 4 ], "aTargets": [ 2 ] }
    ]
  } );
} );
// Using aoColumns
$(document).ready( function() {
  $('#example').dataTable( {
    "aoColumns": [
      { "aDataSort": [ 0, 1 ] },
      { "aDataSort": [ 1, 0 ] },
      { "aDataSort": [ 2, 3, 4 ] },
      null,
      null
    ]
  } );
} );
asSorting：控制列的升序或降序到自定义列

Default:    [
 'asc', 'desc' ]
Type:   array
// Using aoColumnDefs
$(document).ready( function() {
  $('#example').dataTable( {
    "aoColumnDefs": [
      { "asSorting": [ "asc" ], "aTargets": [ 1 ] },
      { "asSorting": [ "desc", "asc", "asc" ], "aTargets": [ 2 ] },
      { "asSorting": [ "desc" ], "aTargets": [ 3 ] }
    ]
  } );
} );
// Using aoColumns
$(document).ready( function() {
  $('#example').dataTable( {
    "aoColumns": [
      null,
      { "asSorting": [ "asc" ] },
      { "asSorting": [ "desc", "asc", "asc" ] },
      { "asSorting": [ "desc" ] },
      null
    ]
  } );
} );
bSearchable：设置列是否能被快速检索框搜索到
Default:    true
Type:   boolean
// Using aoColumnDefs
$(document).ready( function() {
  $('#example').dataTable( {
    "aoColumnDefs": [
      { "bSearchable": false, "aTargets": [ 0 ] }
    ] } );
} );
// Using aoColumns
$(document).ready( function() {
  $('#example').dataTable( {
    "aoColumns": [
      { "bSearchable": false },
      null,
      null,
      null,
      null
    ] } );
} );
bSortable：启用或禁用列排序。
Default:    true
Type:   boolean
// Using aoColumnDefs
$(document).ready( function() {
  $('#example').dataTable( {
    "aoColumnDefs": [
      { "bSortable": false, "aTargets": [ 0 ] }
    ] } );
} );
// Using aoColumns
$(document).ready( function() {
  $('#example').dataTable( {
    "aoColumns": [
      { "bSortable": false },
      null,
      null,
      null,
      null
    ] } );
} );
bUseRendered ：高版本已经废弃这个属性，没有使用过，直接翻译吧！当使用fnRender()为一个列,您可能希望使用原始的数据(在呈现之前)进行排序和过滤(默认是用于呈现的数据,用户可以看到)。这可能是有用的日期等。请注意,该选项已被弃用,将被删除的下一个版本的datatable。请用mRender / mData而不是fnRender。
Default:    true
Type:   boolean

无例子代码；
bVisible：启用或禁用本列显示。
Default:    true
Type:   boolean
// Using aoColumnDefs
$(document).ready( function() {
  $('#example').dataTable( {
    "aoColumnDefs": [
      { "bVisible": false, "aTargets": [ 0 ] }
    ] } );
} );
// Using aoColumns
$(document).ready( function() {
  $('#example').dataTable( {
    "aoColumns": [
      { "bVisible": false },
      null,
      null,
      null,
      null
    ] } );
} );
fnCreatedCell：开发人员可定义的函数,就会调用一个细胞被创建(Ajax源等)或处理输入(DOM源)。这可以作为一种恭维,mRender允许您修改DOM元素(例如添加背景颜色)当元素是可用的。

Default:     
Type:   function
$(document).ready( function() {
  $('#example').dataTable( {
    "aoColumnDefs": [ {
      "aTargets": [3],
      "fnCreatedCell": function (nTd, sData, oData, iRow, iCol) {
        if ( sData == "1.7" ) {
          $(nTd).css('color', 'blue')
        }
      }
    } ]
  });
} );
fnRender：高版本已经废弃这个属性。mRender这个代替

iDataSort：列索引(从0开始!),你想要执行一个在本专栏时被选中进行排序。这可以用于排序在隐藏列例如。这个也没使用过
Default:    -1 Use
 automatically calculated column index
Type:   int
// Using aoColumnDefs
$(document).ready( function() {
  $('#example').dataTable( {
    "aoColumnDefs": [
      { "iDataSort": 1, "aTargets": [ 0 ] }
    ]
  } );
} );
// Using aoColumns
$(document).ready( function() {
  $('#example').dataTable( {
    "aoColumns": [
      { "iDataSort": 1 },
      null,
      null,
      null,
      null
    ]
  } );
} );
mData：这个属性可以用来读取JSON数据从任何数据源属性,包括深层嵌套对象/属性。可以给mData在许多不同的方面影响其行为:

integer - treated as an array index for the data source. This is the default that DataTables uses (incrementally increased for each column).
string - read an object property from the data source. Note that you can use Javascript dotted notation to read deep properties / arrays from the data source.
null - the sDefaultContent option will be used for the cell (null by default, so you will need to specify the default content you want - typically an empty string). This can be useful on generated columns such as edit / delete action columns.
function - the function given will be executed whenever DataTables needs to set or get the data for a cell in the column. The function takes three parameters:
{array|object} The data source for the row
{string} The type call data requested - this will be 'set' when setting data or 'filter', 'display', 'type', 'sort' or undefined when gathering data. Note that when undefined is given for the type DataTables expects to get the raw data for the object back
{*} Data to set when the second parameter is 'set'.
The return value from the function is not required when 'set' is the type of call, but otherwise the return is what will be used for the data requested.
Default:    null Use
 automatically calculated column index
Type:   string
// Read table data from objects
$(document).ready( function() {
  var oTable = $('#example').dataTable( {
    "sAjaxSource": "sources/deep.txt",
    "aoColumns": [
      { "mData": "engine" },
      { "mData": "browser" },
      { "mData": "platform.inner" },
      { "mData": "platform.details.0" },
      { "mData": "platform.details.1" }
    ]
  } );
} );
// Using mData as a function to provide different information for
// sorting, filtering and display. In this case, currency (price)
$(document).ready( function() {
  var oTable = $('#example').dataTable( {
    "aoColumnDefs": [ {
      "aTargets": [ 0 ],
      "mData": function ( source, type, val ) {
        if (type === 'set') {
          source.price = val;
          // Store the computed dislay and filter values for efficiency
          source.price_display = val=="" ? "" : "[        DISCUZ_CODE_7        ]quot;+numberFormat(val);
          source.price_filter  = val=="" ? "" : "[        DISCUZ_CODE_7        ]quot;+numberFormat(val)+" "+val;
          return;
        }
        else if (type === 'display') {
          return source.price_display;
        }
        else if (type === 'filter') {
          return source.price_filter;
        }
        // 'sort', 'type' and undefined all just use the integer
        return source.price;
      }
    } ]
  } );
} );
mRender ：This property is the rendering partner to mData and it is suggested that when you want to manipulate data for display (including filtering, sorting etc) but not altering the underlying data for the table, use this property. mData can actually do everything this property can and more, but this parameter is easier to use since there is no 'set' option. Like mData is can be given in a number of different ways to effect its behaviour, with the addition of supporting array syntax for easy outputting of arrays (including arrays of objects):

integer - treated as an array index for the data source. This is the default that DataTables uses (incrementally increased for each column).
string - read an object property from the data source. Note that you can use Javascript dotted notation to read deep properties / arrays from the data source and also array brackets to indicate that the data reader should loop over the data source array. When characters are given between the array brackets, these characters are used to join the data source array together. For example: "accounts[, ].name" would result in a comma separated list with the 'name' value from the 'accounts' array of objects.
function - the function given will be executed whenever DataTables needs to set or get the data for a cell in the column. The function takes three parameters:
{array|object} The data source for the row (based on mData)
{string} The type call data requested - this will be 'filter', 'display', 'type' or 'sort'.
{array|object} The full data source for the row (not based on mData)
The return value from the function is what will be used for the data requested.
Default:    null Use
 mData
Type:   string
// Create a comma separated list from an array of objects
$(document).ready( function() {
  var oTable = $('#example').dataTable( {
    "sAjaxSource": "sources/deep.txt",
    "aoColumns": [
      { "mData": "engine" },
      { "mData": "browser" },
      {
        "mData": "platform",
        "mRender": "[, ].name"
      }
    ]
  } );
} );
// Use as a function to create a link from the data source
$(document).ready( function() {
  var oTable = $('#example').dataTable( {
    "aoColumnDefs": [ {
      "aTargets": [ 0 ],
      "mData": "download_link",
      "mRender": function ( data, type, full ) {
        return '<a href="'+data+'">Download</a>';
      }
    } ]
  } );
} );
sCellType：Change the cell type created for the column - either TD cells or TH cells. This can be useful as TH cells have semantic meaning in the table body, allowing them to act as a header for a row (you may wish to add scope='row' to the TH elements).
Default:    td
Type:   string
// Make the first column use TH cells
$(document).ready( function() {
  var oTable = $('#example').dataTable( {
    "aoColumnDefs": [ {
      "aTargets": [ 0 ],
      "sCellType": "th"
    } ]
  } );
} );
sClass：给列加上自己定义的属性
Default:    Empty
 string
Type:   string
// Using aoColumnDefs
$(document).ready( function() {
  $('#example').dataTable( {
    "aoColumnDefs": [
      { "sClass": "my_class", "aTargets": [ 0 ] }
    ]
  } );
} );
// Using aoColumns
$(document).ready( function() {
  $('#example').dataTable( {
    "aoColumns": [
      { "sClass": "my_class" },
      null,
      null,
      null,
      null
    ]
  } );
} );
sContentPadding ：When DataTables calculates the column widths to assign to each column, it finds the longest string in each column and then constructs a temporary table and reads the widths from that. The problem with this is that "mmm" is much wider then "iiii", but the latter is a longer string - thus the calculation can go wrong (doing it properly and putting it into an DOM object and measuring that is horribly(!) slow). Thus as a "work around" we provide this option. It will append its value to the text that is found to be the longest string for the column - i.e. padding. Generally you shouldn't need this, and it is not documented on the general DataTables.net documentation

Default:    Empty
 string
Type:   string
// Using aoColumns
$(document).ready( function() {
  $('#example').dataTable( {
    "aoColumns": [
      null,
      null,
      null,
      {
        "sContentPadding": "mmm"
      }
    ]
  } );
} );
sDefaultContent ：Allows a default value to be given for a column's data, and will be used whenever a null data source is encountered (this can be because mData is set to null, or because the data source itself is null).
Default:    null
Type:   string
// Using aoColumnDefs
$(document).ready( function() {
  $('#example').dataTable( {
    "aoColumnDefs": [
      {
        "mData": null,
        "sDefaultContent": "Edit",
        "aTargets": [ -1 ]
      }
    ]
  } );
} );
// Using aoColumns
$(document).ready( function() {
  $('#example').dataTable( {
    "aoColumns": [
      null,
      null,
      null,
      {
        "mData": null,
        "sDefaultContent": "Edit"
      }
    ]
  } );
} );
sName：This parameter is only used in DataTables' server-side processing. It can be exceptionally useful to know what columns are being displayed on the client side, and to map these to database fields. When defined, the names also allow DataTables to reorder information from the server if it comes back in an unexpected order (i.e. if you switch your columns around on the client-side, your server-side code does not also need updating).

Default:    Empty
 string
Type:   string
// Using aoColumnDefs
$(document).ready( function() {
  $('#example').dataTable( {
    "aoColumnDefs": [
      { "sName": "engine", "aTargets": [ 0 ] },
      { "sName": "browser", "aTargets": [ 1 ] },
      { "sName": "platform", "aTargets": [ 2 ] },
      { "sName": "version", "aTargets": [ 3 ] },
      { "sName": "grade", "aTargets": [ 4 ] }
    ]
  } );
} );
// Using aoColumns
$(document).ready( function() {
  $('#example').dataTable( {
    "aoColumns": [
      { "sName": "engine" },
      { "sName": "browser" },
      { "sName": "platform" },
      { "sName": "version" },
      { "sName": "grade" }
    ]
  } );
} );
sSortDataType ：Defines a data source type for the sorting which can be used to read real-time information from the table (updating the internally cached version) prior to sorting. This allows sorting to occur on user editable elements such as form inputs.
Default:    std
Type:   string
// Using aoColumnDefs
$(document).ready( function() {
  $('#example').dataTable( {
    "aoColumnDefs": [
      { "sSortDataType": "dom-text", "aTargets": [ 2, 3 ] },
      { "sType": "numeric", "aTargets": [ 3 ] },
      { "sSortDataType": "dom-select", "aTargets": [ 4 ] },
      { "sSortDataType": "dom-checkbox", "aTargets": [ 5 ] }
    ]
  } );
} );
// Using aoColumns
$(document).ready( function() {
  $('#example').dataTable( {
    "aoColumns": [
      null,
      null,
      { "sSortDataType": "dom-text" },
      { "sSortDataType": "dom-text", "sType": "numeric" },
      { "sSortDataType": "dom-select" },
      { "sSortDataType": "dom-checkbox" }
    ]
  } );
} );
sTitle ：The title of this column.
Default:    null Derived
 from the 'TH' value for this column in the original HTML table.
Type:   string
// Using aoColumnDefs
$(document).ready( function() {
  $('#example').dataTable( {
    "aoColumnDefs": [
      { "sTitle": "My column title", "aTargets": [ 0 ] }
    ]
  } );
} );
// Using aoColumns
$(document).ready( function() {
  $('#example').dataTable( {
    "aoColumns": [
      { "sTitle": "My column title" },
      null,
      null,
      null,
      null
    ]
  } );
} );
sType：The type allows you to specify how the data for this column will be sorted. Four types (string, numeric, date and html (which will strip HTML tags before sorting)) are currently available. Note that only date formats understood by Javascript's Date() object will be accepted as type date. For example: "Mar 26, 2008 5:03 PM". May take the values: 'string', 'numeric', 'date' or 'html' (by default). Further types can be adding through plug-ins.
Default:    null Auto-detected
 from raw data
Type:   string
// Using aoColumnDefs
$(document).ready( function() {
  $('#example').dataTable( {
    "aoColumnDefs": [
      { "sType": "html", "aTargets": [ 0 ] }
    ]
  } );
} );
// Using aoColumns
$(document).ready( function() {
  $('#example').dataTable( {
    "aoColumns": [
      { "sType": "html" },
      null,
      null,
      null,
      null
    ]
  } );
} );
sWidth：Defining the width of the column, this parameter may take any CSS value (3em, 20px etc). DataTables applies 'smart' widths to columns which have not been given a specific width through this interface ensuring that the table remains readable.
Default:    null Automatic
Type:   string
// Using aoColumnDefs
$(document).ready( function() {
  $('#example').dataTable( {
    "aoColumnDefs": [
      { "sWidth": "20%", "aTargets": [ 0 ] }
    ]
  } );
} );
// Using aoColumns
$(document).ready( function() {
  $('#example').dataTable( {
    "aoColumns": [
      { "sWidth": "20%" },
      null,
      null,
      null,
      null
    ]
  } );
} );
 三：回调
复制代码
fnCookieCallback：还没有使用过
$(document).ready( function () {
  $('#example').dataTable( {
    "fnCookieCallback": function (sName, oData, sExpires, sPath) {
      // Customise oData or sName or whatever else here
      return sName + "="+JSON.stringify(oData)+"; expires=" + sExpires +"; path=" + sPath;
    }
  } );
} );

fnCreatedRow：顾名思义，创建行得时候的回调函数
$(document).ready( function() {
  $('#example').dataTable( {
    "fnCreatedRow": function( nRow, aData, iDataIndex ) {
      // 为a的话字体加粗
      if ( aData[4] == "A" )
      {
        $('td:eq(4)', nRow).html( '<b>A</b>' );
      }
    }
  } );
} );

fnDrawCallback：draw画 ，这个应该是重绘的回调函数
$(document).ready( function() {
  $('#example').dataTable( {
    "fnDrawCallback": function( oSettings ) {
      alert( 'DataTables 重绘了' );
    }
  } );
} );

fnFooterCallback：底部的回调函数，这个可以用来做总计之类的功能
$(document).ready( function() {
  $('#example').dataTable( {
    "fnFooterCallback": function( nFoot, aData, iStart, iEnd, aiDisplay ) {
      nFoot.getElementsByTagName('th')[0].innerHTML = "Starting index is "+iStart;
    }
  } );
} )

fnFormatNumber：顾名思义，格式化数字的显示方式
$(document).ready( function() {
  $('#example').dataTable( {
    "fnFormatNumber": function ( iIn ) {
      if ( iIn < 1000 ) {
        return iIn;
      } else {
        var
          s=(iIn+""),
          a=s.split(""), out="",
          iLen=s.length;
         
        for ( var i=0 ; i<iLen ; i++ ) {
          if ( i%3 === 0 && i !== 0 ) {
            out = "'"+out;
          }
          out = a[iLen-i-1]+out;
        }
      }
      return out;
    };
  } );
} );

fnHeaderCallback：表头的回调函数
$(document).ready( function() {
  $('#example').dataTable( {
    "fnHeaderCallback": function( nHead, aData, iStart, iEnd, aiDisplay ) {
      nHead.getElementsByTagName('th')[0].innerHTML = "Displaying "+(iEnd-iStart)+" records";
    }
  } );
} )

fnInfoCallback：datatables信息的回调函数
$('#example').dataTable( {
  "fnInfoCallback": function( oSettings, iStart, iEnd, iMax, iTotal, sPre ) {
    return iStart +" to "+ iEnd;
  }
} );

fnInitComplete：datatables初始化完毕后会调用这个方法
$(document).ready( function() {
  $('#example').dataTable( {
    "fnInitComplete": function(oSettings, json) {
      alert( 'DataTables has finished its initialisation.' );
    }
  } );
} )

fnPreDrawCallback：每一次绘datatables时候调用的方法
$(document).ready( function() {
  $('#example').dataTable( {
    "fnPreDrawCallback": function( oSettings ) {
      if ( $('#test').val() == 1 ) {
        return false;
      }
    }
  } );
} );

fnRowCallback：行的回调函数
$(document).ready( function() {
  $('#example').dataTable( {
    "fnRowCallback": function( nRow, aData, iDisplayIndex, iDisplayIndexFull ) {
      // Bold the grade for all 'A' grade browsers
      if ( aData[4] == "A" )
      {
        $('td:eq(4)', nRow).html( '<b>A</b>' );
      }
    }
  } );
} );

fnServerData：这个是结合服务器模式的回调函数，用来处理服务器返回过来的数据
// POST data to server
$(document).ready( function() {
  $('#example').dataTable( {
    "bProcessing": true,
    "bServerSide": true,
    "sAjaxSource": "xhr.php",
    "fnServerData": function ( sSource, aoData, fnCallback, oSettings ) {
      oSettings.jqXHR = $.ajax( {
        "dataType": 'json',
        "type": "POST",
        "url": sSource,
        "data": aoData,
        "success": fnCallback
      } );
    }
  } );
} );

fnServerParams：向服务器传额外的参数
$(document).ready( function() {
  $('#example').dataTable( {
    "bProcessing": true,
    "bServerSide": true,
    "sAjaxSource": "scripts/server_processing.php",
    "fnServerParams": function ( aoData ) {
      aoData.push( { "name": "more_data", "value": "my_value" } );
    }
  } );
} );

fnStateLoad：读取状态的回调函数
$(document).ready( function() {
  $('#example').dataTable( {
    "bStateSave": true,
    "fnStateLoad": function (oSettings) {
      var o;
       
      // Send an Ajax request to the server to get the data. Note that
      // this is a synchronous request.
      $.ajax( {
        "url": "/state_load",
        "async": false,
        "dataType": "json",
        "success": function (json) {
          o = json;
        }
      } );
       
      return o;
    }
  } );
} );

fnStateLoadParams：和上面的不知道什么区别，没用过
// Remove a saved filter, so filtering is never loaded
$(document).ready( function() {
  $('#example').dataTable( {
    "bStateSave": true,
    "fnStateLoadParams": function (oSettings, oData) {
      oData.oSearch.sSearch = "";
    }
  } );
} );
// Disallow state loading by returning false
$(document).ready( function() {
  $('#example').dataTable( {
    "bStateSave": true,
    "fnStateLoadParams": function (oSettings, oData) {
      return false;
    }
  } );
} );

fnStateLoaded：又是这个，加了ed 不知道意思没用过
// Show an alert with the filtering value that was saved
$(document).ready( function() {
  $('#example').dataTable( {
    "bStateSave": true,
    "fnStateLoaded": function (oSettings, oData) {
      alert( 'Saved filter was: '+oData.oSearch.sSearch );
    }
  } );
} );

fnStateSave：状态储存
$(document).ready( function() {
  $('#example').dataTable( {
    "bStateSave": true,
    "fnStateSave": function (oSettings, oData) {
      // Send an Ajax request to the server with the state object
      $.ajax( {
        "url": "/state_save",
        "data": oData,
        "dataType": "json",
        "method": "POST"
        "success": function () {}
      } );
    }
  } );
} );

fnStateSaveParams ：状态储存参数，没用过，不明白
// Remove a saved filter, so filtering is never saved
$(document).ready( function() {
  $('#example').dataTable( {
    "bStateSave": true,
    "fnStateSaveParams": function (oSettings, oData) {
      oData.oSearch.sSearch = "";
    }
  } );
} );
