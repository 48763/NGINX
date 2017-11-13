# Buffer

## Table of Contents
- [proxy_buffering](#proxy_buffering)
- [proxy_buffer_size](#proxy_buffer_size)
- [proxy_buffers](#proxy_buffers)
- [proxy_limit_rate](#proxy_limit_rate)

## proxy_buffering
啟用或禁用代理伺服器的請求緩衝。

```nginx
語法：proxy_buffering on | off;
預設：proxy_buffering on;
範圍：http, server, location
```
*相關：proxy_buffer_size, proxy_buffers, proxy_max_temp_file_size, proxy_temp_file_write_size, proxy_temp_path*

## proxy_buffer_size
設定 `size`，用於讀取所收到代理伺服器回應的第一部分緩衝大小。

```nginx
語法：proxy_buffer_size size;
預設：proxy_buffer_size 4k|8k;
範圍：http, server, location
```
*p.s 4K 或 8K 取決於平台。*

## proxy_buffers
設定 `number` 和 `size`，對於單一連線，所讀取收到代理伺服器回應的緩衝區。

```nginx
語法：proxy_buffers number size;
預設：proxy_buffers 8 4k|8k;
範圍：http, server, location
```
*p.s 4K 或 8K 取決於平台。*

## proxy_limit_rate
限制從受代理的伺服器讀取回應的速度。限制每個請求在每秒傳送多少 `Bytes`(Byte/Second)。 

```nginx
語法：proxy_limit_rate rate;
預設：proxy_limit_rate 0;
範圍：http, server, location
```
*p.s 只有在啟用回應緩衝時，限制才有作用。*
