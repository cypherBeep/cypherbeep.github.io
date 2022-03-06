---

title: Handy Notes
date: 2021-02-24
author: Sanad Kadu

---


# Core CS


## Important Topic for OS


### Process, Thread, IPC

1.  Processes

    -   Process is instance of a computer program being executed by one or many threads.
        process ( tasks, jobs ) ~ program in execution + stack, registers + temporary data ( such as return addresses, subroutine parameters ) + program counter + other activities
        process ≠ program e.g. run same program editor, can have several processes
        -   Child Process is process created by another process usually by using fork();
        -   Parent Process is the process which creates child processes in Unix there is process0 which is special as its created when os boots up and all other processes are its children.
            process 0 (swapper process)  forks into process1 (init process) which is ancestor of all other processes.
        -   When a process needs to go (die) system removes it pid and releases most of the resources but keeps data about its termination code and resource utilization in case its parent wants to know this stuff. As soon as kill signal is made, the system releases those last bits of information about the child process and removes its pid from the process table. However, if the parent process lingers in collecting the child&rsquo;s data (or fails to do it at all), the system has no option but keep the child&rsquo;s pid and termination data in the process table indefinitely. Such a terminated process whose data has not been collected is called a zombie process. Child is dead but its data lingers on&#x2026;
        -   Parent is dead yet the child lingers on&#x2026; orphaned process.
    -   States of a process are needed when a OS supports multitasking
        -   new: the process is being created
        -   ready: process waiting to be assigned to a processor
        -   running: instructions being executed
        -   blocked: waiting for some events to happen ( e.g. I/O or reception of signal )
        -   terminate: process has finished execution
    -   Process table ( PT ) saves Process Control Blocks ( PCB )
        each process has a PCB with the information:
        -   process state
        -   program counter ( PC )
        -   CPU registers
        -   memory-management information
        -   accounting information ( e.g. time limits, process #, &#x2026; )
        -   I/O status information ( e.g. list of open files by the process )
    

2.  Threads

    -   Thread is basically the smallest sequence of instructions that can be managed independently by a scheduler.
    -   process ~ program in execution + resources needed
    -   A thread ( light weight process ( LWP ) ) is a basic unit of CPU utilization ~ PC + registers + stack space
    -   a thread shares with peer threads its coded section, data section, and OS resources ( such as open files and signals ) ~ task
    -   traditionally heavy weight process = thread + task
    -   CPU switching much easier no memory-management related work need to be done
    -   The multiple threads of a given process may be executed concurrently (via multithreading capabilities), sharing resources such as memory, while different processes do not share these resources. In particular, the threads of a process share its executable code and the values of its dynamically allocated variables and non-thread-local global variables at any given time.

3.  IPC

    -   When processes need to communicate with each other they must share parts of their address spaces or use other forms of inter-process communication (IPC). For instance in a shell pipeline, the output of the first process need to pass to the second one, and so on; another example is a task that can be decomposed into cooperating but partially independent processes which can run at once.


### Scheduling Algorithm

Goals for Scheduling Disciplines

-   CPU utilization &#x2013; keep CPU as busy as possible
-   Fairness &#x2013; each process has fair share of CPU
-   Minimize response time &#x2013; time from the submission of a request until the first response is produced
-   Minimize waiting time &#x2013; total time spent waiting in the ready queue
-   Minimize overhead (context swaps)
-   Minimize turnaround time &#x2013; the interval from the time of submission to the time of completion: waiting time to get into memory + waiting in ready queue + execution on CPU + I/O time
-   Maximize throughput &#x2013; the number of processes that are completed per unit time

1.  First Come First Serve

    -   Run until completed.
    -   Finished ususally means blocked.
    -   Most simple scheduling algorithm.
    -   Whichever process comes first goes to the cpu and all others have to wait until it finishes.
    -   Needs a FIFO queue.
    
    1.  Problem
    
        Any one process can monopolize the CPU resulting in too long waiting.

2.  Round Robin

    -   RR = FCFS + preemption i.e we can switch between processes (context switch).
    -   Run a process for a specified duration of time (quantum) then put it back to the queue.
    -   Each process gets equal share of CPU and quantum ~= 10 to 100ms.
    
    1.  Problem
    
        May produce poor turnaround time.
        
        if quantum = 20 ms
        switching time = 5 ms
        % of CPU time &rsquo;wasted&rsquo; = 5 / ( 20 + 5 ) = 20%
        if quantum = 500 ms, waste < 1%
        faster CPU can increase efficiency

3.  Shortest Job First

    -   shortest time to completion first. This minimizes the average response time.
    -   SJF can be preemptive or nonpreemptive
        -   nonpreemptive &#x2013; when shortest job comes in, it has to wait until the currently-executed job has finished
        -   preemptive &#x2013; when shortest job comes in, currently-executed job will be suspended then, shortest-remaining-time-first
    
    1.  Problem
    
        SJF works quite nicely; unfortunately, it requires knowledge of the future. Instead, we can use past performance to predict future performance.

4.  Multilevel queue

    -   Attach priority to each process ( e.g. faculty processes has higher priorities than students ).
    -   In general, give newly runnable process a high priority and a very short time slice. If process uses up the time slice without blocking then decrease priority by 1 and double time slice for next time.
    -   A process running too long may have priority adjusted.
    -   Techniques like this one are called adaptive. They are common in interactive systems.


### Memory Allocation

1.  Paging

    -   Each program has an address space, each address space has multiple blocks in physical memory, the block is called a page.
    -   MMU manages conversion of address space and physical memory. Page table stores mapping of abstract page address (program address space) to its real memory address in physical memory (page frames).

2.  Virtual Memory

    -   The purpose of virtual memory is to expand the physical memory into a larger logical memory, so that the program can get more available memory.

3.  Segmentation

    -   Virtual memory uses paging technology, that is, the address space is divided into fixed-size pages, and each page is mapped with the memory. But in multiple tables are created after compilation of a program like call stack, symbol table, constant table, parse tree etc. Now some of these tables need to expand and contract which is can&rsquo;t be done if we have fixed paging system. So we use segmentation.
    -   The method of segmentation is to divide each table into segments, and a segment constitutes an independent address space. The length of each segment can be different and can grow dynamically.
    -   Paging is mainly used to implement virtual memory, thereby obtaining a larger address space; segmentation is mainly to enable programs and data to be divided into logically independent address spaces and to facilitate sharing and protection.


### Multiprogramming vs Multiprocessing vs Multitasking vs Multithreading

-   Multiprogramming – A computer running more than one program at a time (like running Excel and Firefox simultaneously).
-   Multiprocessing – A computer using more than one CPU at a time.
-   Multitasking – Tasks sharing a common resource (like 1 CPU).
-   Multithreading is an extension of multitasking.


### Page replacement Algorithms

-   When a program is running and the page it needs is not in memory then a page fault interrupt occurs and the page is loaded into memory.
-   But if there is no space in memory for the needed page then some of the existing pages need to swapped or removed or managed somehow to make space for this newly arrived page. That&rsquo;s where page replacement algorithms come into play.
-   The main goal of these algorithms is to reduce the page fault interrupt rates and ensure that there is always some space for the needed pages when they are required.

1.  Optimal Page Replacement (Best one) (Theoretical)

    -   Swap that page which we know won&rsquo;t be required for the longest period in the forcible future thus ensuring least page fault rate.
    -   But as no one can know which page won&rsquo;t be used for longest time in future this algorithm is theoretical.

2.  Least Recently Used

    -   We can&rsquo;t know the future but we can sure learn from the past. LRU swaps that page which has been unused most recently.
    -   In order to implement LRU, a linked list of all pages needs to be maintained in memory. When a page is accessed, move the page to the head of the linked list. This will ensure that the page at the end of the linked list is the least visited recently. Because the linked list needs to be updated for each access, the LRU implemented in this way is expensive.


### Deadlock

-   In an operating system, a deadlock occurs when a process or thread enters a waiting state because a requested system resource is held by another waiting process, which in turn is waiting for another resource held by another waiting process. Most operating systems use Ostrich Strategy which means to simply ignore the deadlocks as they don&rsquo;t affect the user.
-   Conditions for deadlock
    -   Mutually exclusive: Each resource has either been allocated to a process or is available.
    -   Hold and wait: A process that has already obtained a certain resource can request a new resource.
    -   Non-preemption: The resource that has been allocated to a process cannot be preempted forcibly, it can only be explicitly released by the process that occupies it.
    -   Circular waiting: There are two or more processes forming a loop, and each process in the loop is waiting for the resources occupied by the next process.


### Critical section problem

-   Concurrent access to shared resources might cause problems so we put those resources in a protected section of our program code which is Critical section.
-   Pseudo Code: Keep threads in wait state -> Execute the current thread -> Release the critical section


### Mutex, Semaphore

1.  Mutex

    -   In the case of mutual exclusion (Mutex), one thread blocks a critical section by using locking techniques when it needs to access the shared resource and other threads have to wait to get their turn to enter into the section. This prevents conflicts when two or more threads share the same memory space and want to access a common resource.[2]

2.  Semaphores

    -   Basically and variable or abstract data type which is used to control access to a common resource by multiple threads.
    -   To enter a critical section, a thread must obtain a semaphore, which it releases on leaving the section. Other threads are prevented from entering the critical section at the same time as the original thread, but are free to gain control of the CPU and execute other code, including other critical sections that are protected by different semaphores.


## Important Topic for DBMS


### Advantage of DBMS

-   Better data integration
-   Minimized Data Inconsistency
-   Faster data Access


### Database Engines

-   Database engines or storage engines or sometimes even called embedded databases is software library that a database management software uses to store data on disk and do CRUD (create update delete). Also called storage engine and embedded database.
-   DBMS can use the engine and build features on top of it like replication, isolation etc.
-   It can be a simple key value store or complex as full support for ACID.


### R-DBMS

1.  All type of Keys

    -   Super Key: a set of attributes that uniquely identifies each tuple of a relation.
    -   Candidate key: Minimized superkey i.e it is any set of columns that have a unique combination of values in each row.
    -   Primary Key: Its the candidate key selected by DBA. Its a a key in a relational database that is unique for each record.
    -   Foreign Key; It refers to primary key of some other table. Its a link between two tables.

2.  Normalization

    -   Database normalization is all about controlling the size of the data as well as preserving its validity.
    -   Database people found that they could reduce the size of their database and avoid data corruption if they simply followed a few rules.
    -   data depends on the key [1NF], the whole key [2NF] and nothing but the key [3NF].
    
    1.  1NF (Atomic Values)
    
        -   First normal form (1NF) says that values in a record need to be atomic and not composed of embedded arrays or some such.
    
    2.  2NF ( Columns Depend On a Single Primary Key)
    
        -   Our task to comply with 2NF means that we need to identify columns that uniquely define the data in our table.
    
    3.  3NF (Non-keys Describe The Key And Nothing Else)
    
        -   3NF says that every non-primary field in our table must describe the primary key field, and no field should exist in our table that does not describe the primary key field.
    
    4.  BCNF (3.5 NF)
    
        -   BCNF acts differently from 3NF only when there are multiple overlapping candidate keys. The reason is that the functional dependency X -> Y is of course true if Y is a subset of X. So in any table that has only one candidate key and is in 3NF, it is already in BCNF because there is no column (either key or non-key) that is functionally dependent on anything besides that key.

3.  How to scale RDBMS

    -   Denormalization
        Denormalization attempts to improve read performance at the expense of some write performance. Redundant copies of the data are written in multiple tables to avoid expensive joins. Some RDBMS such as PostgreSQL and Oracle support materialized views which handle the work of storing redundant information and keeping redundant copies consistent.
        -   Problems
            -   A denormalized database under heavy write load might perform worse than its normalized counterpart.
            -   Its only good for read performance but complex.
    -   Sharding
        -   Sharding distributes data across different databases such that each database can only manage a subset of the data. Taking a users database as an example, as the number of users increases, more shards are added to the cluster.
        -   Similar to Federation, sharding results in less read and write traffic, less replication, and more cache hits.
        -   Problems
            -   You&rsquo;ll need to update your application logic to work with shards, which could result in complex SQL queries.
    -   Federation
        Federation (or functional partitioning) splits up databases by function. For example, instead of a single, monolithic database, you could have three databases: forums, users, and products, resulting in less read and write traffic to each database and therefore less replication lag. Smaller databases result in more data that can fit in memory, which in turn results in more cache hits due to improved cache locality. With no single central master serializing writes you can write in parallel, increasing throughput.
        -   Problems
            -   Federation is not effective if your schema requires huge functions or tables.
            -   You&rsquo;ll need to update your application logic to determine which database to read and write.
            -   Joining data from two databases is more complex with a server link.
            -   Federation adds more hardware and additional complexity.
    -   Master-slave replication
        The master serves reads and writes, replicating writes to one or more slaves, which serve only reads. Slaves can also replicate to additional slaves in a tree-like fashion. If the master goes offline, the system can continue to operate in read-only mode until a slave is promoted to a master or a new master is provisioned.
        -   Problems
            Additional logic to promote slave to master.
    -   Master-master replication
        Both masters serve reads and writes and coordinate with each other on writes. If either master goes down, the system can continue to operate with both reads and writes.
        -   Problems
            -   You&rsquo;ll need a load balancer or you&rsquo;ll need to make changes to your application logic to determine where to write.
            -   Most master-master systems are either loosely consistent (violating ACID) or have increased write latency due to synchronization.
            -   Conflict resolution comes more into play as more write nodes are added and as latency increases.
    -   SQL tuning


### NoSQL

-   NoSQL is a collection of data items represented in a key-value store, document store, wide column store, or a graph database. Data is denormalized, and joins are generally done in the application code. Most NoSQL stores lack true ACID transactions and favor eventual consistency.

-   BASE is often used to describe the properties of NoSQL databases. In comparison with the CAP Theorem, BASE chooses availability over consistency.
    -   Basically available - the system guarantees availability.
    -   Soft state - the state of the system may change over time, even without input.
    -   Eventual consistency - the system will become consistent over a period of time, given that the system doesn&rsquo;t receive input during that period.


### ACID properties

-   ACID-compliance means that you have certain guarantees when writing data in a transaction. In summary form, each transaction will be:
    -   Atomic: This means that a single transaction happens, or it doesn’t. There is no concept of a “partial” transaction
    -   Consistent: Any transaction will bring the database from one valid state to another.
    -   Isolated: One transaction will not affect another if they happen at the same time. Executing transactions concurrently has the same results as if the transactions were executed serially.
    -   Durable: When a transaction concludes it concludes. Nothing can change the data back unless another transaction changes the data back to the way it was. In essence: there is no undo.


### CAP

-   Consistency. The same meaning as with ACID above; the state of the database will change with each transaction.
-   Availability. The distributed system will respond in some way to a request.
-   Partition tolerance. A distributed system relies on a network of some sort to function. If part of that network goes offline (thus “partitioning” the system), the system will continue to operate.


### SQL

1.  Trigger, cursor, view

    1.  View
    
        -   The view is a virtual table, which does not contain data, so it cannot be indexed. The operation on the view is the same as the operation on the ordinary table.
    
    2.  Cursor
    
        -   A stored procedure can be seen as a batch of a series of SQL operations.
            -   The benefits of using stored procedures:
                -   Code encapsulation to ensure a certain degree of security;
                -   Code reuse;
                -   Because it is pre-compiled, it has high performance.
        -   Use the cursor in the stored procedure to move and traverse a result set. The cursor is mainly used in interactive applications, where the user needs to browse and modify any row in the data set.
        -   Four steps to use cursors:
            -   Declare the cursor, this process did not actually retrieve the data;
            -   Open the cursor;
            -   Fetch data;
            -   Close the cursor;
    
    3.  Triggers
    
        -   Triggers are automatically executed when the following statements are executed in a table: DELETE, INSERT, UPDATE.
        -   The trigger must specify whether to execute automatically before or after the statement is executed. Before execution, use the BEFORE keyword and after execution use the AFTER keyword. BEFORE is used for data verification and purification, AFTER is used for audit trail, and the modification is recorded in another table.
        -   Example:
            CREATE TRIGGER mytrigger AFTER INSERT ON mytable
            FOR EACH ROW SELECT NEW.col into @result;
            SELECT @result;

2.  Joins

    -   


### Dirty read problem


### Serializability


### Indexing


## Important Topic for OOPS


### Class and Objects.


### Feature/characteristics of OOPs.


### Abstraction


### This pointer


### Compile time and Runtime poly-morphism.


### Variable scopes.


### static (variables, Functions, Objects).


### Inheritance (Type and Mode)


### Virtual (Functions and Class)


### Abstract class and Interface.


### Friend function and Friend class.


### Call by value, reference.


### Exception Handling


### Constructor and Destructor.


### Copy constructor


### copy assignment operator


### References variable


### Const (variable, Function, Argument)


### Overloading (Function, Constructor, Operator)


### Function overriding and Inline function.


## Important Topic for CN


### OSI layer (Open Systems Interaction)

1.  7 Application Layer

    1.  HTTP or HTTPS
    
        GET / 10.0.0.3.80 HTTP headers, cookies, content-type
        
        -   HTTP is a method for encoding and transporting data between a client and a server. It is a request/response protocol: clients issue requests and servers issue responses with relevant content and completion status info about the request. HTTP is self-contained, allowing requests and responses to flow through many intermediate routers and servers that perform load balancing, caching, encryption, and compression.
        -   It&rsquo;s an application layer protocol that works over TCP.

2.  6 Presentation Layer

    -   Encrypt if necessary.

3.  5 Session Layer

    -   Established session tag.

4.  4 Transport Layer

    -   We can talk to a computer somewhere around the world, but that computer is running lots of different programs. How should it know which one to deliver your message to? The transport layer takes care of this, usually with port numbers.We  tag the big stuff we got from session layer and we attach the source and destination ports to the that slug.

5.  3 Network Layer

    -   Knows that the slug/packet has segments and figures out destination and source IP and attaches it to the slug/packet and blindly sends it to next layer (Data Link)

6.  2 Data Link Layer

    -   Adds the target and destination MAC address to the slug/packet.

7.  1 Physical Layer

    -   We turn the whole slug/pattern into binary and shove them into the wire.
    -   But as electricity/light has no direction so the slug/packet goes everywhere. The layers receive the slug and the data link layer checks if the frame is intended for itself or not. So basically all the nodes in the network are constantly receiving and discarding garbage packets/slugs.

8.  After&#x2026;

    After the node knows the packet was meant for itself then the packet gets simplified and headers are removed by each respective layer that added them and the cycle goes on.


### ipv4 vs ipv6


### subnetting in IP


### Flow vs Error control


# System Design/Engineering


## BackEnd Engineering


### Synchronous, Asynchronous, Multithreaded and Multiprocessor

-   Synchronous means to just block the code until i/o operation or whatever is completed.
-   Multithreading is very hard to write. Putting mutexes on parts of code that might race is hard.
-   Comes, single threaded non blocking i/o (asynchroncity). We just never wait.
    -   We use call me backs (callbacks) and events to just skip through the blocking requests and execute it when its unblocked by emitting events and calling it back.
    -   This can lead to callback hell where callbacks also need callbacks an so on. JS community invented Promises (.then() &#x2026; catch {}). To avoid .then&rsquo;s()s we got async/await as syntactical sugar where we just add await in front of potentially blocking piece of code.
-   Multiprocessing we have each process with each having its own resources and stuff so we can use IPC to communicate between processes.


### Stateless v Stateful

<table border="2" cellspacing="0" cellpadding="6" rules="groups" frame="hsides">


<colgroup>
<col  class="org-left" />

<col  class="org-left" />
</colgroup>
<thead>
<tr>
<th scope="col" class="org-left">Statefull</th>
<th scope="col" class="org-left">Stateless</th>
</tr>
</thead>

<tbody>
<tr>
<td class="org-left">&#xa0;</td>
<td class="org-left">&#xa0;</td>
</tr>
</tbody>
</table>


### Web Servers

-   How web servers work?
-   Dynamic vs Static Content
-   E-Tags
-   HTTP protocol
-   Relational vs NoSQL
-   ACID


### Communication Protocol

1.  TCP,UDP

    1.  TCP (Transmission Control Protocol)
    
        -   It&rsquo;s a connection oriented stream over an IP network.
        -   It&rsquo;s stateful. It stores state for ex. when we get disconnected we can resume from where we were because we know that internet will always route to the same host.
        -   Connection is established and terminated using a handshake.
        -   Data from sever is guaranteed to reach the target/client in correct order.
            -   To ensure correct order it uses,
                -   Sequence numbers and checksum fields for each packet
                -   Acknowledgement of the packets and if they ain&rsquo;t right then re-transmit the whole thing. If we are facing multiple re-transmission (timeouts) it just drops it and quits.
        -   It can cause time delays and is less efficient than UDP.
        -   Used for HTTP, HTTPs, FTP, SMTP, and Telnet.
        -   Node JS Example
        
            const net = require("net")
            
            const server = net.createServer(socket => {
                socket.write("Hello.")
                socket.on("data", data => {
                    console.log(data.toString())
                })
            })
            
            server.listen(8080)
    
    2.  UDP
    
        -   It&rsquo;s a connection-less protocol.
        -   Communication is datagram oriented. The integrity is guaranteed only on the single datagram.
        -   Datagrams can arrive out of order or maybe can&rsquo;t even arrive at all.
        -   It is more efficient than TCP because it uses non ACK. It&rsquo;s generally used for real time communication, where a little percentage of packet loss rate is preferable to the overhead of a TCP connection.
        -   UDP is used for DNS and DHCP.
            -   DHCP
                The Dynamic Host Configuration Protocol (DHCP) is a network management protocol used on Internet Protocol (IP) networks for automatically assigning IP addresses and other communication parameters to devices connected to the network using a client–server architecture.
                UDP is used as there is no IP address yet and TCP need IP to work and make sense.
            -   DNS
                It translates more readily memorized domain names to the numerical IP addresses needed for locating and identifying computer services and devices with the underlying network protocols.
        -   Pros and Cons
            
            <table border="2" cellspacing="0" cellpadding="6" rules="groups" frame="hsides">
            
            
            <colgroup>
            <col  class="org-left" />
            
            <col  class="org-left" />
            </colgroup>
            <thead>
            <tr>
            <th scope="col" class="org-left">Pros</th>
            <th scope="col" class="org-left">Cons</th>
            </tr>
            </thead>
            
            <tbody>
            <tr>
            <td class="org-left">Smaller Packets</td>
            <td class="org-left">No Acknowledgement</td>
            </tr>
            
            
            <tr>
            <td class="org-left">Less Bandwidth</td>
            <td class="org-left">No Guaranteed Delivery</td>
            </tr>
            
            
            <tr>
            <td class="org-left">Faster</td>
            <td class="org-left">Connectionless</td>
            </tr>
            
            
            <tr>
            <td class="org-left">Stateless</td>
            <td class="org-left">No Congestion Control</td>
            </tr>
            
            
            <tr>
            <td class="org-left">&#xa0;</td>
            <td class="org-left">No Ordered Packets</td>
            </tr>
            
            
            <tr>
            <td class="org-left">&#xa0;</td>
            <td class="org-left">Security</td>
            </tr>
            </tbody>
            </table>
        
        -   Node JS Example
        
            const dgram = require('dgram');
            const socket = dgram.createSocket('udp4');
            
            socket.on('message', (msg, rinfo) => {
                console.log(`server got: ${msg} from ${rinfo.address}:${rinfo.port}`);
            });
            
            socket.bind(8081);

2.  HTTP

    -   Layer 7 Protocol
    -   For Client/Server Model
    -   Built on TCP.
    -   Stateless
    -   HTTP Request
        
        <table border="2" cellspacing="0" cellpadding="6" rules="groups" frame="hsides">
        
        
        <colgroup>
        <col  class="org-left" />
        </colgroup>
        <thead>
        <tr>
        <th scope="col" class="org-left">URL</th>
        </tr>
        </thead>
        
        <tbody>
        <tr>
        <td class="org-left">Method Type</td>
        </tr>
        </tbody>
        
        <tbody>
        <tr>
        <td class="org-left">Headers</td>
        </tr>
        </tbody>
        
        <tbody>
        <tr>
        <td class="org-left">Body</td>
        </tr>
        </tbody>
        </table>
    -   HTTP Response
        
        <table border="2" cellspacing="0" cellpadding="6" rules="groups" frame="hsides">
        
        
        <colgroup>
        <col  class="org-left" />
        </colgroup>
        <thead>
        <tr>
        <th scope="col" class="org-left">Status Codes</th>
        </tr>
        </thead>
        
        <tbody>
        <tr>
        <td class="org-left">Headers</td>
        </tr>
        </tbody>
        
        <tbody>
        <tr>
        <td class="org-left">Body</td>
        </tr>
        </tbody>
        </table>
    -   HTTP status codes
        -   2xx, if everything was okay,
        -   3xx, if the resource was moved,
        -   4xx, if the request cannot be fulfilled because of a client error (like requesting a resource that does not exist),
        -   5xx, if something went wrong on the API side (like an exception happened).
    
    1.  HTTP 1.1
    
        -   Browsers can make multiple TCP connection with the server.
        -   This becomes a problem when we need to request a lot of resources.
    
    2.  HTTP/2
    
        -   We use a single TCP socket efficiently and put all the requests in that socket its called multiplexing.
        -   We tag the packets with stream ID.
        -   After the server gets the result and shove all the responses in the same TCP socket back to client.
        -   It also supports server push where you make a single request and server returns multiple responses based on the resourse requested and the files its based on or links to.

3.  WebSockets

    -   Made for HTTP 1.1 as opening and closing connections everytime was bad (in HTTP 1.0) so, websockets just make a connection and keeps it open.
    -   It&rsquo;s bidirectional.
    -   It&rsquo;s stateful (as client and server are aware of each other).
    -   Used in chatting, live feed, multiplayer game etc.
    -   Pros and Cons
        
        <table border="2" cellspacing="0" cellpadding="6" rules="groups" frame="hsides">
        
        
        <colgroup>
        <col  class="org-left" />
        
        <col  class="org-left" />
        </colgroup>
        <thead>
        <tr>
        <th scope="col" class="org-left">Pros</th>
        <th scope="col" class="org-left">Cons</th>
        </tr>
        </thead>
        
        <tbody>
        <tr>
        <td class="org-left">Full Duplex</td>
        <td class="org-left">Proxing is tricky</td>
        </tr>
        
        
        <tr>
        <td class="org-left">HTTP Compatible</td>
        <td class="org-left">L7 L/B is hard</td>
        </tr>
        
        
        <tr>
        <td class="org-left">Firewall Friendly</td>
        <td class="org-left">Stateful (difficult to h. scale)</td>
        </tr>
        </tbody>
        </table>

4.  QUIC

    -   UDP Protocol (made by google) for HTTP 3

5.  gRPC

    -   Client libraries (HTTP for java, c# etc) for each programming language to know about the protocol they communicate in sucks. So Google made one library for all popular languages.
    -   Protocol: HTTP/2 (hidden implementation) so as to make for east upgrades.
    -   Message format: Protocol Buffers as format as they are language agnosti.


### Message Formats (JSON, protobuf)

-   JSON & protobuf


### Message queue, Pub/Sub

1.  Pub Sub Model

    Youtube upload example
    
                                           ┌─────────────┐      ┌────────────────────┐
                                           │  Raw Video  ├──────┼─►                  │
                                           │             │      │  Compress Service  │
                                           │ Compressed ◄├──────┼┐            │
    ┌──────────┐                           │             │      └┴───────────────────┘
    │          │                           │             │
    │          │      ┌───────────────┐    │             │
    │          │      │               ├────┤►            │       ┌───────────────────┐
    │  Client  ├──────► Upload Service│    │             ◄───────┤                   │
    │          │      │               ◄────┤             ┌───────┼─► Format Service  │
    │          ◄──────┤               │    │             │       │                   │
    └──────────┘      └───────────────┘    │             │       └───────────────────┘
                                           │             │
                                           │             │
                                           │             │       ┌──────────────────────┐
                                           │             │       │                      │
                                           │            ◄├───────┼─                     │
                                           │             │       │ Notification Service │
                                           │            ─┼───────┼─►                    │
                                           │MessageQueue │       └──────────────────────┘
                                           └─────────────┘
    
    Client can leave after the video is in queue and rest of services will do their stuff without interacting with each other and keep adding stuff to the queue
    
    -   Pros and Cons of Pub Sub
        
        <table border="2" cellspacing="0" cellpadding="6" rules="groups" frame="hsides">
        
        
        <colgroup>
        <col  class="org-left" />
        
        <col  class="org-left" />
        </colgroup>
        <thead>
        <tr>
        <th scope="col" class="org-left">Pros</th>
        <th scope="col" class="org-left">Cons</th>
        </tr>
        </thead>
        
        <tbody>
        <tr>
        <td class="org-left">Scales w/ multiple receivers</td>
        <td class="org-left">Message delivery issues (Byzantine General Problem)</td>
        </tr>
        
        
        <tr>
        <td class="org-left">Great for microservices</td>
        <td class="org-left">Complexity</td>
        </tr>
        
        
        <tr>
        <td class="org-left">Loose Coupling</td>
        <td class="org-left">Network Saturation</td>
        </tr>
        
        
        <tr>
        <td class="org-left">Works when client is not running</td>
        <td class="org-left">&#xa0;</td>
        </tr>
        </tbody>
        </table>
    -   Where Request Response model fails?
        -   Client Server Model Make a request -> do something -> get something from request -> do something with it
        -   Chaining multiple services leads to risk of breaking everything
            Client -> Upload Service -> Compress Service -> Notification Service (one fails all fails)
        -   Also its hard to add new services.
    -   Request Response Pros and Cons
        
        <table border="2" cellspacing="0" cellpadding="6" rules="groups" frame="hsides">
        
        
        <colgroup>
        <col  class="org-left" />
        
        <col  class="org-left" />
        </colgroup>
        <thead>
        <tr>
        <th scope="col" class="org-left">Pros</th>
        <th scope="col" class="org-left">Cons</th>
        </tr>
        </thead>
        
        <tbody>
        <tr>
        <td class="org-left">Elegant Simple</td>
        <td class="org-left">Bad for multiple receivers</td>
        </tr>
        
        
        <tr>
        <td class="org-left">Stateless(HTTP)</td>
        <td class="org-left">High Coupling (Services know each other)</td>
        </tr>
        
        
        <tr>
        <td class="org-left">Scalable (Horizontal)</td>
        <td class="org-left">Client and Server have to be running</td>
        </tr>
        
        
        <tr>
        <td class="org-left">&#xa0;</td>
        <td class="org-left">Chaining and circuit break</td>
        </tr>
        </tbody>
        </table>
    -   When to use PubSub messaging first queue.

2.  Message Queue

    1.  RabbitMQ
    
        -   Overview
                   <span class="underline"><span class="underline"><span class="underline"><span class="underline"><span class="underline"><span class="underline"><span class="underline"><span class="underline">\_\_</span></span></span></span></span></span></span></span>                        <span class="underline"><span class="underline"><span class="underline"><span class="underline"><span class="underline"><span class="underline"><span class="underline"><span class="underline"><span class="underline">\_</span></span></span></span></span></span></span></span></span>
            Pub   O\_\_\_\_<sub>Channel</sub>\_\_\_\_\_<sub>O</sub>  Rabbit MQ Server    O\_\_<sub>Channel</sub>\_\_\_\_\_\_\_\_<sub>O</sub> Consumer
                                    Exchange Queue
        -   Publisher Client


### Key Value Store

1.  Redis: In memory key-value store NoSQL DB

    -   Key is a string and value can be anything.
    -   Redis is in memory first (RAM).
    -   Single threaded (except when durability is enables).
    -   Optional Durability.
        -   Journaling
        -   Snapshotting
    -   Replication/Clustering
        -   One Leader, Many followers model.
        -   Clustering: Shard data across multiple nodes.


### Proxies (Reverse Proxies, Load balancer)

-   What is difference between Proxy vs Reverse Proxy
-   Layer 7 Proxy vs Layer 4 Proxy
-   Reverse Proxy applications
-   Load Balancing algorithms


### REST

-   Representational
-   State Transfer?
    Stateless: All the stuff all the time. (REST)
    
    -   Slow
    -   But Scales
    
    Statefull: We know state beforehand.

1.  Constraints

    -   Client/Server
    -   Stateless
    -   Cachebility
    -   Layer Systems
    -   Uniform Interface

2.  Why GraphQL?

    -   Rest is not specific and gives out too much data in request.


### Caching

-   When to use Caching


### Microservices

<table border="2" cellspacing="0" cellpadding="6" rules="groups" frame="hsides">


<colgroup>
<col  class="org-left" />

<col  class="org-left" />
</colgroup>
<thead>
<tr>
<th scope="col" class="org-left">Pros</th>
<th scope="col" class="org-left">Cons</th>
</tr>
</thead>

<tbody>
<tr>
<td class="org-left">Polyglot architecture**</td>
<td class="org-left">Very Complicated</td>
</tr>


<tr>
<td class="org-left">Easy Scaling*</td>
<td class="org-left">&#xa0;</td>
</tr>
</tbody>
</table>

\`\*\` -> Because we scale only those microservices which need scaling.
\`\*\*\`-> Each microservice can have its own DB or programming language etc.


### Web Frameworks (API authoring)

-   Express, Django, Node JS


### Security

-   TLS, Encryption, Firewalls


### Containers

1.  Docker

    -   


## Testing


### Unit Testing


### Functional Testing


### Integration Testing

