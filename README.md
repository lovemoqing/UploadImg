# UploadImg
上传图片并预览的效果

2018年2月6日14:45:02 
前端图片上传并预览，后端代码没写，表单提交以后通过request["file"]即可得到文件对象，存储就不赘述，主要是最近做上传遇到上传预览的功能需求，所以就自己写了一个。
网上插件也有的，功能肯定也会有很多，不过对于简单功能来说，插件太臃肿了，有很多不需要的功能，引用插件也避免不了引用一些JS文件，还是有些影响加载速度的。
本案例写的上传方法已经可以实现图片上传时的预览、文件格式以及大小的校验，因此这也是不推荐再用插件的理由，基本上都能满足普遍需求了。
当然，以后遇到别的不具备的需求的时候也会继续优化和新增功能.


2018-8-16 15:08:57
JS写法改成面向对象写法， 同时DOM元素在页面上只留一个div，其余都用JS动态绑定，可以在Init的时候传参（html字符串），目前的自定义限制了DOM元素的ID，后续会开放多个参数，更大程度支持自定义html（如果有必要的话，目前推荐不传参）。
//提供两种方式进行实例化  1.var uploadimg = UploadImg(); 2.var uploadimg = new UploadImg();

2018年10月26日11:05:13
最近的一次需求遇到一个场景，一个页面上涉及到多个表单提交，并且点击某个提交按钮的时候，如果页面的表单都填写完毕，将一次性把页面所有的表单数据提交到后台。
可以说比较棘手了，因为表单里涉及到文件上传操作，所有的内容如果合并在一起做提交就分辨不出哪些文件是那个表单的。所以首先想到的就是ajax异步进行表单提交。
原理其实很简单，就是进行两次ajax。
新建的文件是“UploadImg-多表单上传.html”，相关代码已经上传，由于图片上传的其他代码都一样，因此只写了核心的ajax部分。

2019年4月10日16:04:20
网上看到了一个前端分片后端合并的文件上传操作，能用，加上自己的修改之后更完善了。
html已经上传GitHub了，后端代码如下：
`
        public void SaveFile(HttpContext context)
        {
            var timeFlag = context.Request.Form["timeFlag"];
            var fileName = context.Request.Form["fileName"];
            //保存文件到指定目录
            var filePath = @"D:\penglong\study\WebApplication1\WebApplication1\uploads\" + timeFlag + fileName;
            //创建一个追加（FileMode.Append）方式的文件流
            using (FileStream fs = new FileStream(filePath, FileMode.Append, FileAccess.Write))
            {
                using (BinaryWriter bw = new BinaryWriter(fs))
                {
                    //读取文件流
                    BinaryReader br = new BinaryReader(context.Request.Files[0].InputStream);
                    //将文件留转成字节数组
                    byte[] bytes = br.ReadBytes((int)context.Request.Files[0].InputStream.Length);
                    //将字节数组追加到文件
                    bw.Write(bytes);
                }
            }
        }
`
详细描述可以见我的博客：[asp.net 文件分片上传](https://www.cnblogs.com/sunshine-wy/p/10681765.html)
