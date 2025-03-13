

用来获取当前的总毫秒数

`System.currentTimeMillis()`

​										毫秒:millis





**RateLimiter** rt = RateLimiter.create(1, 3, TimeUnit.SECONDS);

创建一个限流器，在 **3 秒预热期**内，从初始低速率**平滑过渡**到**每秒 1 个请求**的稳态速率。