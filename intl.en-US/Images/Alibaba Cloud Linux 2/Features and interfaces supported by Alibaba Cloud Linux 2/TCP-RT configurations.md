# TCP-RT configurations

TCP-layer service monitoring \(TCP-RT\) is supported on Alibaba Cloud Linux 2 with a kernel version of 4.19.91-21.al7 or later.

TCP-RT is a TRACE method. TCP-RT allows you to configure event tracking in a kernel-based TCP stack to identify a request and response when a single connection carries only one concurrent request and response. You can then obtain information such as the amount of time taken to receive the request in the TCP stack and the amount of time taken to process the task. In addition, TCP-RT supports statistical analysis in the kernel system and generates statistical information of specified connections on a regular basis.

TCP-RT applies to services that have only one concurrent request and response on a single connection, such as HTTP/1.1 web services, MySQL database services, and Redis services.

## Principle

The following figure shows different phases when TCP-RT is used for a connection that carries one concurrent request and response.

![TCP-RT](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/0527703061/p146983.png)

In the preceding figure, the Nth request sent by the client to the server is displayed as ReqN. The request is composed of the `ReqN-1` and `ReqN-2` data packets. The time when the server receives the first packet is recorded as T0, and the time when the second packet is received is recorded as T1. The server processes the received request and sends the `RspN-1` and `RspN-2` response packets to the client. The time when the server sends the first response packet is recorded as T2. When the client receives a response packet, it sends an acknowledgement \(ACK\) message to the server. The time when the server receives the last ACK message is recorded as T3.

TCP-RT helps obtain the following information based on the recorded points in time:

-   upload\_time: the amount of time taken to upload the request.
-   process\_time: the amount of time taken by the server to process the request. It is the period that starts when the server receives the last data packet of the request and ends when the server sends the first response packet to the client.
-   download\_time: the amount of time taken to download the data. It is the period that starts when the server sends the first response packet to the client and ends when the server receives the last ACK message from the client. This time period is essential for downloading large amounts of response data.

## Information output

TCP-RT collects and generates the parameters related to the TCP service in kernel mode. The following table describes the output modes and output time of files.

|File type|Output mode|Output time|
|---------|-----------|-----------|
|Log file|Log files are the `rt-network-log*` files generated in the /sys/kernel/debug/tcp-rt directory by using debugfs. Log files have the following features:-   The suffix of the file name is the serial number of the CPU core. For example, the log files generated by a 32-core server are named from `rt-network-log0` to `rt-network-log31`.
-   Each log file can be up to 2 MB in size. When a log file exceeds this size limit, previous data is deleted.
-   Log files generated by using debugfs are one-time files. After the data of a one-time file is read for the first time, the data is deleted.

|-   Time 1: For a TCP connection, information about the previous task is generated when the previous task is complete and the next task starts. A task consists of a request and response.
-   Time 2: Information is generated when the connection is closed.

After data is generated by TCP-RT, the application layer can immediately read the data. |
|stats file|stats files are the `rt-network-stats` files generated in the /sys/kernl/debug/tcp-rt directory. These files are generated based on the server port- or client port-related statistics.|stats files are generated at regular intervals. By default, data is generated once a minute.|

## Output formats

This section describes the formats of generated log files and stats files.

**Note:** Definitions of task and TCP connection lifecycle:

-   A task consists of a request and response.
-   The TCP connection lifecycle includes multiple tasks.

-   Log file format

    In log files, each column of a record corresponds to a different set of information. The following figure shows an example of a log file.

    ![log file](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/0527703061/p135930.png)

    The following section describes the parameters from left to right as recorded in the figure:

    -   The version number. In this example, the version number is V6.
    -   The scenario record identifier. The identifiers are divided into five types: R, E, W, N, and P.
        -   R: An R record is generated for the TCP connection when a request is received by the local server and a response is sent.
        -   E: An E record is generated when the connection is closed.
        -   W: A W record is generated when the connection is closed during data transmission.
        -   N: An N record is generated when the connection is closed during data reception.
        -   P: A P record is generated for the TCP connection when the local server sends a request to the peer server and a response is sent.
    -   The seconds part of the start time of the task.
    -   The microseconds part of the start time of the task.
    -   The peer IP address of the TCP connection.
    -   The peer port of the TCP connection.
    -   The local IP address of the TCP connection.
    -   The local port of the TCP connection.
    The preceding parameters are followed by different parameters in different scenarios. The following table describes the parameters.

    |Scenario record identifier|Parameter|
    |--------------------------|---------|
    |R|An R record records the normal startup and shutdown of a task. Each TCP connection can have multiple R records.    -   The amount of data sent by the task. Unit: bytes.
    -   The duration of the task. The interval between the time when the TCP service receives a request from the client and the time when the ACK message is received from the client. Unit: μs.
    -   The minimum TCP Round Trip Time \(RTT\) for the task. Unit: μs.
    -   The number of TCP segments retransmitted by the task.
    -   The sequence number of the task. After a TCP connection is established, the sequence number of the first task is 1.
    -   The latency of the task. The interval between the time when `the last request segment is received` and the time when `the first response segment is sent`. Unit: μs.
    -   The latency for sending request segments of the task. The interval between the time when `the first request segment is received` and the time when `the last request segment is received`. Unit: μs.
    -   The amount of data received by the task. Unit: bytes.
    -   Indicates whether unordered reception occurs during the task. A value of 1 indicates that unordered reception occurred, and a value of 0 indicates that unordered reception did not occur.
    -   The TCP maximum segment length \(MSS\) used during the task. Unit: bytes. |
    |P|A P record records the normal startup and shutdown of a task. Each TCP connection can have multiple P records. P is a new record type in V6 and indicates the request information from the client. This record is available only when the `pports` or `pports_range` option is configured.    -   The amount of data sent by the task. Unit: bytes.
    -   The duration of the task. The task starts when the local server sends data and ends when it receives the last response from the peer end. Unit: μs.
    -   The minimum TCP RTT for the task. Unit: μs.
    -   The number of TCP segments retransmitted by the task.
    -   The sequence number of the task. After a TCP connection is established, the sequence number of the first task is 1.
    -   The service time of the task. It is the period that starts when the request is sent and ends when the first response is received. Unit: μs.
    -   The time for receiving responses of the task. The time period is from the time when the first response packet is received to the time when the last response packet is received. Unit: μs.
    -   The total size of the response packets received by the task. Unit: bytes.
    -   Indicates whether unordered reception occurs during the task. A value of 1 indicates that unordered reception occurred, and a value of 0 indicates that unordered reception did not occur.
    -   The TCP MSS used during the task. Unit: bytes. |
    |E|An E record records information of a closed TCP connection. Each closed TCP connection has an E record. TCP connections that have the `pports` or `pports_range` option configured also have this record.    -   The sequence number of the last task.
    -   The amount of data sent during the TCP connection lifecycle. Unit: bytes.
    -   The amount of data for which the TCP connection has sent a response but has not received the ACK message. A value of 0 indicates that no such data exists. Unit: bytes.
    -   The amount of data received during the TCP connection lifecycle. Unit: bytes.
    -   The number of TCP segments retransmitted by the task.
    -   The minimum TCP RTT in the TCP lifecycle. Unit: μs. |
    |N|An N record records information about a TCP connection that is closed when the task is requesting response segments. Each TCP connection may have one N record or none.    -   The sequence number of the last task.
    -   The duration of the last task. Only the receiving time is available because the connection is closed. Unit: μs.
    -   The amount of data received during the TCP connection lifecycle. Unit: bytes.
    -   Indicates whether unordered reception occurs during the task. A value of 1 indicates that unordered reception occurred, and a value of 0 indicates that unordered reception did not occur.
    -   The TCP MSS used during the task. Unit: bytes. |
    |W|A W record records information about a TCP connection that is closed when the task is responding to request segments. Each TCP connection may have one W record or none.    -   The amount of data sent in the response of the last task. Unit: bytes.
    -   The duration of the last task. The receiving time is incomplete because the connection is closed. Unit: μs.
    -   The minimum TCP RTT for the last task. Unit: μs.
    -   The number of TCP segments retransmitted by the last task.
    -   The sequence number of the last task.
    -   The latency of the last task. Unit: μs.
    -   The latency for sending request segments of the last task. Unit: μs.
    -   The amount of data for which the last task has sent a response but has not received the ACK message. A value of 0 indicates that no such data exists. Unit: bytes.
    -   The TCP MSS used during the last task. Unit: bytes. |

    **Note:** If the connection was closed normally, the `snd_netx` value is deducted from the amount of data sent by the last task. However, if the connection was closed in an abnormal way, for example, if a reset packet is received after a task is complete and the connection is ended by responding to `reset`, the actual amount of data sent is one byte larger than the displayed value. Typically, parameter values related to the amount of data sent by a task are correct, and the margin of error is only one byte.

-   stats file format

    The following section describes the parameters from left to right as recorded:

    -   The timestamp encoded at the time the record was generated.
    -   The reserved field. Valid value: `all`.
    -   The port number.
    -   The average value of the total time taken by tasks recorded in R records.
    -   The average latency of tasks recorded in R records.
    -   The number of packets lost per 1,000 packets.
    -   The average value of RTT. Unit: μs.
    -   The number of tasks closed per 1,000 tasks in the data sent by requests.
    -   The average size of data sent by tasks.
    -   The average latency for sending requests of tasks.
    -   The average amount of data received by tasks.
    -   The number of tasks.

## Operations

You can perform the following operations to use TCP-RP:

1.  Load and configure a module.

    You can use one of the following methods to load and configure a module:

    -   Configure the parameters when you load the module.

        Example: `modprobe tcp_rt lports=80`.

    -   Load the module and then configure the parameters.
        1.  Run the following command to load the module:

            ```
            modprobe tcp_rt
            ```

        2.  After the module is loaded, run the command and configure the parameters in the /sys/module/tcp\_rt/parameters/ directory. The following table describes the parameters and corresponding command configurations.

            |Parameter|Description|Default value|Command configurations|
            |---------|-----------|-------------|----------------------|
            |stats|Specifies whether to generate stats files. A value of 0 indicates that stats files are generated, and 1 indicates that stats files are not generated.|0|`echo 0 > stats`|
            |stats\_interval|The interval at which to generate stats files. Unit: seconds.|60|`echo 60 > stats_interval`|
            |lports|The local server ports for collecting data. A maximum of six ports can be specified.|None|`echo 80,800,8080 > lports`|
            |pports|The peer port of the TCP connection.|None|`echo 80,800,8080 > pports`|
            |lports\_range|The local server port range. Set a start port number and end port number to specify a port range. Examples: port 80 to port 200, and port 1000 to port 2000.|None|`echo 80,100,1000,2000 >lports_range`|
            |pports\_range|The peer port range of the TCP connection. Set a start port number and end port number to specify a port range. Examples: port 80 to port 200, and port 1000 to port 2000.|None|`echo 80,100,1000,2000 >pports_range`|
            |log\_buf\_num|The maximum size of a log file. `log_buf_num * 256 k` indicates that the maximum size is 256 KB. This parameter can be configured only when you load the module.|8|`modprobe tcp_rt log_buf_num=10`|
            |stats\_buf\_num|The maximum size of a stats file. `stats_buf_num * 16 k` indicates that the maximum size is 16 KB. This parameter can be configured only when you load the module.|8|`modprobe tcp_rt stats_buf_num=10`|

2.  Uninstall the module.

    1.  Run the following command to disable the `tcp-rt` module to make sure that no new connections use TCP-RT:

        ```
        echo 1 > /sys/kernel/debug/tcp-rt/deactivate
        ```

    2.  Run the following command to make sure that no connections are using the `tcp-rt` module.

        ```
        lsmod
        ```

        If `Used by` in the `tcp-rt` module is displayed as `0`, no connections are using the `tcp-rt` module.

    3.  When no connections are using the `tcp-rt` module, run the following command to uninstall the `tcp-rt` module.

        ```
        rmmod tcp_rt
        ```


