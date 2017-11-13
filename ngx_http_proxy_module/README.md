# ngx_http_proxy_module
該模組設定請求通過到其它服務器的相關需求設定。

## Foreword
Description

## Table of contents
- [Proxy](#proxy)
- [Buffer](#buffer)
- [SSL](#ssl)

## Example

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

### [Proxy](proxy)
代理請求的代理設定。（e.g. header）

- [proxy_bind](proxy#proxy_bind)
- []()

### [Buffer](buffer)
代理請求的緩衝設定。（e.g. beffer, cache）

- [proxy_buffering](buffer#proxy_buffering)
- [proxy_buffer_size](buffer#proxy_buffer_size)
- [proxy_buffering](buffer#proxy_buffering)

### [SSL](ssl)
代理請求的憑證設定。（e.g. SSL）
- list 
