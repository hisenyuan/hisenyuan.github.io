---
title: html - 原生javascript动态显示当前时间和日期
keywords: [html动态显示时间,js动态显示时间]
date: 2017/4/12 18:22
tags: []
categories:
---
显示当前时间：

2017年04月12日 18:26:37 星期三

示例如下：
<!--more-->
```
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<title>显示当前时间 - 原生javascript</title>
<script type="text/javascript" language="javascript">    
function show_cur_times(){    
//获取当前日期    
var date_time = new Date();    
//定义星期    
var week;    
//switch判断    
switch (date_time.getDay()){    
case 1: week="星期一"; break;    
case 2: week="星期二"; break;    
case 3: week="星期三"; break;    
case 4: week="星期四"; break;    
case 5: week="星期五"; break;    
case 6: week="星期六"; break;    
default:week="星期天"; break;    
}    
//年    
var year = date_time.getFullYear();    
 	//判断小于10，前面补0    
if(year<10){    
 	year="0"+year;    
}    
//月    
var month = date_time.getMonth()+1;    
 	//判断小于10，前面补0    
if(month<10){    
month="0"+month;    
}    
//日    
var day = date_time.getDate();    
 	//判断小于10，前面补0    
if(day<10){    
 	day="0"+day;    
}    
//时    
var hours =date_time.getHours();    
 	//判断小于10，前面补0    
if(hours<10){    
 	hours="0"+hours;    
}    
//分    
var minutes =date_time.getMinutes();    
 	//判断小于10，前面补0    
if(minutes<10){    
 	minutes="0"+minutes;    
}    
//秒    
var seconds=date_time.getSeconds();    
 	//判断小于10，前面补0    
if(seconds<10){    
 	seconds="0"+seconds;    
}    
//拼接年月日时分秒    
var date_str = year+"年"+month+"月"+day+"日 "+hours+":"+minutes+":"+seconds+" "+week;    
//显示在id为showtimes的容器里    
document.getElementById("showtimes").innerHTML= date_str;    
}    
//设置1秒调用一次show_cur_times函数    
setInterval("show_cur_times()",100);    
</script>
</head>
<body>
显示当前时间：<p id="showtimes"></p>
</body>
</html>
```