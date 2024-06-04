> [Haskell Snippets](../../README.md) / [02. Haskell 기초](../README.md) / [각종 오류상황 대처하기](README.md) / stack new, stack init fail.md
## stack new, stack init fail
[https://stackoverflow.com/questions/57516944/stack-new-command-failing-to-download-build-plan-for-lts-14-1](https://stackoverflow.com/questions/57516944/stack-new-command-failing-to-download-build-plan-for-lts-14-1)

```
$ stack new my-project

[...]

Downloading lts-14.1 build plan ...
RedownloadInvalidResponse Request {
  host                 = "raw.githubusercontent.com"
  port                 = 443
  secure               = True
  requestHeaders       = []
  path                 = "/fpco/lts-haskell/master//lts-14.1.yaml"
  queryString          = ""
  method               = "GET"
  proxy                = Nothing
  rawBody              = False
  redirectCount        = 10
  responseTimeout      = ResponseTimeoutDefault
  requestVersion       = HTTP/1.1
}
 "/home/michid/.stack/build-plan/lts-14.1.yaml" (Response {responseStatus = Status {statusCode = 404, statusMessage = "Not Found"}, responseVersion = HTTP/1.1, responseHeaders = [("Content-Security-Policy","default-src 'none'; style-src 'unsafe-inline'; sandbox"),("Strict-Transport-Security","max-age=31536000"),("X-Content-Type-Options","nosniff"),("X-Frame-Options","deny"),("X-XSS-Protection","1; mode=block"),("X-GitHub-Request-Id","10DA:4457:1D507:285B9:5D55DA2D"),("Content-Length","15"),("Accept-Ranges","bytes"),("Date","Thu, 15 Aug 2019 22:18:21 GMT"),("Via","1.1 varnish"),("Connection","keep-alive"),("X-Served-By","cache-mxp19828-MXP"),("X-Cache","MISS"),("X-Cache-Hits","0"),("X-Timer","S1565907502.529821,VS0,VE176"),("Vary","Authorization,Accept-Encoding"),("Access-Control-Allow-Origin","*"),("X-Fastly-Request-ID","9f869169dd207bbd8bb8a8fd4b274acf6580ba4f"),("Expires","Thu, 15 Aug 2019 22:23:21 GMT"),("Source-Age","0")], responseBody = (), responseCookieJar = CJ {expose = []}, responseClose' = ResponseClose})
```


Solution

```
stack upgrade 
```