# fasthttp-reverse-proxy
![](https://img.shields.io/badge/LICENSE-MIT-blue.svg) [![Go Report Card](https://goreportcard.com/badge/github.com/yeqown/fasthttp-reverse-proxy)](https://goreportcard.com/report/github.com/yeqown/fasthttp-reverse-proxy) [![GoReportCard](https://godoc.org/github.com/yeqown/fasthttp-reverse-proxy?status.svg)](https://godoc.org/github.com/yeqown/fasthttp-reverse-proxy)

reverse http proxy hander based on fasthttp.

## features:

* [x] proxy client has `pool` supported

* [x] faster than golang standard `httputil.ReverseProxy`

* [x] simple warpper of `fasthttp.HostClient` 

* [x] websocket proxy

## quick start

#### HTTP
```go
import (
	"log"

	"github.com/valyala/fasthttp"
	proxy "github.com/yeqown/fasthttp-reverse-proxy"
)

var (
	proxyServer = proxy.NewReverseProxy("localhost:8080")
)

// ProxyHandler ... fasthttp.RequestHandler func
func ProxyHandler(ctx *fasthttp.RequestCtx) {
	// all proxy to localhost
	proxyServer.ServeHTTP(ctx)
}

func main() {
	if err := fasthttp.ListenAndServe(":8081", ProxyHandler); err != nil {
		log.Fatal(err)
	}
}
```

#### Websocket

```go
import (
	"log"
	"text/template"

	"github.com/valyala/fasthttp"
	proxy "github.com/yeqown/fasthttp-reverse-proxy"
)

var (
	proxyServer = proxy.NewWSReverseProxy("localhost:8080", "/echo")
)

// ProxyHandler ... fasthttp.RequestHandler func
func ProxyHandler(ctx *fasthttp.RequestCtx) {
	switch string(ctx.Path()) {
	case "/":
		proxyServer.ServeHTTP(ctx)
	default:
		ctx.Error("Unsupported path", fasthttp.StatusNotFound)
	}
}

func main() {
	if err := fasthttp.ListenAndServe(":8081", ProxyHandler); err != nil {
		log.Fatal(err)
	}
}
```

## usage

* [use it alone](./examples/fasthttp-reverse-proxy/proxy.go)
* [use it with pool](./examples/fasthttp-reverse-proxy-with-pool/pool.go)
* [websocket](./examples/ws-fasthttp-reverse-proxy)

## contrast

* [HTTP benchmark](./docs/http-benchmark.md)
* [Websocket benchmark](./docs/ws-benchmark.md)

## links:

* [fasthttp](https://github.com/valyala/fasthttp)
* [standard httputil.ReverseProxy](https://golang.org/pkg/net/http/httputil/#ReverseProxy)
* [fasthttp/websocket](https://github.com/fasthttp/websocket)
* [koding/websocketproxy](https://github.com/koding/websocketproxy)
