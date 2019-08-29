# cosbench
performance test for tencent cos

-- Function Description:
    This tool is mainly used to perform the performance test function of the object storage system. It can automatically generate the defined test data size for object upload and download, which is excellent in performance and execution efficiency.
    Customize the number of concurrently configured and uploaded objects, customize the test business interface, and currently support most object interface operations.
    The tool is adapted to the Huawei cloud object storage service interface. If other object interfaces are connected, the appropriate interface header field modification can be adapted.
    Is the best tool for performance testing verification in the industry.
    
1. The following operations are supported.
                 100:'ListUserBuckets', 
                 101:'CreateBucket', 
                 102:'ListObjectsInBucket', 
                 103:'HeadBucket', 
                 104:'DeleteBucket', 
                 105:'BucketDelete',
                 106:'OPTIONSBucket',
                 111:'PutBucketVersioning',
                 112:'GetBucketVersioning',
                 141:'PutBucketWebsite',
                 142:'GetBucketWebsite',
                 143:'DeleteBucketWebsite',
                 151:'PutBucketCORS',
                 152:'GetBucketCORS',
                 153:'DeleteBucketCORS',
                 161:'PutBucketTag',
                 162:'GetBucketTag',
                 163:'DeleteBucketTag',
                 164:'PutBucketLog',
                 165:'GetBucketLog',
                 167:'PutBucketStorageQuota',
                 168:'GetBucketStorageQuota',
                 170:'PutBucketAcl',
                 171:'GetBucketAcl',
                 173:'PutBucketPolicy',
                 174:'GetBucketPolicy',
                 175:'DeleteBucketPolicy',
                 176:'PutBucketLifecycle',
                 177:'GetBucketLifecycle',
                 178:'DeleteBucketLifecycle',
                 179:'PutBucketNotification',
                 180:'GetBucketNotification',
                 182:'GetBucketMultiPartsUpload',
                 185:'GetBucketLocation',
                 188:'GetBucketStorageInfo',
                 201:'PutObject',
                 202:'GetObject',
                 203:'HeadObject',
                 204:'DeleteObject',
                 205:'DeleteMultiObjects',
                 206:'CopyObject',
                 207:'RestoreObject',
                 211:'InitMultiUpload',
                 212:'UploadPart',
                 213:'CopyPart',
                 214:'CompleteMultiUpload',
                 215:'AbortMultiUpload',
                 216:'MultiPartsUpload',
                 217:'GetObjectUpload',
                 218:'PutObjectAcl',
                 219:'GetObjectAcl',
                 221:'OptionsObject',
                 226:'PostObject',
                 900:'MixOperation'

2. Support long and short connections.
     3. Support domain name and IP two ways to request.
     4. Support HTTP/HTTPs configuration in two ways.
     5. Support MD5 value when uploading and downloading objects.
     6. Support random object name and random object size when uploading objects.
     7. Performance result statistics function: including real-time statistical results: including online concurrent numbers, error rate, TPS, throughput, and performance data in a real-time period.
-- Installation & Configuration /INSTALL&CONFIG:
     1. Requires Python environment 2.6.x or 2.7.x. To test the HTTPS encryption algorithm of TSL1.1 and above, you need >=2.7.9
     2. If you want to use the DNS domain name request method, you need to:
      a) The domain name cache service is not running on the execution machine of the program. Otherwise, the request is sent to the same OSC. The query closes the nscd method: service nscd status|stop
      b) The configured domain name can be parsed normally. Configure the dns server in /etc/resolv.conf.
      
      
-- Run /HOW TO RUN:

     1. Create a test account:
        To configure AK SK authentication, you need to construct a test account in the users.dat file in the following format for the tool to read. (1 or more, configured as needed)
      accountName,accessKey,secretKey,
      accountName1,accessKey1,secretKey1,
      accountName2,accessKey2,secretKey2,
      ...
	2. Edit config.dat to configure the test model
	
	3. Run, you can specify parameters, the specified parameters override the parameters in the configuration file
     ./run.py [test case number] [number of users] [specify loaded configuration file]
    
	4. View the results, directory ./result/:
	  example:
	  2013.12.05_06.14.50_HeadBucket_200_brief.txt indicates the final test result of the 200 user concurrent HeadBucket operation.
      2013.12.05_06.14.50_HeadBucket_200_detail.csv indicates the detailed result of all requests of 200 users concurrent HeadBucket operation.
      Archive.csv The result of the archive after each execution.
        ProcessId,UserName,RequestSq,Operation,Start_At,End_At,Latency(s),DataSend(Bytes),DataRecv(Bytes),Mark,RequestID,Response
        0,zz.account.0,1,ListUserBuckets,1394000293.383760,1394000293.409535,0.025775,0,500,,D4B110AFF9760000014490D9C2E4AB2B,200 OK
    2014.03.05_06.18.13_MixOperation_2_realtime.txt indicates the performance statistics of the sampling period of 2 users concurrently with the MixOperation operation and real-time interval of 5 seconds (configurable parameter StatisticsInterval).
	
     NO      StartTime           OK          Requests    ErrRate(%)  TPS       AvgLatency（S）   SendBytes        RecvBytes
     1       03/05 06:18:13.382  279         279         0.0         55.8      0.037           173195           100000
     2       03/05 06:18:18.382  75          75          0.0         15.0      0.13            180061           0
     3       03/05 06:18:23.382  86          86          0.0         17.2      0.116           229280           0	

Other instructions:
     1. There are successive dependencies between requests. If you upload an object, you need to run the creation bucket before.
     2. The tool prints the log file log/obsPyTool.log. The log level can be configured in the logging.conf configuration file:
         Optional level: DEBUG (details for all requests), WARNING (>=400 request log), ERROR (>=500 request)
     3. Error code description:
            'connection reset by peer': '9998', # connection class error: server refused to connect
            'broken pipe': '9997', # rupture of the connecting pipe during reading and writing
            'timed out': '9996', # server and other server side response timeout, time configuration parameter ConnectTimeout
            'badstatusline': '9995', # Client read HTTP response code format error or read empty, common on server side disconnect
            'connection timed out': '9994', # request connection connection establishment timeout
            'the read operation timed out': '9993', # read response timeout from the server side
            'cannotsendrequest': '9992', # client sends a request error
            'keyboardinterrupt': '9991', # Keyboard Ctrl+C interrupt request
            'name or service not known': '9990', # server-side domain name cannot be resolved
            'no route to host': '9989', # to server-side IP unreachable, routing error
            'data error md5': '9901', # Download object data validation error, or the data length is incorrect
            'data error content-length': '9902', # The length of the received message is inconsistent with the content-length header field value returned by the server.
            'other error': '9999' # Other errors, refer to the tool log stack location. Search for stack keywords directly.
-- Update instructions / UPDATES:
2018.2.1
1, increase the segment repeat upload function

2018.1.29
1, increase anonymous access

2018.1.16
1, increase the time window limit function

2018.1.12
1, modify the handle_from_obj_v bug
2. Modify the bug that the virtual host does not take effect.

2018.1.8
1, the detail file increases the x-amz-id2 header field information printing
2, the log finally result increases x-amz-id2 header field information printing

2017.11.24
1. Fix 205 batches to delete bugs

2017.11.06
1. Add a new bucket, upload an object, initialize a multi-session task, and cool the header field.

2017.11.03
1. Optimize distributed data collection methods, no longer count realtime

2017.10.18
1. Add the current tool running directory to the statistics.

	
============================chinese spec==========================================================================      

-- 功能说明：
   本工具主要为进行对象存储系统的性能测试功能机，能够自动产生定义的测试数据大小进行对象上传 下载，在性能和执行效率上俱佳。
   自定义配置并发数和上传对象数，自定义测试业务接口，当前支持绝大多数 对象接口操作，
   工具适配华为云 对象存储服务接口， 若对接其他对象接口，做适度接口头域修改就可适配。
   是业内进行性能测试验证的最佳工具.

   1. 支持如下操作。
                 100:'ListUserBuckets', 
                 101:'CreateBucket', 
                 102:'ListObjectsInBucket', 
                 103:'HeadBucket', 
                 104:'DeleteBucket', 
                 105:'BucketDelete',
                 106:'OPTIONSBucket',
                 111:'PutBucketVersioning',
                 112:'GetBucketVersioning',
                 141:'PutBucketWebsite',
                 142:'GetBucketWebsite',
                 143:'DeleteBucketWebsite',
                 151:'PutBucketCORS',
                 152:'GetBucketCORS',
                 153:'DeleteBucketCORS',
                 161:'PutBucketTag',
                 162:'GetBucketTag',
                 163:'DeleteBucketTag',
                 164:'PutBucketLog',
                 165:'GetBucketLog',
                 167:'PutBucketStorageQuota',
                 168:'GetBucketStorageQuota',
                 170:'PutBucketAcl',
                 171:'GetBucketAcl',
                 173:'PutBucketPolicy',
                 174:'GetBucketPolicy',
                 175:'DeleteBucketPolicy',
                 176:'PutBucketLifecycle',
                 177:'GetBucketLifecycle',
                 178:'DeleteBucketLifecycle',
                 179:'PutBucketNotification',
                 180:'GetBucketNotification',
                 182:'GetBucketMultiPartsUpload',
                 185:'GetBucketLocation',
                 188:'GetBucketStorageInfo',
                 201:'PutObject',
                 202:'GetObject',
                 203:'HeadObject',
                 204:'DeleteObject',
                 205:'DeleteMultiObjects',
                 206:'CopyObject',
                 207:'RestoreObject',
                 211:'InitMultiUpload',
                 212:'UploadPart',
                 213:'CopyPart',
                 214:'CompleteMultiUpload',
                 215:'AbortMultiUpload',
                 216:'MultiPartsUpload',
                 217:'GetObjectUpload',
                 218:'PutObjectAcl',
                 219:'GetObjectAcl',
                 221:'OptionsObject',
                 226:'PostObject',
                 900:'MixOperation'（混合操作）

    2.支持长短连接。
    3.支持域名和IP两种方式请求。
    4.支持HTTP/HTTPs两种方式配置。
    5.支持上传下载对象时计算 MD5值。
    6.支持上传对象时随机对象名，随机对象大小。
    7.性能结果统计功能：包括实时统计结果：包括在线并发数，错误率，TPS，吞吐量，实时某个时间段内的性能数据。


-- 安装&配置/INSTALL&CONFIG:
    1. 要求python环境2.6.x或2.7.x，若要测试TSL1.1以及以上版本的HTTPS加密算法，需要>=2.7.9 
    2. 如果要使用DNS域名请求方式，需要：
     a) 本程序的执行机上没有运行域名缓存服务，否则造成请求均发送到同一个OSC,查询关闭nscd的方法：service nscd status|stop
     b) 配置的域名可以正常解析。在/etc/resolv.conf内配置dns服务器。

-- 运行/HOW TO RUN:

    1. 创建测试帐户：
       配置使用AK SK鉴权，则需要在users.dat文件中按如下格式构造测试帐户供工具读取。（1个或多个，根据需要配置）
      accountName,accessKey,secretKey,
      accountName1,accessKey1,secretKey1,
      accountName2,accessKey2,secretKey2,
      ...

    2. 编辑 config.dat，配置测试模型

    3. 运行，可指定参数，指定的参数覆盖配置文件中的参数
    ./run.py  [测试用例编号] [用户数] [指定加载的配置文件]

    4. 查看结果，目录./result/：
     2013.12.05_06.14.50_HeadBucket_200_brief.txt 表示200用户并发HeadBucket操作最终测试结果。     
     2013.12.05_06.14.50_HeadBucket_200_detail.csv 表示200用户并发HeadBucket操作所有请求的详细结果。
     archive.csv 每次执行后归档的结果。
        ProcessId,UserName,RequestSq,Operation,Start_At,End_At,Latency(s),DataSend(Bytes),DataRecv(Bytes),Mark,RequestID,Response
        0,zz.account.0,1,ListUserBuckets,1394000293.383760,1394000293.409535,0.025775,0,500,,D4B110AFF9760000014490D9C2E4AB2B,200 OK
    
     2014.03.05_06.18.13_MixOperation_2_realtime.txt表示2用户并发MixOperation操作，实时间隔5秒（可配置参数StatisticsInterval）采样周期的性能统计结果。
     NO      StartTime           OK          Requests    ErrRate(%)  TPS       AvgLatency（S）   SendBytes        RecvBytes
     1       03/05 06:18:13.382  279         279         0.0         55.8      0.037           173195           100000
     2       03/05 06:18:18.382  75          75          0.0         15.0      0.13            180061           0
     3       03/05 06:18:23.382  86          86          0.0         17.2      0.116           229280           0

    
其它说明：
    1. 请求间有先后依赖关系。如上传对象需要之前运行过创建桶。
    2. 工具打印日志文件log/obsPyTool.log,日志级别在logging.conf配置文件内可配置：
        可选级别：DEBUG（所有请求的详细信息)、WARNING（>=400请求日志)、ERROR(>=500请求) 
    3. 错误码描述：
            'connection reset by peer': '9998',      # 连接类错误：服务器拒绝连接
            'broken pipe': '9997',                   # 读写过程中连接管道破裂
            'timed out': '9996',                     # 客户端等服务器端响应时间超时，时间配置参数ConnectTimeout
            'badstatusline': '9995',                 # 客户端读HTTP响应码格式错误或读到为空，常见于服务器端断开连接
            'connection timed out': '9994',          # 请求前连接建立超时
            'the read operation timed out': '9993',  # 从服务器端读响应超时
            'cannotsendrequest': '9992',             # 客户端发送请求报错
            'keyboardinterrupt': '9991',             # 键盘Ctrl+C中断请求
            'name or service not known': '9990',     # 服务器端域名无法解析
            'no route to host': '9989',              # 到服务器端IP不可达，路由错误
            'data error md5': '9901',                # 下载对象数据校验错误，也可能数据长度不正确
            'data error content-length': '9902',     # 收到消息长度与服务器端返回的content-length头域值不一致
            'other error': '9999'                    # 其它错误，参考工具日志堆栈定位。直接搜索堆栈关键字。




--  更新说明/UPDATES:
2018.2.1
1、增加段重复上传功能

2018.1.29
1、增加匿名访问功能

2018.1.16
1、增加时间窗口限制功能

2018.1.12
1、修改handle_from_obj_v的bug
2、修改虚拟主机不生效的bug

2018.1.8
1、detail文件增加x-amz-id2头域信息打印
2、log中finally result增加x-amz-id2头域信息打印

2017.11.24
1、修正205批删bug

2017.11.06
1、新增创桶，上传对象，初始化多段任务温冷头域，可随机选择

2017.11.03
1、优化分布式收集数据方式，不再统计realtime

2017.10.18
1、统计结果中加入当前工具运行目录
