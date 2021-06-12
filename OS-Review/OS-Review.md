### 银行家算法（避免死锁）

$Request_i$ is a request vector of process $P_i$, if $P_i$ needs $K$ resources of $R_j$ type, when $P_i$ sends requests, the system conforms following:

```python
if Request[i] <= Need[i]:
	if Request[i] <= Available:
		Available -= Request[i]
		Allocation += Request[i]
		Need[i] -= Request[i]
		if CheckSafety() == True:
			ALLOCATE
		else:
			RESET
			Pi waits
	else:
		Pi waits until resources are enough
else:
	ERROR
    
def CheckSafety():
    Work = Available
    for i in range(ALL_PROCESSES):
    	Finish[i] = False
    for i in range(ALL_PROCESSES):
       	if Finish[i] == False and Need[i] <= Work:
        	Work += Allocation
            Finish[i] = True
    for i in range(ALL_PROCESSES):
    	if Finish[i] == False:
            return False
    return True
```

### 磁盘读取并处理数据

磁盘需要旋转完整的块才可以读出数据，这之后才能处理！考虑处理每一个区块的占用时间。

### P-V操作

P检查信号量初值是否大于0 ，如果大于0，减1，继续执行；如果等于0，进程被直接阻塞（将当前进程从运行队列移动到信号量的队列），减1的操作暂时不做；

V首先将信号量S增加1 (原子操作)。但是如果有一个或者多个进程在信号量的队列睡眠（这时S=1），就会随机唤醒一个进程（将进程从信号量的队列移入就绪队列），并使得其运行后能完成P 操作的减1，所以这时S还是0。

> 有一个超市，最多可容纳 N 个人进入购物，当 N 个顾客满员时，后到的顾客在超市外等待;超市中有 1 个收银员。可以把顾客和收银员看作两类进程，两类进程间存在同步关系。请利用 P、V 操作描述这些进程之间的同步关系。

```shell
Var		inStore, waitingQueue, empty	:semaphore
inStore			:=	0
waitingQueue	:=	0
empty			:=	N

# CASHIER
begin
	while True do
		P(inStore)
		Cash()
		V(waitingQueue)
	end
end

# CUSTOMER
begin
	while True do
		P(empty)
		EnterStore()
		V(inStore)
		P(waitingQueue)
		Pay()
		V(empty)
	end
end
```

### 存储管理

**页号**：

> 若某计算机的逻辑地址空间和物理地址空间均为$64KB$，按字节编址。页的大小为$1KB$。要访问逻辑地址$17CA$，该逻辑地址对应的页号是多少？

$17CA$ -> $0001\ 0111\ 1100\ 1010$

$\because1KB$

$\therefore the\ last\ ten\ bits\ should\ be\ page,\ and\ the\ first\ 6\ is\ the\ page\ number$

则页号为$000101$，即$5$。

**FIFO**（先进先出）：

在时刻 260 前，某进程内存分配与访问情况如下表所示：

| 页号 | 页框号 | 装入时间 | 访问时间 |
| :--: | :----: | :------: | :------: |
|  0   |   7    |   130    |   250    |
|  1   |   4    |   230    |   230    |
|  2   |   2    |   200    |   240    |
|  3   |   9    |   160    |   245    |

最先进入的最先出，则页框号7将被置换出，所以物理地址为0001 1111 1100 1010，二进制为1FCA。

**LRU**（最近最久未使用）：

目前是260时刻，最久远的访问时间是230，页框号为4，所以物理地址为0001 0011 1100 1010，二进制为13CA。

