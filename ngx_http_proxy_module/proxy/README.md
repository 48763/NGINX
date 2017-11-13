# Base

## Foreword

```
Server {
  listen 80;
  ...
  location / {
    proxy_pass https://yukifans.com;
    ...
  }
  
}
```

## Table of Contents
- [proxy_pass](#proxy_pass)
- [proxy_bind](#proxy_bind)
- [proxy_http_version](#proxy_http_version)
- [proxy_hide_header](#proxy_hide_header)
- [proxy_pass_header](#proxy_pass_header)
- [proxy_ignore_headers](#proxy_ignore_headers)
- [proxy_intercept_errors](#proxy_intercept_errors)
- [proxy_ignore_client_abort](#proxy_ignore_client_abort)

## proxy_pass
設定受代理的伺服器協議(`http`|`https`)和位址，以及一個可選的 `uri` 映射到某區域。

```nginx
語法：proxy_pass URL;
預設：none
範圍：location, limit_except
```
### 範例：
#### 1. 這是最一般的設定，直接讓 `NGINX` 預設網頁位址，映射到該網站的 `/ie` 位置(目錄)。
```nginx 
location / {
  proxy_pass https://yukifans.com:443/ie;
}
```

URL 的映射關係：
```
http://ip-address/ => https://yukifans/ie
```

#### 2. `NGINX` 的 `/yuki/` 位址，映射到目標網站的 `/yuki/` 位址。
```nginx
location /yuki/ {
  proxy_pass https://yukifans.com:443;
}
```

URL 的映射關係：
```
http://ip-address/yuki/ => https://yukifans/yuki/
```
*ps. 在此範例中所連結的網頁位址是不存在的，所以請放心*

#### 3. `NGINX` 的 `/yuki/` 位址，映射到目標網站的預設位址。
```nginx
location /yuki/ {
  proxy_pass https://yukifans.com:443/;
}
```

URL 的映射關係：
```
http://ip-address/yuki/ => https://yukifans/
```

從上面幾個範例來看，可以發現當沒有指定目標網站的位址(`uri`，及是 `/`)時，將會直接引用 `location` 作為映射目標的位置。

## proxy_bind
從指定的 IP 與選定的端口發出連線到受代理的伺服器。

```nginx
語法：proxy_bind address [transparent] | off;
預設：none
範圍：http, server, location
```

## proxy_http_version
設定代理的 `HTTP` 協議版本。

```nginx
語法：proxy_http_version 1.0 | 1.1;
預設：proxy_http_version 1.0;
範圍：http, server, location
```

## proxy_hide_header
在預設中，`NGINX` 不會讓受代理的伺服器標頭欄位 - `Date`, `Server`, `X-Pad`, `X-Accel-*` 通過到達用戶端

```nginx
語法：proxy_hide_header field;
預設：none
範圍：http, server, location
```

### 範例
```nginx
location / {
  ...
  proxy_hide_header field;
  ...
}
```

*Response Hearders*
```hearder
Cache-Control:max-age=3, must-revalidate
Connection:keep-alive
Date:Fri, 06 Oct 2017 21:13:51 GMT
Server:nginx/1.4.6 (Ubuntu)
Vary:Accept-Encoding,Cookie
```
*可與 `proxy_pass_header` 一起看，比較之間的差異。*

## proxy_pass_header
允許標頭欄位從受代理的伺服器傳送到用戶端。該方法與 `proxy_hide_header` 相反。

```nginx
語法：proxy_pass_header field;
預設：none
範圍：http, server, location
```

### 範例
```nginx
location / {
  ...
  proxy_pass_header field;
  ...
}
```

*Response Hearders*
```http
Cache-Control:max-age=3, must-revalidate
Connection:keep-alive
Content-Encoding:gzip
Content-Length:5397
Content-Type:text/html; charset=UTF-8
Date:Fri, 06 Oct 2017 21:22:17 GMT
Last-Modified:Wed, 01 Nov 2017 00:56:23 GMT
Server:Apache
Vary:Accept-Encoding,Cookie
```

*可與 `proxy_hide_header` 一起看，比較之間的差異。*

## proxy_ignore_headers 
不處理受代理的伺服器所回應的某些標頭欄位。

```nginx
語法：proxy_ignore_headers field ...;
預設：none
範圍：http, server, location
```

如果沒有禁用，在處理這些標頭欄位時，將會有以下效果：
- `X-Accel-Expires`, `Expires`, `Cache-Control`, `Set-Cookie`, & `Vary`：設定回應快取的參數。
- `X-Accel-Redirect`：執行內部重新導向到指定的 `URI`。
- `X-Accel-Limit-Rate`：設置對用戶端傳送速率的限制。
- `X-Accel-Buffering`：啟用或禁用回應的緩衝。
- `X-Accel-Charset`：設定需要的字符回應。

## proxy_intercept_errors
是否讓代理狀態碼大於或等於 300 的回應，通過到用戶端，或是攔截並重新導向至 `NGINX`，讓 `error_page` 指令進行處理。

```nginx
語法：proxy_intercept_errors on | off
預設：proxy_intercept_errors off;
範圍：http, server, location
```

## proxy_ignore_client_abort
當用戶端關閉連線不等待回應時，是否要關閉與受代理的伺服器連線？

```nginx
語法：proxy_ignore_client_abort on | off;
預設：proxy_ignore_client_abort off;
範圍：http, server, location
```

### 測試

```nginx
location / {
  proxy_ignore_client_abort off;
}
```

#### off
當設定為 `off` 時，使用瀏覽器重整再停止，就可以看到下列訊息。

```bash
$ sudo vi /var/log/nginx/access.log

10.211.55.2 - - [07/Oct/2017:06:58:45 +0800] "GET /yuki/ HTTP/1.1" 499 0 "-" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_6) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/61.0.3163.100 Safari/537.36"
```

*補充：
499 - Client Closed Request*

#### on
當設定為 `on` 時，使用瀏覽器重整再停止，就可以看到下列訊息。

```bash
$ sudo vi /var/log/nginx/access.log

10.211.55.2 - - [07/Oct/2017:07:03:35 +0800] "GET /yuki/ HTTP/1.1" 200 5397 "-" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_6) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/61.0.3163.100 Safari/537.36"
```
## 

```nginx
語法：
預設：
範圍：http, server, location
```

## 建構中...
