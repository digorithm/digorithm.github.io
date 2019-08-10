---
layout: post
title: Golang + TLS for the lazy engineer
keywords: "architecture, software, systems, async, kafka, go, golang"
tags: [architecture, software, thoughts, systems]
fullview: true
permalink: post/golang-tls
categories: [software-architecture, systems]
---
There are copious amounts of blog posts that talk about what's TLS, but those never really caught my attention when I was starting with Go. I won't go over in details about what is TLS and how it works, there's plenty resource out there for that (links). Instead, I'm going to show you why TLS is a good and practical idea that can be done super quickly. 

In a simplistic way, TLS is a cryptographic protocol that provides end-to-end communication security. In simpler terms, when your requests fly through the internet, TLS makes them encrypted rather than plain-text.

Let's start with an insecure version of a client sending data to a server over a POST request.

Server code:
```golang
package main

import (
	"io"
	"io/ioutil"
	"log"
	"net/http"
)

func helloHandler(w http.ResponseWriter, r *http.Request) {
	
	// Read incoming request 
	body, err := ioutil.ReadAll(r.Body)
	if err != nil {
		log.Fatalln(err)
	}

	log.Println(string(body))

	// Write "Hello, world!" to the response body
	io.WriteString(w, "Hello, world!\n")
}

func main() {
	// Set up a /hello resource handler
	http.HandleFunc("/hello", helloHandler)

	// Listen to port 8282 and wait
	log.Fatal(http.ListenAndServe(":8282", nil))
}
```
Client code:
```golang
package main

import (
	"bytes"
	"encoding/json"
	"fmt"
	"io/ioutil"
	"log"
	"net/http"
)

func main() {
	// Request /hello over port 8080 via a POST method and send some payload
	reqBody, _ := json.Marshal(map[string]string{"name": "rodrigo"})
	r, err := http.Post("http://localhost:8282/hello", "application/json", bytes.NewBuffer(reqBody))
	if err != nil {
		log.Fatal(err)
	}

	// Read the response body
	defer r.Body.Close()
	body, err := ioutil.ReadAll(r.Body)
	if err != nil {
		log.Fatal(err)
	}

	// Print the response body to stdout
	fmt.Printf("%s\n", body)
}
```

Nothing new here, dead simple client-server communication. At this point, many people ask themselves: *why is this insecure*? I could just say that the data being exchanged between the client and server are not encrypted and can be intercepted by a middleman. Well, that is certainly not very convincing, why would you believe me? 

Let's act as the middleman and use wireshark to sniff the communication between the server and the client:

![](/content/images/images/Untitled-3433b94a-77d6-4245-b313-b418640b61c5.png)

Here we see the client/server doing the TCP dance: syn, acks, and fin. In between we see the actual post request to `/hello`. Let's see the data inside that request. The problematic bit is:

![](/content/images/images/Untitled-024e15d3-f96f-45db-9558-67aac6bc9e9a.png)

Right here we can see the plaintext being transferred between them. That's no bueno. Very, very  insecure websites could put your login details there, or, God forbid, your credit card information. After all, these bits of information must go from the client (you, using their website) to their servers. So it better be encrypted.

Many people think this is a hard thing to  do. It is not. It's dead simple, and Go makes it even simpler.

Let's see the secure version: 

First we need to generate a certificate, which will be shared between the client service and the server service:

    openssl req -newkey rsa:2048 \
      -new -nodes -x509 \
      -days 3650 \
      -out cert.pem \
      -keyout key.pem \
      -subj "/C=US/ST=California/L=Mountain View/O=Your Organization/OU=Your Unit/CN=localhost"

I won't go over the details here. You need to know that it generates the `cert.pem` and the `key.pem`. 2 files. That's it.

This is how the server will look like, you'll need to change from `http.Listen(...)` to:

```golang
http.ListenAndServeTLS(":8282", "cert.pem", "key.pem", nil)
```

The client changes are bit more elaborate:
```golang
package main

import (
	"bytes"
	"crypto/tls"
	"crypto/x509"
	"encoding/json"
	"fmt"
	"io/ioutil"
	"log"
	"net/http"
)

func main() {

	// Create a CA certificate pool and add cert.pem to it
	caCert, err := ioutil.ReadFile("cert.pem")
	if err != nil {
		log.Fatal(err)
	}
	caCertPool := x509.NewCertPool()
	caCertPool.AppendCertsFromPEM(caCert)

	// Create a HTTPS client and supply the created CA pool
	client := &http.Client{
		Transport: &http.Transport{
			TLSClientConfig: &tls.Config{
				RootCAs: caCertPool,
			},
		},
	}

	reqBody, _ := json.Marshal(map[string]string{"name": "rodrigo"})

	r, err := client.Post("https://localhost:8282/hello", "application/json", bytes.NewBuffer(reqBody))
	if err != nil {
		log.Fatal(err)
	}

	// Read the response body
	defer r.Body.Close()
	body, err := ioutil.ReadAll(r.Body)
	if err != nil {
		log.Fatal(err)
	}

	// Print the response body to stdout
	fmt.Printf("%s\n", body)
}
```

But that's it. I promise that is just it. Now, Let's sniff the communication between the client and server again:

![](/content/images/images/Untitled-73a44730-3806-45e3-8f1a-a90872feb873.png)

Now we see an additional dance: not only TCP, but also the TLS dance, working its magic. The cool part is, the data that we send from the client to the server is now encrypted and we can't read it!

![](/content/images/images/Untitled-72615ad7-f5f1-40a9-a624-c10041aeade5.png)