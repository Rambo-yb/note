进程、exec函数族、管道联合调用实现system()函数，执行命令并重定向到某个文件，如不需要重载到文件，去掉管道部分和写文件即可

1.进程
fork(): 是创建一个子进程，并把父进程的内存数据copy到子进程中；父进程和子进程是部分先后的。
vfork(): 是创建一个子进程，并和父进程的内存数据share一起用；保证子进程先执行，在子进程执行了exit()和exec()后，父进程往下执行。

为什么调用return程序会挂掉，而exit()不会？

在子进程中return，那么基本就是下面的过程

1)子进程的main()函数return，于是程序的函数栈发生变化；
2)main()函数return后，通常会调用exit()或相似的函数
3)这时，父进程收到子进程的exit()，开始从vfork返回，但是父进程的栈被子进程给return修改了，就无法执行了(注：栈会返回一个诡异一个栈地址，对于某些内核版本的实现，直接报“栈错误”就结束了。然而，对于某些内核版本的实现，于是有可能会再次调用main()，于是进入了一个无限循环的结果，直到vfork调用返回 error)
再回到return和exit，return会释放局部变量，并弹栈，回到上级函数执行；exit直接退掉。在C++中，return会调用局部对象的析构函数，exit不会.(注：exit不是系统调用，是glibc对系统调用 \_exit()或\_exitgroup()的封装)
可见，子进程调用exit()没有修改函数栈，所以，父进程得以顺利执行。但是，如果你调用exit()函数还是会有问题的，正确的方法应该是调用\_exit()函数，因为exit()函数会flush并close所有的标准I/O，这样会导致父进程受到影响。(这个情况在fork下也会受到影响，会导致一些被buffer的数据被flush两次，可以参看《https://coolshell.cn/articles/7965.html》)
原文：https://coolshell.cn/articles/12103.html

2.exec函数族


3.管道(问题3)


4.dup和dup2

int dup(int fd) fd一个已打开的文件描述符，返回当前进程可用的最小文件描述符，返回的文件描述符与fd表示同一个文件

int dup2(int oldfd, int newfd) 函数会先判断oldfd与newfd是否为同一个值，如果是直接返回newfd；如果不是，会先把newfd指向的文件关闭，然后把oldfd复制给newfd，然后返回newfd

本例中，我们使用 dup2(pipefd\[1\], STDOUT\_FILENO) 将STDOUT\_FILENO指向管道的写端；使用 dup2(pipefd\[0\], STDIN\_FILENO) 将 STDIN\_FILENO 指向管道的读端(问题1)。子进程将原本写到标准输出的内容写到管道中，父进程再从管道里面读出数据写到文件里面，实现输出重定向的功能(问题2)。


问题1：如果只是需要将原来的标准输出，写入到文件中，我此时是不是可以直接读取pipefd\[0\]的内容，而不需要将标准输入指向pipefd\[0\]？

问题2：重定向如何还原？

问题3：管道使用完毕之后怎么关闭管道，使用close(pipefd\[0\])和close(pipefd\[1\])之后，是否可以认定管道已经被关闭呢？

```cpp
#include <stdio.h>
#include <unistd.h>
#include <stdlib.h>
#include <sys/types.h>
#include <string.h>
#include <sys/wait.h>
#include 
#include 


int main(int argc, char** argv)
{
	char buf[] = "ls /home -l";
	std::vectorstd::string splitStr;
	char* p;
	p = strtok(buf, " ");
	while(p)
	{
		splitStr.push_back(p);
		p = strtok(NULL, " ");
	}
	int size = splitStr.size();
	char* param[size+1];
	for(int i = 0; i < size; i++)
	{
		param[i] = (char*)splitStr[i].c_str();
	}
	param[size] = NULL;
	int stdinBck = dup(STDIN_FILENO);
	int stdoutBck = dup(STDOUT_FILENO);
	int pipefd[2];
	if(pipe(pipefd))
	{
		printf("pipe error\n");
		return 0;
	}
	int pid = vfork();
	if(pid < 0)
		printf("vfork error\n");
	else if(pid == 0)
	{
		close(pipefd[0]);
		dup2(pipefd[1], STDOUT_FILENO);
		execvp((const char*)param[0], (char* const*)param);
		_exit(1);
	}
	else
	{
		printf("line : %d\n", __LINE__);
		wait(NULL);
		close(pipefd[1]);
		dup2(pipefd[0], STDIN_FILENO);
		FILE* fp = fopen("./debug.txt", "w+");
		if(fp == NULL)
		{
			perror("fopen");
			return 0;
		}
		char buf[1024] = {0};
		int ret = 0;
		printf("line : %d\n", __LINE__);
		while((ret = read(pipefd[0], buf, sizeof(buf))) != 0)
		{
			fwrite(buf, 1, ret, fp);
		}
		fclose(fp);
		close(pipefd[0]);
		dup2(stdinBck, STDIN_FILENO);
		dup2(stdoutBck, STDOUT_FILENO);
		close(stdinBck);
		close(stdoutBck);
	}
	return 0;
}
```
