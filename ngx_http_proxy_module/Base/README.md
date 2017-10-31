# Base

## Foreword

```
Server {
  listen 80;
  
  location / {
    proxy_pass https://yukifans.com;
  }
  
}
```

## Table of Contents
- [proxy_pass](#proxy_pass)
- [proxy_bind](#proxy_bind)
- [proxy_hide_header](#proxy_hide_header)

## proxy_pass
設定所代理的伺服器協議(`http`|`https`)和位址，以及一個可選的 `uri` 映射到某區域。

```nginx
語法：proxy_pass URL;
預設：none
範圍：location, limit_except
```
### 範例：
1. 這是最一般的設定，直接讓 `NGINX` 預設網頁位址，映射到該網站的 `/ie` 位置(目錄)。
```nginx 
location / {
  proxy_pass https://yukifans.com:443/ie;
}
```

URL 的映射關係：
```
http://ip-address/ => https://yukifans/ie
```

2. `NGINX` 的 `/yuki/` 位址，映射到目標網站的 `/yuki/` 位址。
```nginx
location /yuki/ {
  proxy_pass https://yukifans.com:443;
}
```

URL 的映射關係：
```
http://ip-address/yuki/ => https://yukifans/yuki/
```
***ps. 在此範例中所連結的網頁位址是不存在的，所以請放心***

3. `NGINX` 的 `/yuki/` 位址，映射到目標網站的預設位址。
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
從指定的 IP 與選定的端口發出連線到所代理的伺服器。

```nginx
語法：proxy_bind address [transparent] | off;
預設：none
範圍：http, server, location
```

## proxy_hide_header field
在預設中，`NGINX` 不會讓所代理的伺服器標頭欄位 - `Date`, `Server`, `X-Pad`, `X-Accel-*` 通到用戶端
```nginx
語法：proxy_hide_header field;
預設：none
範圍：http, server, location
```

## 建構中...
