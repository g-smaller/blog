---
title: curl
date: 2017-08-26 15:45:24
categories:
- curl
tags:
- curl
---

curl是一种命令行工具，作用是发出网络请求，然后得到和提取数据，显示在"标准输出"(stdout)上。

### 支持

> DICT, FILE, FTP, FTPS, Gopher, HTTP, HTTPS, IMAP, IMAPS, LDAP, LDAPS, POP3, POP3S, RTMP, RTSP, SCP, SFTP, SMB, SMBS, SMTP, SMTPS, Telnet and TFTP

> curl 支持 SSL 证书，HTTP POST, HTTP PUT HTTP GET, FTP uploading, HTTP 表单提交, HTTP/2, 身份验证(user+password)[Basic, Plain, Digest, CRAM-MD5, NTLM, Negotiate and Kerberos]

<!--more-->

### 请求

> curl http://www.google.com

#### 参数
- `-o`
    > curl -o [文件名] http://www.google.com 
    > 把请求返回的结果，保存到文件中
- `-O`
    > curl -O -# http://blog.51yip.com/wp-content/uploads/2010/09/compare_varnish.jpg  
    > 显示下载进度
- `-L`
    > curl -L http://www.google.com
    > 跳转到新的网址
- `-i`
    > curl -i http://www.google.com
    > 显示http response的头信息，连同网页代码一起
- `-I`
    > curl -I http://www.google.com 
    > 只显示http response的头信息
- `-v`
    > curl -v http://www.google.com 
    > 显示一次http通信的整个过程，包括端口连接和http request头信息
    > 
- `--trace`
    > curl --trace [文件] http://www.google.com 
    > 查看更详细的通信过程
- `--trace--ascii`
    > curl --trace-ascii [文件] http://www.google.com 
    > 查看更详细的通信过程
- `-X`
    描述发送请求的动作(方法)
    - GET
    - POST
    - PUT
    - DELTE
    
    ##### GET请求
        > curl -X GET www.google.com?data=ff
    ##### POST请求(表单)
        > curl -X POST --data "date=33" www.google.com
        > curl -X POST --data-urlencode "date=33" www.google.com
        
        - `--data-urlencode`对curl编码
        
        设置表单数据
        > -d/--data <data>
        > -F/--form <data>
        > --form-string <data>
    ##### 文件上传
        > curl url -F file=@filepath -X POST 
    
- `-H`
    > 添加请求头
    > curl -H 'Accept-Encoding: gzip' -H 'Accept: */*' www.google.com
- `--cookie`
    > curl --cookie "key=value" www.google.com
    > 设置请求Cookie
    > `-c cookie-file`可以保存服务器返回的cookie到文件，`-b cookie-file`可以使用这个文件作为cookie信息，进行后续的请求。
- `--user`
    > curl --user name:password www.google.com
    > HTTP认证
- `--compressed`   
    > 要求返回是压缩的形势 (using deflate or gzip)
