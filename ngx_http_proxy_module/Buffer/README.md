# Buffer

## Foreword

```nginx
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
- [proxy_limit_rate](#proxy_limit_rate)

## proxy_limit_rate
限制從受代理的伺服器讀取回應的速度。限制每個請求在每秒傳送多少 `Bytes`(Byte/Second)。 

```nginx
語法：proxy_limit_rate rate;
預設：proxy_limit_rate 0;
範圍：http, server, location
```
*p.s 只有在啟用回應緩衝時，限制才有作用。*
