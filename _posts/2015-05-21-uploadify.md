---
date: 2015-05-21 22:54:00
title: C# + uploadify实现断点续传
layout: post
tags:
   - c#
   - 断点续传
categories:
   - lebornjose
---

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;由于最近工作的关系,一直想写技术博客，但是时间一致都不是那么充裕，零整的时间倒是不是，但一致没有一个整的时间更新博客，因为最近解决了我在一年前就碰到的技术问题,所以在这里还是应该纪录一下。

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;大文件的上传，想比有很多的场景需要这样做吧,用post提交数据流是不现实的,在程序中实现用FTP来进行上传，其实也不是好的解决方案，因为http的响应时间最长为90秒，如果遇到文件很大，或者网络不行的时候，就不能一个很好的解决方案了。断点续传是一个最好的解决方案！

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;uploaddify想必大家都听说过，是一个以php为后台实现的断点续传插件，但是我用的后台语言是C#,但我还是想用这个插件来实现我的断点续传

 前端代码:前端代码在官网上和网页上搜索，都可以找到，但这里我还是贴出来吧！

     <script type="text/javascript" src="/res/dev/debug/js/base/jquery.uploadify.min.js"></script>
     <link type="text/css" rel="stylesheet" href="/res/dev/debug/css/base/uploadify.css">
      <script type="text/javascript">
      $(function () {
       $('#file_upload').uploadify({
        'buttonText'       : '请选择上传文件',
        'swf'		: '/res/dev/debug/js/base/uploadify.swf',
        'uploader'   : '/weitou01/home/upload'
       });
      }
    });
    </script>
       <input type="file" id="file_upload" name="file_upload" />

后台C# 接受并保存文件

       public void upload(JabinfoContext context)
  {
    context.Response.ContentType = "text/plain";
    context.Response.Charset = "utf-8";

    System.Web.HttpPostedFile file = context.Request.Files["Filedata"];  

    string uploadPath = "upload/";

    if (file != null) {
      if (!Directory.Exists (uploadPath)) {  
        Directory.CreateDirectory (uploadPath);  
      }
      file.SaveAs (uploadPath + file.FileName);  
      context.Cookie.Add ("vaddress", uploadPath + file.FileName,1);
      //下面这句代码缺少的话，上传成功后上传队列的显示不会自动消失
      context.Response.Write ("1");  
    }
    else {
      context.Response.Write("0");
    }
  }

### 到这里断点续传大文件便实现了，但是还有有一个bug,应该说是uploadify插件的一个bug,在谷歌浏览器上打开该页面的时候，会出现页面崩溃.这个谷歌的缓存机制造成了。从LOG里也能看到。 解决这个问题

+ 正常的情况下，会请求文件（jquery.uploadify.min.js）；而崩溃的情况下，则没请求它。
## 这里提供两种解决方案
1. 在引用js的时候加上随机数，欺骗chrome浏览器防止缓存，使每次都发起请求

           <script type="text/javascript" src="js/jquery.uploadify
           .min.js/<?php echo rand(0,9999);?>"


2.在uploadify.js后面加上一个随机时间以防止使用chrome的缓存，其实这个办法并不能完全解决崩溃问题，比如在uplodify页面进入其他页页后，再点击后退返回到这个uploadify页面，同样会出现崩溃问题。


+ 其实如果不用缓存每次去请求服务器其实是个很浪费的事，关键是这样做根本就没有解决这个问题。真正的解决的办法也很简单，就是用setTimeout,让uplodify的初始化和浏览器缓存模块的功能不要在同时进行，操作如下：

        $(function(){
        setTimeout(function(){
          $('#file_upload').uploadify({
              'swf'      : 'tools/uploadify/uploadify.swf',
              'uploader' : 'upload.php',
              'onUploadSuccess' : function(file, data, response) {

              }
          });
        },10);
        });
           
