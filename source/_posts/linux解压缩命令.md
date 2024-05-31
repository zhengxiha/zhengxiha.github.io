---
title: linux-解压缩命令
date: 2022-03-08 17:20:15
categories: #分类
- 软件测试
tags: #标签
  - Linux


---
-c表示备份；<br />-r 表示增加文件；<br />-u 表示更新文件；<br />-t 列出文件；<br />-x表示解压；<br />注：这五个是独立的命令，压缩解压都要用到其中一个，可以和别的命令连用但只能用其中一个<br />帮助文档详情：<br />![image.png](https://cdn.nlark.com/yuque/0/2022/png/25987745/1646752301195-1a5dddd1-d74e-450b-a2f3-9da324f63493.png#averageHue=%23fdfdfd&clientId=ua4455a91-6456-4&from=paste&height=186&id=u718bc84b&originHeight=228&originWidth=579&originalType=binary&ratio=1&rotation=0&showTitle=false&size=21525&status=done&style=none&taskId=u190ea2b2-a6b6-4211-a4e5-2448cb9759c&title=&width=472.5)<br />-f: 使用档案名字，切记，这个参数是最后一个参数，后面只能接档案名。<br />![image.png](https://cdn.nlark.com/yuque/0/2022/png/25987745/1646752380651-e898b01e-d4e4-4720-9940-62619a3a3813.png#averageHue=%23fefefe&clientId=ua4455a91-6456-4&from=paste&height=78&id=u3bbfb90d&originHeight=116&originWidth=682&originalType=binary&ratio=1&rotation=0&showTitle=false&size=8375&status=done&style=none&taskId=ua199a1e1-cac4-4f13-acd4-c10b26c74a5&title=&width=456)<br />-v, --verbose              详细地列出处理的文件<br />![image.png](https://cdn.nlark.com/yuque/0/2022/png/25987745/1646752432425-cd4cf943-a3df-4f64-a68f-2cbe1be946c9.png#averageHue=%23fdefee&clientId=ua4455a91-6456-4&from=paste&height=91&id=u87e9e72e&originHeight=125&originWidth=602&originalType=binary&ratio=1&rotation=0&showTitle=false&size=13816&status=done&style=none&taskId=ua798fb6b-b83f-46d5-aefc-10a2eb69d09&title=&width=439)[

](https://blog.csdn.net/weixin_42359436/article/details/107949145)
<a name="xg8AS"></a>
## 1、tar
使用 tar 程序打出来的包我们常称为 tar 包，tar 包文件的命令通常都是以 .tar 结尾的。生成 tar 包后，就可以用其它的程序来进行压缩<br />**tar -cf  **：压缩<br />**tar -xf  **：解压<br />例：tar -cf test1.tar test1 <br />![image.png](https://cdn.nlark.com/yuque/0/2022/png/25987745/1646751976348-f361c2bf-a675-41e5-892f-83efcd0e1718.png#averageHue=%23fdf0ef&clientId=u40ea92a5-a5fa-4&from=paste&height=73&id=u4e25e04c&originHeight=111&originWidth=572&originalType=binary&ratio=1&rotation=0&showTitle=false&size=14915&status=done&style=none&taskId=uc262c46d-0b2a-410b-ac11-22f3f53ee91&title=&width=378)<br />tar -xf test2.tar<br />![image.png](https://cdn.nlark.com/yuque/0/2022/png/25987745/1646752134272-20e5f3d0-578a-4633-ae50-b24c70208fb2.png#averageHue=%23f8f5f5&clientId=ua4455a91-6456-4&from=paste&height=108&id=ud039d410&originHeight=160&originWidth=639&originalType=binary&ratio=1&rotation=0&showTitle=false&size=18050&status=done&style=none&taskId=uea0a8696-1686-4c0c-896f-ddf8c92e4bd&title=&width=431.5)
<a name="bbZ3D"></a>
## 2、zip/unzip
zip -r test3.zip test3  ---->压缩test3目录生成test3.zip<br />![image.png](https://cdn.nlark.com/yuque/0/2022/png/25987745/1646752659560-bc6398a5-acac-4091-8bfe-64ad790ebd09.png#averageHue=%23faf3f3&clientId=ua4455a91-6456-4&from=paste&height=118&id=u0c5d4c09&originHeight=177&originWidth=703&originalType=binary&ratio=1&rotation=0&showTitle=false&size=25001&status=done&style=none&taskId=uafdaf98c-5918-4296-90a6-2d1a45f2a03&title=&width=468.5)<br />unzip test3.zip  --------->将test3.zip解压生成test3文件夹<br />![image.png](https://cdn.nlark.com/yuque/0/2022/png/25987745/1646752749545-42b067b3-393f-4031-a863-f064d62e46ba.png#averageHue=%23fcf7f7&clientId=ua4455a91-6456-4&from=paste&height=149&id=u8936bb85&originHeight=237&originWidth=701&originalType=binary&ratio=1&rotation=0&showTitle=false&size=30448&status=done&style=none&taskId=u6bf380c9-562e-4ca2-a03e-943256e343a&title=&width=441.5)

<a name="CEQ71"></a>
## 3、  .tar.gz文件

压缩命令：<br />tar **-zcvf** 压缩文件名 .tar.gz 被压缩文件名<br />解压命令：<br />tar **-zxvf **压缩文件名.tar.gz<br />#把当前目录下的222.tar.gz解压到ee/下，前提要保证存在ee这个目录<br />** tar -zxvf **222.tar.gz **-C** ee/<br />扩展：<br />1、*.tar 用 tar –xvf 解压<br />2、*.gz 用 gzip -d或者gunzip 解压<br />3、*.tar.gz和*.tgz 用 tar –xzf 解压<br />4、*.bz2 用 bzip2 -d或者用bunzip2 解压<br />5、*.tar.bz2用tar –xjf 解压<br />6、*.Z 用 uncompress 解压<br />7、*.tar.Z 用tar –xZf 解压<br />8、*.rar 用 unrar e解压<br />9、*.zip 用 unzip 解压<br />10、tar –cvf jpg.tar *.jpg       // 将目录里所有jpg文件打包成 tar.jpg<br />11、rar a jpg.rar *.jpg          // rar格式的压缩，需要先下载 rar for linux<br />12、zip jpg.zip *.jpg            // zip格式的压缩，需要先下载 zip for linux<br />13、tar –xvf file.tar            // 解压 tar 包


权限管理<br />改变文件拥有者的权限：chown <br />![image.png](https://cdn.nlark.com/yuque/0/2022/png/25987745/1647251448374-e06fb07c-d955-457a-985d-c32f4ad01cfa.png#averageHue=%23faf9f9&clientId=u1132c3e6-7f24-4&from=paste&height=170&id=u01fb01e3&originHeight=340&originWidth=515&originalType=binary&ratio=1&rotation=0&showTitle=false&size=32805&status=done&style=none&taskId=u25179c38-1092-4ec3-af96-9ff157aa652&title=&width=257.5)<br />[<br />](https://blog.csdn.net/weixin_42359436/article/details/107949145)新增用户组：groupadd -g 组名<br />例 groupadd -g 201 iread:增加一个名为iread,用户组id为201的用户组<br />删除用户组：groupdel 组名<br />修改用户组：groupmod -g 10000 -n group3 group2:将组group2的标识号改为10000，组名修改为为group3<br />创建用户：useradd 用户名 <br />例：useradd -g iread -d /home/wap -s /usr/bin/csh -m wap<br />---》新增一个用户wap、-g表示用户组，归属用户组为iread、-d用户主目录，-s用户使用shell，-m用户名
