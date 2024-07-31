Code Challenge: Debugging a Simple Web Server in Go

Challenge Description

You are given a simple HTTP web server written in Go. The server is intended to handle requests to two endpoints: /hello and /goodbye. The /hello endpoint should respond with "Hello, World!" and the /goodbye endpoint should respond with "Goodbye, World!".

Recently, this code was deployed to production, and users have reported that both endpoints are returning a 404 Not Found error. 

Your task is to identify and fix the issues causing this behavior.

```
package main

import (
	"fmt"
	"net/http"
)

func main() {
	http.HandleFunc("/hello", helloHandler)
	http.HandleFunc("/goodbye", goodbyeHandler)

	fmt.Println("Starting server at port 8080")
	err := http.ListenAndServe(":8080", nil)
	if err != nil {
		fmt.Println("Error starting server:", err)
	}
}

func helloHandler(w http.ResponseWriter, r *http.Request) {
	if r.Method != http.MethodGet {
		http.Error(w, "Invalid request method", http.StatusMethodNotAllowed)
		return
	}
	fmt.Println(w, "Hello, World!")
}

func goodbyeHandler(w http.ResponseWriter, r *http.Request) {
	if r.Method != http.MethodGet {
		http.Error(w, "Invalid request method", http.StatusMethodNotAllowed)
		return
	}
	fmt.Fprintln("Goodbye, World!")
}
```