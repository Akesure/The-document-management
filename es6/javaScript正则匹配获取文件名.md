```
//获取文件名，不带后缀
var file_name=file_path.replace(/(.*\/)*([^.]+).*/ig,"$2");

//获取文件后缀
1.var FileExt=file_path.replace(/.+\./,"");

2.var fileExtension = file_path.substring(file_path.lastIndexOf('.') + 1);

//截取文件后缀
var reg = /\.\w+$/;
var file_name = file_path.replace(reg,'');
```
