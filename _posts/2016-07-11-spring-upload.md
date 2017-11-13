---
layout: post
title:  "Spring学习笔记---文件上传"
date:   2016-07-11 22:00:00
categories: JavaWeb
tags: Spring
author: 薛彬
---

* content
{:toc}

说是Spring的学习笔记，其实就没涉及到多少Spring的知识。。实现了一个将zip包上传到服务器上并解压的功能。。





我们先在html中写一个表单，通过表单的形式来上传：

```html
<form action="/upload.do?"enctype="multipart/form-data" method="post">
    <p>
        选择文件：<input type="file" name="filename">
    </p>
    <input type="submit" value="上传">
</form>
```

注意要在form标签中加上`enctype="multipart/form-data"`表示该表单是要处理文件的,

然后就可以开始写后台程序了,为了不繁琐只贴出有用的代码：

```java
//  url:/upload.do?filename=??
@RequestMapping(value="/upload.do",method=RequestMethod.POST)
    public void upload(@RequestParam("filename") MultipartFile upload_file,HttpServletResponse response){
    if(!upload_file.isEmpty()){
        //创建一个随机标识
        String uuid = UUID.randomUUID().toString();
        //当前系统缓存目录下的临时文件夹地址
		String tmpDir = System.getProperty("java.io.tmpdir") + File.separator + uuid;
        //获取上传zip包的文件名
        String recv_filename=upload_file.getOriginalFilename();
        String firstDir;
        try{
            String filepath=tmpDir+File.separator;
            //创建临时文件夹
            if(!new File(filepath).exists()){
                new File(filepath).mkdir();
            }
            String path=filepath+recv_filename;
            //将zip包复制到临时文件夹（文件夹必须是存在的）
            upload_file.transferTo(new File(path));
            //解压缩
            firstDir=ZipUtils.unzip(path,tmpDir);
            if(null == firstDir || firstDir.isEmpty()){
            }else{
            	//将解压后的文件复制到目标文件夹
                File srcDir = new File(tmpDir + File.separator + firstDir);
				File destDir = new File();
				destDir.mkdirs();
				FileUtils.copyDirectory(srcDir, destDir);
				FileUtils.forceDelete(srcDir);
            } 
        }catch(IllegalStateException | IOException e1){
        }finally{
            try{
            	//删除临时文件夹
                FileUtils.forceDelete(new File(tmmDir));
            }catch(IOException e){}
        }
    }
}
```

## 笔记

### MultipartFile

MultipartFile常见的方法有：

- String getContentType()：获取文件MIME类型
- String getName()：获取表单中文件组件的名字
- String getOriginalFilename()：获取上传文件的原名
- long getSize()：获取文件的字节大小，单位byte
- boolean isEmpty()：是否为空
- void transferTo(File dest)：保存到一个目标文件中

在这里注意一点，使用`transferTo`时，目标文件夹必须是存在的。也就是说如果目标文件夹不存在，需要提前创建，像这样

```java
if(!new File(filepath).exists()){
    new File(filepath).mkdir();
}
```

### UUID.randomUUID().toString()

这是java提供的一个自动生成主键的方法，其实就是为了生成一个随机文件夹名。

>UUID(Universally Unique Identifier)全局唯一标识符,是指在一台机器上生成的数字，它保证对在同一时空中的所有机器都是唯一的，是由一个十六位的数字组成,表现出来的形式。

### System.getProperty("java.io.tmpdir")

`System.getProperty`用于获取当前的系统属性。

具体的有：（网上找的）

```java
System.out.println("java版本号：" + System.getProperty("java.version")); // java版本号
System.out.println("Java提供商名称：" + System.getProperty("java.vendor")); // Java提供商名称
System.out.println("Java提供商网站：" + System.getProperty("java.vendor.url")); // Java提供商网站
System.out.println("jre目录：" + System.getProperty("java.home")); // Java，哦，应该是jre目录
System.out.println("Java虚拟机规范版本号：" + System.getProperty("java.vm.specification.version")); // Java虚拟机规范版本号
System.out.println("Java虚拟机规范提供商：" + System.getProperty("java.vm.specification.vendor")); // Java虚拟机规范提供商
System.out.println("Java虚拟机规范名称：" + System.getProperty("java.vm.specification.name")); // Java虚拟机规范名称
System.out.println("Java虚拟机版本号：" + System.getProperty("java.vm.version")); // Java虚拟机版本号
System.out.println("Java虚拟机提供商：" + System.getProperty("java.vm.vendor")); // Java虚拟机提供商
System.out.println("Java虚拟机名称：" + System.getProperty("java.vm.name")); // Java虚拟机名称
System.out.println("Java规范版本号：" + System.getProperty("java.specification.version")); // Java规范版本号
System.out.println("Java规范提供商：" + System.getProperty("java.specification.vendor")); // Java规范提供商
System.out.println("Java规范名称：" + System.getProperty("java.specification.name")); // Java规范名称
System.out.println("Java类版本号：" + System.getProperty("java.class.version")); // Java类版本号
System.out.println("Java类路径：" + System.getProperty("java.class.path")); // Java类路径
System.out.println("Java lib路径：" + System.getProperty("java.library.path")); // Java lib路径
System.out.println("Java输入输出临时路径：" + System.getProperty("java.io.tmpdir")); // Java输入输出临时路径
System.out.println("Java编译器：" + System.getProperty("java.compiler")); // Java编译器
System.out.println("Java执行路径：" + System.getProperty("java.ext.dirs")); // Java执行路径
System.out.println("操作系统名称：" + System.getProperty("os.name")); // 操作系统名称
System.out.println("操作系统的架构：" + System.getProperty("os.arch")); // 操作系统的架构
System.out.println("操作系统版本号：" + System.getProperty("os.version")); // 操作系统版本号
System.out.println("文件分隔符：" + System.getProperty("file.separator")); // 文件分隔符
System.out.println("路径分隔符：" + System.getProperty("path.separator")); // 路径分隔符
System.out.println("直线分隔符：" + System.getProperty("line.separator")); // 直线分隔符
System.out.println("操作系统用户名：" + System.getProperty("user.name")); // 用户名
System.out.println("操作系统用户的主目录：" + System.getProperty("user.home")); // 用户的主目录
System.out.println("当前程序所在目录：" + System.getProperty("user.dir")); // 当前程序所在目录
```

### ZipUtils.unzip

ZipUtils是一个压缩/解压缩zip包处理类，有zip、zipFile、unZip等方法。

这里用到的是`unzip(String zipFilePath, String targetPath)`需要的参数就是源文件目录和目标文件夹。

### FileUtils

FileUtils是一个文件操作类，简化了对文件的操作。

这里用到了`copyDirectory`和`forceDelete`方法，用来复制目录和删除目录。

特别提一下删除目录，在这个工具类中，删除是有多种方法的：

```java
public static void main(String[] args) throws Exception {  
    //删除一个目录和他的所有子目录，如果文件或者目录不存在会抛出异常  
    FileUtils.deleteDirectory(new File("D:/fileUtis/targetFile/"));  
    //删除一个目录或者一个文件，如果这个目录或者目录不存在不会抛出异常  
    FileUtils.deleteQuietly(new File("D:/fileUtis/targetFile/"));  
    //清除一个目录下面的所有文件跟目录。  
    FileUtils.cleanDirectory(new File("D:/fileUtis/targetFile/"));  
    //删除一个文件，如果是目录则递归删除forceDelete(File file),跟deleteDirectory基本一样  
    FileUtils.forceDelete(new File("D:/fileUtis/targetFile/"));  
} 
```
