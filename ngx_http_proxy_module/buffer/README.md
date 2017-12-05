# Buffer

## Table of Contents
- [proxy_buffering](#proxy_buffering)
- [proxy_buffer_size](#proxy_buffer_size)
- [proxy_buffers](#proxy_buffers)
- [proxy_busy_buffers_size](#proxy_busy_buffers_size)
- [proxy_cache](#proxy_cache)
- [proxy_cache_background_update](#proxy_cache_background_update)
- [proxy_limit_rate](#proxy_limit_rate)


## proxy_buffering
啟用或禁用代理伺服器的請求緩衝。

```nginx
語法：proxy_buffering on | off;
預設：proxy_buffering on;
範圍：http, server, location
```
*相關：
proxy_buffer_size, 
proxy_buffers, 
proxy_max_temp_file_size, 
proxy_temp_file_write_size, 
proxy_temp_path*


## proxy_buffer_size
設定 `size`，用於讀取所收到代理伺服器回應的第一部分緩衝大小。

```nginx
語法：proxy_buffer_size size;
預設：proxy_buffer_size 4k|8k;
範圍：http, server, location
```
> 4K 或 8K 取決於平台。


## proxy_buffers
設定 `number` 和 `size`，對於單一連線，所讀取收到代理伺服器回應的緩衝區。

```nginx
語法：proxy_buffers number size;
預設：proxy_buffers 8 4k|8k;
範圍：http, server, location
```
> 4K 或 8K 取決於平台。


## proxy_busy_buffers_size
當啟用代理伺服器的回應緩衝（[proxy_buffering](#proxy_buffering)），`size` 為其緩衝的大小。 Nginx 在回應尚未讀取完時，就會將其送至用戶端。

```nginx
語法：proxy_busy_buffers_size size;
預設：proxy_busy_buffers_size 8k|16k;
範圍：http, server, location
```
> 一般 `size` 會設置為 `proxy_buffers` 單一分頁大小的兩倍。


## proxy_cache
定義給快取使用的共享記憶體區塊。

```nginx
語法：proxy_cache zone | off;
預設：proxy_cache off;
範圍：http, server, location
```


## proxy_cache_background_update
允許在背景用子請求來更新已過期的快取項目，而過期的快取請求會回傳給用戶端。

```nginx
語法：proxy_cache_background_update on | off;
預設：proxy_cache_background_update off;
範圍：http, server, location
```

## proxy_limit_rate
限制從受代理的伺服器讀取回應的速度。限制每個請求在每秒傳送多少 `Bytes`(Byte/Second)。 

```nginx
語法：proxy_limit_rate rate;
預設：proxy_limit_rate 0;
範圍：http, server, location
```
> 只有在啟用回應緩衝時，限制才有作用。
