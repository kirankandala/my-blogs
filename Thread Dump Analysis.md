## How to perform Java/JVM Thread Dump Analysis


Use "top" command using. If its a java application process that is using CPU and there is no work being done by the application, its a possibility that there is thread leak in the application/server.


Use the following steps to identify the threads potentially causing the high CPU usage and use the information of each thread to perform thread dump analysis.

 
```
Command:

$ ps –ef | grep java
```
 

Take the PID from above command’s output and run the following using that <PID> in the below commands (works on Linux OS).

 
```
Command:

$ ps -eLo pid,lwp,nlwp,ruser,pcpu,stime,etime,args | grep <PID> > java_threads_info.txt

Example:

$ ps -eLo pid,lwp,nlwp,ruser,pcpu,stime,etime,args | grep 26145  > java_threads_info.txt
```
 

output:

26145 32533  852 app_user 0.0 Oct24    19:26:56 /apps/java/JDK64_1.7.0.55.0.01/bin/java -Xbootclasspath/a:...

26145 32535  852 app_user 0.0 Oct24    19:26:54 /apps/java/JDK64_1.7.0.55.0.01/bin/java -Xbootclasspath/a:...

26145 32583  852 app_user 0.0 Oct24    19:26:54 /apps/java/JDK64_1.7.0.55.0.01/bin/java -Xbootclasspath/a:...

26145 26465  852 app_user 51.4 Oct24    19:41:34 /apps/java/JDK64_1.7.0.55.0.01/bin/java -Xbootclasspath/a:...

26145 26466  852 app_user 51.9 Oct24    19:41:34 /apps/java/JDK64_1.7.0.55.0.01/bin/java -Xbootclasspath/a:...

 

Identify lwd (second column) for threads that are consuming higher percentage of CPU. Lwd is in binary format convert that to hexadecimal.

http://www.binaryhexconverter.com/decimal-to-hex-converter

 

For example 26465's hexadecimal value is 0x6761. Search for 0x6761 in the thread dump

 

One of the commands to take the thread dump

 
```
Command:

$ jstack  -l <PID> > java_thread_dump_lst.txt

Example:

jstack -l 26145 java_thread_dump_lst.txt
```