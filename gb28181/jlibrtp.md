jlibrtp中会获取环境变量"LOGNAME"的值，一般没有设置该值。临时使用可以使用export进行设置。也可以修改源码(/jlibrtp根目录/src/rtpsession.cpp)

```plsql
int RTPSession::CreateCNAME()
{
    ...
    // char *logname = getenv("LOGNAME");
    char *logname="root"
    ...
}
```
![image](images/qj1tLqe_2nxi4JzA7Bzrl1taBWp9GOQ_EEZfiQHIjiE.png)