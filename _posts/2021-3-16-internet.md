# SA_SIGINFO头文件大全

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <sys/socket.h>
#include <netinet/in.h>
#include <errno.h>
#include <netdb.h>
#include <arpa/inet.h>
#include <unistd.h>
```



```mermaid
graph LR
socket --> bind --> listen --> accept --> read --> write --> close
```

server

```mermaid
graph LR
socket --> connet --> write --> read --> close
```

client





# socket

```c
	//1.创建网络端点
	sockfd=socket(AF_INET,SOCK_STREAM,0);
	if(sockfd==-1){
		printf("can't create socket\n");
		exit(1);
	}
```

int socket(int domain, int type, int protocol);

domain--协议族 AF_UNIX AF_INET AF_ISO

 type--socket类型 SOCK_STREAM SOCK_DGRAM SOCK_RAW

protocol 0默认



# 地址

通用 socketaddr Linux sockaddr_in

```c
struct sockaddr_in
{
  short int sin_family;  //地址类型：AF_XXX
  unsigned short int sin_port;
  struct in_addr sin_addr;
  unsigned char - - pad[- - SOCKET_SIZE__-sizeof(short int)-
						sizeof(unsigned short int)-sizeof(struct in_addr)];
}

struct in_addr
{
  _u32 s_addr;		//uint
}


#define PORT 3000

srvaddr.sin_family=AF_INET;
srvaddr.sin_port=htons(PORT);
srvaddr.sin_addr.s_addr=htonl(INADDR_ANY);

//INADDR_ANY 接受任何地址 0
```

htonl host to network long

htons host to network short

ntohs network to host short

char * inet_ntoa(struct in_addr in);网络二进制数字转换为网络地址

int inet_aton(const char * cp, struct in_addr *inp); cp的字符串 网络地址转网络二进制，存在inp

atoi字符串转数字

struct hostent *gethostbyname(const char *hostname); 域名/主机名转ip

bzero置0



# bind

```c
//2.绑定服务器地址和端口
	if(bind(sockfd,(struct sockaddr *)&srvaddr,sizeof(struct sockaddr))==-1){
		printf("bind error\n");
		exit(1);
	}
```

int bind(int sockfd, sockaddr* myaddr, int addrlen);



# connect

```c
	//2.连接服务器
	if(connect(sockfd,(struct sockaddr *)&srvaddr,sizeof(struct sockaddr))==-1){
		printf("connect error\n");
		exit(1);
	}
```

int connect(int sockfd, sockaddr* servaddr, int addrlen);



# listen

```c
	//3. 监听端口
	//BACKLOG 请求队列的最大长度
	if(listen(sockfd,BACKLOG)==-1){
		printf("listen error\n");
		exit(1);
	}
```

int listen(int sockfd, int backlog);

backlog请求队列的最大长度



# accept

```c
		//4.接受客户端连接
		sin_size=sizeof(struct sockaddr_in);
		if((new_fd=accept(sockfd,(struct sockaddr *)&clientaddr,&sin_size))==-1){
			printf("accept error\n");
			continue;
		}
```

int accept(int sockfd, struct sockaddr* addr,int* addrlen); 

addr、addrelen存放结果，addr客户机地址，addrlen客户机地址长度



# read

```c
		//5.接收请求
		getchar();		
		nbytes=read(new_fd,buf,MAXDATASIZE);
		buf[nbytes]='\0';
		printf("client:%s\n",buf);
```

int read(int fd,char* buf,int len);



# write

```c
		//6.回送响应
		sprintf(buf,"wellcome!");
		write(new_fd,buf,strlen(buf));
```

int write(int fd,char* buf,int len);



# close 

```c
	close(sockfd);
```

int close(int sockfd);



# host ip

struct hostent* gethostbyname(const char *name)

查询域名对应的IP

```
struct hostent{
	char	 h_name;	/*主机正式名称*/
	char	**h_aliases;	/*别名列表，以NULL结束*/
	int 	h_addrtype;	/*主机地址类型：AF_INET*/
	int 	h_length;	/*主机地址长度：4字节32位*/
	char 	**h_addr_list;	/*主机网络地址列表，以NULL结束*/
}
#define 	h_addr 	h_addr_list[0]; //主机的第一个网络地址

struct hostent *he;
he=gethostbyname(argv[1]);
```



struct hostent *gethostbyaddr(const char *addr,size_t len,int family); 

查询IP对应的域名

```
	struct in_addr addr;
	if(inet_aton(argv[1],&addr) != 0)
	{
		he=gethostbyaddr((char *)&addr,4,AF_INET);
		if(he!=NULL)
			printf("h_name:%s\n",he->h_name);
		else
			printf("gethostbyaddr error:%s\n",hstrerror(h_errno));
	}
```

# 多路复用

接受请求



```
			fd_set rdset;
			FD_ZERO(&rdset);
			if(!fd1_finished)
				FD_SET(sockfd1,&rdset);
			if(!fd2_finished)
				FD_SET(sockfd2,&rdset);
```

FD_ZERO(fd_set *fdset)－清空描述符集合 

FD_SET(int fd,fd_set *fdset)－将一个描述符添加到描述符集合 



```
struct timeval{
	long tv_sec;//秒
	long tv_usec;//毫秒
} 


struct timeval tv;
tv.tv_sec=0;
tv.tv_usec=100;

int n=select(max(sockfd1,sockfd2)+1,&rdset,NULL,NULL,&tv);
```

int select(int maxfd,fd_set *rdset,fd_set *wrest,fd_set *exset,struct timeval *timeout);

检查多个文件描述符（socket描述符）是否就绪，当某一个描述符就绪（可读、可写或发生异常）时函数返回。可以实现输入输出多路复用

返回值：有描述符就绪则返回就绪的描述符个数；超时时间内没有描述符就绪返回0；执行失败返回-1。

maxfd－需要测试的描述符的最大值，实际测试的描述符从0－maxfd-1 

rdset－需要测试是否可**读**的描述符集合（包括处于listen状态的socket接收到连接请求） 

pwrset－需要测试是否可**写**的描述符集合（包括以非阻塞方式调用connect是否成功） 

pexset－需要测试是否**异常**的描述符集合（包括接收带外数据的socket有带外数据到达） 

timeout－指定测试超时的时间 





```
if(!fd1_finished && FD_ISSET(sockfd1,&rdset)){
	if((nbytes=read(sockfd1,buf,MAXDATASIZE))==-1){
		printf("read error\n");
		exit(1);
	}
	buf[nbytes]='\0';
	printf("(%ld) server1 respons:%s\n",endtime.tv_sec,buf);
	fd1_finished=1;
}
```

检测是否就绪

FD_ISSET(int fd,fd_set *fdset)－检测一个描述符是否就绪 

# fork

```c
//typedef int pid_t;

pid_t child_pid=fork();

if(child_pid==0){
	//子进程
}

else if(child_pid>0){
		//父进程程序
		wait(&child_pid);
}
else {
    //失败
}
```

僵尸进程（ zombie ） 

子进程终止时如果父进程存在且未处理SIGCHLD信号则子进程变为僵尸进程

僵尸进程占据系统进程表项

exec

```
execlp(argv[1],s,NULL);
```



清除僵尸进程的方法1

忽略SIGCHLD信号(信号处理函数为SIG_IGN)，系统将清除子进程的进程表项，这种方法依赖于Linux版本的实现



信号

sigaction

```c
struct sigaction {                  
	void (*sa_handler)(int);  			   //函数指针
	void (*sa_sigaction)(int, siginfo_t *, void *); //函数指针
	sigset_t sa_mask; 		//屏蔽的信号集
	int sa_flags;			//标志，SA_SIGINFO
	void (*sa_restorer)(void); 	//已废弃
}


act.sa_handler=SIG_IGN; //忽略信号时设置为SIG_IGN
//使用默认动作时设置为SIG_DFL
act.sa_handler = my_op;
static void my_op(int signum)
{
	printf("receive signal %d \n", signum);
}


sigemptyset(&act.sa_mask);//清空信号集set 

act.sa_flags=0;//?
//SA_SIGINFO 设此标志后参数才可以传递给信号处理函数

if(sigaction(SIGCHLD,&act,&oldact)<0){
    //SIGCHLD，被终止进程的PID 
	printf("sigaction error.");
	exit(1);
}
```

SA_SIGINFO



处理信号

int sigaction(int signum, const struct sigaction *act,struct sigaction *oldact); 

signum—指定需要捕获的信号,SIGKILL和SIGSTOP不能指定

act—指定新动作

oldact—存储旧动作



unsigned int alarm(unsigned int seconds);

在指定的时间(seconds秒)后，将向进程自身发送SIGALRM信号 

# 信号

每个信号有 阻塞block 未决pending 函数指针handler

|         | block | pending | handler |
| ------- | ----- | ------- | ------- |
| SIGUP   | 0     | 0       | SIG DFL |
| SIGINT  | 1     | 1       | SIG IGN |
| SIGQUIT | 1     | 0       |         |



sigget_t信号集 1-32号信号

int sigemptyset(sigset_t *set); 清零

int sigfillset(sigset_t *set); 置为1

int sigaddset(sigset_t *set,int signum); 添加

sigdelset(sigset_t *set,int signum); 删除

int sigismember(const sigset_t *set,int signum); 是否含有



屏蔽

int sigprocmask( int how, sigset_t *set, sigset_t *oldset);

how：

1. SIG_BLOCK set希望添加的信号
2. SIG_UNBLOCK 希望解除
3. SIG_SETMASK 屏蔽字为set指向

set位非空，按照how更改**当前进程**

oldset非空，当前进程写入oldset



int sigpending(sigset_t *set)

读当前进程未决信号集，通过set传出





捕捉



sighandler_t signal(int signum,sighandler_t handler); 

signum捕捉信号序号



int sigaction(int signum, const struct sigaction *act,struct sigaction *oldact); 

读取和修改与指定信号相关联的处理动作。

```
struct sigaction {
               void     (*sa_handler)(int);//信号的处理动作
               void     (*sa_sigaction)(int, siginfo_t *, void *);
               sigset_t   sa_mask;//当正在执行信号处理动作时，希望屏蔽的信号。当处理结束后，自动解除屏蔽
               int        sa_flags;//一般为0
               void     (*sa_restorer)(void);
           };
```



int pause(void);

进程挂起等到有信号



sigaction

接受

```c
    struct sigaction act,oldact;
    //初始化
    sigemptyset(&act.sa_mask);
    act.sa_flags = SA_SIGINFO;
    act.sa_handler = new_op;

	int sig;
	//捕捉
    if (sigaction(sig, &act, &oldact) < 0)
    {
        printf("install sigal error\n");
    }
```

发送

```c
    union sigval mysigval;

	mysigval.sival_int = 8; //不代表具体含义，只用于说明问题

    if (sigqueue(pid, signum, mysigval) == -1)
        printf("send error\n");
```



# 并发服务器

清除僵尸

```
	if(strcmp(argv[2],"-c")==0)
	{
		cout<<"clear zombie"<<endl;
		struct sigaction act,oldact;
	
		act.sa_handler=SIG_IGN;
		sigemptyset(&act.sa_mask);
		act.sa_flags=0;
		if(sigaction(SIGCHLD,&act,&oldact)<0)
		{
			cout<<"sigaction error.";
			exit(1);
		}
	}
```

复用

```

	if(strcmp(argv[3],"-r")==0)
	{
		int on=1;
		setsockopt(sockfd,SOL_SOCKET,SO_REUSEADDR,&on,sizeof(on));
		cout<<"reuse addr"<<endl;
	}
```

int setsockopt( int socket, int level, int option_name,const void *option_value, size_t option_len);  

第二个参数level是被设置的选项的级别，套接字级别：SOL_SOCKET。

第三个参数设置选项

accpet

```
	for(;;){
		//4.接受客户端连接
		sin_size=sizeof(struct sockaddr_in);
		if((new_fd=accept(sockfd,(struct sockaddr *)&clientaddr,&sin_size))==-1){
			cout<<"accept error"<<endl;
			continue;
		}
		
		pid_t pid=fork();
		
		if(pid==0){
			//子进程完成客户端处理
			cout<<"client addr:"<<inet_ntoa(clientaddr.sin_addr)
				<<", "<<ntohs(clientaddr.sin_port)<<endl;
			//5.接收请求
			nbytes=read(new_fd,buf,RECVSIZE);
			
			write(new_fd,(char*)&m,sizeof(int));
			
			sleep(60)
			exit(0);
		}
		else if(pid>0){
			//父进程继续接受其他客户端连接
			close(new_fd);
		}
		else{
			cout<<"create child process error."<<endl;		
		}
	}
```

# 守护



# 管道

单向、父子、只读/写

```
	int pipe1[2],pipe2[2]; //[0]读 [1]写
	char pstr[]="parent data";
	char cstr[]="child data";
	char buf[100];
	
	if(pipe(pipe1)<0||pipe(pipe2)<0)
		cout<<"pipe error"<<endl;
	pid_t pid=fork();
	if(pid>0)
	{
		//子进程,用管道1写数据,管道2读数据
		close(pipe1[0]);//关闭pipe1读端口
		close(pipe2[1]);//关闭pipe2写端口
		write(pipe1[1],pstr,sizeof(pstr));
		if(read(pipe2[0],buf,100)>0)
			cout<<"parent received:"<<buf<<endl;
	}	
	else if(pid==0)
	{
		//父进程,用管道1读数据,管道2写数据
		close(pipe1[1]);//关闭pipe1写端口
		close(pipe2[0]);//关闭pipe2读端口
		if(read(pipe1[0],buf,100)>0)
			cout<<"child received:"<<buf<<endl;
		write(pipe2[1],cstr,sizeof(cstr));
		exit(0);
	}
	else
		cout<<"fork error"<<endl;
	return 0;
```



# 命名管道

以文件存在

可以无父子关系

阻塞模式

int mkfifo ( char *pathname, mode_t mode );

```c
写进程 mkfifo open写堵塞 write
读进程 open读堵塞  read
```

# Unix socket

双向通信，同一台机器，不是真正网络协议

地址

```c
struct socketaddr_un{
	short int sun_family;	//AF_UNIX
	char sun_path[104];	//文件名的绝对路径
}； 
```

命名unix socket类似socket

非命unix socket：socketpair 父子进程

# 信号灯

内核信号灯

POSXI 无名信号灯



# 共享内存

ftok根据文件名建立ipc通信

shmget创建

shmat启动

shmdt分离，当前进程不使用

# IO模型

堵塞式IO

超时控制

1. 调用alarm

2. 设置socket选项

​	设置SO_RCVTIMEO和SO_SNDTIMEO选项

​	设置了这两个选项之后，所有的读写操作可以保证在超时范围内返回

​	只需设置一次选项，对以后的读写操作均有效

​	不适用于accept和connect



非堵塞IO:长时间占用CPU

函数fcntl

int flags;

flag=fcntl(sockfd,F_GETFL,0);

fcntl(sockfd,F_SETFL,flag|O_NONBLOCK);

函数ioctl

int on=1;

ioctl(sockfd,FIONBIO,&on);



多路复用

接受

默认接收下限为1（TCP为1字节，UDP为1个数据报），可以用SO_RCVLOWAT修改默认值

发送

TCP默认下限为2048字节，可以用SO_SNDLOWAT修改默认值

UDP协议没有实际的发送缓冲区，其发送缓冲区空间总是大于发送下限，所以UDP socket总是写就绪



信号驱动通信

更适用于UDP协议