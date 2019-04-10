- CORS会不会携带Cookie？
  - 默认不会，标准的CORS请求不对cookies做任何事情，既不发送也不改变cookie。如果希望改变这一情况，就需要将withCredentials设置为true。
- JSONP会不会携带cookie？
  - 会发送，所以容易引起CSRF攻击
- Fetch是什么
- Fetch和AJAX的区别在哪里，优势在哪里
  - 当接收到一个代表错误的 HTTP 状态码时，从 `fetch()`返回的 Promise **不会被标记为 reject，** 即使该 HTTP 响应的状态码是 404 或 500。相反，它会将 Promise 状态标记为 resolve （但是会将 resolve 的返回值的 `ok` 属性设置为 false ），仅当网络故障时或请求被阻止时，才会标记为 reject。
  - 默认情况下，`fetch` **不会从服务端发送或接收任何 cookies**, 如果站点依赖于用户 session，则会导致未经认证的请求（要发送 cookies，必须设置 [credentials](https://developer.mozilla.org/zh-CN/docs/Web/API/GlobalFetch/fetch#%E5%8F%82%E6%95%B0) 选项）。
- 

