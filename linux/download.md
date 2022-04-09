
## curl

1. get
```
curl  https://www.baidu.com
```

2. post
```
for /l %i in (1,1,1000) do
  curl -d "user=xman&passwd=yyyyy" https://www.baidu.com
```

3. 下载
```
curl ftp://ftp.funet.fi/README
##  保存 -o 参数
curl -o thatpage.html http://www.netscape.com/
```

4. 配置
- user-agent
```
curl -A "Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/535.1 (KHTML, like Gecko) Chrome/14.0.835.163 Safari/535.1"  www.baidu.com
http://localhost:8080/Restful/Archive/sayHello
```





