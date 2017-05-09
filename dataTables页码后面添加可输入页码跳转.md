dataTables页码后面添加可输入页码跳转
修改jQuery.dataTables.bootstrap.js源文件，找到

if ( btnDisplay ) {
node = $('<li>', {
'class': classes.sPageButton+' '+btnClass,
'aria-controls': settings.sTableId,
'tabindex': settings.iTabIndex,
'id': idx === 0 && typeof button === 'string' ?
settings.sTableId +'_'+ button :
null
} )
在for循环外面添加：

f($("#redirect")!=null)
$("#redirect").remove();
$("<input type=\"text\" style=\"width: 50px;padding-top: 10px;height: 30px;border-radius: 0px 4px 4px 0px;\" id=\"redirect\" class=\"redirect\">").appendTo( Container );

这样就添加了输入框在页码按钮后面了，

接下来就是绑定事件，在定义dataTable的时候添加如下属性：

"fnDrawCallback": function(){
           var oTable = $(tb_div).dataTable();
           $('#redirect').keyup(function(e){
               if(e.keyCode==13){
               
if($(this).val() && $(this).val()>0){
                   var redirectpage = $(this).val()-1;
               }else{
                   var redirectpage = 0;
               }
               oTable.fnPageChange( redirectpage );
               }
           });
       },
即当输入回车时执行跳转