# 使用方法

@sichem/react-axios

依赖及使用方法 [react-axios](https://www.npmjs.com/package/react-axios)

- 自动提取了 Token 与 HeaderKey 注入在Post请求中
- 增加重试请求，降低错误率，间隔1000ms
- 请求超时提示
- 自定义登录超时回调
- 自定义请求错误回调



## Custom Axios Instance

Include in your file

```
import axiosInstance from '@sichem/react-axios'
```

Config

```
axiosInstance.config({
    baseUrl:URL, // 默认URL
    retry:0, // 重试次数
    loginCode:301, // 登录状态码
    loginCall:(res)=>{ // 登录回调
        console.log(res)
    },
    errCall:(res)=>{ // 请求错误回调
        console.log(res)
    }
})
 ```


Pass down through a provider
```
<AxiosProvider instance={axiosInstance}>
  <Get url="test">
    {(error, response, isLoading, makeRequest, axios) => {
      ...
    }}
  </Get>
</AxiosProvider>
```

Or pass down through props
```
<Get url="test" instance={axiosInstance}>
  {(error, response, isLoading, makeRequest, axios) => {
    ...
  }}
</Get>
```

Retrieve from custom provider (when you need to directly use axios). The default instance will be passed if not inside an <AxiosProvider/>.
```
<AxiosProvider instance={axiosInstance}>
  <MyComponent/>
</AxiosProvider>
```


[react-axios的使用说明](https://www.npmjs.com/package/react-axios)

## Components & Properties

#### Base Request Component

```
<Request
  instance={axios.create({})} /* custom instance of axios - optional */
  method="" /* get, delete, head, post, put and patch - required */
  url="" /*  url endpoint to be requested - required */
  data={} /* post data - optional */
  params={} /* queryString data - optional */
  config={} /* axios config - optional */
  debounce={200} /* minimum time between requests events - optional */
  debounceImmediate={true} /* make the request on the beginning or trailing end of debounce - optional */
  isReady={true} /* can make the axios request - optional */
  onSuccess={(response)=>{}} /* called on success of axios request - optional */
  onLoading={()=>{}} /* called on start of axios request - optional */
  onError=(error)=>{} /* called on error of axios request - optional */
/>
```

#### Helper Components

```
    <Get ... />
    <Delete ... />
    <Head ... />
    <Post ... />
    <Put ... />
    <Patch ... />
```

## Example

Include in your file

```
import { AxiosProvider, Request, Get, Delete, Head, Post, Put, Patch, withAxios } from 'react-axios'
```

Performing a GET request

```
// Post a request for a user with a given ID
render() {
  return (
    <div>
      <Get url="/api/user" params={{id: "12345"}}>
        {(error, response, isLoading, makeRequest, axios) => {
          if(error) {
            return (<div>Something bad happened: {error.message} <button onClick={() => makeRequest({ params: { reload: true } })}>Retry</button></div>)
          }
          else if(isLoading) {
            return (<div>Loading...</div>)
          }
          else if(response !== null) {
            return (<div>{response.data.message} <button onClick={() => makeRequest({ params: { refresh: true } })}>Refresh</button></div>)
          }
          return (<div>Default message before request is made.</div>)
        }}
      </Get>
    </div>
  )
}
```
