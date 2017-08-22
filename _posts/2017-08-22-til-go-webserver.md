---
layout: post
title:  "Go - WebServer"
date:   2017-08-22 22:30:00 +0900
categories: Go, WebServer
---

간단한 웹서버
- 사용자 브라우저에서 실행되는 HTML/JS 채팅 클라이언트 제공
- 웹소켓 연결

```
import (
"log"
"net/http"
)

func main() {
    http.HandleFunc("/", func(w http.ResponseWriter, r *http.Request) {
        w.Write([]byte(`
        <html>
        <head>
            <title>Chat</title>
        </head>
        <body>
            gogogo
        </body>
        </html>
        `))
    })

    // webserver start
    if err := http.ListenAndServe(":8081", nil); err != nil {
        log.Fatal("ListenAndSerfve:", err)
    }
}
```

