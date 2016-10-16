## picture_selector *微信图片选择器*  
### 目标：
1. 尽可能的去避免内存溢出  
a. 根据图片的显示大小去压缩图片[注意：如果预览格式中b子项内容没有另起一行而是紧贴a子项屁股后面，可以考虑在a项内容末尾加两个空格]  
b. 使用缓存对我们的图片进行管理（LruCache）  
2. 用户操作UI控件必须充分的流畅  
a. getView里面尽可能不去做耗时操作（异步加载+回调显示）  
3. 用户预期显示的图片尽可能的快（图片加载策略选择 先进先出还是后进先出）

### 重要类Imageloader*图片加载类*  
类里面的重要方法：


1. getView(){  
url->LruCache查找  
->找到返回  
->找不到url->Task->TaskQueue且放松一个通知去提醒后台轮询线程  
}  
2. Task->run(){  
a. 获得图片显示的大小  
b. 使用Options对图片进行压缩  
c. 加载图片且放入LruCache  
}  
3. 后台轮询线程TaskQueue->Task->线程池去执行  
实现方式：  
a. new Thread(){run(){
while(true){
……
}*没有采用，效率不高*  
b. Handle+Looper+Message
