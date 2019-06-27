---
layout: post
title:  "Windows线程同步小结"
date:   2011-05-09 19:00 +0800
tags: javascript
categories: chn
---

为了做毕设从上个月末开始就在纠结这些问题了，头疼到今天才总算有一点成果，写个小结。

# 第一篇

线程在Windows里是进程下做实际工作的，Windows实际上是在调度着众多线程来进行工作，也就是说是允许有一个进程没有任何线程的，但那没什么用。与创建一个进程相比，创建一个线程的开销要小得多，线程没有自己的地址空间，而是在进程的空间上划出一块地方来给他用。因此，只要能获得指针，线程可以访问整个进程的所有东西。

关于Windows和MSVC是如何开启一个进程再开启一个线程，以及如何设置返回值等等，这些是线程的实现机理，可以去看Windows via C/C++的第5章。本文只写一些能用得上的API和线程同步方法。

## 1. 首先创建线程

如果线程里有用到C函数，并且涉及到操作C的全局变量如errno等，为了保证它们的正确性，那么就使用`_beginthreadex()`这个MSVC的扩展函数，否则用Win32API的`CreateThread`即可。它们的参数含义完全一致，但是类型稍有不同，前者用的都是C的类型，而后者的是Windows SDK中用typedef过的Win32数据类型。

```c
HANDLE WINAPI CreateThread(
  __in_opt   LPSECURITY_ATTRIBUTES lpThreadAttributes,
  __in       SIZE_T dwStackSize,
  __in       LPTHREAD_START_ROUTINE lpStartAddress,
  __in_opt   LPVOID lpParameter,
  __in       DWORD dwCreationFlags,
  __out_opt  LPDWORD lpThreadId
); // in <windows.h>, import kernel32.lib(by default)

uintptr_t _beginthreadex(
   void *security,
   unsigned stack_size,
   unsigned ( *start_address )( void * ),
   void *arglist,
   unsigned initflag,
   unsigned *thrdaddr
); // in <process.h>
```

至于为什么推荐用`_beginthreadex`而不用`_beginthread`也在书里有说，另外可以不必手动调用`_endthreadex`，CRT里已经自动调用了。总之都全用`_beginthreadex`就对了orz


## 2. 然后结束线程

不推荐手动调用ExitThread/SuspendThread等等函数来控制另一个线程的生死，书里有提到原因，而要采用其他的思想：设法让这个线程执行结束。然而，如果这个待关闭的线程正在忙着工作，就没办法干掉了，除非是决定结束整个进程，那可以不用担心这个线程会有不能释放的东西，因为进程结束操作系统会自动释放（虽然这个办法也很不符合程序员的美学就是了囧）。

比如，一个线程正在监听着一个Socket，那不应该选择让这个线程结束，而是应该用closesocket关闭这个Socket。然后线程继续下面的操作，处理了socket被关闭的错误之后，它自己做清理工作然后函数返回。

所以，要想办法能让一个线程知道他自己应该结束了。这样就是一些下面的线程同步的技巧了。

## 3. 线程的同步

先从通常意义上说一下线程同步的API，然后再说如何利用其中的一些API来实现上面所说的让线程自动结束的方法。

传统的操作系统概念中有很多线程同步的技术，Critical Section、Semaphore、Monitor等等。这几天我只看过前两个。

> 虽然我们可以自己实现Critical Section，用while循环，但这样会让线程凭空运转直到它可以访问一个互斥变量。所以需要利用Windows提供的Critical Section的API，一旦进入Critical Section时系统会自动把线程调整为阻塞状态，系统就不会调度该线程，节省CPU资源。

```c
/* Following four APIs are used for Critical Sections
InitializeCriticalSection();
DeleteCriticalSection();
EnterCriticalSection();
LeaveCriticalSection();
*/

int    g_int = 0;
CRITICAL_SECTION g_cs;
unsigned int ThreadFunc(void*);

int main() {
    InitializeCriticalSection(&g_cs);
    HANDLE hThread = _beginthreadex(NULL, NULL, ThreadFunc, NULL, NULL, NULL);
    WaitForSingleObject(hThread, INFINITE);
    DeleteCriticalSection(&g_cs);
    return 0;
}

unsigned int ThreadFunc(void*) {    // we don't care about the parameter here
    EnterCriticalSection(&g_cs);
    // do stuff with the mutual/global variable
    g_int++;
    LeaveCriticalSection(&g_cs);
    return 0;
}
```

但是，当一个CRITICAL_SECTION变量没有被初始化过直接就被错误地Delete时，程序运行时会触发错误，所以我推荐用CRITICAL_SECTION的指针。

```c
CRITICAL_SECTION* pcs = NULL;

int main() {
    pcs = new CRITICAL_SECTION;
    // InitializeCriticalSection(pcs);    // Oops, foget to add this line

    /* thread functions blablablablablabla */

    if (pcs != NULL) {    // Still works
        DeleteCriticalSection(pcs);
        delete pcs;
    }
    return 0;
}
/*
当然，如果没有初始化一个CRITICAL_SECTION，自然也无法在线程函数中使用它。这里只是演示一个方法，
把DeleteCriticalSection写到析构函数中，却把InitializeCriticalSection写到构造函数以外的函数中
（比如使用Two-Phase Constructor技巧的时候）,一旦没有调用该函数变量就被析构了，那就会触发错误。
*/
```

至于使用Semaphore其实也就几个API，使用WaitForSingleObject可以等待该信号量值大于0，由于信号量是内核对象，所以实际使用时获得的是信号量的Handle，最后用CloseHandle关闭它即可。

CreateSemaphore创建一个信号量（如果已存在则直接打开它）并设定初始值，WaitForSingleObject等待一个信号量大于0，ReleaseSemaphore来增加一个信号量的值。

不过看情况吧，信号量的作用就是对所有等待它的进程，大于0时可以执行，等于0时一直等待，不是所有的同步情况都适用它的。

信号量和Critical Section用过火都可能会造成死锁——Wait来Wait去的话，用的时候得小心orz

# 第二篇

毕设写差不多了，准备数据写完不敢测试 Orz 就像Yanqing Wang上课时引述的：Software is like a church, first we build it, then we pray. 于是写blog换换脑子囧，顺带把之前的坑填了……

上一篇主要提到了用内核对象Semaphore和用户对象Critical Section来访问进程互斥变量的方法。其实，对于简单的整型变量有方便的互斥访问API：InterlockedIncrement和InterlockedDecrement来做加减1的运算。对64位整数则有对应的InterlockedIncrement64和InterlockedDecrement64。简单地用一下就足够了。

```c
LONG __cdecl InterlockedIncrement(  __inout  LONG volatile* Addend);
```

下面提到的一些使用异步I/O操作的方法来用来做线程同步，其实它们不一定必须要用于与I/O操作相关的工作中。

## 1. 普通线程进程对象

直接使用WaitForSingleObject等一系列的WaitForXXX函数来等待一个线程完成是最简单的方法。

```c
WaitForSingleObject(m_hCheckingThread, INFINITE);
```

指定等待时间，可以让进行上述调用的线程挂起，腾出CPU和Context Switch的开销，直到线程执行结束或者超时系统自动把该线程重新设回可用状态。

通过判断返回值来检查究竟是线程结束或是超时。

## 2. Event

手动地用CreateEvent来创建一个Event的HANDLE，Event只有Signaled

```c
HANDLE WINAPI CreateEvent(  __in_opt  LPSECURITY_ATTRIBUTES lpEventAttributes,  __in      BOOL bManualReset,  __in      BOOL bInitialState,  __in_opt  LPCTSTR lpName);

//....m_hAllJobClearEvent = CreateEvent(NULL, TRUE, TRUE, NULL);
```
ResetEvent将这个被设置为需要手动Reset的Event设置为Nonsignaled状态，使用SetEvent将其设置为Signaled状态。同样使用上面的WaitForXXX系列函数来等待这个Event，当Signaled状态时线程被唤醒继续执行。


## 3. Alertable I/O

每一个线程都拥有一个APC Queue，可以异步地让此线程去做一些事情，当这个线程因为任何API而陷入睡眠状态时，实际上并不会进入该状态，而是转去执行交给自己的那些异步过程调用(APC)。

可见这些工作只会由这个线程本身来做，而且需要定义很多全局或静态的代码，因此不适合大范围采用。不过用它可以实现上一篇提到的优雅地结束线程的要求。

首先需要写一个符合这样APCFunc声明的函数，然后手动地把它提交给某个线程。

```c
VOID WINAPI APCFunc(ULONG_PTR);
// Function pointer, thread handle and function parameterQueueUserAPC(APCFunc, m_hCheckingThread, NULL);
```

所有会使线程陷入suspended状态的API都有一个Ex版本，包括WaitForXXX系列、Sleep和GetQueuedCompletionStatusEx(后面会说)：

```c
DWORD WINAPI WaitForSingleObjectEx(  __in  HANDLE hHandle,  __in  DWORD dwMilliseconds,  __in  BOOL bAlertable);

DWORD WINAPI SleepEx(  __in  DWORD dwMilliseconds,  __in  BOOL bAlertable);

BOOL WINAPI GetQueuedCompletionStatusEx(  __in   HANDLE CompletionPort,  __out  LPOVERLAPPED_ENTRY lpCompletionPortEntries,  __in   ULONG ulCount,  __out  PULONG ulNumEntriesRemoved,  __in   DWORD dwMilliseconds,  __in   BOOL fAlertable);
```

它们相比没带Ex后缀的原API而言，都多了最后一个参数，传入TRUE则表示允许此线程在等待时被APC唤醒。在唤醒后线程会去执行APCFunc函数，然后从原WaitForXXX函数的位置返回WAIT_IO_COMPLETION。

```c
do {    // do something......

    dwSleepRtn = SleepEx(3600000, TRUE);    // wait for an hour to do next, or until others wake up this thread} while (dwSleepRtn != WAIT_IO_COMPLETION);

// jumps out the infinit loop and do some cleanup
```

## 4. I/O Completion Port

这个又是另一个内核对象，保证尽可能少地使用与其绑定的线程去做某个I/O设备的操作。首先创建一个该对象：

```c
// Create an I/O completion portg_hIOCP = ::CreateIoCompletionPort(INVALID_HANDLE_VALUE, NULL, NULL, NULL);

#define CK_FILE1_TEST_OPERATION 1#define CK_OTHER_OPERATION 2

// Bind a file, socket or any other I/O handles to the port with a key, but// we will not use any I/O in this blog topic.//::CreateIoCompletionPort(hFile, g_hIOCP, CK_FILE1_TEST_OPERATION, NULL);

```

由于这里只说它对线程同步的用处，所以不涉及所有I/O操作。

```c
#define CK_EXIT_THREAD    10#define .....blablabla    ...    // define other Completion Keys

// in any other thread, use this API to simulate an I/O completion notification::PostQueuedCompletionStatus(g_hIOCP, 0, CK_EXIT_THREAD, (LPOVERLAPPED)((LONG_PTR)-1));

// in the thread we do:while (true) {    ULONG_PTR completionKey;    OVERLAPPED* pOverlapped;    DWORD dwNumBytes;    // this value is only used to trap the System API    if (!::GetQueuedCompletionStatus(g_hIOCP, &dwNumBytes, &completionKey, &pOverlapped, INFINITE)) {        // error, do something        break;    }

    // check the completion key (CK)    if (completionKey == CK_EXIT_THREAD) {        break;    } else if (completion Key == .....) {        // .......    }}
```

通过外部主控线程使用PostQueuedCompletionStatus来通知这个I/O Completion Port应该做什么事，然后由该Port调用等待的线程去做。自然，如果有很多线程在等待，那就应该Post特定数量的退出消息，然后让所有线程都结束操作。使用这个Post方法时，虽然没有涉及到OVERLAPPED结构，但也不能直接给NULL，否则API会报错，直接给个-1强转的错误值即可，反正在GetQueuedCompletionStatus调用之后我们也不会去使用它。


—————-分割————–

这个I/O Completion Port耗了我不少时间。要注意的是调用GetQueuedCompletionStatus的线程仍然要我们手动创建。

I/O Completion Port在创建时的最后一个参数可以指定最多在此port等待的线程数，设置为NULL默认为系统处理器的数量。事实上保证最多只有处理器数量的线程在执行是最有效的，可以减少Context Switch开销。

这个本来是用来做异步I/O响应的，所谓异步就是在调用某个函数时不会阻塞，函数可以立刻返回，由系统进程和驱动继续去做这个函数操作。例如异步文件读写，首先CreateFile创建一个支持异步的文件，然后使用ReadFile/WriteFile+对应该操作的OVERLAPPED结构体去提交给系统一个异步操作，然后这个调用ReadFile的线程立刻返回继续执行下面代码，而当这个异步操作结束，那就可以由I/O Completion Port来接收系统发来的notification了。所以这个东西才会起名叫做"Completion"的Port啊。

OVERLAPPED结构中还有一个hEvent的HANDLE，在不使用这个I/O completion port时可以使用它采取上面的CreateEvent的方法来接收I/O操作完成的提醒。

———分割2—————–

实际上理解这些倒是不难，但是如何在程序设计中设置这些内核变量的位置，该怎么封装，访问性如何，就很麻烦了orz 还是得多看代码才行。
