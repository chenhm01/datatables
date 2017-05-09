 jQuery DataTables插件分页允许输入页码跳转
 背景说明
项目中使用jQuery DataTables插件来实现分页表格，但是默认的分页样式不能输入页码进行跳转，在页数非常多的时候使用很不方便，最主要的还是没有达到产品部门的设计要求，所以我需要寻找相应的解决方案。
 
原始效果图
20161227_01
目标效果图
20161227_02
方案分析
一开始，我在网上搜索到了相关资料。
 
【官方】Navigation with text input
https://www.datatables.net/plug-ins/pagination/input
这个是jQuery DataTables官方提供的插件，虽然实现了输入页码跳转的功能，但是和我目标效果不一致
 
【CSDN】DataTables页码后面添加可输入页码跳转
http://blog.csdn.net/cw370008359/article/details/51516427
这个方案是直接修改jquery.datatables.js的源码，在原有分页的后面添加输入框，然后在表格加载完成事件fnDrawCallback中为输入框添加事件。
 
本来，我想沿用官方的方式，单独写一个插件，这样不会污染DataTables插件原有代码，但是实在是水平有限，所以采用了CSDN直接修改插件代码的方式。
在添加输入框的原理上和CSDN博客描述的一致
但是对于事件的处理，不在fnDrawCallback中进行，而是直接在添加输入框之后，紧接着根据id对输入框和确认按钮进行事件设置
而且CSDN博客中的事件代码在我这个版本的DataTables插件中无法执行，我转而参考了官方插件的事件代码，最终形成了我的方案
插入代码
注意：我使用的是DataTables 1.10.12版本，不同版本实现可能不同。
[javascript] view plain copy 在CODE上查看代码片派生到我的代码片
// START 2016-12-27添加，用于输入分页页码  
$(".gotoPage").remove();  
var pageHtml = "<span class='gotoPage' style='margin-left: 10px;'>" +  
        "<span>到第</span>" +  
        "<input type='text' style='width: 40px; padding-left: 2px; padding-right: 2px; text-align: center;' class='integer' id='textGotoPage' data-prev='"+(page+1)+"' value='"+(page+1)+"'>" +  
        "<span>页</span>" +  
        "<a class='paginate_button' style='width: 40px; border-right: 1px solid #e4e4e4;' id='btnGotoPage'>确认</a>" +  
        "</span>";  
$(pageHtml).appendTo(container);  
  
// 对页码输入进行限制，只能输入数字  
var sfn = function() {  
    var value = $(this).val();  
    if (value == '') {  
        $(this).data("prev", $(this).val());  
        return;  
    }  
  
    var max = $(this).attr("maxlength");  
    if (value.length > max)  
        $(this).val(value.slice(0, max));  
  
    var regex = /^\d+$/;  
    if (!regex.test(value)) {  
        $(this).val($(this).data("prev"));  
    }  
  
    $(this).data("prev", $(this).val());  
};  
  
var testinput = document.createElement('input');       
if('oninput' in testinput){   
    document.getElementById("textGotoPage").addEventListener("input", sfn, false);   
} else {  
    $("#textGotoPage").onpropertychange = sfn;   
}  
  
// 为确认按钮添加点击事件，执行分页跳转  
$("#btnGotoPage").click(function(){  
    var textGotoPage = $("#textGotoPage").val();  
    if (textGotoPage == null || textGotoPage === '' || textGotoPage.match(/[^0-9]/)) {  
        // 没有输入或者输入了非数字，清除非数字  
        $("#textGotoPage").val(textGotoPage.replace(/[^\d]/g, ''));  
        return;  
    }  
  
    if(parseInt(textGotoPage) > 0){  
        var oSettings = settings;  
  
        var iNewStart = oSettings._iDisplayLength * (textGotoPage - 1);  
        if (iNewStart < 0) {  
            iNewStart = 0;  
        }  
        if (iNewStart >= oSettings.fnRecordsDisplay()) {  
            iNewStart = (Math.ceil((oSettings.fnRecordsDisplay()) / oSettings._iDisplayLength) - 1) * oSettings._iDisplayLength;  
        }  
  
        oSettings._iDisplayStart = iNewStart;  
        _fnDraw(oSettings);  
    }  
});  
// END  


http://www.alanzeng.cn/2016/12/jquery-datatables-input-pagination/

谢谢！