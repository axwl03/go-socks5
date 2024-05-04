go-socks5
=========

Provides the `socks5` package that implements a [SOCKS5 server](http://en.wikipedia.org/wiki/SOCKS).
SOCKS (Secure Sockets) is used to route traffic between a client and server through
an intermediate proxy layer. This can be used to bypass firewalls or NATs.

Feature
=======

The package has the following features:
* "No Auth" mode
* User/Password authentication
* Support for the CONNECT command
* Rules to do granular filtering of commands
* Custom DNS resolution
* Unit tests

TODO
====

The package still needs the following:
* Support for the BIND command
* Support for the ASSOCIATE command


Example
=======

Below is a simple example of usage

```go
package main

import (
	"fmt"
	"github.com/axwl03/go-socks5"
	"os"
)

func main() {
	if len(os.Args) != 2 {
		fmt.Println("Usage: ./program 0.0.0.0:1080")
		return
	}

	// Create a SOCKS5 server
	creds := socks5.StaticCredentials{
		"user": "pass",
	}
	authenticator := socks5.UserPassAuthenticator{Credentials: creds}
	conf := &socks5.Config{
		AuthMethods: []socks5.Authenticator{authenticator},
	}

	server, err := socks5.New(conf)
	if err != nil {
		panic(err)
	}

	// Create SOCKS5 proxy on specified interface
	if err := server.ListenAndServe("tcp", os.Args[1]); err != nil {
		panic(err)
	}
}
```

