# BreakPointFile
此 Demo 简单演示了如何利用多线程来对文件进行断点续传、下载。目前很多下载工具都支持文件的持续下载：即使上一次下载未完成，下次下载的时候还会继续上次的下载，已经下载过的数据就不会重复下载了，可以避免重复下载。这在与文档有关的应用中特别重要，在磁盘稀缺的移动应用中也非常重要，不仅可以省流量，还能减少磁盘的开销。


### 如何使用
        //参考 App.java 中文件
        String source = "http://*****/demo.doc";
        //第一个参数代表文件的url地址，第二个参数代表文件的保存路径
        BreakPointDownload file = new BreakPointDownload(source,"D:/BreakPointFile");
        file.download();
  
### 如何使用多线程
当下载一个大文件时，启用多线程下载显然是一个更好的选择。每个线程分别下载不同范围的数据，待全部线程下载完成时，再将下载的数据合并。在这个Demo 中，是否启用多线程会自动判断：文件的大小每超出 10M（在 BreakPointDownload.java 中控制），就会启用一个线程去下载对应范围的数据。
###
        比如当文件大小为40M时，会用4个线程下载：
        第一个线程下载大小：10M，范围：0 - (10*1024*1024-1) 
        第二个线程下载大小：10M，范围：10*1024*1024 - (20*1024*1024-1)
        第三个线程下载大小：10M，范围：20*1024*1024 - (30*1024*1024-1)
        第四个线程下载大小：10M，范围：30*1024*1024 - (40*1024*1024-1)
# 示例
### 第一次下载的时候，如果没有找到相应的数据文件，会生成对应的数据文件
![](https://github.com/DistChen/BreakPointFile/blob/master/images/1.png?raw=true "第一次下载的时候")
### 下载过程中生成的临时文件
![](https://github.com/DistChen/BreakPointFile/blob/master/images/2.png?raw=true "下载时生成的一些临时文件")
### 如果上次下载未成功，再次下载时，从上次下载的位置继续下载，不会重复下载已经下载过的数据
![](https://github.com/DistChen/BreakPointFile/blob/master/images/3.png?raw=true "如果上传下载未成功，再次下载时，从上次下载的位置继续下载，不会重复下载已经下载过的数据")
### 下载成功后，临时文件会自动删除
![](https://github.com/DistChen/BreakPointFile/blob/master/images/4.png?raw=true "下载成功后，临时文件会自动删除")

# 结束：关键点
    1、利用 http 中的 range 头，控制请求指定范围内的数据；
    2、我这里用的 CyclicBarrier 来控制多线程的同步；
    3、断点续传的原理也是类似的，只不过换作服务端来记录下载的信息。