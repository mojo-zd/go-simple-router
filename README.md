# go-simple-router
[![GoDoc](https://godoc.org/github.com/go-chinese-site/go-simple-router?status.svg)](http://godoc.org/github.com/go-chinese-site/go-simple-router)

golang实现的简单http路由器,用于学习理解http.

## Installation

    go get -u github.com/go-chinese-site/go-simple-router

## Support

* **全局处理函数**
* **静态路由**
* **路由分组**

## Usage

```go
package main

import (
	"fmt"
	router " github.com/go-chinese-site/go-simple-router"
	"log"
)

//GlobalHandle 全局处理函数
func GlobalHandle(c *router.Context) {
	fmt.Fprint(c.Writer, "begin GlobalHandle!\n")
	c.Next()
	fmt.Fprint(c.Writer, "end GlobalHandle!\n")
}

func Index(c *router.Context) {
	fmt.Fprint(c.Writer, "Welcome!\n")
}

//GroupHandle 分组处理函数
func GroupHandle(c *router.Context) {
	fmt.Fprint(c.Writer, "begin GroupHandle!\n")
	c.Next()
	fmt.Fprint(c.Writer, "end GroupHandle!\n")
}

func Hello(c *router.Context) {
	fmt.Fprint(c.Writer, "hello!\n")
}

func main() {
	r := router.New()
	//添加全局处理函数
	r.Use(GlobalHandle)

	r.GET("/", Index)

	//增加路由分组
	r.Group("/api", func() {
		r.GET("/hello", Hello)
	}, GroupHandle)

	log.Fatal(r.Run(":8080"))
}

```