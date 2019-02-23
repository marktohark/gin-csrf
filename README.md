# gin-csrf [![Build Status](https://travis-ci.org/utrack/gin-csrf.svg?branch=master)](https://travis-ci.org/utrack/gin-csrf)

CSRF protection middleware for [Gin]. This middleware has to be used with [gin-contrib/sessions](https://github.com/gin-contrib/sessions).

Original credit to [tommy351](https://github.com/tommy351/gin-csrf) and [utrack](https://github.com/utrack/gin-csrf)

## What did I make?
following condition will update csrf token
1. every ignore methods (ex. "GET","HEAD"...)
2. token is valid
3. session have not csrf salt

add whitelist Url and blacklist Url
whitelist is unconditional valid.
blacklist is skip "IgnoreMethods" condition and mandatory verification.

## Installation

``` bash
$ go get github.com/utrack/gin-csrf
```

## Usage

``` go
import (
    "errors"

    "github.com/gin-gonic/gin"
    "github.com/gin-contrib/sessions"
    "github.com/utrack/gin-csrf"
)

func main(){
    r := gin.Default()
    store := cookie.NewStore([]byte("cookie_secret"))
    r.Use(sessions.Sessions("session_name_in_cookie", store))
    r.Use(csrf.Middleware(csrf.Options{
	Secret: "csrf_secret",
	ErrorFunc: func(c *gin.Context){
		c.String(400, "CSRF token mismatch")
		c.Abort()
	},
    }))

    r.GET("/protected", func(c *gin.Context){
        c.String(200, csrf.GetToken(c))
    })

    r.POST("/protected", func(c *gin.Context){
        c.String(200, "CSRF token is valid")
    })
}
```

[Gin]: http://gin-gonic.github.io/gin/
[gin-sessions]: https://github.com/utrack/gin-sessions
