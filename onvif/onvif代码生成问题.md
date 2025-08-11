#### typemap.bat修改
/custom/duration.c文件报错：

1.未知的LONG64类型

2.soap\_default\_xsd\_\_duration函数被定义未宏

![image](images/gTiM894DwMVieXkdbUdFLfrsFRWF5gKePZ3p95JBfy4.png)

将typemap.bat中,此处注释的关于xsd\_duration的内容放开。

![image](images/yDw_-oG4QXeMJW23jZm-fTnJZwBlR8xdfrWzHVzydig.png)

#### 鉴权编译问题
1.编译中会包struct\_timeval.c中找不到关于‘SOAP\_TYPE\_xsd\_\_dateTime’的申明(有其他方法解决此问题，但是依旧回报第二个问题)

![image](images/Z7yS24YX8og2JLQ807pRw_Hs1M2fIJTZTNcUNpp9464.png)

2.编译中会包wsseapi.h中找不到关于struct ds\_\_KeyInfoType的定义

![image](images/FHPa6-48FS3QpvnlCyOODQjDaSBbp-gia_ZMk6FmEPk.png)

需要在生成gsoap.h头文件后，在文件中添加#include "wsse.h"。

![image](images/YkKVkKC7GIHJj7XRxhrgKjR1ysYLkMReNKpZIcxzEI0.png)

